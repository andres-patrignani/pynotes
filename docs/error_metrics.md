# Error Metrics

Qunatifying the error between predictions and observations is central to assessing the performance of a model. Error metrics also play an important role assessing the accuracy of sensors relative to ground-truth observations. 

There are many error metrics, each with its pros and cons. In this exercise we will code some of the most common error metrics and we will briefly discuss their application. There are also several Python libraries (e.g. sciki-learn) that contain multiple error metrics. Using libraries is a good idea since it allows for reproducibility and there is also a high chance that the code has been tested by multiple people. However, the compulsive use of libraries by beginners also hinders understanding. This is the reason why in this exercise we will code our own error metrics.

>In most examples below the error metrics are self-contained. This means that in most cases the computation of an error metric does not rely on the results of another cell. This is certainly not ideal, but from the point of view of learning the computation of the different error metrics it prevents tracking back the different variables.


## Inputs

We will use a dataset obtained from the calibration of an actual soil moisture sensor. The soil moisture readings were obtained with a calibrated soil water reflectometer and the ground-truth values we re obtained using the thermo-gravimetric method (i.e. soil samples were oven-dried to find out the actual soil water content).



```python
# Dataset
y_obs =  [0.190,0.438,0.304,0.408,0.003,0.459,0.409,0.403,0.174,0.033,0.317,0.023]
y_pred = [0.233,0.481,0.319,0.450,0.004,0.466,0.458,0.497,0.166,0.013,0.396,0.003]

```


```python
# Convert to Numpy array
y_obs = np.array(y_obs)
y_pred = np.array(y_pred)

```


```python
# Import modules
import pandas as pd
import numpy as np

```

## Residuals

The difference between observed and predicted: $residuals = y_{obs} - y_{pred}$


```python
residuals = y_obs - y_pred
print(residuals)
```

    [-0.043 -0.043 -0.015 -0.042 -0.001 -0.007 -0.049 -0.094  0.008  0.02
     -0.079  0.02 ]


## Sum of residuals (SRES)


```python
sres = np.nansum(residuals)
print(sres)
```

    -0.325


## Sum of the absolute of residuals (SARES)


```python
sares = np.nansum(np.abs(residuals))
print(sares)
```

    0.4210000000000001


## Sum of squared errors (or residuals)


```python
sse = np.nansum(residuals**2)
print(sse)
```

    0.024079000000000007


## Mean bias error (MBE)

**Positive** = under-prediction. 

**Negative** = Over-prediction.

A bias equal to zero can be a consequence of small errors or very large errors balanced by opposite sign. It is always recommended to include other error metrics in addition to the mean bias error.


```python
mbe = np.nanmean(residuals)
print(mbe)
```

    -0.027083333333333334


## Mean squared error (MSE)


```python
mse = np.nanmean(residuals**2)
print(mse)
```

    0.0020065833333333337


## Root mean squared error (RMSE)

One of the most popular error metrics in modeling. When comparing two estimates where none of them represents the ground truth it is better to name this error metric the Root Mean Squared "Difference" to emphasize that is the difference between two estimates. the word error is typically reserved for deviations from a gold standard or ground truth value.

>Sensitive to outliers. The computation of the mean of the squared residuals is largely dominated by outliers.


```python
rmse = np.sqrt(np.nanmean(residuals**2))
print(rmse)
```

    0.04479490298385893


## Relative root mean squared error (RRMSE)

More meaningful than RMSE to compare the errors from two different data sets with different units or ranges. Sometimes the RRMSE is computed by dividing the RMSE over the range of the observed values rather than the average of the observed values.



```python
rrmse = np.sqrt(np.nanmean(residuals**2)) / np.nanmean(y_obs)
print(rrmse)
```

    0.1700534121500497


## Mean absolute error (MAE)

Robust error metric less sensitive to outliers compared to the RMSE. The computation of the mean of the absolute residuals is not squared, and thus less impacted by outliers. Read Willmott and Matsuura, 200 for more details about RMSE vs MAE.

For even a more robust error metric you can compute the median absolute error, which represents the most typical error.



```python
mae = np.nanmean(np.abs(residuals))
print(mae)
```

    0.03508333333333334


## Willmott index of agreement (D)

A perfect model will have y_obs = y_pred, and consequently the SSE will be zero, and hence d=1.


```python
abs_diff_pred = np.abs(y_pred - np.nanmean(y_obs))
abs_diff_obs  = np.abs(y_obs  - np.nanmean(y_obs))

willmott = 1 - np.nansum(residuals**2) / np.nansum((abs_diff_pred + abs_diff_obs)**2)
print(willmott)
```

    0.9841462973021908


## References

Willmott, C.J., Robeson, S.M. and Matsuura, K., 2012. A refined index of model performance. International Journal of Climatology, 32(13), pp.2088-2094.

Willmott, C.J. and Matsuura, K., 2005. Advantages of the mean absolute error (MAE) over the root mean square error (RMSE) in assessing average model performance. Climate research, 30(1), pp.79-82.

Willmott, C.J., 1981. On the validation of models. Physical geography, 2(2), pp.184-194.
