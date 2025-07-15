const axios = require('axios');
const fs = require('fs');

// Replace with your CoinMarketCap API key
const API_KEY = 'YOUR_API_KEY_HERE'; // 👈 Replace with your actual API key

const headers = {
  'X-CMC_PRO_API_KEY': API_KEY,
};

const tokenData = [];

async function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function fetchTokenBatch(start) {
  const listingsUrl = `https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest?start=${start}&limit=100`;

  try {
    const listingResponse = await axios.get(listingsUrl, { headers });
    const coins = listingResponse.data.data;

    for (const coin of coins) {
      const coinId = coin.id;
      const name = coin.name;

      // Delay to respect rate limits
      await delay(3000);

      const infoUrl = `https://pro-api.coinmarketcap.com/v1/cryptocurrency/info?id=${coinId}`;
      const infoResponse = await axios.get(infoUrl, { headers });
      const websiteList = infoResponse.data.data[coinId.toString()].urls.website;
      const website = websiteList && websiteList.length ? websiteList[0] : 'N/A';

      tokenData.push({ name, website });
      console.log(`Fetched: ${name}`);
    }

  } catch (error) {
    console.error(`❌ Error fetching batch starting at ${start}:`, error.message);
  }
}

async function fetchAllTokens() {
  console.log("🚀 Starting to fetch top 1000 tokens...");

  for (let start = 1; start <= 1000; start += 100) {
    console.log(`🔄 Fetching tokens ${start} to ${start + 99}...`);
    await fetchTokenBatch(start);
  }

  saveToCSV();
}

function saveToCSV() {
  const header = "Token Name,Website\n";
  const rows = tokenData.map(t => `"${t.name}","${t.website}"`).join("\n");
  const csv = header + rows;

  fs.writeFileSync("top_1000_tokens.csv", csv);
  console.log("✅ Done! Saved to top_1000_tokens.csv");
}

fetchAllTokens();
