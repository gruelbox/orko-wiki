### Introduction

Heroku is how I host Orko. Because of that, Orko is preconfigured to work out of the box. A Hobby account is cheap at \$7/pm per server if running constantly, SSL is provided out of the box and the continuous deployment features are great.

**These instructions are intended for advanced users who want to set up every stage manually**. For most users the [one-click installer](One-click-installation-on-Heroku) is the right way to deploy Orko. Don't try and follow these instructions unless you are 100% confident with building and deploying applications yourself. It is also recommended that you have first followed the [local installation](Local-installation) guide fully and understand all the moving parts before getting stuck in setting up a publicly-accessible server manually.

## Warning
Due to the phenomenal increase in market activity over the last few months, Heroku instances are too slow to handle more than a handful of coins at once. This is being handled under [#360](https://github.com/gruelbox/orko/issues/360). A proper solution will be in place soon. In the meantime, either:

- keep the number of coins you track as low as possible (particularly making sure you only track one BTC/Fiat pair, which have the highest volume)
- or avoid server-side ("soft") orders, since there is a good chance they will trigger too late if the server is lagging
- or avoid Heroku.

### Create a 2FA key

Before your deploy, you should [create a two-factor authentication key](Enable-two-factor-authentication). You will use these to set up your environment variables shortly.

### Set up Telegram so you can get notifications on your phone

[Set up a Telegram channel](Telegram-Notifications) so you can get mobile push notifications.

### Clone the code

Heroku works by pushing a git repository of the code, so you'll need to clone the repository locally.

```
git clone -b stable https://github.com/gruelbox/orko.git
cd orko
```

### Get set up on Heroku

Note that the following settings will result in an application running on the Heroku Free Tier, which will shut down your application after a few minutes of inactivity. This is OK for a demo, but totally unsuitable for running in real life, since monitoring the market in the background is the whole point! If you want to use the application. you'll need Hobby Tier, which means a credit card. If you want to be a skinflint, just take it down when you're not using it.

Let's get started. Create a Heroku account and [install Heroku CLI locally](https://devcenter.heroku.com/articles/heroku-cli).

Now, from the the directory where you've cloned the Orko code:

```
> heroku create
> git push heroku stable:master
```

Once that's done, provision a MySQL database and some proper logging support:

```
> heroku addons:create jawsdb:kitefin
> heroku addons:create papertrail:choklad
```

Provisioning thee add-ons can alternatively be performed from the Heroku web interface.

Now set up the following environment variables in addition to those already configured by the add-ons you've provisioned. Each one can be set using the syntax:

```
> heroku config:set VARIABLE=value
```

Alternatively, you can do this easily from the Heroku web interface.

| Variable                           | Set to                                                                                                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `JAVA_OPTS`                        | This is regularly tweaked as part of code changes, so check the latest recommendation in [app.json](../blob/master/app.json)                                                                       |
| `MAVEN_CUSTOM_OPTS`                | This is regularly tweaked as part of code changes, so check the latest recommendation in [app.json](../blob/master/app.json)                                                                       |
| `MAVEN_CUSTOM_GOALS`               | `clean package`                                                                                                                                                                                    |
| `TELEGRAM_BOT_TOKEN`               | Optional. See [Telegram Notifications](Telegram-Notifications)                                                                                                                                     |
| `TELEGRAM_CHAT_ID`                 | Optional. See [Telegram Notifications](Telegram-Notifications). Required if `TELEGRAM_BOT_TOKEN` is set.                                                                                           |
| `AUTH_TOKEN`                       | Part of [security setup](Enable-two-factor-authentication).                                                                                                                                        |
| `SIMPLE_AUTH_USERNAME`             | Part of [security setup](Enable-two-factor-authentication).                                                                                                                                        |
| `SIMPLE_AUTH_PASSWORD`             | Part of [security setup](Enable-two-factor-authentication).                                                                                                                                        |
| `SIMPLE_AUTH_SECRET`               | Part of [security setup](Enable-two-factor-authentication).                                                                                                                                        |
| `SIMPLE_AUTH_TOKEN_EXPIRY_MINUTES` | The time before each token issued will expire and the user will be forced to log in again, in minutes. 1440 is a sensible default.                                                                 |
| `SIMPLE_AUTH_SECOND_FACTOR`        | Part of [security setup](Enable-two-factor-authentication).                                                                                                                                        |
| `SCRIPT_SIGNING_KEY`               | A random, long sequence of any characters. These are used to cryptographically sign any script jobs in the database so if the database is compromised, an attacker can't inject malicious scripts. |

Optionally, you can add any of these to add authenticated support for exchanges where you have API keys:

| Variable           | Set to                 |
| ------------------ | ---------------------- |
| `GDAX_API_KEY`     | Your GDAX API key.     |
| `GDAX_SECRET`      | Your GDAX secret.      |
| `GDAX_PASSPHRASE`  | Your GDAX passphrase.  |
| `BINANCE_API_KEY`  | Your Binance API key.  |
| `BINANCE_SECRET`   | Your Binance secret.   |
| `BITFINEX_API_KEY` | Your BitFinex API key. |
| `BITFINEX_SECRET`  | Your Binance secret.   |
| `BITTREX_API_KEY`  | Your Bittrex API key.  |
| `BITTREX_SECRET`   | Your Bittrex secret.   |
| `KUCOIN_API_KEY`   | Your Kucoin API key.   |
| `KUCOIN_SECRET`    | Your Kucoin secret.    |
| `KUCOIN_PASSPHRASE` | Your Kucoin passphrase.    |
| `BITMEX_API_KEY`   | Your Bitmex API key.   |
| `BITMEX_SECRET`    | Your Bitmex secret.    |

There are some [more settings](Optional-Heroku-settings) if you want to start tweaking.

That's it! Visit https://your-app-name.herokuapp.com to go through security and log in.
