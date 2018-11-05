**This feature has now been removed. This page is retained for reference in case the feature is resurrected.**

If you have Orko deployed on Heroku, but you either don't trust Orko's own authentication or prefer to use a cloud provider (perhaps for SSO), Orko has support for this using [Okta](https://www.okta.com/).

Assuming you having a running application on Heroku, here's a quick start for switching to Okta authentiation:

1. Create a basic (free) account at https://www.okta.com/
1. Add any 2FA or whatever you feel appropriate to this account.
1. Create new application of type Single Page App (SPA), allowing both ID token and Access Token
1. Set your Login redirect URI and Initiate login URI to the address of your front-end server.
1. Note down the client ID and set it in your backend app's environment variables.
1. Go to the Sign On tab and note the `Issuer` and `Client id`.
1. Now change your Heroku environment variables as follows:

| Variable                           | Set to                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------- |
| `AUTH_TOKEN`                       | Your 2FA secret key. Can be omitted if you don't want this additional layer of security. |
| `OKTA_BASEURL`                     | The Okta issuer.                                                                         |
| `OKTA_CLIENTID`                    | The Okta client ID.                                                                      |
| `OKTA_ISSUER`                      | The Okta issuer appended with `/oauth2/default`                                          |
| `SIMPLE_AUTH_USERNAME`             | REMOVE THIS                                                                              |
| `SIMPLE_AUTH_PASSWORD`             | REMOVE THIS                                                                              |
| `SIMPLE_AUTH_SECRET`               | REMOVE THIS                                                                              |
| `SIMPLE_AUTH_TOKEN_EXPIRY_MINUTES` | REMOVE THIS                                                                              |