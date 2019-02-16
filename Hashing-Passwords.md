# What's this?

Under normal circumstances, it is an obvious move to ensure that the password stored in Heroku's environment variables be [hashed](https://security.blogoverflow.com/2013/09/about-secure-password-hashing/) so that someone can't gain access to your application by hacking into Heroku's database.

The thing is, if someone gets at your Heroku environment variables, they already have the keys to the castle in the form of your Exchange API keys. Gaining access to Orko itself gains them nothing at this point; they can already trade freely on your accounts. You need to understand the risk inherent in storing your API keys _anywhere_ and take suitable precautions.

However, because hey, there's a chance they might steal your password and _not_ your exchange keys, it does no harm to hash your password anyway.

# Instructions

## Download Orko

Head over to [the latest release](../releases/latest) and download **orko-app.jar**. We will be using some built-in command-line tools. Make sure you have a Java JRE installed, at least Java 8 (1.8).

## Decide on a nice, long password

Long is the important bit. Easy to remember is good. You've [seen this](https://xkcd.com/936/), right?

## Generate a secure salt and hashed password

Run the following to generate a random salt, and hash your password with it:

```
> java -jar orko-app.jar hash YOURPASSWORD
Salt used: YOURSALT
Hashed result: YOURHASHEDPASSWORD
```

Note these both down.

## Configure your application

If you're using a local installation, locate this section of your config.yml (`example-config.yml` is supplied with each release):

```
  jwt:
    userName: joe
    password: bloggs
```

Replace the username and password as follows:

```
  jwt:
    userName: joe
    password: YOURHASHEDPASSWORD
    passwordSalt: YOURSALT
```

Make sure you include the `HASH(...)` wrapping text.

If you're using Heroku, set the environment variables `SIMPLE_AUTH_SALT` and `SIMPLE_AUTH_PASSWORD` with your salt and hashed password (including the `HASH(...)` wrapping text) respectively.

Your password will now be checked by secure one-way hash.
