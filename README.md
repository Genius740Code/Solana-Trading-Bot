# Solana Telegram Bot

A Telegram bot for Solana token trading and wallet management.

## Features

- 🔐 Secure wallet generation and storage with strong encryption
- 💎 Real-time SOL price updates via CoinGecko API (free tier)
- 🔍 Token analysis with Helius API (marketcap, liquidity, volume)
- 📊 Position tracking and management with P/L display
- 📝 Limit order functionality
- 👥 Referral system with 11% fee discount and 35% referral earnings
- ⚙️ Customizable user settings
- 💳 Wallet management and balance display
- 📈 Take profit/stop loss settings
- 🔔 Price alerts


## Prerequisites

- Node.js 14+
- MongoDB Atlas account (free tier works fine)
- Telegram Bot Token (from BotFather)
- Helius API Key (from helius.xyz)

## Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd telegrambot
```

2. Install dependencies:

```bash
npm install
```

3. Set up your environment variables in the `.env` file:

```
BOT_TOKEN=<your_telegram_bot_token>
MONGODB_URI=<your_mongodb_connection_string>
EXTENSION_DB_URI=<your_extension_db_connection_string>
ENCRYPTION_KEY=<your_encryption_key>
HELIUS_API_KEY=<your_helius_api_key>
```

4. Generate a secure encryption key:

```bash
npm run generate-key
```

This will automatically update your `.env` file with a secure key.

5. Start the bot:

```bash
npm start
```

For development with auto-reload:

```bash
npm run dev
```

## Security Features

### Extension Verification Codes

The bot implements secure verification codes for the browser extension:

- Verification codes are dynamically generated 40-character strings
- Codes expire after 5 minutes for enhanced security
- Codes are deleted after successful use (one-time use only)
- Extension users must exist in the main user database
- Weekly automatic logout for enhanced security

### Database Architecture

The system uses a two-database architecture for enhanced security:

1. **Main User Database**: Stores user accounts, wallets, and trading data
2. **Extension Database**: Stores extension-specific user data and verification

These databases are linked by the telegramId field, ensuring that:
- Only verified Telegram users can use the extension
- User settings sync between the bot and extension
- Trading preferences are consistent across platforms

## Database Management

### Setting Up MongoDB

For development and testing, you need a MongoDB database. You have two options:

1. **Local MongoDB**: Install MongoDB locally and use `mongodb://localhost:27017/telegrambot`
2. **MongoDB Atlas**: Use the free cloud-hosted MongoDB (recommended)

If you don't have MongoDB set up, run the setup helper:

```bash
# Windows
setup-test-db.bat

# Unix/Linux/Mac
node src/cli/setupDatabase.js --setup
```

This will guide you through setting up a free MongoDB Atlas database.

### Seeding Test Data

For development and testing, there are two ways to populate the database with test data:

#### 1. Using the CLI Tools (Command Line Interface)

```bash
# Add test data without dropping existing collections
npm run seed-db

# Drop existing collections and recreate test data (CAUTION!)
npm run seed-db:drop

# Create a specific number of test users (e.g., 10)
npm run seed-db:count 10
# or with drop option
node src/cli/seedDatabase.js --drop --count=10

# Use a specific MongoDB connection string
node src/cli/seedDatabase.js --uri=mongodb://username:password@hostname/database
```

Alternatively, you can use the Windows batch file:

```bash
# Add test data
seed-db.bat

# Drop existing collections and recreate test data
seed-db.bat --drop

# Create a specific number of test users
seed-db.bat --count=10
# or with drop option
seed-db.bat --drop --count=10

# Use a specific MongoDB connection string
seed-db.bat --uri=mongodb://username:password@hostname/database
```

#### 2. Using the Interactive Database Population Tool

For a more interactive experience, you can use the database population tool that will prompt you for the number of users to create:

**Option 1: Using the wrapper script (easiest):**
```bash
# On Windows/Mac/Linux:
node populate-db.js
```

**Option 2: Using the batch file (Windows only):**
```bash
# Double-click on populate-db.bat
# Or run from command prompt:
populate-db.bat
```

**Option 3: Running the script directly:**
```bash
# From the telegrambot directory:
node tools/populateDatabase.js
```

The interactive tool will:
1. Prompt you for the number of users to generate
2. Create random users with wallets, referral codes, etc.
3. Save them to your database

## Troubleshooting

### Database Connection Issues

If you see MongoDB connection errors:

1. Check your `MONGODB_URI` in the `.env` file
2. Make sure your IP address is whitelisted in MongoDB Atlas
3. The bot includes a retry mechanism for database connections

### CoinGecko API Issues

The bot now uses the public CoinGecko API which has rate limits. If you're experiencing issues:

1. Consider upgrading to CoinGecko Pro and updating the code in `utils/wallet.js`
2. The bot includes a fallback mechanism for SOL price

### Telegram Bot Issues

If the bot isn't responding:

1. Make sure your `BOT_TOKEN` is correct
2. Check that your bot is running (`npm start`)
3. Try sending `/start` to reset the bot state

## Bot Commands

- `/start` - Start the bot and show main menu
- `/buy` - Buy new tokens
- `/sell` - Sell tokens from your positions
- `/help` - Show help information
- `/settings` - Configure bot settings
- `/positions` - View your trading positions
- `/orders` - View your limit orders
- `/referrals` - View and manage referrals
- `/wallets` - Manage your wallets

The extension verification code feature is only available through the extension for enhanced security.

## Trading Features

### Buy and Sell

The bot implements buy and sell functionality for any Solana token with:
- 0.8% trading fee (0.712% with referral)
- Real-time price data from Helius
- Transaction confirmations

### Positions

View your open positions with:
- Current P/L calculations (both absolute and percentage)
- Token information and current price
- Total portfolio value

### Limit Orders

Create limit orders to:
- Buy at a certain price
- Sell when price reaches targets
- Set take profit and stop loss

## Referral System

The referral system includes:
- 11% discount on fees for referred users (0.8% → 0.712%)
- 35% of fees as earnings for referrers
- Custom referral links and tracking

## Security Considerations

- Private keys are encrypted with AES-128-GCM (using 64-bit derived keys)
- Rate limiting to prevent API abuse
- Error handling and logging

## Development Notes

- The bot is designed to handle 1000+ concurrent users
- Code is organized in a modular way for easy maintenance
- Logs are stored in the `logs` directory

## License

MIT

## Disclaimer

This bot is for educational purposes only. Use at your own risk. The creator is not responsible for any financial losses incurred through the use of this software.

## Configuration

You can adjust various settings in the `utils/constants.js` file:
- API timeouts and retries
- Wallet balance thresholds
- Fee percentages
- Security settings

For DNS resolution issues, try adding these DNS servers to your network configuration:
- 8.8.8.8 (Google)
- 1.1.1.1 (Cloudflare) 