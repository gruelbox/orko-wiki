## Do I need this?

If you answer **yes** to all the following questions, you can ignore this section and use the default, insecure settings:

- [ ] Orko is not directly visible on the public web (e.g. I must use a virtual desktop to view it, or tunnel in via SSH or a VPN)
- [ ] I am running Orko on a private PC or a private server to which only I have access
- [ ] _If it's a private PC:_ the PC is physically secure and protected by OS-level security.
- [ ] _If it's private server:_ I am confident in the authentication mechanism used to access it (long password, second factor authentication such as a private key and passphrase).

If you answered **no** to the first question, please read and follow the rest of this document.  If you answered **no** to any of the other questions, _please consider running Orko somewhere else or improving your OS-level/physical security first_. No amount of web security will help you if someone else has free access to your server!

## Background

It is optional, but highly recommended, if you use two or three factor authentication when logging in.

Security in Orko is optionally double-layered. The first layer is the same username/password/two-factor solution that most cryptocurrency exchanges use. When you set up an account, you enter a code (or scan an RFID) with a mobile app like Google Authenticator, and when logging in you enter both your username/password and the current code from the app. In Orko, this two-factor part is optional. If left blank in configuration, you just leave it blank when logging in.

It's _highly_ recommended that you _do_ use two-factor authentication. This way, if your password is compromised, a hacker still can't log in. The guide below talks you through setting this up.

So far, so good. **However**, there is still a potential weakness. Being "logged in" to a web application (pretty much all modern ones) relies on the application storing a piece of information in your browser, such as a cookie, which verifies that you have successfully logged in. Some information from this is sent with each request to the server like a key, and the server makes sure that the key is correct before doing anything. That's what "logged in" means - your browser has been given a temporary key, much like an electronic hotel room pass, which will work until the server revokes it. This key is called a JSON Web Token (JWT).

So what happens if someone steals the JWT out of your browser? Yes, they can now pretend to be you and access your account. This potential weakness exists in almost all web applications, including all your favourite cryptocurrency exchanges.

Now, crypto exchanges have the luxury of their code being secret, and presumably (let's hope) can afford to pay security consultants. They can be pretty sure (again, let's really, really hope) that they have written the browser-side code so that it's very, very hard for some other website to steal your JWT. Orko follows the same best practice, but firstly, as open source code, if there are any weaknesses in security, they are visible for anyone to exploit. Secondly, it's not had the attention of expensive security consultants.

In order to mitigate this, Orko implements **another** (yes, again, optional) layer of security, such that if a hacker steals the JWT, they still can't use it. When you first connect, you get asked for a 2FA key. If successfully entered, this "whitelists" your IP address. You can then proceed to normal login.

This means that a hacker needs to have **both** your 2FA key **and** your JWT (or your login details) since the JWT only works from your IP address.

You have the option of setting up:

- **Insecure:** Just username and password
- **Moderately secure:** Username, password and 2FA
- **Recommended security:** Username, password, 2FA and IP whitelisting using the **same** 2FA code
- **Paranoid:** Username, password, 2FA and IP whitelisting using a **different** 2FA code

Which you use is up to you.

## Instructions

**This urgently needs to be made more accessible to non-technical users. [Can you help](../issues/196)?**

### Download Orko

One of:
 - _Either_ head over to [the latest release](../releases/latest) and download **orko-app.jar**. We will be using some built-in command-line tools. Make sure you have a Java JRE installed, at least Java 8 (1.8).
 - _Or_ simply use Docker: `docker pull gruelbox/orko:stable`

### Create a key

Open a command line/terminal in the same directory that you put **orko-app.jar**. Run the following command to generate a new 2FA key:
```
java -jar orko-app.jar otp
```
Or if you're using Docker:
```
docker run gruelbox/orko:stable otp
```
Note it down - we'll need it when configuring the application. That done, enter it into Google Authenticator app on your phone.

Repeat this, creating a second code, if you are intending to use two 2FA codes (see above).

### Local application/personal server

Locate this section of your config.yml (`example-config.yml` is supplied with each release).

```
#  ipWhitelisting:
#    whitelistExpirySeconds: 86400
#    secretKey: YOURTOKEN
#  jwt:
#    userName: joe
#    password: bloggs
#    secret: CHANGEME!!!!!!!!23423rwefsdf13cr123de1234d1243d1ewdfsdfcsdfsdf12222
#    secondFactorSecret: YOURTOKEN
#    expirationMinutes: 1440
```

Uncomment these lines, replacing the value of:

- `secretKey` with the 2FA code that you want to use for IP whitelisting
- `secondFactorSecret` with the 2FA code that you want to use for login (can be the same as `secretKey`)
- `secret` with a long random string of characters. Don't copy this from somewhere else - just mash the keyboard randomly. This is used as a cryptographic seed.
- `userName` and `password` with your chosen username and password. Consider [hashing](Hashing-Passwords) the password just in case your machine is compromised.

### Heroku setup

In Heroku, you don't touch the YML file - everything is substituted in with environment variables.

Set up the following environment variables:

| Variable                    | Set to                                                                                                                                           |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `AUTH_TOKEN`                | The 2FA code that you want to use for IP whitelisting                                                                                            |
| `SIMPLE_AUTH_USERNAME`      | The username you want to use when logging in.                                                                                                    |
| `SIMPLE_AUTH_PASSWORD`      | The password you want to use when logging in. Consider [hashing](Hashing-Passwords) the password just in case your machine is compromised.       |
| `SIMPLE_AUTH_SECRET`        | A long random string of characters. Don't copy this from somewhere else - just mash the keyboard randomly. This is used as a cryptographic seed. |  |
| `SIMPLE_AUTH_SECOND_FACTOR` | The 2FA code that you want to use for login (can be the same as `secretKey`)                                                                     |

### Docker setup

You can configure Docker either using the local install approach (with a custom YML file), environment variables (as with Heroku) or Docker secrets.  See the [Docker Hub](https://hub.docker.com/r/gruelbox/orko) page for configuration information.