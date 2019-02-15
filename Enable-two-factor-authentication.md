It is optional, but highly recommended, if you use two or three factor authentication when logging in. Follow this guide to create your authentication keys.

**This urgently needs to be made more accessible to non-technical users. [Can you help](../issues/196)?**

# Install locally first

Currently the utilities for creation of 2FA keys have to be compiled locally:

```
sudo apt-get install maven git
git clone -b release https://github.com/gruelbox/orko.git
cd orko
./build.sh
./start.sh
```

# Create a key

1. Generate a new 2FA secret using `./generate-key.sh`.
1. Note it down - we'll need it when configuring the application.
1. Enter it into Google Authenticator on your phone.

Now consider doing this a *second time*.  Orko is at its most secure when using two separate 2FA keys: one to verify your IP address, and the other to verify your login.

# Understanding how 2FA keys are used in Orko

Security is (optionally) double-layered:

- The first layer is a conventional login/JWT. We're going to set this up in the simplest possible way, but you can optionally delegate this to Okta.
- The second layer is a dynamic IP whitelisting. The UI must supply a valid Google Authenticator code to the backend which will whitelist the originating IP for a fixed period. All other entry points will return an HTTP 402 until this is done.

Generating the key is done offline for additional security.

