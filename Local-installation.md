Running Orko on your local machine is the quickest way to get started. As long as your machine is safe from prying eyes, you don't have to worry about web security.

**Note that from 0.15.1, Java 11 is required.**

## Using Docker
Docker is now the simplest way to run Orko.

- Make sure you have Docker and Docker Compose installed. See https://docs.docker.com/install/ for the right guide for your host OS.
- Follow the instructions on https://hub.docker.com/r/gruelbox/orko.

## Running directly on Windows

- Install Java 11 on your PC if it's not installed already. [AdoptOpenJDK](https://adoptopenjdk.net/?variant=openjdk11&jvmVariant=hotspot) is recommended.
- Head over to the [latest official release](../releases/latest) (recommended) or the [bleeding-edge snapshot](../releases) (if you hit problems with the latest release or need an unreleased fix).
- Download **orko-app.jar** and **example-config.yml**. Copy these to wherever you want to run the application from.
- Ctrl-Shift-Right-Click in the Explorer window where these two files are and select either **Open PowerShell window here** or **Open command window here**, whichever is visible.
- Type the following to start the application:
```
java -jar orko-app.jar server example-config.yml
```
- Navigate to http://localhost:8080 to view the application.
- In the **Coins** tab, click the **+** button to select an exchange and coin pair to start seeing what Orko can do.
- Note that without exchange account details, you get a limited experience with most "real" exchanges. You can get a feel for Orko's trading features by using the **Simulator** exchange, which is a simulated exchange running on your PC.
- To trade against your real exchange accounts, shut down Orko (the command window), modify `example-config.yml`, filling in the relevant sections with your exchange api key, secret and (in some cases) passphrase. Leave any exchanges you don't have API details for blank. Then run again.

## On Ubuntu/Debian

- First make sure you have a Java JRE installed, at least Java 11. This is usually enough:
```
sudo apt-get install openjdk-11-jdk
```
- Head over to the [latest official release](../releases/latest) (recommended) or the [bleeding-edge snapshot](../releases) (if you hit problems with the latest release or need an unreleased fix).
- Download **orko-app.jar** and **example-config.yml**. Copy these to wherever you want to run the application from.
- Then run:
```
java -jar orko-app.jar server example-config.yml
```
- Navigate to http://localhost:8080 to view the application.
- In the **Coins** tab, click the **+** button to select an exchange and coin pair to start seeing what Orko can do.
- Note that without exchange account details, you get a limited experience with most "real" exchanges. You can get a feel for Orko's trading features by using the **Simulator** exchange, which is a simulated exchange running on your PC.
- To trade against your real exchange accounts, shut down Orko (**Ctrl-C**), modify `example-config.yml`, filling in the relevant sections with your exchange api key, secret and (in some cases) passphrase. Leave any exchanges you don't have API details for blank. Then run again.

## Security warning

The above instructions create a completely unsecured instance of Orko. If you are intending to add your exchange account API details, only install this on a computer to which you have exclusive access (such as a personal PC).

On the other hand, **don't deploy this anywhere which is visible on the internet** without _at least_ [enabling full two-factor authentication](Enable-two-factor-authentication) and deploying the application behind an appropriately configured **HTTPS proxy**. To do so manually is complicated, and not recommended.  Instead, you can do this automatically by [deploying to Heroku](One-click-installation-on-Heroku), which is simple and the recommended way to deploy a public instance.

## Optional extras

### Set up Telegram so you can get notifications on your phone

Want your phone to beep when stuff happens?

[Read more...](Telegram-Notifications)
