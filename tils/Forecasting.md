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

**Autocorreltations** are the correlation between lagged values of a time series.

- When data have a trend, the autocorrelations for small lags tend to be large and positive because observations nearby in time are also nearby in value.
  So the ACF of a trended time series tends to have positive values that slowly decrease as the lags increase.
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
