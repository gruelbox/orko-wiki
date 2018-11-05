Under normal circumstances, it is an obvious move to ensure that the password stored in Heroku's environment variables be [hashed](https://security.blogoverflow.com/2013/09/about-secure-password-hashing/) so that someone can't gain access to your application by hacking into Heroku's database.

The thing is, if someone gets at your Heroku environment variables, they already have the keys to the castle in the form of your Exchange API keys. Gaining access to Orko itself gains them nothing at this point; they can already trade freely on your accounts. You need to understand the risk inherent in storing your API keys _anywhere_ and take suitable precautions.

However, because hey, there's a chance they might steal your password and _not_ your exchange keys, it does no harm to hash your password anyway.  If you have the application [built locally](Local-installation), run the following to generate a random salt:
```
> ./generate-salt.sh
Salt: YOURSALT
```
Copy the salt produced into the environment variable `SIMPLE_AUTH_SALT`. Now run:
```
> ./generate-hash.sh <YOURPASSWORD> <YOURSALT>
Password: YOURPASSWORD
Salt: YOURSALT
Hashed: HASH(HASHEDPASSWORD)
```
Copy the hashed result (including the `HASH(...)` wrapper - that's how Orko can tell to treat it as a hashed password rather than plaintext) into the environment variable `SIMPLE_AUTH_PASSWORD`.

Your password will now be checked by secure one-way hash.