
# Soil texture

A simple code to determine the soil textural class based on the percent of sand and clay according to the US Department of Agriculture.

## References

- E. Benham and R.J. Ahrens, W.D. 2009. Clarification of Soil Texture Class Boundaries. Nettleton National Soil Survey Center, USDA-NRCS, Lincoln, Nebraska.


```python
# Inputs
sand = 10
clay = 90
soiltexturalclass(sand,clay)
```


```python
def soiltexturalclass(sand,clay):
    """Function that returns the USDA soil textural class given the percent sand and clay."""
    
    silt = 100 - sand - clay
    
    if sand + clay > 100 or sand < 0 or clay < 0:
        raise Exception('Inputs adds over 100% or are negative')

    elif silt + 1.5*clay < 15:
        textural_class = 'sand'

    elif silt + 1.5*clay >= 15 and silt + 2*clay < 30:
        textural_class = 'loamy sand'

    elif (clay >= 7 and clay < 20 and sand > 52 and silt + 2*clay >= 30) or (clay < 7 and silt < 50 and silt + 2*clay >= 30):
        textural_class = 'sandy loam'

    elif clay >= 7 and clay < 27 and silt >= 28 and silt < 50 and sand <= 52:
        textural_class = 'loam'

    elif (silt >= 50 and clay >= 12 and clay < 27) or (silt >= 50 and silt < 80 and clay < 12):
        textural_class = 'silt loam'

    elif silt >= 80 and clay < 12:
        textural_class = 'silt'

    elif clay >= 20 and clay < 35 and silt < 28 and sand > 45:
        textural_class = 'sandy clay loam'

    elif clay >= 27 and clay < 40 and sand > 20 and sand <= 45:
        textural_class = 'clay loam'

    elif clay >= 27 and clay < 40 and sand <= 20:
        textural_class = 'silty clay loam'

    elif clay >= 35 and sand > 45:
        textural_class = 'sandy clay'

    elif clay >= 40 and silt >= 40:
        textural_class = 'silty clay'

    elif clay >= 40 and sand <= 45 and silt < 40:
        textural_class = 'clay'

    else:
        textural_class = 'na'

    return textural_class

```


```python
print(textural_class)
```

    sand



```python
# Sand
class (1,:) = silt + 1.5*clay < 15

# Loamy sand.
class (2,:) = silt + 1.5*clay >= 15 & silt + 2*clay < 30

# Sandy loam. 
class (3,:) = (clay >= 7 & clay < 20 & sand > 52 & silt + 2*clay >= 30) |
              (clay < 7 & silt < 50 & silt + 2*clay >= 30)           

# Loam
class (4,:) = clay >= 7 & clay < 27 & silt >= 28 & silt < 50 & sand <= 52

# Silt loam.
class (5,:) = (silt >= 50 & clay >= 12 & clay < 27) |
              (silt >= 50 & silt < 80 & clay < 12)

# Silt
class (6,:) = silt >= 80 & clay < 12

# Sandy clay loam
class (7,:) = clay >= 20 & clay < 35 & silt < 28 & sand > 45

# Clay loam
class (8,:) = clay >= 27 & clay < 40 & sand > 20 & sand <= 45

# Silty clay loam
class (9,:) = clay >= 27 & clay < 40 & sand <= 20

# Sandy clay
class (10,:) = clay >= 35 & sand > 45

# Silty clay
class (11,:) = clay >= 40 & silt >= 40

# Clay
class (12,:) = clay >= 40 & sand <= 45 & silt < 40
```
