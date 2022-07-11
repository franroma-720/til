# Forecasting

References:
- [Forecasting: Principles and Practice](https://otexts.com/fpp3/)

## R packages and functions

The `fpp3` package is a useful library for time series Analysis in R.
Time series data can be stored in `tsibble` objects, which are extended tidy data frames with a temporal structure.

### Plot Type

- `autoplot`: standard time plot
- `gg_season`: seasonal plots
- `gg_subseries`: standard time plot broken out by season
- `gg_lag`: each time series value is plotted against itself across a variety of different shifts, or lags (i.e. `y_t against y_{t+k}`)
  - The `ACF()` function calculates the autocorrelation of the time series.
    This function in combination with `autoplot` can be used to see how the correlations change with lag k, also known as a correlogram.

## Time Series Patterns

- Trend: long term increase or decrease in the data
- Seasonal: time series if affected by seasonal factors such as the time of year or day of the week (fixed and known period))
- Cyclic: data exhibits rises and falls that are not of a fixed frequency

**Autocorrelations** are the correlation between lagged values of a time series.

- When data have a trend, the autocorrelations for small lags tend to be large and positive because observations nearby in time are also nearby in value.
  So the AutoCorrelation Function (ACF) of a trended time series tends to have positive values that slowly decrease as the lags increase.
- When data are seasonal, the autocorrelations will be larger for the seasonal lags (at multiples of the seasonal period) than for other lags.
- When data are both trended and seasonal, you see a combination of these effects.
- Time series that shows no autocorrelation are called **white noise**.
  For a white noise series, we expect 95% of the spikes in the ACF to lie within ±2/√T where T is the length of the time series.
  If one or more large spikes are outside these bounds, or if substantially more than 5% of spikes are outside these bounds, then the series is probably not white noise.


## Time Series Decomposition

Time series are typically decomposed into three componenets: a trend-cycle componenet, a seasonal component, and a remainder component.
We can use additive or multiplicative decompositions.
Before decomposing, it can sometimes be helpful to transform the data using calendar adjustments, population adjustments, inflation adjustments, or mathematical transformations.

- There are classical decomposition methods originating in the 1920s, which are rarely used today due to shortcomings (although more advanced methods build off of this approach).
- There is also the STL decomposition method - “Seasonal and Trend decomposition using Loess”.


## Time Series Features

Interesting features to calculate over a time series include quantiles (0%, 25%, 50%, 75%, 100%), autocorrelation features, STL features, and a number of other features from the `feasts` package, which includes 55 features. PCA can be a useful method for identifying which features are most significant rather than reviewing them all individually against one another.


## Forecasting Toolbox

### Simple Forecasting Methods

These simple methods typically serve as benchmarks rather than the desired models of choice. If a more advanced model is not better than these simple methods, then it is not worth considering.

- Mean Method: the forecast of all future values are equal to the average of the historical data
- Naïve Method: the forecast of all future values are equal to the value of the last observation
- Seasonal Naïve Method: the forecast of all future values are equal to the values of the last observation from the same season (e.g., the same month of the previous year)
- Drift Method: the equivalent of drawing a line between the first and last observations and extrapolating it into the future


### Residuals

The residuals in a time series model are what is left over after fitting a model. If a transformation has been used in the model, the residuals on the transaformed scale are called **innovation residuals**.

A good forecastign method will yield innovation residuals with the following properties:

1. The innovation residuals are uncorrelated.
2. The innovation residuals have zero mean.

Additionally, it is useful but not necessary for the innovation residuals to also have the following properties:

3. The innovation residuals have constant variance. This is known as “homoscedasticity”.
4. The innovation residuals are normally distributed.

Note that residuals are calculated on the training set while forecast "errors" are calculated on the test set. Additionally, residuals are based on one-step forecasts while forecast errors can involve multi-step forecasts.

### Errors

Error metrics are a way to measure point forecast accuracy.

The two primary scale dependent error metrics used in forecasting are the Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE). A forecast method that minimises the MAE will lead to forecasts of the median, while minimising the RMSE will lead to forecasts of the mean.

Both MAE and RSME have scaled error counterparts, MeanAbsolute Scaled Error (MASE) and Root Mean Squared Scaled Error (RMSSE), scaling the errors based on the training MAE from a (seasonal or not) naïve forecast method. A scaled error is less than one if it arises from a better forecast than the average one-step naïve forecast computed on the training data. Conversely, it is greater than one if the forecast is worse than the average one-step naïve forecast computed on the training data.

Percentage error metrics like Mean Absolute Percentage Error (MAPE) are commonly used but will be infinite or undefined is any observed values in the time series are 0. They also have the disadvantage that they put a heavier penalty on negative errors than on positive errors.

