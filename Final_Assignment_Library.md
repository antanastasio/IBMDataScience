<center>
    <img src="https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/Logos/organization_logo/organization_logo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>


<h1>Extracting Stock Data Using a Python Library</h1>


A company's stock share is a piece of the company more precisely:

<p><b>A stock (also known as equity) is a security that represents the ownership of a fraction of a corporation. This
entitles the owner of the stock to a proportion of the corporation's assets and profits equal to how much stock they own. Units of stock are called "shares." [1]</p></b>

An investor can buy a stock and sell it later. If the stock price increases, the investor profits, If it decreases,the investor with incur a loss.  Determining the stock price is complex; it depends on the number of outstanding shares, the size of the company's future profits, and much more. People trade stocks throughout the day the stock ticker is a report of the price of a certain stock, updated continuously throughout the trading session by the various stock market exchanges.

<p>You are a data scientist working for a hedge fund; it's your job to determine any suspicious stock activity. In this lab you will extract stock data using a Python library. We will use the <coode>yfinance</code> library, it allows us to extract data for stocks returning data in a pandas dataframe. You will use the lab to extract.</p>


<h2>Table of Contents</h2>
<div class="alert alert-block alert-info" style="margin-top: 20px">
    <ul>
        <li>Using yfinance to Extract Stock Info</li>
        <li>Using yfinance to Extract Historical Share Price Data</li>
        <li>Using yfinance to Extract Historical Dividends Data</li>
        <li>Exercise</li>
    </ul>
<p>
    Estimated Time Needed: <strong>30 min</strong></p>
</div>

<hr>



```python
!pip install yfinance==0.1.67
#!pip install pandas==1.3.3
```

    Collecting yfinance==0.1.67
      Downloading yfinance-0.1.67-py2.py3-none-any.whl (25 kB)
    Requirement already satisfied: pandas>=0.24 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.1.67) (1.3.4)
    Requirement already satisfied: requests>=2.20 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.1.67) (2.26.0)
    Requirement already satisfied: lxml>=4.5.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.1.67) (4.6.4)
    Collecting multitasking>=0.0.7
      Downloading multitasking-0.0.10.tar.gz (8.2 kB)
      Preparing metadata (setup.py) ... [?25ldone
    [?25hRequirement already satisfied: numpy>=1.15 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from yfinance==0.1.67) (1.21.4)
    Requirement already satisfied: python-dateutil>=2.7.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from pandas>=0.24->yfinance==0.1.67) (2.8.2)
    Requirement already satisfied: pytz>=2017.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from pandas>=0.24->yfinance==0.1.67) (2021.3)
    Requirement already satisfied: certifi>=2017.4.17 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.20->yfinance==0.1.67) (2021.10.8)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.20->yfinance==0.1.67) (1.26.7)
    Requirement already satisfied: idna<4,>=2.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.20->yfinance==0.1.67) (3.1)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from requests>=2.20->yfinance==0.1.67) (2.0.8)
    Requirement already satisfied: six>=1.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from python-dateutil>=2.7.3->pandas>=0.24->yfinance==0.1.67) (1.16.0)
    Building wheels for collected packages: multitasking
      Building wheel for multitasking (setup.py) ... [?25ldone
    [?25h  Created wheel for multitasking: filename=multitasking-0.0.10-py3-none-any.whl size=8500 sha256=0efcc0fac868c8ce95fddfa7e97ad28f3b29eeb182ac2c676b1bb1061a5c0ec1
      Stored in directory: /home/jupyterlab/.cache/pip/wheels/34/ba/79/c0260c6f1a03f420ec7673eff9981778f293b9107974679e36
    Successfully built multitasking
    Installing collected packages: multitasking, yfinance
    Successfully installed multitasking-0.0.10 yfinance-0.1.67



```python
import yfinance as yf
import pandas as pd
```

## Using the yfinance Library to Extract Stock Data


Using the `Ticker` module we can create an object that will allow us to access functions to extract data. To do this we need to provide the ticker symbol for the stock, here the company is Apple and the ticker symbol is `AAPL`.



```python
apple = yf.Ticker("AAPL")
```

Now we can access functions and variables to extract the type of data we need. You can view them and what they represent here [https://aroussi.com/post/python-yahoo-finance](https://aroussi.com/post/python-yahoo-finance?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01).


### Stock Info


Using the attribute  <code>info</code> we can extract information about the stock as a Python dictionary.



```python
apple_info=apple.info
apple_info
```




    {'zip': '95014',
     'sector': 'Technology',
     'fullTimeEmployees': 154000,
     'longBusinessSummary': 'Apple Inc. designs, manufactures, and markets smartphones, personal computers, tablets, wearables, and accessories worldwide. It also sells various related services. In addition, the company offers iPhone, a line of smartphones; Mac, a line of personal computers; iPad, a line of multi-purpose tablets; AirPods Max, an over-ear wireless headphone; and wearables, home, and accessories comprising AirPods, Apple TV, Apple Watch, Beats products, HomePod, and iPod touch. Further, it provides AppleCare support services; cloud services store services; and operates various platforms, including the App Store that allow customers to discover and download applications and digital content, such as books, music, video, games, and podcasts. Additionally, the company offers various services, such as Apple Arcade, a game subscription service; Apple Music, which offers users a curated listening experience with on-demand radio stations; Apple News+, a subscription news and magazine service; Apple TV+, which offers exclusive original content; Apple Card, a co-branded credit card; and Apple Pay, a cashless payment service, as well as licenses its intellectual property. The company serves consumers, and small and mid-sized businesses; and the education, enterprise, and government markets. It distributes third-party applications for its products through the App Store. The company also sells its products through its retail and online stores, and direct sales force; and third-party cellular network carriers, wholesalers, retailers, and resellers. Apple Inc. was incorporated in 1977 and is headquartered in Cupertino, California.',
     'city': 'Cupertino',
     'phone': '408 996 1010',
     'state': 'CA',
     'country': 'United States',
     'companyOfficers': [],
     'website': 'https://www.apple.com',
     'maxAge': 1,
     'address1': 'One Apple Park Way',
     'industry': 'Consumer Electronics',
     'ebitdaMargins': 0.32867,
     'profitMargins': 0.25882,
     'grossMargins': 0.41779,
     'operatingCashflow': 104037998592,
     'revenueGrowth': 0.288,
     'operatingMargins': 0.29782,
     'ebitda': 120233000960,
     'targetLowPrice': 128.01,
     'recommendationKey': 'buy',
     'grossProfits': 152836000000,
     'freeCashflow': 73295003648,
     'targetMedianPrice': 180,
     'currentPrice': 164.51,
     'earningsGrowth': 0.662,
     'currentRatio': 1.075,
     'returnOnAssets': 0.20179,
     'numberOfAnalystOpinions': 41,
     'targetMeanPrice': 179.87,
     'debtToEquity': 216.392,
     'returnOnEquity': 1.47443,
     'targetHighPrice': 210,
     'totalCash': 62639001600,
     'totalDebt': 136521998336,
     'totalRevenue': 365817004032,
     'totalCashPerShare': 3.818,
     'financialCurrency': 'USD',
     'revenuePerShare': 21.904,
     'quickRatio': 0.91,
     'recommendationMean': 1.8,
     'exchange': 'NMS',
     'shortName': 'Apple Inc.',
     'longName': 'Apple Inc.',
     'exchangeTimezoneName': 'America/New_York',
     'exchangeTimezoneShortName': 'EST',
     'isEsgPopulated': False,
     'gmtOffSetMilliseconds': '-18000000',
     'quoteType': 'EQUITY',
     'symbol': 'AAPL',
     'messageBoardId': 'finmb_24937',
     'market': 'us_market',
     'annualHoldingsTurnover': None,
     'enterpriseToRevenue': 7.657,
     'beta3Year': None,
     'enterpriseToEbitda': 23.297,
     '52WeekChange': 0.21451008,
     'morningStarRiskRating': None,
     'forwardEps': 6.19,
     'revenueQuarterlyGrowth': None,
     'sharesOutstanding': 16334399488,
     'fundInceptionDate': None,
     'annualReportExpenseRatio': None,
     'totalAssets': None,
     'bookValue': 3.841,
     'sharesShort': 95908325,
     'sharesPercentSharesOut': 0.0058,
     'fundFamily': None,
     'lastFiscalYearEnd': 1632528000,
     'heldPercentInstitutions': 0.59062,
     'netIncomeToCommon': 94679998464,
     'trailingEps': 5.61,
     'lastDividendValue': 0.22,
     'SandP52WeekChange': 0.17640221,
     'priceToBook': 42.82999,
     'heldPercentInsiders': 0.00071000005,
     'nextFiscalYearEnd': 1695600000,
     'yield': None,
     'mostRecentQuarter': 1632528000,
     'shortRatio': 0.84,
     'sharesShortPreviousMonthDate': 1638230400,
     'floatShares': 16389662475,
     'beta': 1.202736,
     'enterpriseValue': 2801118478336,
     'priceHint': 2,
     'threeYearAverageReturn': None,
     'lastSplitDate': 1598832000,
     'lastSplitFactor': '4:1',
     'legalType': None,
     'lastDividendDate': 1636070400,
     'morningStarOverallRating': None,
     'earningsQuarterlyGrowth': 0.622,
     'priceToSalesTrailing12Months': 7.345673,
     'dateShortInterest': 1640908800,
     'pegRatio': 1.9,
     'ytdReturn': None,
     'forwardPE': 26.576736,
     'lastCapGain': None,
     'shortPercentOfFloat': 0.0058,
     'sharesShortPriorMonth': 112598907,
     'impliedSharesOutstanding': None,
     'category': None,
     'fiveYearAverageReturn': None,
     'previousClose': 166.23,
     'regularMarketOpen': 166.98,
     'twoHundredDayAverage': 147.0463,
     'trailingAnnualDividendYield': 0.0051133973,
     'payoutRatio': 0.1515,
     'volume24Hr': None,
     'regularMarketDayHigh': 169.64,
     'navPrice': None,
     'averageDailyVolume10Day': 88606660,
     'regularMarketPreviousClose': 166.23,
     'fiftyDayAverage': 167.7942,
     'trailingAnnualDividendRate': 0.85,
     'open': 166.98,
     'toCurrency': None,
     'averageVolume10days': 88606660,
     'expireDate': None,
     'algorithm': None,
     'dividendRate': 0.88,
     'exDividendDate': 1636070400,
     'circulatingSupply': None,
     'startDate': None,
     'regularMarketDayLow': 164.23,
     'currency': 'USD',
     'trailingPE': 29.324419,
     'regularMarketVolume': 90211048,
     'lastMarket': None,
     'maxSupply': None,
     'openInterest': None,
     'marketCap': 2687172083712,
     'volumeAllCurrencies': None,
     'strikePrice': None,
     'averageVolume': 92550356,
     'dayLow': 164.23,
     'ask': 163.1,
     'askSize': 1300,
     'volume': 90211048,
     'fiftyTwoWeekHigh': 182.94,
     'fromCurrency': None,
     'fiveYearAvgDividendYield': 1.17,
     'fiftyTwoWeekLow': 116.21,
     'bid': 163.05,
     'tradeable': False,
     'dividendYield': 0.0053,
     'bidSize': 800,
     'dayHigh': 169.64,
     'regularMarketPrice': 164.51,
     'preMarketPrice': 166.85,
     'logo_url': 'https://logo.clearbit.com/apple.com'}



We can get the <code>'country'</code> using the key country



```python
apple_info['country']
```




    'United States'



### Extracting Share Price


A share is the single smallest part of a company's stock  that you can buy, the prices of these shares fluctuate over time. Using the <code>history()</code> method we can get the share price of the stock over a certain period of time. Using the `period` parameter we can set how far back from the present to get data. The options for `period` are 1 day (1d), 5d, 1 month (1mo) , 3mo, 6mo, 1 year (1y), 2y, 5y, 10y, ytd, and max.



```python
apple_share_price_data = apple.history(period="max")
```

The format that the data is returned in is a Pandas DataFrame. With the `Date` as the index the share `Open`, `High`, `Low`, `Close`, `Volume`, and `Stock Splits` are given for each day.



```python
apple_share_price_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Dividends</th>
      <th>Stock Splits</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980-12-12</th>
      <td>0.100453</td>
      <td>0.100890</td>
      <td>0.100453</td>
      <td>0.100453</td>
      <td>469033600</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-12-15</th>
      <td>0.095649</td>
      <td>0.095649</td>
      <td>0.095213</td>
      <td>0.095213</td>
      <td>175884800</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-12-16</th>
      <td>0.088661</td>
      <td>0.088661</td>
      <td>0.088224</td>
      <td>0.088224</td>
      <td>105728000</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-12-17</th>
      <td>0.090408</td>
      <td>0.090845</td>
      <td>0.090408</td>
      <td>0.090408</td>
      <td>86441600</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-12-18</th>
      <td>0.093029</td>
      <td>0.093466</td>
      <td>0.093029</td>
      <td>0.093029</td>
      <td>73449600</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



We can reset the index of the DataFrame with the `reset_index` function. We also set the `inplace` paramter to `True` so the change takes place to the DataFrame itself.



```python
apple_share_price_data.reset_index(inplace=True)
```

We can plot the `Open` price against the `Date`:



```python
apple_share_price_data.plot(x="Date", y="Open")
```




    <AxesSubplot:xlabel='Date'>




![png](output_23_1.png)


### Extracting Dividends


Dividends are the distribution of a companys profits to shareholders. In this case they are defined as an amount of money returned per share an investor owns. Using the variable `dividends` we can get a dataframe of the data. The period of the data is given by the period defined in the 'history\` function.



```python
apple.dividends
```




    Date
    1987-05-11    0.000536
    1987-08-10    0.000536
    1987-11-17    0.000714
    1988-02-12    0.000714
    1988-05-16    0.000714
                    ...   
    2020-11-06    0.205000
    2021-02-05    0.205000
    2021-05-07    0.220000
    2021-08-06    0.220000
    2021-11-05    0.220000
    Name: Dividends, Length: 73, dtype: float64



We can plot the dividends overtime:



```python
apple.dividends.plot()
```




    <AxesSubplot:xlabel='Date'>




![png](output_28_1.png)


## Exercise


Now using the `Ticker` module create an object for AMD (Advanced Micro Devices) with the ticker symbol is `AMD` called; name the object <code>amd</code>.



```python
amd = yf.Ticker("AMD")
```

<b>Question 1</b> Use the key  <code>'country'</code> to find the country the stock belongs to, remember it as it will be a quiz question.



```python
amd.info['country']
```




    'United States'



<b>Question 2</b> Use the key  <code>'sector'</code> to find the sector the stock belongs to, remember it as it will be a quiz question.



```python
amd.info['sector']
```




    'Technology'



<b>Question 3</b> Obtain stock data for AMD using the `history` function, set the `period` to max. Find the `Volume` traded on the first day (first row).



```python
amd_share_price = amd.history(period="max")
amd_share_price.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Dividends</th>
      <th>Stock Splits</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980-03-17</th>
      <td>0.0</td>
      <td>3.302083</td>
      <td>3.125000</td>
      <td>3.145833</td>
      <td>219600</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-03-18</th>
      <td>0.0</td>
      <td>3.125000</td>
      <td>2.937500</td>
      <td>3.031250</td>
      <td>727200</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-03-19</th>
      <td>0.0</td>
      <td>3.083333</td>
      <td>3.020833</td>
      <td>3.041667</td>
      <td>295200</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-03-20</th>
      <td>0.0</td>
      <td>3.062500</td>
      <td>3.010417</td>
      <td>3.010417</td>
      <td>159600</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1980-03-21</th>
      <td>0.0</td>
      <td>3.020833</td>
      <td>2.906250</td>
      <td>2.916667</td>
      <td>130800</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
amd_share_price.reset_index(inplace=True)
```


```python
amd_share_price.plot(x="Date", y="Open")
```




    <AxesSubplot:xlabel='Date'>




![png](output_39_1.png)


<h2>About the Authors:</h2> 

<a href="https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.

Azim Hirjani


## Change Log

| Date (YYYY-MM-DD) | Version | Changed By    | Change Description        |
| ----------------- | ------- | ------------- | ------------------------- |
| 2020-11-10        | 1.1     | Malika Singla | Deleted the Optional part |
| 2020-08-27        | 1.0     | Malika Singla | Added lab to GitLab       |

<hr>

## <h3 align="center"> © IBM Corporation 2020. All rights reserved. <h3/>

<p>

