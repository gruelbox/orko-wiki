## Next release (0.13.1)

Here is the current set of completed changes which are ready to go out in the next release.

**Upgrading:**

This update does not affect the database schema, so if there are problems, it can be easily rolled back to 0.12.2. The upgrade approach depends on how you have Orko deployed:

TODO

**Enhancements:**

* Now warns the user if any of the Bitfinex, Coinbase Pro or Binance socket connections to the exchanges drop for more than 10m (was previously only Binance).
* Added simulated exchange, which allows integration testing without any socket connectivity to an exchange.  This will get improved over time and used heavily in Orko's own tests.
* Improved the login/authentication experience with more descriptive fields, validation and tooltip help
* On-screen alerts for new releases.

**Bug fixes:**

* Much better and more consistent handling of exchange errors from background processing. Most of the time these errors are now dealt with silently and without spamming the logs (particularly around common transient issues such as socket timeouts or load control at Cloudflare). Only if the error type is unexpected is the user alerted and a stacktrace logged. 
* Improved logging around Bitfinex connection errors.
* Fix bugs where Binance and Bifinex trades were displayed as buys when they were actually sells and vice versa under certain conditions.
* Fix `UndeclaredThrowableException` errors in logs when using Kraken. Kraken remains in early stage support and is not recommended for use.

**Developer quality-of-life:**

* Greatly increased unit test coverage, and the simulated exchange now means this can be expanded even further.
* Lots of dependency updates (nothing marked as security critical)
* Switched from Semantic-UI to Fomantic-UI, eliminating security warnings in gulp build

## After that

Current milestones:

- [Initial public announcement](../projects/3)
- [v1.0.0 release](../projects/5)

These, the following larger items are top of the list to tackle next:

- A proper user guide or [in-app tutorial](../issues/116).
- Improving [security](../issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Asecurity) in a number of ways.
- Improving browser support (currently only tested on Chrome Desktop and Chrome Mobile)
- Improving [mobile UI](../issues/21) for "thumbability".
- [Refinements](../issues/10) [to the](../issues/11) [trading](../issues/13) [experience](../issues/14) (lots of these!)
- [Completing](../issues/144) scripting support and making it [easier to test](../issues/109) and [understand](../issues/122).
- Support for margin trading [on Bitfinex](../issues/83) and Bitmex.
- Improving the way the server [handles exchange downtime](../issues/124) such as long maintenance windows, and detects and reports issues in the server process. Currently log monitoring is required for this.
- [Better management](../issues/125) of large numbers of coins (sorting, grouping etc)
- More deployment options, such as [publishing an image in Docker Hub](../issues/51) and [creating rpm/deb/brew packages](../issues/115) as part of releases.
- Switching to the full commercial [TradingView widget](../issues/35) where kline feeds are publicised by the exchanges, allowing orders and reference prices to be shown on the chart, drawings to be retained between sessions, and charts to be shown for exchanges with no native TradingView support (such as Kucoin).
- Bring Bitmex and Kraken support up to production strength, and support more exchanges