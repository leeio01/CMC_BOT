# 🚀 CoinMarketCap Token Scraper (Top 1000)

This Node.js bot fetches the **top 1000 tokens** from CoinMarketCap along with their **names and official websites**, saving the result to a `.csv` file.

---

## 📌 Features ##

- ✅ Fetches top 1000 cryptocurrencies
- 🌐 Extracts official website for each token
- 💾 Saves data to `top_1000_tokens.csv`
- 🔐 Respects CoinMarketCap free API rate limits

---

## 📁 Project Structure

CMC_BOT/
├── fetch_top_1000.js # Main bot script
├── top_1000_tokens.csv # Output file (generated after run)
├── package.json # Project configuration
├── .gitignore # Ignores node_modules and secrets
└── README.md # Project documentation


---

## 🛠 Requirements

- Node.js (v16 or higher)
- A free [CoinMarketCap API key](https://coinmarketcap.com/api)

---

## 🚀 How to Use

### 1. Set Up the Project

```bash
mkdir CMC_BOT
cd CMC_BOT
npm init -y
npm install axios

### 2. Add the Script
Create a file named index.js and paste the full bot code there.

🔑 Replace this line in the script:
const API_KEY = 'YOUR_API_KEY_HERE'; // with your actual API key.

### 3. Run the Bot
node index.js
⏳ It may take 30–40 minutes due to rate limits.

📦 Output
The script will generate a file:
top_1000_tokens.csv

Sample format:
Token Name,Website
Bitcoin,https://bitcoin.org
Ethereum,https://ethereum.org
...

🧠 Notes
Each token info request is delayed by 3 seconds to stay within the free tier limit (30 requests per minute).

Tokens are fetched in batches of 100 to collect all 1000 efficiently.

🧡 License
This project is open-source and free to use under the MIT License.
