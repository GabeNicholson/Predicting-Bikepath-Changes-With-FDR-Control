# Predicting Bike Path Changes While Controlling for Multiple Testing

This project is a proof of concept attempt at correlating the residuals from OLS with time to make inferences on which bike paths changed over the course of two years. As far as I know, nothing like this has been tried before and it's validity/benefits with FDR control is discussed further in the code notebook. Because we are dealing with over 1.3 million samples and thousands of bike routes, the potential for false discoveries is substantial.

The notebook is split into two parts. The first is the [data exploration](https://github.com/GabeNicholson/Predicting-Bikepath-Changes-With-FDR-Control/blob/main/(1)%20data_exploration.ipynb) notebook that downloads and cleans the data. It also offers explanations for why certain data manipulations are made. For instance, deciding on discrete cutoff points for the time of day or which bike routes to include and which ones to discard as outliers. All of these decisions were made a-priori and were not data dependent.

The [second notebook](https://github.com/GabeNicholson/Predicting-Bikepath-Changes-With-FDR-Control/blob/main/(2)%20Model%20Prediction.ipynb) includes the statistical analysis. There are comments and explanations within that notebook which walk through the code and the results.

### **High Level Overview of the Methodology**

To account for the effects of confounders, we fit an ordinary least squares
regression of bike ride duration on the confounder variables and obtained the residuals—we fit an OLS model for each route, which allows the effects of these variables to vary by route. This is important because
some routes might be away from busy roads, meaning that riders would be less affected by traffic
and hence the day of week or time of day. Other routes may be between residential areas and
commercial centers, meaning that they would be very affected by traffic patterns and thus, the
day of week or time of day. Under the null hypothesis that the duration it takes to travel a given
route does not change over time given these confounding factors, these residuals should be
independent and identically distributed relative to the number of days since January 1st, 2010. In
other words, if there was no change to the route itself, the confounding factors should predict
seasonal changes, etc. that could make it appear as if the time it takes to travel the route changed
and the number of days since January 1st, 2010 should not help predict the residuals. If the
residuals become larger (smaller) as time passes, we are systematically underpredicting
(overpredicting) the duration of the rides later in our data; this could be *evidence that the time to
travel the route has increased (decreased) over time*. The most intuitive way to test this is to compute the correlation between these residuals and the days since the start of our data. By permuting the residuals and computing the correlation between the permuted residuals and the days since the start of our data many times, we can obtain a null distribution for this correlation and use a two-sided test to obtain a p-value on the correlation we observed.

There are several benefits to this method over others First. unlike a traditional
permutation test, it allows us to account for confounding variables. It we wanted to account for
the confounders with a permutation test, we would have to permute the durations across
observations that have the same values of the confounding variables. With enough confounders,
the number of observations that have the same value for each confounder becomes very small. In
the extreme case where there is onlv one observation per combination of confounding values,
there would be only one valid permutation. Our method allows us to permute regardless of the
values of confounding variables because we already accounted for them in our linear regression.
Furthermore, this method is more reliable than including the days since January 1st, 2010 in the
regression and using traditional t-tests on the regression coefficient because it relies on fewer
assumptions. The standard error and t-statistic obtained from a regression assume that the days
since January 1st, 2010 variable enters the model linearly. For inference to be valid, the errors
from the regression are also assumed to be homoscedastic and normally distributed. Here, we do
not use either assumption to have valid inference.
