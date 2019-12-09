## Next release
For the changes available in the current SNAPSHOT release, see [Releases](https://github.com/gruelbox/orko/releases). Alternatively, `pull gruelbox/orko:latest` on Docker.

Current contents:

0.15 is heavily focused on internal work. I've been putting a lot of time into the deployment architecture and updating the code to prepare for the next big feature push. This release introduces a Java 11 requirement.

**Features:**

 - Started work on breaking up the application into smaller docker services to optimise containerised deployment or use of only parts of the application. The first to be split out is the [market data](https://hub.docker.com/layers/gruelbox/orko/0.15.1-marketdata/images/sha256-75407bfcdcaebc5979b6e115fd2f0d8d3f148f0bd8535dbfe4ffe492ae4a5aa3) application. Note that this isn't ready for production use and is not currently a documented feature.
 - Experimental support for using an [Apprise](https://github.com/caronc/apprise) microservice to push notifications to anything supported by Apprise (e.g. Slack, Mattermost, etc) rather than only Telegram.  This is also not currently documented.

**Bug fixes:**

 - Fixed bug where an IP whitelisting expiry could cause the application to go into an endless loop

**Security:**

 - Added a robots.txt file to the web root

**Developer quality-of-life:**

 - Switched to Java 11.
 - Build now supports forks deploying to private docker repositories.
 - Significant rework of the backend Java configuration architecture to improve modularity.
 - Removed the need to maintain a modified `cypress.json` file locally when running end-to-end tests locally.
 - [WIP] Partially eliminated redux/reselect from UI code and replaced with more sensible modular Contexts. This work will continue into the next release.


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