# TIME SERIES PROJECT
# REAL ESTATE PRICES FORECAST - ZILLOW
Name: Julie chepngeno
## Introduction
### Overview
In the dynamic world of real estate investment, making informed decisions is paramount to success. In 2021, USA's real estate industry boasted a staggering valuation of USD 3.69 trillion. Forecasts projected a promising future, with an estimated compound annual growth rate of 5.2% anticipated between 2022 and 2030. This projected growth, coupled with the rising population rates, presents an immensely lucrative opportunity for real estate investors to capitalize on substantial profits.

This project dives into the realm of time series modeling, using data sourced from [Zillow Research](https://www.zillow.com/research/data/), a renowned real estate data provider, and the main focus is to leverage time series modeling techniques to forecast real estate prices for the top 5 best zip codes to invest in. It offers well-thought-out and insightful recommendations which will guide the firm's decisions which will go a long way in mitigating risks associated with each investment based on some factors like market volatility. A market analysis is conducted to identify areas of high demand and growth for optimal investment and prioritize investments with the potential substantial returns based on factors like property appreciation and market demand.

### Problem Statement
*"What are the top 5 best locations to invest in ?"*

This seemingly straightforward question, however, it's actually quite tricky and has many important details that need to be thought about very carefully. The choice of where to invest in should not be based solely on profit margins; it must encompass a holistic understanding of various factors, including risk, market trends, and investment horizons. Specifically the aim is to identify the top five locations that offer the highest Return on Investment (ROI) potential by conducting an analysis of various factor including past price trends, growth rates, market demand, and other relevant indicators

### Objectives
**main objective:**
- Develop a forecasting model that can accurately predict real estate price movements in different areas and assist in identifying the most favorable locations for investment between the period of April 1996 to April 2018.
**specific objectives:**
 Evaluate Return on Investment (RoI).
- Identify underlying patterns, trends, and seasonality in the real estate price data.
- Build a time series predictive model that can forecast real estate prices.
- Evaluate the forecasting model's performance by comparing its predictions against actual real estate prices.
- forecast house prices in the next subsequent years.
### Success Metrics
In this project, to determine the best model for our analysis three important metrics will be considered: **AIC (Akaike Information Criterion)**, **BIC (Bayesian Information Criterion)**,  **Mean Absolute Percentage Error (MAPE)**, **RMSE (Root Mean Square Error)**. These metrics will allow us to assess the goodness of fit and predictive performance of different models.

## Data Understanding
The dataset used in this project consists of historic median house prices from various regions in the USA. It covers a time period of 22 years, specifically from April 1996 to April 2018. The dataset was obtained from the [Zillow website](https://www.zillow.com/research/data/).

The dataset has a total of 14,723 rows and 272 columns. Four columns are categorical while the rest are numerical.

More details about the  columns are described as below:

- ``RegionID:`` A unique identifier for each region.

- ``RegionName:`` The names of the regions, represented by zip codes.

- ``City:`` The corresponding city names for each region.

- ``State:`` The names of the states where the regions are located.

- ``Metro:`` The names of the metropolitan areas associated with the regions.

- ``County Name:`` The names of the counties where the regions are situated.

- ``Size Rank:`` The ranking of the zip codes based on urbanization.

- ``Date Columns (265 Columns):`` These columns represent median house prices for each region over the years.

## Data Preparation
The dataset has 14723 entries and 272 columns
The dataset has 0 duplicates
- For consistency in the representation of zipcodes, the column will be restructured to have all the digits as five in number. Some zip codes contain four digits, suggesting a missing zero at the beginning. Therefore, the column will be modified to include a leading zero for zipcodes with four digits, aligning them with the standard format of five-digit zip codes ensuring uniformity.

## Data Preprocessing
### Feature Engineering
We'll feature engineer two new columns: one for calculating the ``return on investment (ROI)`` and another for determining the ``coefficient of variation (CV)``.

- ``Return on Investment (ROI)`` is an approximate measure of an investment's profitability.

- ``coefficient of variation (CV)`` measures the extent of data point dispersion around the mean and indicates the ratio of standard deviation to the mean. This enables investors to evaluate the level of risk involved relative to the ROI and allows them to assess the risk-to-reward ratio before investing the money. A lower coefficient provides the optimum risk-to-reward ratio with high returns and lower risks.

### Reshaping the data to Long Format
The data has been in the Wide Format, and it makes the dataframe intuitive and easy to read. However, there are problems with this format when it comes to actually learning from the data, because the data only makes sense if you know the name of the column that the data can be found in. Since column names are metadata, our algorithms will miss out on what dates each value is for.

This means that before we pass this data to our time  series models, we'll need to reshape our dataset to Long Format. This transformation involves restructuring the dataframe to have a single column for the Date and another column for the corresponding values. The Date column will be set as the index to establish the temporal order of the data points.

### Indexing Datetime
Working with time series data in Python, having dates (or datetimes) in the index can be very helpful, especially if they are of DatetimeIndex type as the index allows for intuitive and efficient time-based indexing and slicing operations. It enables easy access to specific time periods, such as a particular day, month, or year. Additionally, the DatetimeIndex provides convenient methods for resampling, time shifting, and frequency conversion.

## EDA
In this section we seek to answer various questions including top 5 Prerrable zipcodes to invest in, top 5 zipcodes with high ROI and more.

- The distribution of median house prices is highly skewed to the right, as indicated by the long tail on the right side of the distribution. The majority of the values are concentrated around lower values, with a few extreme values on the higher end.="images\output1.png"/> 
 When making investment decisions balance your risk-return trade-off by considering both ROI (return potential) and CV (risk). Properties with a history of high ROI and low CV may be preferable.= "images\output2.png"/>
 - The plot of the housing prices indicates an overall upward trend from 1996 to around 2007, followed by a downward trend until approximately 2013, and then an upward trend again.="images\output3.png" />

 ## Modelling
 The presence of seasonality and trend in a time series can significantly impact the forecasted values. These components introduce systematic patterns and directional movements that need to be considered when making predictions. By understanding and accounting for the effects of seasonality and trend, more accurate forecasts can be generated for the time series data.

 ### Time Series Decomposition
 - Breaking the non-stationary time series into its three components—trend, seasonality, and residuals—is indeed a helpful approach for investigating the pattern in the past and aiding in the forecasting of future house values."images\output 4.png" />

 ### Stationarity Check
 By computing the **rolling mean** and **rolling standard deviation**, this code aims to assess the stationarity of the series. Stationarity refers to the property of a time series where its statistical properties such as mean and variance remain constant over time. The rolling mean and standard deviation can help identify trends or patterns in the data and provide insights into its stationarity.="images\output5.png" />

 ### Detrending
 There is trend and seasonality in the dataset and so we'll use differencing to detrend the data to remove them from the series.

 ### Auto-correlation(ACF) and Partial Auto-correlation(PACF) plots
 - From the acf plot, lags between 1 and 14 are in the statistically significant region meaning time periods within that span can affect present values. The plot decays exponentially (tails off) suggesting the presence of a seasonal pattern and the presence of an autoregressive (AR) process.

- From the pacf plot it shows significant spikes at multiple lags but decays afterward, it suggests the presence of a mixed autoregressive-moving average (ARMA) process.

### Building Models
1. AR(1) Model - Baseline Model
2.ARMA Model
3.ARIMA model
4.Auto_ARIMA
5.SARIMA Model
6.SARIMA tuned
SARIMA (tuned) model shows improvement compared to the previous models.
Based on the following observations from the diagnostics above the model is retained.
- Lower AIC value indicating a better fit.
- No correlation in the residuals. This suggests it captures underlying patterns effectively.
- Q-Q plot indicates residuals are taken from a N(0,1) distribution.
- Histogram of residuals shows a mean of 0, indicating unbiased predictions.

### Model Evaluation and Performance
We compare predicted values to real values of the time series, which will help us understand the accuracy of our forecasts
#### Non dynamic forecast
**non-dynamic forecast** refers to a type of forecasting where the model's predictions for future time periods are not updated or revised as new actual data becomes available. In other words, once the forecast is generated, it remains fixed and does not adapt or change based on incoming information.="images\output6.png" />

#### Dynamic Forecast
**Dynamic forecasting**, also known as rolling forecasting or real-time forecasting, refers to a method of making predictions for future values in a time series by continually updating the forecast as new data becomes available. Unlike static or non-dynamic forecasting, which generates fixed forecasts that do not change over time, dynamic forecasting is adaptive and responsive to changes in the underlying data.

- The mean absolute percentage error (MAPE) between the forecasted values and the actual values for the dynamic forecast is 3.011, indicating a higher error rate compared to the non-dynamic forecast.

- Also the RMSE is 26101.66 higher than the non-dynamic model

#### Future Prediction
- The model predicts an upward trend for the next three years. This shows that price will continue to rise but by a slow rate.
## Conclusion
- After few iterations and model tuning the model was able to achieve low AIC and BIC values which were 1714.743 and 1749.967 respectively. This low values indicates a better model that had a good fit.

- Mean Absolute Percentage Error was also used as an evaluetion metric of the non dynamic and dynamic forecasting. Non-Dynamic forecating had a better MAPE of 0.0505 indicating that, on average, the forecasted values from the non-dynamic SARIMA model have an absolute percentage error of approximately 5%. This means that the forecasted values deviate from the actual values by around 5% on average.

- Through ROI and CV we were able to get the topfive zipcodes with a high ROI and low CV as when making investment decisions you should balance your risk-return trade-off by considering both ROI (return potential) and CV (risk). This cities were also selected considering the positive correlation between ROI and CV which means that the higher risk the higher the Return on Investment (ROI). 

## Recommendations
- The company should invest in three cities newyork, Sea Island, and Kilauea which had the highest ROI and low CV with specific zipcodes being; ``New York;`` 10021, 10014, 11217, ``Sea Island;`` 31561  ``Kilauea;`` 96714. New york potentially being the most profitable, as the predictions are that the price will slightly increase in the future.

- The city Atherton should be looked into carefully due to the high price volatility and low returns.

- The risk is abit high but considering how high the return on investment is and the positive correlation between the risk and return, including also the increase in price in the the future predictions then these locations are preferrable.

- Other factors like amenities, infrustructure development, urbanization of the cities should be looked into as this could also majorly have a big effect on the house prices. 








