Security is (optionally) double-layered:

- The first layer is a conventional login/JWT. We're going to set this up in the simplest possible way, but you can optionally delegate this to Okta.
- The second layer is a dynamic IP whitelisting. The UI must supply a valid Google Authenticator code to the backend which will whitelist the originating IP for a fixed period. All other entry points will return an HTTP 402 until this is done.

Generating the key is done offline for additional security.

1. Generate a new 2FA secret using `./generate-key.sh`.
1. Note it down - we'll need it when configuring the application.
1. Enter it into Google Authenticator on your phone.