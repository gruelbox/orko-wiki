# Crossover Trader (V1.2)

This is meant as a full example that uses many of the script engine's features. This implementation will be updated as new features are added to the script engine.

Crossover Trader is a simple automatic trader that buys or sells the selected trading pair when two moving averages of different lengths cross eachother. Crossover Trader is a simple buy/sell automatic trader based either Simple Moving Averages [SMA](https://www.investopedia.com/terms/s/sma.asp) or Exponential Moving Averages [EMA](https://www.investopedia.com/terms/e/ema.asp). 

In the chart below the SMA(12) and SMA(26) are plotted, and the crossover points indicate where the Crossover Trader would place trades.
![image](https://github.com/AwooOOoo/orko-wiki/blob/master/images/CrossoverTrader-BuySell.png)

You specify the lengths of the fast (i.e SMA(12)) and slow averages (SMA(26)) and the interval at which to sample the price.

For instance

|  Parameter | Value |
|------------|-------|
|interval | 5 |
|algorithm | SMA |
|fast average| 12 |
|slow average| 26 |

In the example above the current price will be sampled every 5 minutes an each of these samples will be fed to the slow and fast simple moving averages (SMA). Since the fast moving average has a smaller length it is more responsive to changes in the market and this moves 'faster'. If the fast average crosses above the slow average a 'BUY' order is issued since the market is trending upwards. If the fast average crosses below the slow averages a 'SELL' order is issued since the market is trending downwards. The orders are currently set as limit orders at the current price so they should (partially) imitate market orders.
 
The algorithm will continue trading until cancelled.

### Features

### Input Parameters
The following are the  input parameters for the script. If you are going to use this script please create these variables.

|  Name | Description | Default | Required |
|--------|--------------|----------|-----------|
| intervalSize | Interval (in minutes) to to take each sample | 5 | Yes |
| fastAverageLength | Length of fast average. Needs to be smaller than slow average.| 12 | Yes |
| slowAverageLength | Length of slow average. Needs to be larger than fast average | 26 | Yes |
| algorithm | Algorithm for Moving Average (SMA or EMA) | EMA | Yes |

The _intervalSize_ defines an interval at which a ticker is created internally to tracker the price at the defined interval. The _fastAverageLength_ and _slowAverageLength_ define the number of ticks from the ticker defined by the _intervalSize_. Since there is no current access to history data in the script it you need to wait for the moving averages to fill up before they can be used. This time is the _intervalSize_ * _[fast|slow]AverageLength_ so since the defaults are 5, 12 & 26, this means you need to wait 5 minutes * 12 samples for the fast average and 5 minutes * 26 samples for the slow average so be careful what you set these to as it will take a while to be ready.

Here is Crossover Trader configuration with the default values.

![image](https://github.com/AwooOOoo/orko-wiki/blob/master/images/CrossoverTraderMenu.png)

Here is what you can expect from the Server Notifications. The trades themselves are highlighted and updates to the SMA values are provided at each interval.

![image](https://user-images.githubusercontent.com/17175274/90343512-3666e400-e011-11ea-9c7c-601a62a38c7c.png)

### Code

```
var lastPrice;           // The last price data received
var subscriptionTicker;  // Subscription for Trade updates
var subscriptionBalance; // Subscription for Balance updates
var interval;            // Interval Object 
var lastEMA = {};        // The last EMA calculated

var quotes = [];         // Contains arrays of historical data to calculate the Averages
var balances = {};       // Contains balance updates
var pendingOrders;       // Whether we have pending orders
var debug = false;       // Notifies with extra information if true
var version = 1.2;       // Version of this trader

/*
Initiates a trade

Improvement: Track Trade execution
*/
function trade(side) {
  var selected = parameters.selectedCoin;

  var params = {
      market: parameters.selectedCoin,
      price: lastPrice
  };

  if (side === "BUY") { 
    params.direction = BUY,  // BUY is not quoted so I can't use a variable
    params.amount = balances[selected.counter] / lastPrice
  } else { 
    params.direction = SELL, // SELL is not quoted so I can't use a variable
    params.amount = balances[selected.base];
  }

  notifications.info("Trade: " + side + "ing " + params.amount + " for " + lastPrice);

  // Place the order on the exchange
  trading.limitOrder(params);
}

/*
Calculates a simple moving average (SMA)

Improvement: When #985 is addressed we won't have to manually create a SMA function
*/
function simpleMovingAverage(averageLength) {     
  var sma = 0;


  if (quotes.length < averageLength) {
    if (parameters.algorithm === "SMA") { // Hide text if called from EMA
      notifications.info("Filling the SMA(" + averageLength  + "): " + quotes.length + "/" + averageLength);
    }
    sma = undefined;
  } else {
    // Work from the end as this may be the fast average and doesn't need all the samples
    for(c = quotes.length - averageLength; c < quotes.length; c++) { sma += quotes[c]; }
    sma /= averageLength;
 
    if (parameters.algorithm === "SMA") { // Hide text if called from EMA
      notifications.info("SMA(" + averageLength + ") = " + parseFloat(sma).toFixed(2));
    }
  }

  return sma;
}

/*
Calculates an exponential moving average (EMA)

Improvement: When #985 is addressed we won't have to manually create an EMA function
*/
function exponentialMovingAverage(averageLength) {     
  var ema = undefined; // Default, buffer still filling 
  
  notifications.info("Lengths: " + quotes.length + " " + lastEMA[averageLength]);

  if (quotes.length <= averageLength && lastEMA[averageLength] == undefined) { 
    // Initialize the EMA with an SMA
    notifications.info("Initializing EMA!!!");

    if (quotes.length == averageLength) { // Wait until buffer is full
      ema = simpleMovingAverage(averageLength) / averageLength;
      lastEMA[averageLength] = ema;
      notifications.info("EMA initially set to: " + ema);
    }

    notifications.info("Filling the EMA(" + averageLength  + "): " + quotes.length + "/" + averageLength)
  } else if (quotes.length == averageLength) { // Array is full
    // Calculate EMA
    notifications.info("Running EMA!!! " + lastPrice + " " + lastEMA[averageLength]);

    var multiplier = (2 / (averageLength + 1));
    ema = (lastPrice - lastEMA[averageLength]) * multiplier + lastEMA;
    lastEMA[averageLength] = ema;

    notifications.info("EMA(" + averageLength + ") = " + parseFloat(ema).toFixed(2));
  } 

  return ema;
}

/*
Update the moving averages and check if conditions to trade have been met
 
Improvement: When we can load history, we won't have to wait before we can trade 
*/
function updateAveragesAndCheckConditions() {

  // Update the historic quote data
  if (quotes.length < parameters.slowAverageLength) { // Since it is bigger than the fast
    quotes.push(lastPrice); // Buffer is not yet full, push more
  } else {
    quotes.shift();     // Buffer full, make some room
    quotes.push(lastPrice);
  }

  // Update the moving averages for the selected algorithm
  var fast, slow;
  if (parameters.algorithm === "EMA") {
    fast = exponentialMovingAverage(parameters.fastAverageLength);
    slow = exponentialMovingAverage(parameters.slowAverageLength);
  } else {
    fast = simpleMovingAverage(parameters.fastAverageLength);
    slow = simpleMovingAverage(parameters.slowAverageLength);
  }

  // Determine the trade state (do we own more of the base currency of the quote currency
  var selected = parameters.selectedCoin;
  var baseValue = balances[selected.base] * lastPrice;
  var quoteValue = balances[selected.counter];

  // Provided we have valid balances and aren't currently waiting on a trade to complete
  if (baseValue !== undefined && quoteValue !== undefined && pendingOrders.length === 0) {  
 
    // Determine if we are in the trade already
    var inTrade = false;
    if (baseValue > quoteValue) inTrade = true;
    /*if (debug === true)*/ notifications.info("inTrade:" + inTrade + " - base value: " + baseValue + " quote value: " + quoteValue);

    // Don't allow trades until the averages can be fully computed (i.e. are defined)
    if (slow !== undefined && fast !== undefined) {  
      if (fast > slow && inTrade === false) trade("BUY");
      else if (fast < slow && inTrade === true) trade("SELL");
    }
  }
}

/*
Executes when the script is started
*/
function start() {
  notifications.info("Crossover Trader starting... v" + parseFloat(version).toFixed(2));

  // Initialize the lastEMA hash if we are using EMA
  if (parameters.algorithm === "EMA") {
    var slowLength = parameters.slowAverageLength;
    var fastLength = parameters.fastAverageLength;
    if (!(slowLength in lastEMA)) lastEMA[slowLength] = undefined;
    if (!(fastLength in lastEMA)) lastEMA[fastLength] = undefined;
  } 

  // Check our lengths are the right way around or the trades will be backwards
  if (fastLength >= slowLength) {
    notifications.alert("The fast average needs to be smaller than the slow average. Aborting!!!");
    return FAILURE_PERMANENT
  } 

  // Setup a time to trigger at the requested interval
  interval = setInterval(
    function(event) {
      updateAveragesAndCheckConditions();
    },
    Math.round(1000 * 60 * parameters.intervalSize) 
  )

  // Subscribe to the ticker / trades of the selected trade pair
  subscriptionTicker = events.setTick(
    function(event) {
      lastPrice = event.ticker().getLast();
    },
    parameters.selectedCoin
  )

  // Subscribe to the balances of the selected trade pair
  var subscriptionBalance = events.setBalance(
    function(event) {
      var b = event.balance();
      
      balances[b.currency] = b.available;
      if (debug === true) notifications.info("Balance Update:" + b.currency + " = " + b.available);
      
    },
    parameters.selectedCoin
  );

  var subscriptionOpenOrders = events.setOpenOrders(
    function(event) {
      pendingOrders = event.openOrders().getOpenOrders();
    },
    parameters.selectedCoin
  );

  return RUNNING
}

/* 
Executes when the script is stopped
*/
function stop() {
  notifications.info("Stopping");
  events.clear(subscriptionTicker);     // Stop the subscription to the ticker
  events.clear(subscriptionBalance);    // Stop the subscription to the balance(s)
  events.clear(subscriptionOpenOrders); // Stop the subscription to the order order(s)
  clearInterval(interval);              // Clear the interval
}
```

### Potential Future Improvements

- Add the ability to pre-load history so you don't need to wait for averages to be filled
- Add the ability to call SMA, EMA (and other) algorithms directly from a financial package like in the chart window instead of 
having to reimplement them.
- Offer Market orders so fullfillment is guarenteed 
- Loss Protection - Limit amount of initial capital that can be lost, otherwise exit algorithm
- Investment Limitation - Limit amount of counter/quote currency that can be used for trading 
- Make Algorithm type dropdown selectable instead of being typed in

### Known Issues

- Cancelling the job still leaves subscriptions running (engine problem or script problem?)

### Version History

|  Version | Changes  |
|----------|----------|
| 1.0 | EMA Support - Added support for Exponential Moving Average |
|     | InTrade Detection - Checks the balances of both currencies of the selected trading pair to determine which side of the trade you are on |
| 1.1 | Balance Support - Looks at your balance of each currency in the selected trading pair to see what funds are available |
| 1.2 | SMA Support (Fixed amount traded, requires manual initial inTrade status as Parameter) |

### Disclaimer
> Trading cryptocurrencies (or any other financial market) involves substantial risk, and there is always the potential for loss. Your trading results may vary. No representation is being made that this algorithm will guarantee profits, or not result in losses from trading.
