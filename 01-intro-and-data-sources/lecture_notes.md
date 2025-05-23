# Data Sources – Lecture Notes

This document summarizes the main data sources and code snippets used for macroeconomic and stock market analysis in Week 1.

---

## 1. Macroeconomic Data (FRED)

**Source:** [FRED – Federal Reserve Economic Data](https://fred.stlouisfed.org/)

- **GDP (Potential)**
  - [Link](https://fred.stlouisfed.org/series/GDPPOT)
  - Code:  
    ```python
    gdppot = pdr.DataReader("GDPPOT", "fred", start=start)
    ```

- **Inflation – Core CPI**
  - [Link](https://fred.stlouisfed.org/series/CPILFESL)
  - *Description:* The "Consumer Price Index for All Urban Consumers: All Items Less Food & Energy" (Core CPI) excludes food and energy due to their price volatility.
  - Code:  
    ```python
    cpilfesl = pdr.DataReader("CPILFESL", "fred", start=start)
    ```

- **Interest Rates – Federal Funds Rate**
  - [Link](https://fred.stlouisfed.org/series/FEDFUNDS)
  - Code:  
    ```python
    fedfunds = pdr.DataReader("FEDFUNDS", "fred", start=start)
    ```

- **U.S. Treasury Securities (5-Year, 1/3/10-Year)**
  - [Link](https://fred.stlouisfed.org/series/DGS5)
  - Code:  
    ```python
    dgs5 = pdr.DataReader("DGS5", "fred", start=start)
    ```

- **Gold Reserves & Volatility**
  - Gold reserves (excluding China): [Link](https://fred.stlouisfed.org/series/TRESEGCNM052N)  
    ```python
    gold_reserves = pdr.DataReader("TRESEGCNM052N", "fred", start=start)
    ```
  - CBOE Gold ETF Volatility Index (GVZCLS): [Link](https://fred.stlouisfed.org/series/GVZCLS)  
    ```python
    gold_volatility = pdr.DataReader("GVZCLS", "fred", start=start)
    ```

---

## 2. Web Scraping for Macro Data

Example: Scraping macro indicators from TradingEconomics

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://tradingeconomics.com/united-states/indicators"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"
}
response = requests.get(url, headers=headers)

if response.status_code == 200:
    soup = BeautifulSoup(response.content, "html.parser")
    table_div = soup.find("div", class_="table-responsive")
    table = table_div.find("table")
    df = pd.read_html(str(table))[0]
    print(df)
else:
    print("Failed to retrieve data from the webpage.")
```

---

## 3. Stock & ETF Data (Yahoo Finance / yfinance)

- **About yFinance:** [Documentation](https://zoo.cs.yale.edu/classes/cs458/lectures/yfinance.html)

### 3.1 Indexes

- **DAX Index**
  - [Link](https://finance.yahoo.com/quote/%5EGDAXI)
  - Code:  
    ```python
    ticker_obj = yf.Ticker("^GDAXI")
    dax_daily = ticker_obj.history(start=start)
    ```

- **S&P 500**
  - [Link](https://finance.yahoo.com/quote/%5EGSPC)

- **Dow Jones Industrial Average**
  - [Link](https://finance.yahoo.com/quote/%5EDJI)

### 3.2 ETFs

- **S&P 500 ETF (VOO)**
  - [Link](https://finance.yahoo.com/quote/VOO)
  - Code:  
    ```python
    ticker_obj = yf.Ticker("VOO")
    ```

- **WisdomTree India Earnings Fund (EPI)**
  - [Link](https://finance.yahoo.com/quote/EPI/history?p=EPI)
  - Code:  
    ```python
    epi = yf.Ticker('EPI')
    epi.get_dividends()  # Get dividends as Series
    ```

---

## 4. Paid Data Sources

### 4.1 Polygon.io (News Endpoint)
- [Article](https://pythoninvest.com/long-read/chatgpt-api-for-financial-news-summarization)
- [API Docs](https://polygon.io/docs/stocks/get_v2_reference_news)

### 4.2 Alpha Vantage
- [Article](https://pythoninvest.com/long-read/stock-screening-using-paid-data)
- [API Docs](https://www.alphavantage.co/documentation/#fundamentals)

---

## 5. Financial Reporting (Yahoo Finance)

```python
nvda = yf.Ticker('NVDA')
nvda.financials      # Yearly financials for the last 4 years
nvda.balance_sheet   # Balance sheet
```

---

## 6. Company Info (Web Scraping Example)

```python
import requests
from bs4 import BeautifulSoup

url = "https://companiesmarketcap.com/"
# Example: emulate clicking the link and downloading the content
```

---

## Remarks

- **OHLCV** = Open, High, Low, Close, Volume