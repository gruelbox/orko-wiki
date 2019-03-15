This page explains how to set up Telegram so that you can get notifications of trades, price alerts and so on via your phone.

**This urgently needs to be made more accessible to non-technical users.. [Can you help](../issues/195)?**

## Create your private Telegram channel

1. Create a Telegram bot using the [BotFather](https://core.telegram.org/bots). Note down the API token.
1. Create a new channel from the Telegram app, and make it public (we'll make it private shortly).
1. Add your bot as a member of the channel, so it can post to it.
1. Use the following URL to get the ID of your channel: https://api.telegram.org/YOURBOTID:YOURTOKEN/getChat?chat_id=@YourChannelName
1. Once you've noted down the channel ID, make your channel private from the app so no-one else can access it (you can't use the above API to get the IP of a private channel).

You should now have the bot token and chat ID noted down.

## Configure the application

Configuration is different depending on how the application is deployed. Select the appropriate section below:

### Local installations

Set the appropriate section in your `example-config.yml` file. Uncomment (remove the # symbols) from the following lines, replacing the values with the token and chat id you noted down.

```
# telegram:
#   botToken: YOU
#   chatId: REALLYWANTTHIS
```

Then restart. The application will now use this bot to send you notifications on your private channel. You can add more people to the channel if you like.

### Heroku

Set the `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` environment variables.  The application will automatically restart.

## Testing

To test that you are getting notifications correctly, create a price alert which will fire immediately.  You should get the alert both on-screen and in your Telegram app.