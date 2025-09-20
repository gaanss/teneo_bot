# Teneo Farm Bot 🚀

**Automated Teneo Protocol farming bot with advanced features**

<p align="center">
  <a href="https://t.me/gans_software">
    <img src="https://img.shields.io/badge/Telegram-Channel-blue?style=for-the-badge&logo=telegram" alt="Telegram Channel">
  </a>
  <a href="https://t.me/ganssoftwarechat">
    <img src="https://img.shields.io/badge/Telegram-Chat-blue?style=for-the-badge&logo=telegram" alt="Telegram Chat">
  </a>
</p>

> **Note:** This is paid software. 
> To purchase, contact via Telegram: [@gaansss](https://t.me/gaansss)

A Python-based automation tool for Teneo Protocol that mimics browser extension behavior to maximize point earning efficiency. Professional-grade farming bot with advanced security and scalability features.

![Interface](interface.png)

## Features ✨

- 🔄 **Automated Farming** - Continuous point collection with WebSocket connections
- ⏰ **Smart Timing** - Internal 15-minute heartbeat timer matching browser behavior  
- 🔐 **Secure Authentication** - Captcha solving and token management
- 🌐 **Proxy Support** - 1:1 proxy-to-account distribution for scalability
- 📊 **Database Integration** - PostgreSQL for account and statistics tracking
- 🛡️ **Code Protection** - PyArmor obfuscation for security
- 🚀 **Multi-threading** - Concurrent farming across multiple accounts
- 📱 **Cross-platform** - Windows and Linux executable builds
- 👤 **Account Registration** - Automated registration with referral code support
- 💼 **Wallet Integration** - ETH wallet linking to accounts
- 📝 **Comprehensive Logging** - Detailed logging system with rotation

## Technical Stack 💻

- **Language**: Python 3.11+
- **WebSocket**: Real-time farming connections  
- **Database**: PostgreSQL with asyncpg
- **Security**: PyArmor obfuscation, proxy rotation
- **Build**: PyInstaller for single-file executables
- **Platforms**: Windows and Linux

## Architecture 🏗️

Modular design with separate components for:
- **Farming Module** - WebSocket connections with browser-like behavior
- **Authentication** - Secure login and token management
- **Proxy Handler** - Intelligent proxy rotation and management
- **Captcha Solver** - Automated captcha solving (2captcha/capsolver)
- **Wallet Manager** - ETH wallet integration and management
- **Database** - Account statistics and data persistence

## Installation 🔧

1. Download the executable file for your platform
2. Edit configuration file `settings.yaml` (see example below)

## Configuration ⚙️

Edit the `settings.yaml` to match your environment:

```yaml
database:
  host: your_database_host
  port: 5432
  name: your_database_name
  user: your_database_user
  password: your_database_password

captcha:
  solver: "2captcha"      # or "capsolver"
  api_key: "YOUR_API_KEY"

registration:
  threads: 1              # concurrent registrations
  delay_min: 1            # minimum delay between registrations (seconds)
  delay_max: 5            # maximum delay between registrations (seconds)

farming:
  threads: 5              # concurrent farming connections
  delay_min: 10           # minimum delay before each farming start (seconds)
  delay_max: 15           # maximum delay before each farming start (seconds)
  heartbeat_interval: 900 # internal heartbeat timer (15 minutes = 900 seconds)
  ping_interval: 10       # WebSocket ping interval (seconds)
  initial_delay: 1        # initial delay after connection (seconds)
  passive_data_enabled: false # enable passive data collection

email:
  mode: "single_imap"     # single_imap | multi_imap | forwarding
  single_imap:
    host: "imap.example.com"
  multi_imap:
    gmail: "imap.gmail.com"
    outlook: "imap.mail.outlook.com"
    yahoo: "imap.mail.yahoo.com"
  forwarding:
    host: "imap.gmail.com"
    username: your_email@example.com
    password: your_email_password

logging:
  level: INFO             # DEBUG | INFO | WARNING | ERROR
  retention_days: 7       # days to keep log files
  path: ./logs            # directory for logs
  debug: false            # enable debug-level logs

paths:
  registrations: ./data/registrations.txt
  proxies: ./data/proxies.txt
  wallets: ./data/private_keys.txt
  accounts: ./data/accounts.txt
```

### Data Files Format

1. `data/registrations.txt` - list of accounts for registration:
   ```
   email:password
   ```

2. `data/reff_codes.txt` - list of referral codes:
   ```
   code
   ```

3. `data/proxies.txt` - list of proxies (one per line):
   ```
   username:password@host:port
   ```

4. `data/farming.txt` - list of accounts for farming:
   ```
   email:password
   ```

5. `data/task.txt` - list of accounts for task processing:
   ```
   email:password
   ```

6. `data/wallet_connect.txt` - list of accounts for wallet linking:
   ```
   email:password
   ```

7. `data/authorizations.txt` - list of accounts for updating account data:
   ```
   email:password
   ```

8. `data/private_keys.txt` - list of Ethereum private keys:
   ```
   private_key
   ```

## License System 🔑

The application uses a license system to authenticate users:

1. When you run the application for the first time, you will be prompted to enter your license key
2. The license key will be verified with the license server
3. After successful verification, your license key will be saved in the `config.json` file

If you don't have a license key, please contact the developer.

## Usage 🖥️

### Running the Executable

#### Windows:
1. Run `teneo_farm.exe` by double-clicking it or from command line:
   ```
   teneo_farm.exe
   ```

#### Linux:
1. Make sure the file has execution permissions:
   ```bash
   chmod +x teneo_farm
   ```
2. Run the program:
   ```bash
   ./teneo_farm
   ```

## Main Operations 📝

- **Register new accounts** – load from `data/registrations.txt`, register on Teneo, and save tokens to database.
- **Re-verify unverified accounts** – retry email verification for failed registrations listed in `results/unverified.txt`.
- **Start farming** – load from `data/farming.txt` and perform WebSocket farming sessions with internal heartbeat timer.
- **Check and claim tasks** – load from `data/task.txt` and process tasks for each account.
- **Link wallets** – load from `data/wallet_connect.txt` and link ETH wallets to accounts.
- **Update account data** – load from `data/authorizations.txt`, authenticate accounts, and update their stats.
- **Export account statistics** – export all account data and stats to a CSV file in `results/`.

## Farming Behavior 🎯

The bot precisely mimics browser extension behavior:

1. **Connection Setup** - Establishes WebSocket connection with proper headers and authentication
2. **Initial Requests** - Sends config version and getMyPoints requests upon connection
3. **Heartbeat Timer** - Internal 15-minute timer sends getMyPoints → heartbeatComplete sequence
4. **Server Response** - Handles server pulses and updates account statistics
5. **Ping System** - Regular ping messages to maintain connection health
6. **Reconnection Logic** - Smart reconnection with exponential backoff on failures

### Email Requirements ⚠️

For account registration, you need access to the email accounts for verification. The application supports different IMAP modes:

- **Single IMAP Mode**: One IMAP server to all domain
- **Multi IMAP Mode**: Each account type (gmail, outlook, etc.) has its own IMAP settings
- **Forwarding Mode**: All verification emails are forwarded to a single account

Configure the IMAP settings in `settings.yaml` under the `email` section.

### Proxy Support 🌐

The application supports using proxies to avoid IP-based rate limiting. Configure proxy settings in `settings.yaml`:

- Enable/disable proxy usage
- Automatic proxy rotation on connection errors

Add your proxies to `data/proxies.txt` in the format:
```
username:password@host:port
```

## Database Integration 🗄️

The application uses PostgreSQL database to store:
- Account information
- Farming statistics
- Referral codes

Configure database connection in `settings.yaml` under the `database` section.

### Free Neon Database Option

You can use a free [Neon](https://neon.tech) PostgreSQL database for this application:

1. Create a free account at [neon.tech](https://neon.tech)
2. Create a new project
3. Get your connection details from the dashboard
4. Update your `settings.yaml` with these credentials:
   ```yaml
   database:
     host: your-neon-hostname.neon.tech
     port: 5432
     name: your_database_name
     user: your_username
     password: your_password
     ssl_mode: require
   ```

Neon provides a generous free tier with:
- Unlimited PostgreSQL databases
- 3 GiB of storage
- Auto-scaling compute
- No credit card required

## Troubleshooting 🔍

- If you encounter any issues with the license verification, check your internet connection
- Ensure the `settings.yaml` file is properly configured
- Check the logs in the `logs` directory for detailed error information
- For database connection issues, verify your PostgreSQL credentials and network configuration

## Support 📞

For support, please contact the developer via Telegram: [@gaansss](https://t.me/gaansss) 
