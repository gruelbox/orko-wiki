### Introduction

Heroku is how I host Orko. Because of that, Orko is preconfigured to work out of the box on Heroku, so this is currently by far the easiest way to run it. The Hobby account is cheap at \$7/pm per server if running constantly, SSL is provided out of the box and the continuous deployment features are great.

**These instructions are intended for advanced users who want to set up every stage manually**. For most users the [one-click installer](One-click-installation-on-Heroku) is the right way to deploy Orko. Don't try and follow these instructions unless you are 100% confident building and deploying applications yourself.  It is also recommended that you have first followed the [local installation](Local-installation) guide fully and understand all the moving parts.

### Build the application locally
You'll need this to have access to the authentication tools.
```
sudo apt-get install maven git
git clone -b release https://github.com/gruelbox/orko.git
cd orko
./build.sh
./start.sh
```

### Create a 2FA key

Security is (optionally) double-layered:

- The first layer is a conventional login/JWT. We're going to set this up in the simplest possible way, but you can optionally delegate this to Okta.
- The second layer is a dynamic IP whitelisting. The UI must supply a valid Google Authenticator code to the backend which will whitelist the originating IP for a fixed period. All other entry points will return an HTTP 402 until this is done.

Generating the key is done offline for additional security.

1. Generate a new 2FA secret using `./generate-key.sh`.
1. Note it down - we'll need it when configuring the application.
1. Enter it into Google Authenticator on your phone.

### Get set up on Heroku

Note that the following settings will result in an application running on the Heroku Free Tier, which will shut down your application after a few minutes of inactivity. This is OK for a demo, but totally unsuitable for running in real life, since monitoring the market in the background is the whole point! If you want to use the application. you'll need Hobby Tier, which means a credit card. If you want to be a skinflint, just take it down when you're not using it.

Let's get started. Create a Heroku account and [install Heroku CLI locally](https://devcenter.heroku.com/articles/heroku-cli).

Now, from the the directory where you've cloned this repository (the same directly as this README):

```
> heroku create
> git push heroku master
```

Add the following Heroku addons: **Papertrail** and **JawsDB Maria**.

Set up the following environment variables in addition to those already configured by the add-ons you've provisioned:

| Variable                           | Set to                                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `JAVA_OPTS`                        | This is regularly tweaked as part of code changes, so check the latest recommendation in [app.json](../blob/master/app.json)                                                                       |
| `MAVEN_CUSTOM_OPTS`                | This is regularly tweaked as part of code changes, so check the latest recommendation in [app.json](../blob/master/app.json)                                                                       |
| `MAVEN_CUSTOM_GOALS`               | `clean package`                                                                                                                                                                                    |
| `TELEGRAM_BOT_TOKEN`               | Optional. See [Telegram Notifications](Telegram-Notifications)                                                                                                  |
| `TELEGRAM_CHAT_ID`                 | Optional. See [Telegram Notifications](Telegram-Notifications). Required if `TELEGRAM_BOT_TOKEN` is set.                                                                                                                                   |
| `AUTH_TOKEN`                       | Your 2FA secret key used when whitelisting an IP. Can be omitted if you don't want this additional layer of security.                                                                              |
| `SIMPLE_AUTH_USERNAME`             | The username you want to use when logging in.                                                                                                                                                      |
| `SIMPLE_AUTH_PASSWORD`             | The password you want to use when logging in.                                                                                                                                                      |
| `SIMPLE_AUTH_SECRET`               | A long, randomised string of characters to act as the cryptocraphic seed for issued tokens.                                                                                                        |
| `SIMPLE_AUTH_TOKEN_EXPIRY_MINUTES` | The time before each token issued will expire and the user will be forced to log in again, in minutes. 1440 is a sensible default.                                                                 |
| `SIMPLE_AUTH_SECOND_FACTOR`        | A 2FA secret key used when logging in. Can either be same value as AUTH_TOKEN, or if you want to be super-secure, use a completely different 2FA code.                                             |
| `SCRIPT_SIGNING_KEY`               | A random, long sequence of any characters. These are used to cryptographically sign any script jobs in the database so if the database is compromised, an attacker can't inject malicious scripts. |

Optionally, you can add any of these to add authenticated support for exchanges where you have API keys:

| Variable            | Set to                     |
| ------------------- | -------------------------- |
| `CRYPTOPIA_API_KEY` | Your Cryptopia API key.    |
| `CRYPTOPIA_SECRET`  | Your Cryptopia API secret. |
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
| `BITMEX_API_KEY`    | Your Bitmex API key.       |
| `BITMEX_SECRET`     | Your Bitmex secret.        |

There are some [more settings](Optional-Heroku-settings) if you want to start tweaking.

That's it! Visit https://your-app-name.herokuapp.com to go through security and log in.

### Store the password securely

Unhappy with storing your password as an environment variable in plaintext? You can store it as a [secure hash](Hashing-Passwords).
