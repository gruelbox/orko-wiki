## Next release

**Features:**

- Added Docker support. You can now pull and run directly from [Docker Hub](https://hub.docker.com/r/gruelbox/orko).
- You can now always get a bleeding-edge version of the application from [GitHub releases](https://github.com/gruelbox/orko/releases). Serious issues such as fixes for changes to exchange APIs are usually fixed far quicker than the release cadence, so this can be extremely helpful.

**Bug fixes:**

- #374 - [kucoin] Fix error fetching trade history and open orders due to new API limits
- #375 - Added Github to the Content-Security-Policy. This was preventing new release alerts from appearing.
- A number of low-level fixes from bumps to XChange and xchange-stream dependencies

**Security:**

**Developer quality-of-life:**

- Removed Travis build. All CI is now handled by parallel CircleCI jobs, which is much faster and simpler.

## Current milestones

- [v1.0.0 release](../projects/5)

## After that...

Then the following larger items are top of the list to tackle next. **Can you help**?

- A proper user guide or [in-app tutorial](../issues/116).
- Improving [security](../issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Asecurity) in a number of ways.
- Improving browser support (currently only tested on Chrome Desktop and Chrome Mobile)
- Improving [mobile UI](../issues/21) for "thumbability".
- [Refinements](../issues/10) [to the](../issues/11) [trading](../issues/13) [experience](../issues/14) (lots of these!)
- [Completing](../issues/144) scripting support and making it [easier to test](../issues/109) and [understand](../issues/122).
- Support for margin trading [on Bitfinex](../issues/83) and Bitmex.
- Improving the way the server [handles exchange downtime](../issues/124) such as long maintenance windows, and detects and reports issues in the server process. Currently log monitoring is required for this.
- [Better management](../issues/125) of large numbers of coins (sorting, grouping etc)
- More deployment options, such as [creating rpm/deb/brew packages](../issues/115) as part of releases.
- Switching to the full commercial [TradingView widget](../issues/35) where kline feeds are publicised by the exchanges, allowing orders and reference prices to be shown on the chart, drawings to be retained between sessions, and charts to be shown for exchanges with no native TradingView support (such as Kucoin).
- Bring Bitmex and Kraken support up to production strength, and support more exchanges