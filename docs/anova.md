# Analysis of Variance

Perhaps there is no other statistical analysis as popular as the ANalysis Of VAriance. The main purpose of an ANOVA test is to detect whether the means of a specific observation variable between groups (e.g. grain yield of corn plots subjected to different levels of fertilizer application, biomass production of two watersheds exposed to different burning regimes, etc.) are *significantly* different. The word *significantly* has special meaning in this context and represents conclusion of our analysis under a set  error rate. As you can imagine, detecting whether two or more things are different from each other requires agreement on some baseline.

A one-way anova is used to test the hypothesis that the samples in the objective variable (e.g. biomass production) of different groups are drawn from populations with the same mean against the alternative hypothesis that the population means are not all the same. Rejecting the null hypothesis would imply that at least one of the means is different. 

In this exercise we will use a dataset of corn yield for different treatments of nitrogen fertilizer on multiple US states. The dataset is a subset of the study published by Tremblay et al., 2012 (see references for more details) and it was obtained from (http://www.nue.okstate.edu/).

Does the application of nitrogen fertilizer result in systematically greater corn yields or are the yield responses a mere consequence of random noise due to sampling?



```python
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.formula.api import ols
from statsmodels.stats.anova import anova_lm
from statsmodels.stats.multicomp import MultiComparison

```


```python
# Load data
df = pd.read_csv("../datasets/corn_nue_multiple_locs.csv")
df.head()

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
      <th>Year</th>
      <th>State</th>
      <th>Site</th>
      <th>Textural_class</th>
      <th>Replications</th>
      <th>Treatments</th>
      <th>N_Planting_kg_ha</th>
      <th>N_Sidedress_kg_ha</th>
      <th>N_Total_kg_ha</th>
      <th>Yield_T_ha</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2006</td>
      <td>Illinois</td>
      <td>Pad</td>
      <td>Silt loam</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2006</td>
      <td>Illinois</td>
      <td>Pad</td>
      <td>Silt loam</td>
      <td>1</td>
      <td>3</td>
      <td>36</td>
      <td>0</td>
      <td>36</td>
      <td>4.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2006</td>
      <td>Illinois</td>
      <td>Pad</td>
      <td>Silt loam</td>
      <td>1</td>
      <td>5</td>
      <td>36</td>
      <td>54</td>
      <td>90</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2006</td>
      <td>Illinois</td>
      <td>Pad</td>
      <td>Silt loam</td>
      <td>1</td>
      <td>7</td>
      <td>36</td>
      <td>107</td>
      <td>143</td>
      <td>10.52</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2006</td>
      <td>Illinois</td>
      <td>Pad</td>
      <td>Silt loam</td>
      <td>1</td>
      <td>9</td>
      <td>36</td>
      <td>161</td>
      <td>197</td>
      <td>11.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Print some useful properties of the dataset
print(df['Site'].unique())        # Locations
print(df['Treatments'].unique())   # Treatments
print(df['Replications'].unique()) # Replications
print(df.shape)

```

    ['Pad' 'Dixon' 'Manhattan' 'Copeland' 'Diederick']
    [1 3 5 7 9]
    [1 2 3 4]
    (120, 10)



```python
# Examine yield data using boxplots for all locations combined
df.boxplot(column='Yield_T_ha', by='N_Total_kg_ha')
plt.show()

```


![png](anova_files/anova_4_0.png)



```python
# Examine yield by state
df.boxplot(column='Yield_T_ha', by='State')
plt.show()

```


![png](anova_files/anova_5_0.png)



```python
# Examine yield by site
df.boxplot(column='Yield_T_ha', by='Site')
plt.show()

```


![png](anova_files/anova_6_0.png)


## ANOVA assumptions

1. Samples drawn from a population are normally distributed. **Test**: Shapiro-Wilk

2. Samples drawn from all populations have (approximately) the same variance. This property is called homoscedasticity or homogeneity of variances." **Tests**: Bartlett's and Levene's tests.

3. Samples are independent of each other. Test: No test. Here we rely on the nature of the variable being observed and the experimental design.


## One-way ANOVA

Here we will compare an independent variable with a single predictor. The predictor `N_Total_kg_ha` will be used as a categorical varaible. Alternatively we could use the `Treatments` column, but it is easier to read the table if we present the values using the actual treatment values, so that we quickly devise which Nitrogen rates show statistical differences.



```python
# Anova table with statsmodels
formula = 'Yield_T_ha ~ C(N_Total_kg_ha)'
lm = ols(formula, data=df).fit()
anova_statsmodels = anova_lm(lm)
anova_statsmodels

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
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>C(N_Total_kg_ha)</th>
      <td>4.0</td>
      <td>386.085542</td>
      <td>96.521385</td>
      <td>11.431259</td>
      <td>7.582811e-08</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>115.0</td>
      <td>971.018108</td>
      <td>8.443636</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



The ANOVA table shows the there is significant differences between treatments. The catch is that we don't know which groups are different. The ANOVA table only tells us that at least one  group has a mean value that is substantially (read *significantly*) different from the rest.

The **F** is the F-statistic to test the null hypothesis that the corresponding coefficient is zero.

The **pValue** of the F-statistic indicates whether a factor is not significant at the 5% significance level given the other terms in the model.

A mean multicomparison test can help us identify which treatments show signifcant differences.



```python
# Multicomparison test
groups = MultiComparison(df["Yield_T_ha"],df['N_Total_kg_ha']).tukeyhsd(alpha=0.05)
print(groups)

```

    Multiple Comparison of Means - Tukey HSD, FWER=0.05
    ===================================================
    group1 group2 meandiff p-adj   lower  upper  reject
    ---------------------------------------------------
         0     36   1.3175 0.5165 -1.0074 3.6424  False
         0     90   3.0358  0.004  0.7109 5.3607   True
         0    143   4.3096  0.001  1.9847 6.6345   True
         0    197   4.7454  0.001  2.4205 7.0703   True
        36     90   1.7183 0.2499 -0.6066 4.0432  False
        36    143   2.9921 0.0047  0.6672  5.317   True
        36    197   3.4279  0.001   1.103 5.7528   True
        90    143   1.2738 0.5458 -1.0512 3.5987  False
        90    197   1.7096 0.2547 -0.6153 4.0345  False
       143    197   0.4358    0.9 -1.8891 2.7607  False
    ---------------------------------------------------



```python
# Visualize significantly different groups relative to a specific group
groups.plot_simultaneous(comparison_name=0)
plt.xlabel('Cron yield in T/ha', size=12)
plt.ylabel('Nitrogen Treatments in kg/ha', size=12)
plt.show()

```


![png](anova_files/anova_12_0.png)


## Two-way ANOVA

In this case we will add two predictors variables, soil textural class and total nitrogen applied. In many cases researchers add `Location` as a proxy for local environmental conditions (including soil) and `Year` as a proxy for the particular weather conditions during each growing season. In this case we have soil textural class available, so we will make use of that first, since it will give our results broader applications that, in principle, can be related to soil types elsewhere.



```python
# Two predictors
formula = 'Yield_T_ha ~ C(N_Total_kg_ha) + C(Textural_class)'
anova_lm(ols(formula, data=df).fit())

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
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>C(N_Total_kg_ha)</th>
      <td>4.0</td>
      <td>386.085542</td>
      <td>96.521385</td>
      <td>11.750059</td>
      <td>5.022955e-08</td>
    </tr>
    <tr>
      <th>C(Textural_class)</th>
      <td>1.0</td>
      <td>34.560000</td>
      <td>34.560000</td>
      <td>4.207172</td>
      <td>4.254546e-02</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>114.0</td>
      <td>936.458108</td>
      <td>8.214545</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Soil textual class was barely significant considering all locations and years at the the 0.05 level.


```python
# Two predictors with interaction
formula = 'Yield_T_ha ~ C(N_Total_kg_ha) * C(Textural_class)'
anova_lm(ols(formula, data=df).fit())

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
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>C(N_Total_kg_ha)</th>
      <td>4.0</td>
      <td>386.085542</td>
      <td>96.521385</td>
      <td>12.530231</td>
      <td>1.960683e-08</td>
    </tr>
    <tr>
      <th>C(Textural_class)</th>
      <td>1.0</td>
      <td>34.560000</td>
      <td>34.560000</td>
      <td>4.486516</td>
      <td>3.641510e-02</td>
    </tr>
    <tr>
      <th>C(N_Total_kg_ha):C(Textural_class)</th>
      <td>4.0</td>
      <td>89.119178</td>
      <td>22.279795</td>
      <td>2.892322</td>
      <td>2.547188e-02</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>110.0</td>
      <td>847.338930</td>
      <td>7.703081</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



The interaction of nitrogen rate and textural class also resulted statistically significant at the 0.05 level.


```python
# Classical ANOVA with Treatment, Location, and Year interactions
formula = 'Yield_T_ha ~ C(N_Total_kg_ha) * State * C(Year)'
anova_lm(ols(formula, data=df).fit())

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
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>C(N_Total_kg_ha)</th>
      <td>4.0</td>
      <td>386.085542</td>
      <td>96.521385</td>
      <td>20.522835</td>
      <td>3.270251e-12</td>
    </tr>
    <tr>
      <th>State</th>
      <td>2.0</td>
      <td>234.432785</td>
      <td>117.216393</td>
      <td>24.923106</td>
      <td>1.989675e-09</td>
    </tr>
    <tr>
      <th>C(Year)</th>
      <td>2.0</td>
      <td>112.504702</td>
      <td>56.252351</td>
      <td>11.960642</td>
      <td>2.328310e-05</td>
    </tr>
    <tr>
      <th>C(N_Total_kg_ha):State</th>
      <td>8.0</td>
      <td>140.688973</td>
      <td>17.586122</td>
      <td>3.739245</td>
      <td>7.649526e-04</td>
    </tr>
    <tr>
      <th>C(N_Total_kg_ha):C(Year)</th>
      <td>8.0</td>
      <td>36.595110</td>
      <td>4.574389</td>
      <td>0.972628</td>
      <td>4.621589e-01</td>
    </tr>
    <tr>
      <th>State:C(Year)</th>
      <td>4.0</td>
      <td>3.702425</td>
      <td>0.925606</td>
      <td>0.196807</td>
      <td>9.394924e-01</td>
    </tr>
    <tr>
      <th>C(N_Total_kg_ha):State:C(Year)</th>
      <td>16.0</td>
      <td>17.058994</td>
      <td>1.066187</td>
      <td>0.226698</td>
      <td>9.991834e-01</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>95.0</td>
      <td>446.796537</td>
      <td>4.703121</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



In a multi-treatment, multi-year, and multi-state study, it is not surprising that treatment, site, and year resulted highly significant (P<0.01).

Note that the interactions `N_Total_kg_ha:Year`, `State:Year`, and `N_Total_kg_ha:State:Year` were not significant at the P<0.05 level.

## References

Tremblay, N.,  Y.M. Bouroubi,  C. Bélec,  R.W. Mullen,  N.R. Kitchen,  W.E. Thomason,  S. Ebelhar,  D.B. Mengel,  W.R. Raun,  D.D. Francis,  E.D. Vories, and I. Ortiz-Monasterio. 2012. Corn response to nitrogen is influenced by soil texture and weather. Agron. J. 104:1658–1671. doi:10.2134/agronj2012.018.

