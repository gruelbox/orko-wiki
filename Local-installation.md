This page covers the (currently quite manual) process for installing on a local machine. This is intended for:

* Trying out Orko, to see if you like it.
* Super-paranoid traders who want a setup on their local machine that isn't visible to the outside world
* Developers.

If that's not you, also see:

* [One click](One-click-installation-on-Heroku) installation on Heroku
* [Manual](Manual-installation-on-Heroku) installation on Heroku.

**Note that there are currently no windows/deb/rpm/brew installer.** [Can you help](../issues/115) with this?

**[Docker support](../issues/51) would also be welcome.**  Help required there too.

# Platform specifics

## Ubuntu/Debian

First make sure you have a Java JRE installed, at least Java 8 (1.8). Then run:

```
sudo apt-get install maven git
git clone -b release https://github.com/gruelbox/orko.git
cd orko
./build.sh
./start.sh
```

Navigate to http://localhost:8080 to view the application.

## Windows

The Ubuntu/Debian instructions will work under Windows Subsystem for Linux with either Ubuntu or Debian.

# How it works

- This uses local files (in the current directory) to hold state. It's not production strength, and doesn't support multiple instances of the application For a production deployment, a standalone database is recommended. For more information on this, see below.
- Without your exchange details, no balance or trade history information is available, and all trading is paper-trading. We'll add these in a moment.
- It's not secure to deploy on a public server. **Don't deploy this anywhere public**.
  - There's no out-of-the-box support for SSL (HTTPS). All data is transmitted in the clear, which means a third party can trivially intercept your data. It's provided ready out-of-the-box to deploy on Heroku (more on this below), which should be secure, or you can set up a suitable proxy yourself.
  - Authentication features are all disabled. We talk through enabling these in the [Heroku setup instructions](Manual-installation-on-Heroku).

## Add your exchange account details

By default there are no exchange account details, so trading isn't enabled. To remedy this, modify `orko-all-in-one/example-config.yml`. To the relevant sections, add the API keys details for the exchanges you use. Leave any exchanges you don't have API details for blank. Then run again.

## Set up Telegram so you can get notifications on your phone

Want your phone to beep when stuff happens?

[Read more...](Telegram-Notifications)

## Consider setting up a standalone database.

You may get greater reliability using a standalone MySQL database than the file-based database used by default. Install [MySQL](https://dev.mysql.com/downloads/mysql/), then change your database settings:

```
database:
  connectionString: "mysql://username:password@host.ip.com:3306/databasename?rewriteBatchedStatements=true&useJDBCCompliantTimezoneShift=true
  lockSeconds: 10
```
