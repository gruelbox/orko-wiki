## Next release
The following changes are available in the current SNAPSHOT release (see [Releases](https://github.com/gruelbox/orko/releases) or `pull gruelbox/orko:latest` on Docker).

**Features:**

**Bug fixes:**
 - #408 - Prevent socket disconnects/reconnects from sending constant browser notifications. These are now only shown in the app's notifications panel.
 - Bitfinex trades were arriving from the websocket with bid/ask flipped in seemingly random cases. This is now fixed.
 - Prevent errors in streaming on some exchanges if not authenticated
 - The UI was missing from the app build published to Docker Hub. Fixed.
 - [xchange-stream] Coinbase Pro order books were causing high server CPU usage and intermittent corruption. Fixed.

**Security:**

- Lots of dependency updates

**Developer quality-of-life:**

- Preparation for switch to Java 11. Build now works fine on both Java 8 and 11.
- Fixed numerous intermittent CI/build errors
- Moved to track the standard XChange and xchange-stream releases on Maven. I'm going to experiment with using classpath overrides for expedited fixes for the time being to see how things go. This should speed up release cycles.

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