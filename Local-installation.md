**Note that there is currently no installer. [Can you help](https://github.com/badgerwithagun/orko/issues/115) with this?**

To install and run the application on your PC, first make sure you have a Java JRE installed, at least Java 8 (1.8). Then run:

```
sudo apt-get install maven
./build.sh
./start.sh
```

Navigate to http://localhost:8080 to view the application.

Note that:

- This uses local files (in the current directory) to hold state. It's not production strength, and doesn't support multiple instances of the application For a production deployment, a standalone database is recommended. For more information on this, see below.
- Without your exchange details, no balance or trade history information is available, and all trading is paper-trading. We'll add these in a moment.
- It's not secure to deploy on a public server. **Don't deploy this anywhere public**.
    - There's no out-of-the-box support for SSL (HTTPS). All data is transmitted in the clear, which means a third party can trivially intercept your data. It's provided ready out-of-the-box to deploy on Heroku (more on this below), which should be secure, or you can set up a suitable proxy yourself.
    - Authentication features are all disabled. We talk through enabling these in the [Heroku setup instructions](Manual-installation-on-Heroku).

## Add your exchange account details

By default there are no exchange account details, so trading isn't enabled. To remedy this, modify `orko-all-in-one/example-config.yml`. To the relevant sections, add the API keys details for the exchanges you use. Leave any exchanges you don't have API details for blank. Then run again.

## Set up Telegram so you can get notifications on your phone

While notifications are shown in the UI, it's handy to get them away from your screen.

1. Create a Telegram bot using the [BotFather](https://core.telegram.org/bots). Note down the API token.
1. Create a new channel from the Telegram app, and make it public (we'll make it private shortly).
1. Add your bot as a member of the channel, so it can post to it.
1. Use the following URL to get the ID of your channel: https://api.telegram.org/YOURBOTID:YOURTOKEN/getChat?chat_id=@YourChannelName
1. Once you've noted down the channel ID, make your channel private from the app so no-one else can access it (you can't use the above API to ge the IP of a private channel).

Once you have the connection details, you can set the appropriate section in your `example-config.yml` file. Uncomment (remove the # symbols) from the following lines, replacing the values with the token and chat id you noted down.

```
# telegram:
#   botToken: YOU
#   chatId: REALLYWANTTHIS
```

Then restart. The application will now use this bot to send you notifications on your private channel. You can add more people to the channel if you like.

## Consider setting up a standalone database.

You may get greater reliability using a standalone MySQL database than the file-based database used by default.  Install [MySQL](https://dev.mysql.com/downloads/mysql/), then change your database settings:

```
database:
  connectionString: "mysql://username:password@host.ip.com:3306/databasename?rewriteBatchedStatements=true&useJDBCCompliantTimezoneShift=true
  lockSeconds: 10
```