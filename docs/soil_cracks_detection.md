# Soil Cracks

Soil cracks emerge as a consequence of the conformational changes in the structure of clay minerals with changes in soil moisture that causes the soil to shrink (while drying) and swell (while wetting). Expansive soils are usually characterized by moderate to high clay content, particularly in soils with high content of montmorillonite, a typical clay in vertisols. Soil cracks can lead to preferential water flow and rapid solute transport to deeper soil layers, and the expansion and contraction cof the soil can have adverse effects on building foundations.

>Montmorillonite can increase fifteen times its dry volume and can exhert an expansive pressure of 150,000 kg per square meter!

From the aesthetic point of view, the seemingly random cracking patterns also result attractive to scientists and enthusiasts interested in fractal patterns.

In this exercise we will identify soil cracks from a digital image using filters to detect tubular structures.



```python
# Import modules
import numpy as np
import matplotlib.pyplot as plt
from skimage import io, color, morphology, filters

```


```python
# Read image
RGB = io.imread("../datasets/soil_cracks.jpg")

```


```python
# Display image size
RGB.shape

```




    (3264, 2448, 3)




```python
# Convert RGB to gray scale
BW = color.rgb2gray(RGB)

```


```python
# Compare true color with gray scale image
plt.figure(figsize=(12,8))
plt.subplot(1,2,1)
plt.imshow(RGB)
plt.axis('off')

plt.subplot(1,2,2)
plt.imshow(BW, cmap="gray")
plt.axis('off')

plt.show()

```


![png](soil_cracks_detection_files/soil_cracks_detection_5_0.png)



```python
# Sato tubeness filter
BW_sato = filters.sato(BW, sigmas=range(1, 10, 2), black_ridges=True)

# Frangi vesselness filter
BW_frangi = filters.frangi(BW, sigmas=range(1, 10, 2), black_ridges=True)

```


```python
# Compare the Sato and Frangi filters
plt.figure(figsize=(12,8))

plt.subplot(1,2,1)
plt.title("Sato filter")
plt.imshow(BW_sato, cmap="gray")
plt.axis('off')

plt.subplot(1,2,2)
plt.title("Frangi filter")
plt.imshow(BW_frangi, cmap="gray")
plt.axis('off')

plt.show()

```


![png](soil_cracks_detection_files/soil_cracks_detection_7_0.png)


The Sato filter seems to capture the soil cracks more vividly, although the results strongly depend on the choice of filter paramters. Feel free to go back and re-run the filters with slightly different sigma values. You can also check the official documentation to test additional input arguments to fine tune the filter.

Next we will explore the histograms of filter values. Histograms are useful to detect breaks that can be used to threshold the image. While in this case we could finish at this point, the idea is to remove some of the noise in the classified image andcreate a binary image that encodes each pixel as background or crack.



```python
# Plot histograms
plt.figure(figsize=(12,4))

plt.subplot(1,2,1)
plt.title('Sato filter')
plt.hist(BW_sato.flatten())

plt.subplot(1,2,2)
plt.title('Frangi filter')
plt.hist(BW_frangi.flatten())

plt.show()

```


![png](soil_cracks_detection_files/soil_cracks_detection_9_0.png)


## Denoising image

The top-hat transformation extracts small details from the image based on a the size and shape of a structuring element. So our first step consists of defining a structuring element of a specific shape.



```python
# Generate structuring element (selem)
selem = morphology.disk(4) # Generates an image without noise
print(selem)

```

    [[0 0 0 0 1 0 0 0 0]
     [0 0 1 1 1 1 1 0 0]
     [0 1 1 1 1 1 1 1 0]
     [0 1 1 1 1 1 1 1 0]
     [1 1 1 1 1 1 1 1 1]
     [0 1 1 1 1 1 1 1 0]
     [0 1 1 1 1 1 1 1 0]
     [0 0 1 1 1 1 1 0 0]
     [0 0 0 0 1 0 0 0 0]]



```python
# De-noise image using the white top-hat transformation
noise = morphology.white_tophat(BW_sato, selem) # Gets the noisy part of the image

```


```python
# Subtract noise from classified image
BW_sato_clean = BW_sato - noise

```


```python
# Compare original filter, noise, and cleaned filter
plt.figure(figsize=(12,6))

plt.subplot(1,3,1)
plt.title('Sato')
plt.imshow(BW_sato, cmap="gray")
plt.axis('off')

plt.subplot(1,3,2)
plt.title('Noise')
plt.imshow(noise, cmap="gray")
plt.axis('off')

plt.subplot(1,3,3)
plt.title('Cleaned Sato')
plt.imshow(BW_sato_clean, cmap="gray")
plt.axis('off')
plt.show()

```


![png](soil_cracks_detection_files/soil_cracks_detection_14_0.png)


## Binarize
The process of binaring a gray scale image is to assign a value of either 0 or 1 to each pixel based on its current value. If the gray scale ranges between 0 and 1, the easiest way to binarize the image is to round the numbers. However, this may not produce the desired results in many cases. Using custom thresholds may improve the classification and extraction of the desired features.



```python
# Binarize
idx =  BW_sato_clean > 0.075 
BW_sato_clean[idx] = 1
BW_sato_clean[~idx] = 0

```


```python
# COmpute percentage of the area occupied by the soil cracks
percentage_cracks = np.sum(BW_sato_clean.flatten()) / BW_sato_clean.size * 100
print('Cracks occupy:',round(percentage_cracks,1), '% of the area.')

```

    Cracks occupy: 19.4 % of the area.



```python
# Show final classification
plt.figure(figsize=(12,8))

plt.subplot(1,2,1)
plt.imshow(RGB)
plt.axis('off')

plt.subplot(1,2,2)
plt.imshow(BW_sato_clean, cmap="gray")
plt.axis('off')
plt.show()

```


![png](soil_cracks_detection_files/soil_cracks_detection_18_0.png)

