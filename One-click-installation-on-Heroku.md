Okta is preconfigured and optimised to run securely on [Heroku](https://www.heroku.com/). The Hobby account is cheap at \$7/pm per server if running constantly.

# Installation

To instantly deploy your own instance of the application, just click the button below. The Heroku installer will talk you through the process.

To deploy the last **stable release**:

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/gruelbox/orko/tree/stable)

To deploy the latest **development snapshot**:

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/badgerwithagun/orko)

If that doesn't work, or you don't trust it, you can [install the application manually](Manual-installation-on-Heroku).

# Enable notifications on your phone

Want your phone to beep when stuff happens?

[Read more...](Telegram-Notifications)

# Complete security setup

Your application is not fully secure yet.  Feel free to play with it, but don't add your API keys and play with real money until you have [completed the security setup](Enable-two-factor-authentication).

# Add your API keys

Once your application is secure, you can now set up the following environment variables to start trading against your exchange accounts.

| Variable            | Set to                     |
| ------------------- | -------------------------- |
| `GDAX_API_KEY`      | Your GDAX API key.         |
| `GDAX_SECRET`       | Your GDAX secret.          |
| `GDAX_PASSPHRASE`   | Your GDAX passphrase.      |
| `BINANCE_API_KEY`   | Your Binance API key.      |
| `BINANCE_SECRET`    | Your Binance secret.       |
| `BITFINEX_API_KEY`  | Your BitFinex API key.     |
| `BITFINEX_SECRET`   | Your Binance secret.       |
| `BITTREX_API_KEY`   | Your Bittrex API key.      |
| `BITTREX_SECRET`    | Your Bittrex secret.       |
| `KUCOIN_API_KEY`    | Your Kucoin API key.       |
| `KUCOIN_SECRET`     | Your Kucoin secret.        |
| `KUCOIN_PASSPHRASE` | Your Kucoin passphrase.    |
| `BITMEX_API_KEY`    | Your Bitmex API key.       |
| `BITMEX_SECRET`     | Your Bitmex secret.        |

# Upgrades

TODO
