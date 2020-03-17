## Climate Classification

A simple way of characterizing the different regions of our planet is by computing the ratio between the annual precipitation and the atmospheric demand. The smaller the ratio, the drier the place. The Aridity Index (AI) is used by the United Nations Environmental Program (UNEP) to classify environments based on the annual climate. The Aridity Index has gone through many versions and is currently defined by the Food and Agriculture Organization (FAO) as:

$$ AI = \frac{P}{PET}  $$

where $P$ is the annual precipitation and $PET$ is the annual cummulative potential evapotranspiration.


Table of climate classification according to the aridity index defined by the FAO
```
| Climate class |       Value      |
|---------------|------------------|
| Desert        |        AI ≤ 0.03 |
| Hyper-arid    | 0.03 < AI ≤ 0.05 |
| Arid          | 0.05 < AI ≤ 0.20 |
| Semi-arid     | 0.20 < AI ≤ 0.50 |
| Dry           | 0.50 < AI ≤ 0.65 |
| Sub-humid     | 0.65 < AI ≤ 0.75 |
| Humid         |        AI > 0.75 |
```



```python
# Define annual precipitation and atmospheric demand for a location
P = 1000   # mm per year
PET = 1800 # mm per year

```


```python
#Compute Aridity Index
AI = P/PET

# Find climate class
if AI <= 0.03:
    climate_class = 'Desert'
    
elif AI > 0.03 and AI <= 0.05:
    climate_class = 'Hyper-arid'
    
elif AI > 0.05 and AI <= 0.2:
    climate_class = 'Arid'

elif AI > 0.2 and AI <= 0.5:
    climate_class = 'Semi-arid'

elif AI > 0.5 and AI <= 0.65:
    climate_class = 'Dry'
    
elif AI > 0.65 and AI <= 0.75:
    climate_class = 'Sub-humid'
    
else:
    climate_class = 'Humid'
    
print('Climate classification is:',climate_class,'(AI='+str(round(AI,2))+')')

```

    Climate classification is: Dry (AI=0.56)


## References

Spinoni, J., Vogt, J., Naumann, G., Carrao, H. and Barbosa, P., 2015. Towards identifying areas at climatological risk of desertification using the Köppen–Geiger classification and FAO aridity index. International Journal of Climatology, 35(9), pp.2210-2222.
