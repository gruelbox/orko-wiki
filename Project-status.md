# How ready is it for use?

Orko is in active daily use by me, in production. I rely on it for most of my day-to-day spot trading, so that indicates my level of confidence. However:

- You will notice there are lot of [known bugs](https://github.com/badgerwithagun/orko/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Abug). Please use with caution and [report](https://github.com/badgerwithagun/orko/issues/new/choose) any issues you find. This doesn't stop me using it, but I do often find myself restarting the server or working around known issues.
- There are also some outstanding [security issues](https://github.com/badgerwithagun/orko/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Asecurity), although nothing too serious.  That said, since the code has had no third-party review yet, it's highly likely that there are serious issues which have not yet been reported.  Similarly, there are likely to be exploits which only now will get attention.
- At the moment, this is a labour of love project for **just one person**. The world of crypto moves fast, and most of the time that means it moves faster than I do. It is commonplace for a change to happen on an exchange which breaks Orko and this can take time to resolve. **Make sure you always have a backup option**, usually in the form of your exchange's own apps, so that you can trade while issues are resolved, and **please be respectful** of the fact that this is being done entirely in my spare time.
- Orko is built on top of the [XChange](https://github.com/knowm/XChange) and [xchange-stream](https://github.com/bitrich-info/xchange-stream) open source libraries, and often these sorts of issues have to be resolved there first.  This extra hurdle can make issues take even longer to resolve.

# Roadmap

Following initial release, the following larger items are top of the list to tackle next:

- A proper user guide or [in-app tutorial](https://github.com/badgerwithagun/orko/issues/116).
- Improving [security](https://github.com/badgerwithagun/orko/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Asecurity) in a number of ways.
- Improving browser support (currently only tested on Chrome Desktop and Chrome Mobile)
- Improving [mobile UI](https://github.com/badgerwithagun/orko/issues/21) for "thumbability".
- [Refinements](https://github.com/badgerwithagun/orko/issues/10) [to the](https://github.com/badgerwithagun/orko/issues/11) [trading](https://github.com/badgerwithagun/orko/issues/13) [experience](https://github.com/badgerwithagun/orko/issues/14) (lots of these!)
- [Completing](https://github.com/badgerwithagun/orko/issues/144) scripting support and making it [easier to test](https://github.com/badgerwithagun/orko/issues/109) and [understand](https://github.com/badgerwithagun/orko/issues/122).
- Support for margin trading [on Bitfinex](https://github.com/badgerwithagun/orko/issues/83) and Bitmex.
- Improving the way the server [handles exchange downtime](https://github.com/badgerwithagun/orko/issues/124) such as long maintenance windows, and detects and reports issues in the server process.  Currently log monitoring is required for this.
- [Better management](https://github.com/badgerwithagun/orko/issues/125) of large numbers of coins (sorting, grouping etc)
- More deployment options, such as [publishing an image in Docker Hub](https://github.com/badgerwithagun/orko/issues/51) and [creating rpm/deb/brew packages](https://github.com/badgerwithagun/orko/issues/115) as part of releases.
- Switching to the full commercial [TradingView widget](https://github.com/badgerwithagun/orko/issues/35) where kline feeds are publicised by the exchanges, allowing orders and reference prices to be shown on the chart, drawings to be retained between sessions, and charts to be shown for exchanges with no native TradingView support (such as Kucoin).
- Bring Bitmex and Kraken support up to production strength, and support more exchanges

# Help needed

Please see the [help needed](https://github.com/badgerwithagun/orko/issues/111) issue if you would like to get involved.

