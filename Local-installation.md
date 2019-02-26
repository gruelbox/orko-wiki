This page covers the (currently quite manual) process for installing on a local machine. This is intended for:

- Trying out Orko, to see if you like it.
- Super-paranoid traders who want a setup on their local machine that isn't visible to the outside world
- Developers.

If that's not you, also see:

- [One click](One-click-installation-on-Heroku) installation on Heroku
- [Manual](Manual-installation-on-Heroku) installation on Heroku.

**Note that there are currently no windows/deb/rpm/brew installer.** [Can you help](../issues/115) with this?
**[Docker support](../issues/51) would also be welcome.** Help required there too.

# On Windows

- Install Java on your PC if it's not installed already. [Download it from here](https://www.java.com/en/download/).
- Head over to [the latest release](../releases/latest) and download **orko-app.jar** and **example-config.yml**. Copy these to wherever you want to run the application from.
- Ctrl-Shift-Right-Click in the Explorer window where these two files are and select either **Open PowerShell window here** or **Open command window here**, whichever is visible.
- Type the following to start the application:
```
java -jar orko-app.jar server example-config.yml
```
- Navigate to http://localhost:8080 to view the application.
- To trade against your real exchange accounts, shut down the application (the command window), modify `example-config.yml`, filling in the relevant sections with your exchange api key, secret and (in some cases) passphrase. Leave any exchanges you don't have API details for blank. Then run again.

# On Ubuntu/Debian

- First make sure you have a Java JRE installed, at least Java 8 (1.8). This is usually enough:
```
sudo apt-get install openjdk
```
- Head over to [the latest release](../releases/latest) and download **orko-app.jar** and **example-config.yml**. Copy these to wherever you want to run the application from.
- Then run:
```
java -jar orko-app.jar server example-config.yml
```
- Navigate to http://localhost:8080 to view the application.
- Visit http://localhost:8080 to start.
- To trade against your real exchange accounts, shut down the application, modify `example-config.yml`, filling in the relevant sections with your exchange api key, secret and (in some cases) passphrase. Leave any exchanges you don't have API details for blank. Then run again.

# How it works

- This uses local files (in the current directory) to hold state. It's not production strength, and doesn't support multiple instances of the application For a production deployment, a standalone database is recommended. For more information on this, see below.
- Without your exchange details, no balance or trade history information is available, and all trading is paper-trading. We'll add these in a moment.
- It's not secure to deploy on a public server. **Don't deploy this anywhere public**.
  - There's no out-of-the-box support for SSL (HTTPS). All data is transmitted in the clear, which means a third party can trivially intercept your data. It's provided ready out-of-the-box to deploy on Heroku (more on this below), which should be secure, or you can set up a suitable proxy yourself.
  - Authentication features are all disabled. We talk through enabling these in the [Heroku setup instructions](Manual-installation-on-Heroku).

## (Optional) Set up Telegram so you can get notifications on your phone

Want your phone to beep when stuff happens?

[Read more...](Telegram-Notifications)

## (Optional) use a MySQL database

For more advanced users only.

You may get greater reliability using a standalone MySQL database than the file-based database used by default. Install [MySQL](https://dev.mysql.com/downloads/mysql/), then change your database settings:

```
database:
  connectionString: "mysql://username:password@host.ip.com:3306/databasename?rewriteBatchedStatements=true&useJDBCCompliantTimezoneShift=true
  lockSeconds: 10
```
