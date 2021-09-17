# Finance Data Project
In this project we will focus on the exploratory data analysis of stock prices. We will get the data using pandas datareader and will get the stock information for the following banks: Bank of America, CitiGroup, Goldman Sachs, JPMorgan Chase, Morgan Stanley, Wells Fargo from Jan 1st 2006 to Jan 1st 2016 for each of these banks.<br>

Open, High, Low, Close, Volume are the columns in the dataset we retrieve.

## Data Source:
We will gather our data directly from Yahoo finance using pandas datareader! We will get the stock data from Jan 1st 2011 to Jan 1st 2021 for each of the banks mentioned above.

## Project Outcomes:

### *The Imports*
```
import pandas as pd
from pandas_datareader import data, wb
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import datetime as dt
import cufflinks as cf
```
### *EDA
Let's explore the data a bit!*

### Use datetime to set start and end datetime objects and then use datareader to grab info on the stock.
```
start = dt.datetime(2011,1,1)
end = dt.datetime(2021,1,1)
BAC = data.DataReader('BAC','yahoo',start,end)
C = data.DataReader('C', 'yahoo', start, end)
GS = data.DataReader('GS', 'yahoo', start, end)
JPM = data.DataReader('JPM', 'yahoo', start, end)
MS = data.DataReader('MS', 'yahoo', start, end)
WFC = data.DataReader('WFC', 'yahoo', start, end)
```

### Create a list of the ticker symbols (as strings) in alphabetical order. Call this list: tickers
```
tickers = ['BAC','C','GS','JPM','MS','WFC']
```

### Concatenate the bank dataframes together to a single data frame called bank_stocks along axis 1
```
bank_stocks = pd.concat([BAC, C, GS, JPM, MS, WFC],keys=tickers,axis=1)
```

### Set the column name levels
```
bank_stocks.columns.names=['Bank Ticker','Stock Info']
```

### Check the head of the bank_stocks dataframe.
```
bank_stocks.head()
```

### What is the max Close price for each bank's stock throughout the time period?
```
bank_stocks.xs(key='Close',axis=1,level='Stock Info').max()
```

### Create a new empty DataFrame called returns. This dataframe will contain the returns for each bank's stock. returns are typically defined by: 𝑟𝑡=𝑝𝑡−𝑝𝑡−1𝑝𝑡−1=𝑝𝑡𝑝𝑡−1−1
``` 
returns = pd.DataFrame()
```

### Use pandas pct_change() method on the Close column to create a column representing this return value
```
for tick in tickers:
    returns[tick + " Returns"] = bank_stocks[tick]['Close'].pct_change()
returns.head()
```

### Create a pairplot using seaborn of the returns dataframe
```
sns.pairplot(returns[1:])
```

### Using this returns DataFrame, figure out on what dates each bank stock had the best and worst single day returns.
```
returns.head(2)
returns.idxmin()#min
returns.idxmax()#max
```

### Take a look at the standard deviation of the returns, which stock would you classify as the riskiest over the entire time period?
```
returns.std()
```

### Which would you classify as the riskiest for the year 2015 in terms of standard deviation?
```
returns.loc['2015-01-01':'2015-12-31'].std()
```

### *Visualization
A lot of this project will now focus on visualizations. Here in this project we've used the pandas built in visualisations, matplotlib, seaborn and cufflinks*

### Create a displot using seaborn of the 2015 returns for Morgan Stanley
```
sns.displot(x='MS Returns',data=returns.loc['2015-01-01':'2015-12-31'],aspect=1.5,kde=True,color='g',bins=20)
```

### Create a distplot using seaborn of the 2011 returns for CitiGroup
```
sns.displot(x='C Returns',data=returns.loc['2011-01-01':'2011-12-31'],aspect=1.5,kde=True,color='g',bins=20)
```

### Create a line plot showing Close price for each bank for the entire index of time.
```
bank_stocks.xs(key='Close',axis=1,level='Stock Info').iplot()#Method 1
bank_stocks.xs(key='Close',axis=1,level='Stock Info').plot(figsize=(15,6))#Method 2
```

### Plot the rolling 30 day average against the Close Price for Bank Of America's stock for the year 2016
```
BAC['Close'].loc['2016-01-01':'2016-12-31'].rolling(window=30).mean().plot(label='30 day rolling average',figsize=(10,7))
BAC['Close'].loc['2016-01-01':'2016-12-31'].plot(label='BAC Close')
```

### Create a heatmap of the correlation between the stocks Close Price.
```
sns.heatmap(bank_stocks.xs(level='Stock Info',key='Close',axis=1).corr(),annot=True)
```

### Use seaborn's clustermap to cluster the correlations together:
```
sns.clustermap(bank_stocks.xs(level='Stock Info',axis=1,key='Close').corr(),annot=True)
```

### Use .iplot(kind='candle) to create a candle plot of Bank of America's stock from Jan 1st 2015 to Jan 1st 2016.
```
BAC.loc['2015-01-01':'2016-01-01'].iplot(kind='candle')
```

### Use .ta_plot(study='sma') to create a Simple Moving Averages plot of Morgan Stanley for the year 2015.
```
MS.loc['2015-01-01':'2016-01-01'].ta_plot(study='sma')
```

### Use .ta_plot(study='boll') to create a Bollinger Band Plot for Bank of America for the year 2015.
```
BAC.loc['2015-01-01':'2016-01-01'].ta_plot(study='boll')
```
## Project Setup:
To clone this repository you need to have Python compiler installed on your system alongside pandas and seaborn libraries. I would rather suggest that you download jupyter notebook if you've not already.

To access all of the files I recommend you fork this repo and then clone it locally. Instructions on how to do this can be found here: https://help.github.com/en/github/getting-started-with-github/fork-a-repo

The other option is to click the green "clone or download" button and then click "Download ZIP". You then should extract all of the files to the location you want to edit your code.

Installing Jupyter Notebook: https://jupyter.readthedocs.io/en/latest/install.html<br>
Installing Pandas library: https://pandas.pydata.org/pandas-docs/stable/install.html



















