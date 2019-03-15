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

// Callback on a price change. Here you have access to:
//    ticker.open
//    ticker.last
//    ticker.bid
//    ticker.ask
//    ticker.high
//    ticker.low
//    ticker.vwap
//    ticker.volume
//    ticker.quoteVolume
//    ticker.timestamp
//    ticker.bidSize
//    ticker.askSize
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


