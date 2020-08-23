This page explains how to define your own complex order types or actions using Orko's built-in Javascript API.

**Please note**, this functionality has only just been started and is intentionally initially quite limited.  We'll be adding a lot more features soon.**

## Background

Orko breaks down the tasks you might perform with an exchange into a handful of **Basic Actions** and an indefinite number of **Complex Actions**.  Basic actions include:

- Placing a limit order
- Placing a stop order
- Cancelling an order
- Sending an alert or notification to your browser or phone

Almost all other actions are defined as complex actions. Complex actions can perform any number of basic actions, or indeed other complex actions.  They can do so immediately or run in the background and perform these actions in response to **Events**.  Some complex actions are built into Orko:

- Stop orders on exchanges that do not support stops (places a limit order if the price passes a value)
- Price alerts (sends an alert if the price passes one of two values)
- One-cancels-other orders (places a limit order if the price passes one of two values)

However, Orko additionally opens this up to you to define your own complex orders using Javascript.

## Hello world

TODO

```
function start() {
  notifications.alert("Hello world")
  return SUCCESS
}
```

TODO

## Reference

### Functions to implement

You must implement at the very least a function called `start()`, as shown in the example above.  This must return one of:

- `SUCCESS`: The task is complete and should be deleted. Return this if everything your job needs to do can be done immediately.
- `FAILURE_PERMANENT`: The task has failed and should be deleted. Return this if your job cannot continue.
- `FAILURE_TRANSIENT`: The task failed to do what it needed to do, or failed to start for some reason, but this is likely to be a temporary issue and it should be attempted again (repeatedly).
- `RUNNING`: The job has successfully started and should be considered active.  If the application is shut down, the job will be restarted, but unless this occurs or the job shuts itself down, no further attempts will be made to start the job.

You may, in addition, implement `stop()`.  This will be called when your job shuts down, either due to an application shut down or because you called `control.done()`, `control.fail()` or `control.restart()` (see below).  It should be used to do any cleanup, such as releasing event callbacks (again, see below).

### Running in the background

To enter the `RUNNING` state, you must register callbacks in `start()` and deregister them in `stop()`.  The simplest way to do this is using the standard Javascript `setInterval()` method.  The following will send "Hello world" to the notifications panel in the UI once a second until forcibly killed.

```
var interval

function start() {
  interval = setInterval(
    function(event) {
      notifications.info("Hello again")
    },
    1000
  )
  return RUNNING
}

function stop() {
  clearInterval(interval)
}
```

Now, more usefully, what about reacting to changes in the price of an asset?

```
var subscription
var count = 0

function start() {
  subscription = events.setTick(
    function(event) {
      notifications.info("Price is " + event.ticker().getLast() + ", count=" + count)
      if (count++ >= 5) {
        control.done()
      }
    },
    parameters.selectedCoin
  )
  return RUNNING
}

function stop() {
  events.clear(subscription)
}
```
We're doing several things here:

- Using the built-in **parameter**, `selectedCoin`. This is automatically set to the currently selected coin whenever you submit a complex action.  It takes the form: `{ base: 'BTC', counter: 'USD', exchange: 'bitfinex'}`, should you wish to specify a coin in a hard-coded manner.
- Using some **transient state** in the form of `var`s
- Stopping the job with success from within a callback (this calls `stop()` automatically).

### Actions

In an event callback or on startup or shutdown, you may call any of the following:

```
// Callback repeatedly
var interval = setInterval(function() {}, milliseconds)

// Stop the callback (use in stop() method)
clearInterval(interval)

// Although note that many exchanges do not provide all this information.
// The tickerSpec type is the same as used above, e.g. { base: "BTC", counter: "USD", exchange: "bitfinex" }
events.setTick(function(tickerSpec, ticker) {}, tickerSpec)

// Job control
control.done()    // marks the job as done. You should exit afterwards.
control.fail()    // marks the job as permanently failed, shuts it down and deletes it.
control.restart() // causes a transient failure, causing the job to shut down and be
                  // restarted at some point in the future.

// Log messages to the server log
console.log("Message")
console.log("Message", object)
console.log("Message", error)

// Log messages to the application 'Server Notifications' panel
notifications.info("Message") // Sends a simple message
notifications.alert("Message") // Sends a prominent message (in blue) and also to Telegram, thus beeping your phone
notifications.error("Message") // Sends a prominent message (in red) and also to Telegram, thus beeping your phone

// Trading
var orderId = trading.limitOrder({
  market: { base: "BTC", counter: "USD", exchange: "bitfinex" },
  direction: BUY,
  price: 6000,
  amount: 0.1
})

```

### Ticker

You can access the current price information (last trade) for a given trading pair.

```
balance.setTick(response, tradingPair);
```

#### Fields Available
The ticker exposes the following fields;

```
currencyPair  // Trading pair update is for
open          // Opening price of current interval
last          // Last sale price
bid           // Current Bid price
ask           // Current Ask price
high          // High price of current interval
low           // Low Price of current interval
avg           // Average price during current interval
volume        // Current volume in last 24hrs on this trading pair
quoteVolume   // Volume traded in current interval
timestamp     // Date/time stamp of this update
bidSize       // Size of the current best Bid price
askSize       // Size of the current best Ask price
```

#### Complete Example
The following example gets the ticker of the currently selected exchange / trading pair.

```
var subscription
var count = 0

function start() {
  subscription = events.setTick(
    function(event) {
      var t = event.ticker();
      notifications.info("Price is " + t.currencyPair + " - Bid: " + t.bid + " Ask: " + t.ask);
      if (count++ >= 5) {
        control.done()
      }
    },
    parameters.selectedCoin
  )
  return RUNNING
}

function stop() {
  events.clear(subscription)
}
```

#### Example Output:
Price is ETH/EUR - Bid: 347.55000000 Ask: 348.11000000
Price is ETH/EUR - Bid: 347.55000000 Ask: 348.11000000
Price is ETH/EUR - Bid: 347.55000000 Ask: 348.11000000
Price is ETH/EUR - Bid: 347.55000000 Ask: 348.12000000
Price is ETH/EUR - Bid: 347.55000000 Ask: 348.12000000


### Account Balance

You can access the balance information for a given trading pair.

```
balance.setBalance(response, tradingPair);
```

Note that the *response* is called twice, once for the base and once for the quote. Assuming you had ETH/EUR selected, in the example below, it would call the function once for ETH and once for EUR.


#### Fields Available
The balance exposes the following fields;

```
currency    // Currency type for this balance (i.e. ETH) 
total       // Total Balance
available   // Available Balance
frozen      // Frozen Balance
borrowed    // Borrowed Balance
loaned      // Loaned Balance
withdrawing // Withdrawing Balance
depositing  // Depositing Balance
```

#### Complete Example
The following example gets the balance of the currently selected exchange / trading pair and displays the currency and available balance.

```
var count = 0;

function start() {
  var subscription = events.setBalance(
    function(event) {
      notifications.info(++count + " Balance: " + event.balance().currency + " - " + event.balance().available);
      
    },
    parameters.selectedCoin
  );
  
  return RUNNING
}

function stop() {
  events.clear(subscription);  // Stop the subscription
}
```

#### Example Output:
> Balance: ETH - 1000
> Balance: EUR - 1000

### Open Orders

You can access the open orders information for a given trading pair.

```
events.setOpenOrders(response, tradingPair);
```

#### Fields Available
The open orders exposes the following fields;
[order=LimitOrder [limitPrice=360, Order [type=ASK, originalAmount=10, cumulativeAmount=0, averagePrice=0, fee=0, instrument=ETH/EUR, id=1, timestamp=Thu Aug 20 16:08:14 CEST 2020, status=NEW, flags=[], userReference=175420665]]

#### Complete Example
The following example gets the balance of the currently selected exchange / trading pair and displays the currency and available balance.

```
var count = 0;

function start() {
  var subscription = events.setOpenOrders(
    function(event) {
      notifications.info("*** OPEN ORDERS: " + event.openOrders());
      
      if (count++ >= 1) {
        control.done()
      }
    },
    parameters.selectedCoin
  );
  
  return RUNNING
}

function stop() {
  events.clear(subscription);  // Stop the subscription
}
```

#### Example Output:
> Open orders:
> [order=LimitOrder [limitPrice=360, Order [type=ASK, originalAmount=10, cumulativeAmount=0, averagePrice=0, fee=0, instrument=ETH/EUR, id=1, timestamp=Thu Aug 20 16:08:14 CEST 2020, status=NEW, flags=[], userReference=175420665]]]
> [order=LimitOrder [limitPrice=370, Order [type=ASK, originalAmount=10, cumulativeAmount=0, averagePrice=0, fee=0, instrument=ETH/EUR, id=2, timestamp=Thu Aug 20 16:08:19 CEST 2020, status=NEW, flags=[], userReference=154148732]]]

### Order Book

You can access the order book information for a given trading pair.

```
events.setOrderBook(response, tradingPair);
```

#### Fields Available
The order book exposes the following fields within bids and asks;

```
bids  // Current list of bids
asks  // Current list of asks
```

More details
> limitPrice=347.77000000, Order [type=BID, originalAmount=0.32508000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=, timestamp=Thu Aug 20 15:12:35 CEST 2020, status=null, flags=[], userReference=192475211]], LimitOrder [limitPrice=347.75000000, Order [type=BID, originalAmount=0.16254000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=, timestamp=Thu Aug 20 15:12:37 CEST 2020, status=null, flags=[], userReference=113477373]

#### Complete Example
The following example gets the order book of the currently selected exchange / trading pair.

```
var count = 0;

function start() {
  var subscription = events.setOrderBook(
    function(event) {
      notifications.info("*** OrderBook BIDS: " + event.orderBook().bids);
      notifications.info("*** OrderBook ASKS: " + event.orderBook().asks);
      
      if (count++ >= 1) {
        control.done()
      }
    },
    parameters.selectedCoin
  );
  
  return RUNNING
}

```

#### Example Output:
[LimitOrder [limitPrice=347.77000000, Order [type=BID, originalAmount=0.32508000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=, timestamp=Thu Aug 20 15:12:35 CEST 2020, status=null, flags=[], userReference=192475211]], LimitOrder [limitPrice=347.75000000, Order [type=BID, originalAmount=0.16254000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=, timestamp=Thu Aug 20 15:12:37 CEST 2020, status=null, flags=[], userReference=113477373]], LimitOrder [limitPrice=347.56000000, Order [type=BID, originalAmount=1.07717000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=, timestamp=Thu Aug 20 15:12:26 CEST 2020, status=null, flags=[], userReference=155455028]], LimitOrder [limitPrice=347.38000000, Order [type=BID, originalAmount=2.69161000, cumulativeAmount=null, averagePrice=null, fee=null, instrument=ETH/EUR, id=null, timestamp=null, status=null, flags=[], userReference=115320557]] ...

### User Trades

You can access the user trades information for a given trading pair.

```
events.setUserTrades(response, tradingPair);
```

#### Fields Available
User Trades exposes the following fields;

```
type                // type of trade (BID / ASK) 
originalAmount      // Amount to be traded 
currencyPair        // trading pair (i.e ETH/EUR) 
price               // price bid / asked 
timestamp           // time/date of transaction (i.e. Fri Aug 21 12:48:59 CEST 2020) 
id                  // Identifier
orderId             // Unique identifier for the trade i.e. '12345678' 
feeAmount           // Transaction Fee amount 
feeCurrency         // Curreny 'feeAmount' is in (i.e. 'ETH') 
orderUserReference  // User defined identifier for the transaction (i.e. 'null')
```

#### Complete Example
The following example gets the User trades of the currently selected exchange / trading pair.

```
var count = 0;

function start() {
  var subscription = events.setUserTrades(
    function(event) {
      notifications.info("*** USER TRADES: " + event.trade());
      
      if (count++ >= 1) {
        control.done()
      }
    },
    parameters.selectedCoin
  );
  
  return RUNNING
}
```

#### Example Output:
UserTrade[type=BID, originalAmount=1, currencyPair=ETH/EUR, price=346.10000000, timestamp=Fri Aug 21 12:48:59 CEST 2020, id=1, orderId='1', feeAmount=0, feeCurrency='ETH', orderUserReference='null']'






### Parameters

You can define any number of input parameters for a complex action, and these will be requested from the user when they submit it. You have already seen `selectedCoin`, which is set automatically, but any other named parameters can be accessed in the same way, i.e. `parameters.parameterName`. So, we could make the number of prices that will be shown in the above example limited to a number specified by the user by changing the script to:

```
if (count++ >= parameters.pricesToReport) {
```

### State

**Transient state** means "state that only lasts as long as the current session".  In our scripts, this is "anything in a `var`".  Since jobs are designed to be stopped and restarted automatically (you'd never trust a stop order if it disappeared during a server restart!), any transient state is lost.  For some purposes, this is fine.

However, for jobs where the state must persist between sessions, we have **Persistent state**:

```
state.persistent.set("myValue", 5) // Store myValue=5
state.persistent.get("myValue") // Retrieve myValue
state.persistent.remove("myValue") // Clear the value of myValue
state.persistent.increment("myValue") // Set myValue to myValue + 1
```

Such state is lost permanently when the job completes.


