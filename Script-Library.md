Here are a few simple scripted order types which are both useful and a base for you own creations.

## Sell alt when BTC price drops

As described [here](Example-Use-Cases#sell-your-alts-when-btcs-price-drops).

### Parameters

| Name	| Description	| Default	| Reqd |
| ----- | ------------- | ------------- | ---- |
| amount	| Amount	| | Yes |
| limitPrice	| Limit price| | Yes |
| stopPrice	| Stop price (BTC Binance)| | Yes |	

### Code

```
var subscription

function start() {
  subscription = events.setTick(
    function(event) {
      if (Number(event.ticker().getLast()) < Number(parameters.stopPrice)) {
        notifications.alert("BTC hit stop price (" + event.ticker().getLast() + "). Triggering stop on " + selectedCoin.base)
        trading.limitOrder({
          market: parameters.selectedCoin,
          direction: SELL,
          price: parameters.limitPrice,
          amount: parameters.amount
        })
        control.done()
      }
    },
    { exchange: "binance", base: "BTC", counter: "USDT" }
  )
  return RUNNING
}

function stop() {
  events.clear(subscription)
}
```