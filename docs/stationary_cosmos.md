# Cosmic-ray Neutrons to Soil Moisture

The use of cosmic-ray neutrons for measuring rootzone soil moisture is a fairly new and non-invasive sensing technique capable of providing field average soil water content at high temporal resolution. The instrument fills a unique niche left by traditional point-level sensors and satellite missions.

In this exercise we will go over the process of converting counts of epithermal neutrons into soil moisture at hourly intervals from a CRS-1000/B (Hydroinnova, Albuquerque, NM) neutron detector. The instrument was mounted on a tripod in a small field at the Konza prairie. The tripod also contained a Campbell Scientific air temperature and relative humidity sensor (CS215) and the Hydroinnova device contained two barometers within the enclosure (there is a ventilation valve to ensure atmospheric pressure within the enclosure is the same as the atmospheric pressure.



```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

```


```python
# Load cosmic-ray data
df = pd.read_csv("../datasets/stationary_cosmos_hydroinnova.csv")
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
      <th>RecordNum</th>
      <th>Date Time(UTC)</th>
      <th>P4_mb</th>
      <th>P1_mb</th>
      <th>T1_C</th>
      <th>RH1</th>
      <th>T_CS215</th>
      <th>RH_CS215</th>
      <th>Vbat</th>
      <th>N1Cts</th>
      <th>N1ET_sec</th>
      <th>N1T_C</th>
      <th>N1RH</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2019/12/01 00:00:00</td>
      <td>961.45</td>
      <td>961.4</td>
      <td>7.2</td>
      <td>13.3</td>
      <td>4.7</td>
      <td>50.9</td>
      <td>12.802</td>
      <td>2364</td>
      <td>3601</td>
      <td>10.5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2019/12/01 01:00:00</td>
      <td>961.79</td>
      <td>961.6</td>
      <td>6.4</td>
      <td>12.6</td>
      <td>3.8</td>
      <td>51.4</td>
      <td>12.761</td>
      <td>2282</td>
      <td>3599</td>
      <td>9.3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019/12/01 02:00:00</td>
      <td>961.77</td>
      <td>961.6</td>
      <td>4.8</td>
      <td>12.1</td>
      <td>2.6</td>
      <td>57.3</td>
      <td>12.729</td>
      <td>2283</td>
      <td>3600</td>
      <td>7.7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2019/12/01 03:00:00</td>
      <td>961.97</td>
      <td>961.9</td>
      <td>4.2</td>
      <td>11.9</td>
      <td>2.3</td>
      <td>60.0</td>
      <td>12.710</td>
      <td>2238</td>
      <td>3600</td>
      <td>7.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2019/12/01 04:00:00</td>
      <td>962.73</td>
      <td>962.6</td>
      <td>3.9</td>
      <td>11.8</td>
      <td>1.8</td>
      <td>67.9</td>
      <td>12.692</td>
      <td>2333</td>
      <td>3600</td>
      <td>6.5</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Convert timestamp into Pandas datetime object
df["Date Time(UTC)"] = pd.to_datetime(df["Date Time(UTC)"], format="%Y/%m/%d %H:%M:%S")
df.insert(2,"Date Time(CST)",df["Date Time(UTC)"] - pd.DateOffset(hours=6))
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
      <th>RecordNum</th>
      <th>Date Time(UTC)</th>
      <th>Date Time(CST)</th>
      <th>P4_mb</th>
      <th>P1_mb</th>
      <th>T1_C</th>
      <th>RH1</th>
      <th>T_CS215</th>
      <th>RH_CS215</th>
      <th>Vbat</th>
      <th>N1Cts</th>
      <th>N1ET_sec</th>
      <th>N1T_C</th>
      <th>N1RH</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2019-12-01 00:00:00</td>
      <td>2019-11-30 18:00:00</td>
      <td>961.45</td>
      <td>961.4</td>
      <td>7.2</td>
      <td>13.3</td>
      <td>4.7</td>
      <td>50.9</td>
      <td>12.802</td>
      <td>2364</td>
      <td>3601</td>
      <td>10.5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2019-12-01 01:00:00</td>
      <td>2019-11-30 19:00:00</td>
      <td>961.79</td>
      <td>961.6</td>
      <td>6.4</td>
      <td>12.6</td>
      <td>3.8</td>
      <td>51.4</td>
      <td>12.761</td>
      <td>2282</td>
      <td>3599</td>
      <td>9.3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-12-01 02:00:00</td>
      <td>2019-11-30 20:00:00</td>
      <td>961.77</td>
      <td>961.6</td>
      <td>4.8</td>
      <td>12.1</td>
      <td>2.6</td>
      <td>57.3</td>
      <td>12.729</td>
      <td>2283</td>
      <td>3600</td>
      <td>7.7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2019-12-01 03:00:00</td>
      <td>2019-11-30 21:00:00</td>
      <td>961.97</td>
      <td>961.9</td>
      <td>4.2</td>
      <td>11.9</td>
      <td>2.3</td>
      <td>60.0</td>
      <td>12.710</td>
      <td>2238</td>
      <td>3600</td>
      <td>7.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2019-12-01 04:00:00</td>
      <td>2019-11-30 22:00:00</td>
      <td>962.73</td>
      <td>962.6</td>
      <td>3.9</td>
      <td>11.8</td>
      <td>1.8</td>
      <td>67.9</td>
      <td>12.692</td>
      <td>2333</td>
      <td>3600</td>
      <td>6.5</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Inspect data
plt.figure(figsize=(12,4))
plt.plot(df["Date Time(CST)"], df["N1Cts"]) 
plt.ylabel('Raw neutron counts')
plt.show()

```


![png](stationary_cosmos_files/stationary_cosmos_4_0.png)



```python
# For simplicity, store pandas columsn into shorted variable names
T = df["T_CS215"]
RH = df["RH_CS215"]
P = (df["P1_mb"] + df["P4_mb"])/2
N_raw = df["N1Cts"]

```

## Compute vapor pressure


```python
# Compute Saturation vapor pressure (Pws)
T = T + 273  # Celisus to Kelvin
Tc = 647.1 # Critical temperature,  K
Pc = 220640  # Critical pressure  mb
C1 = -7.86
C2 = 1.84
C3 = -11.77
C4 = 22.68
C5 = -15.96
C6 = 1.80
theta = 1 - T/Tc

# Saturation vapor pressure in mb
Pws = np.exp(Tc/T*(C1*theta + C2*theta**1.5 + C3*theta**3 + C4*theta**3.5 + C5*theta**4 + C6*theta**7.5))*Pc
Pws = Pws * 100 # mb or hPa -> Pa

```

## Correct raw nuetron counts

This correction mainly takes into account the effect of atmospheric pressure and water vapor


```python
# Atmospheric pressure correction (all in milibars)
beta = 0.0077 # atmospheric attenuation coefficient (mb^-1) (see Dong et al. 2014 p.4)
Pref = 970 # Average atmospheric pressure in mbars 
fp = np.exp(beta*(P-Pref))# # Pressure factor (fp) [Eq. 1] Hawdon et al. 2014

# Atmospheric water vapor correction
Pw = Pws * RH/100 # Vapor pressure in Pascals
A_ref = 0 # 0 g m^-3 (dry air)
C = 2.16679 # g K/J;
A = C * Pw/(T+273.15) # Absolute humidity [g/m^3]
fwv = 1 + 0.0054*(A-A_ref)

# Incoming neutron intensity correction
fi = 1 # fi = Im/Iref;

# Apply correction factors
df["N_corrected"] = np.round(N_raw * (fp*fwv/fi))

# Inspect new variable in dataframe
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
      <th>RecordNum</th>
      <th>Date Time(UTC)</th>
      <th>Date Time(CST)</th>
      <th>P4_mb</th>
      <th>P1_mb</th>
      <th>T1_C</th>
      <th>RH1</th>
      <th>T_CS215</th>
      <th>RH_CS215</th>
      <th>Vbat</th>
      <th>N1Cts</th>
      <th>N1ET_sec</th>
      <th>N1T_C</th>
      <th>N1RH</th>
      <th>N_corrected</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2019-12-01 00:00:00</td>
      <td>2019-11-30 18:00:00</td>
      <td>961.45</td>
      <td>961.4</td>
      <td>7.2</td>
      <td>13.3</td>
      <td>4.7</td>
      <td>50.9</td>
      <td>12.802</td>
      <td>2364</td>
      <td>3601</td>
      <td>10.5</td>
      <td>0</td>
      <td>2233.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2019-12-01 01:00:00</td>
      <td>2019-11-30 19:00:00</td>
      <td>961.79</td>
      <td>961.6</td>
      <td>6.4</td>
      <td>12.6</td>
      <td>3.8</td>
      <td>51.4</td>
      <td>12.761</td>
      <td>2282</td>
      <td>3599</td>
      <td>9.3</td>
      <td>0</td>
      <td>2159.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-12-01 02:00:00</td>
      <td>2019-11-30 20:00:00</td>
      <td>961.77</td>
      <td>961.6</td>
      <td>4.8</td>
      <td>12.1</td>
      <td>2.6</td>
      <td>57.3</td>
      <td>12.729</td>
      <td>2283</td>
      <td>3600</td>
      <td>7.7</td>
      <td>0</td>
      <td>2161.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2019-12-01 03:00:00</td>
      <td>2019-11-30 21:00:00</td>
      <td>961.97</td>
      <td>961.9</td>
      <td>4.2</td>
      <td>11.9</td>
      <td>2.3</td>
      <td>60.0</td>
      <td>12.710</td>
      <td>2238</td>
      <td>3600</td>
      <td>7.0</td>
      <td>0</td>
      <td>2123.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2019-12-01 04:00:00</td>
      <td>2019-11-30 22:00:00</td>
      <td>962.73</td>
      <td>962.6</td>
      <td>3.9</td>
      <td>11.8</td>
      <td>1.8</td>
      <td>67.9</td>
      <td>12.692</td>
      <td>2333</td>
      <td>3600</td>
      <td>6.5</td>
      <td>0</td>
      <td>2227.0</td>
    </tr>
  </tbody>
</table>
</div>



## Covert corrected neutron counts into soil water content



```python
# Define approximate bulk density for the field
bulk_density = 1.4 # g/cm^3

# Gravimetric water content
gwc = (0.0808 / (df["N_corrected"]/3500-0.372) - 0.115 - 0.03) # Equation by Desilets

# Volumetric water content
vwc =  gwc * bulk_density/0.998 


# Plot soil moisture
plt.figure(figsize=(12,4))
plt.plot(df["Date Time(CST)"], vwc) 
plt.ylabel('Volumetric Water Content (cm$^3$/cm$^3$)')
plt.show()

```


![png](stationary_cosmos_files/stationary_cosmos_11_0.png)


## References

Franz, T.E., Zreda, M., Ferre, T.P.A., Rosolem, R., Zweck, C., Stillman, S., Zeng, X. and Shuttleworth, W.J., 2012. Measurement depth of the cosmic ray soil moisture probe affected by hydrogen from various sources. Water Resources Research, 48(8).

Hawdon, A., McJannet, D. and Wallace, J., 2014. Calibration and correction procedures for cosmic‐ray neutron soil moisture probes located across Australia. Water Resources Research, 50(6), pp.5029-5043.

Zreda, M., Desilets, D., Ferré, T.P.A. and Scott, R.L., 2008. Measuring soil moisture content non‐invasively at intermediate spatial scale using cosmic‐ray neutrons. Geophysical research letters, 35(21).
