# Module-11-Challenge

## Background
You’re a growth analyst at MercadoLibre Links to an external site.. With over 200 million users, MercadoLibre is the most popular e-commerce site in Latin America. You've been tasked with analysing the company's financial and user data in clever ways to help the company grow. So, you want to find out if the ability to predict search traffic can translate into the ability to successfully trade the stock.

## Table of Contents
1. [Part One:  Find Unusual Patterns in Hourly Google Search Traffic]
2. [Part Two: Mine the Search Traffic Data for Seasonality]
3. [Part Three: Relate the Search Traffic to Stock Price Patterns]
4. [Part Four: Create a Time Series Model by Using Prophet]
5. [Part Five (Optional): Forecast the Revenue by Using Time Series Models]
6. [Conclusion]
7. [Results]

## Part One:  Find Unusual Patterns in Hourly Google Search Traffic
- [x] Read the search data into a DataFrame, and then slice the data to just the month of May 2020.
- Use hvPlot to visualise the results
- Do any unusual patterns exist?
  - **Answer:** Yes
- Calculate the total search traffic for the month.
- Compare the value to the monthly median across all months.
- Did the Google Search traffic increase during the month that MercadoLibre released its financial results?
  - **Answer:** Yes, the Google search traffic had been increased.
 
```python
# Store data in dataframe
df_mercado_trends = pd.read_csv('google_hourly_search_trends.csv', index_col="Date", parse_dates=True)
# Slice the DataFrame to just the month of May 2020
df_may_2020 = df_mercado_trends.loc["2020-05"]
# Use hvPlot to visualize the data for May 2020
df_may_2020.hvplot(title="Search trends MercadoLibre May 2020")

# Calculate the sum of the total search traffic for May 2020
traffic_may_2020 = df_may_2020.loc["2020-05"].sum()

# Calcluate the monhtly median search traffic across all months 
# Group the DataFrame by index year and then index month, chain the sum and then the median functions
median_monthly_traffic = df_mercado_trends["Search Trends"].groupby(by=[df_mercado_trends.index.year, df_mercado_trends.index.month]).median()
```


## Part Two: Mine the Search Traffic Data for Seasonality
- For mining the search traffic data for predictable seasonal patterns of interest in the company, to do so, complete the following steps:
  - Group the hourly search data to plot the average traffic by the day of the week (for example, Monday vs. Friday).

 ```python
# Group the hourly search data to plot (use hvPlot) the average traffic by the day of week 
group_level_weeks = df_mercado_trends.index.dayofweek
df_mercado_trends.groupby(group_level_weeks).mean().hvplot(title = "Average Google search traffic grouped by day of the week")
```

  - Using hvPlot, visualise this traffic as a heatmap, referencing index.hour for the x-axis and index.dayofweek for the y-axis
  - Group the search data by the week of the year.
```python
# Group the hourly search data to plot (use hvPlot) the average traffic by the week of the year
group_hourly_search_data = df_mercado_trends.index.isocalendar().week
df_mercado_trends.groupby(group_hourly_search_data).mean().hvplot(title="The average search traffic grouped by week of the year")
```
  - Does the search traffic tend to increase during the winter holiday period (Weeks 40 through 52)?
    - **Answer:** Yes, the average search traffic increases during the winter holiday period.


## Part Three: Relate the Search Traffic to Stock Price Patterns
  - Read in and plot the stock price data. Concatenate the stock price data to the search data in a single DataFrame.
```python
# Concatenate the df_mercado_stock DataFrame with the df_mercado_trends DataFrame
# Concatenate the DataFrame by columns (axis=1), and drop and rows with only one column of data
mercado_stock_trends_df = pd.concat([df_mercado_trends, df_mercado_stock], axis=1).dropna()

# View the first and last five rows of the DataFrame
mercado_stock_trends_df.head()
```
  - Slice the data to just the first half of 2020 (2020-01 to 2020-06 in the DataFrame)
  - 
