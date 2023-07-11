# Time-series-forcasting
I used ARIMA model to predict gold price

## Prepration
I converted *str* data type of date column to *datetime* and 
set it as the index of the dataframe, to make use of convenient classes and 
methods for representing and manipulating date and time data.
```
data['Date'] = pd.to_datetime(data['Date'])
data.index = data['Date']
```
### The Dataset's time serie  was unevenly (or unequally or irregularly) spaced,
so I get monthly avg from the data and turn it into regular form.

`data = data.resample('m').mean()`
## ARIMA Parameters
ARIMA parameters consist of : (AR = p, I =  d , MA = q )\
### Stationarity and parameter d
I used **Closed** time series for prediction, and check it's stationarity with the use of 
 **Augmented Dickey–Fuller test(ADF)**, as it was already stationary,i set  the d 
parameter to zero.
```
#check stationarity
result = adfuller(data.Close)
print('ADF Statistic: {}'.format(result[0]))
print('p-value: {}'.format(result[1]))
print('Critical Values: ')
for key, value in result[4].items():
  print('\t{0}: {1}'.format(key, round(value, 3)))
```
ADF results :

![ADF results](https://raw.githubusercontent.com/parniame/Time-series-forcasting/main/ADF_RESULTS.png)

It can be observed that the ADF statistic is negetiver than the 5% critical values.so data is stationary.

- P-value ≤ Significance level = 0.05
- Test statistic ≤ critical value
### ACF and PACF plots and the parameters q and p
So I used ACF plot to determine q parameter, and PACF plot to determine p parameter.

![ACF plot](https://github.com/parniame/Time-series-forcasting/blob/main/ACF_plot.png?raw=true)

#### The plot has a sharp cut-off after lag 1, so i set q parameter to 1.

![PACF plot](https://github.com/parniame/Time-series-forcasting/blob/main/PCAF_plot.png?raw=true)
#### The PACF plot has significant spikes at lags 1, 6, 9, 11.
I set p to 6 based on AIC results:

![ADF results](https://github.com/parniame/Time-series-forcasting/blob/main/AIC_results.png?raw=true)


#### The model is ARIMA(6,0,1)
## Evaluating The ARIMA Model
I seperated the dataset into train and test, and i used test dataset for prediction 
evaluate the model with *Mean Absolute Percent Error(MAPE)* and
*Root Mean Squared Error(RMSE)*.

- MAPE = 1.75%
- (RMSE) = 49.69
## Residuals Plot

![Residuals Plot](https://github.com/parniame/Time-series-forcasting/blob/main/Residuals.png?raw=true)

## Actual data vs Predicted data

![Residuals Plot](https://github.com/parniame/Time-series-forcasting/blob/main/Prediction_Actual_plot.png?raw=true)
