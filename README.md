# Challenge3 Bitcoin Arbitrage

## Introduction
In this challenge we use Jupyter Notebook and Pandas to use two seperate databases, clean the data, and determine how much profit we could make from arbitrage.

## Read and Clean the CSV

We use Pandas to read the CSV using the following:
```
coinbase = pd.read_csv(
    Path("Resources/coinbase.csv"),
    index_col="Timestamp",
    parse_dates=True,
    infer_datetime_format=True)
```
The next step is to elimate empty cells so that they don't interfere with our analysis. To do that we use the following:
```
bitstamp = bitstamp.dropna()
bitstamp.isnull().sum()
```
Finally, one of our coloumns has a symbol that will need to be removed, otherwise the data will be viwed as an object and not a number:
```
bitstamp.loc[:, "Close"] = bitstamp.loc[:, "Close"].str.replace("$", "")
bitstamp.loc[:, "Close"] = bitstamp.loc[:, "Close"].astype(float)
```

## Analysis

We will be looking at three randomly selected days within the three month time period and reviewing the spread between the two markets. Using the following we will pull any winning trade that gives us a return of over 1% for the selected day:
```
arbitrage_spread_early = coinbase["Close"].loc["2018-01-10"] - bitstamp["Close"].loc["2018-01-10"]
pos_spread_early = arbitrage_spread_early[arbitrage_spread_early > 0]
spread_return_early = pos_spread_early / bitstamp['Close'].loc["2018-01-10"]
profitable_trades_early = spread_return_early[spread_return_early > .01]
```
## Results
![early](/Images/early.PNG)

![middle](/Images/middle.PNG)

![late](/Images/late.PNG)

## Conclusion
As time goes on the two markets become more efficent, as a result of that our profits shrink.  In January we had the highest number of winning trades and per trade we were also making the most amount of profit.  In febuary our trades drop from 14 to 8 and our average profit drops from $155.47 to 101.89.  Come March our profits have tried up and we don't have any winning trades.  The markets weren't perfectly effient by March, there were still trades available but not below our 1% metric.