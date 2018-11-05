# Before you do anything

This project follows the same [contributing](https://github.com/alfasoftware/morf/wiki/Contributing) and [coding standards](https://github.com/alfasoftware/morf/wiki/Coding-Standards) guides as the [Morf](https://github.com/alfasoftware/morf) project (I wrote them, so I like them). Please read them before submitting pull requests.

# Areas of interest

## Javascript front-end code

I used this project to teach myself JS/React, so the front-end side of things is nowhere near as clean as the back-end.  It's mostly pretty conventional simple React/Redux, but having learned as I was going, it's a bit inconsistent and full of bad practices I haven't had time to clean up.  It's also completely bereft of unit tests.

Orko definitely needs some attention from someone with a lot more Javascript experience. In particular, the performance of the UI leaves something to be desired (see [#120](https://github.com/badgerwithagun/orko/issues/120)).

## How to add an exchange

1. Check that [XChange](https://github.com/knowm/XChange) supports the exchange.  If not, please ask there about adding support first. Until XChange supports it, Orko won't
1. Check on streaming support at [xchange-stream](https://github.com/bitrich-info/xchange-stream).  This isn't necessary, but greatly improves the responsiveness of Orko if it's available.
1. Add the XChange dependency to the parent POM and orko-common's POM.  Don't add xchange-stream at first; it tends to be less reliable, and it can be unclear whether an issue is caused by XChange or xchange-stream if you use both at the same time.
1. Check the name of the `Exchange` subclass for your exchange.  If the exchange is called `FooExchange` then add a new constant in [`Exchanges`](https://github.com/badgerwithagun/orko/blob/master/orko-common/src/main/java/com/grahamcrockford/orko/exchange/Exchanges.java).
1. Start the application and see if the public API components work. In particular, check that the order book is the right way around and ordered correctly (!), prices (ask/bid/etc) come through and market trades are correctly reported with the right direction.
1. Add your exchange details and see if you get balances and trade history, and try placing trades (use a very low buy or very high sell so that it goes onto the exchange book but doesn't fill).  Make sure the trade appears as an open trade.  Try cancelling the trade, and make sure that works.
1. Now try a trade that fills. Make sure the fill causes an alert, disappears from the open orders and appears in the trade history.  Try both buys and sells.
1. Now add the xchange-stream dependency and repeat the last 3 steps - they should all be more responsive but work just the same.

You will never get this far without problems. All exchanges have some quirk or other which requires tweaks.  XChange is not 100% consistent and sometimes exchanges (particularly margin exchanges like Bitfinex or ~~hunting grounds~~ derivatives exchanges like Bitmex) have to be addressed differently.  Generally, the best way to move quickly is to hack the necessary changes into Orko with a TODO, then follow up with a pull request to XChange or xchange-stream.

## Scripting

The scripting feature is barely more than a proof-of-concept right now.  The scripting API itself is hardly easy to use. The plan right now is first to [migrate from Nashorn](https://github.com/badgerwithagun/orko/issues/113) (which only provides Javascript ES5) to a fully ES6 alternative (GraalVM or possibly Rhino) and [layer a more `when(this).then(that)` type of DSL](https://github.com/badgerwithagun/orko/issues/122) over the low level API for less advanced users.  However, alternative ideas are very welcome.

First, though, decent [testing and debugging](https://github.com/badgerwithagun/orko/issues/109) features are needed and the whole thing needs some security work - it's quite possible for a user to [send the server into an endless loop](https://github.com/badgerwithagun/orko/issues/123).