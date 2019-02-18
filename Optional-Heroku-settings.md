The following are defaulted or automatically set up, so you can ignore them unless you need to change the defaults:

| Variable                   | Default                                                                      |
| -------------------------- | ---------------------------------------------------------------------------- |
| `LOOP_SECONDS`             | 15                                                                           |
| `LOCK_SECONDS`             | 45                                                                           |
| `JAWSDB_URL`         | Should already have been set up for you by the JawsDB add-on.          |
| `WHITELIST_EXPIRY_SECONDS` | How long an IP whitelisting should last. 86400 (24 hours) is a good default. |
| `LOG_LEVEL`                | `INFO` (or `DEBUG` if you need it)                                           |
| `GDAX_SANDBOX`             | `true` if you want to connect to the Coinbase Pro sandbox instead of the live exchange. |
| `KUCOIN_SANDBOX`             | `true` if you want to connect to the Kucoin sandbox instead of the live exchange. |
| `exchangename_REMOTE_META`             | `false` to disable fetching of coin metadata from the exchange. This is useful if you're not using an exchange but are getting errors logged about it, since it disables these background fetches. |

Also see  [Hashing Passwords](Hashing-Passwords) for some additional settings.
