# Counting seeds

Example on how to use image analysis to identify and count seeds. In this tutorial we use the scikit-image library.


```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

from skimage.filters import threshold_otsu
from skimage.morphology import area_opening, disk, binary_closing
from skimage.measure import find_contours, label, regionprops
from skimage.color import rgb2gray, label2rgb

```


```python
# Read color image
image_rgb = mpimg.imread('../datasets/seeds.jpg')

```


```python
# Convert image to grayscale
image_gray = rgb2gray(image_rgb)

```


```python
# Visualize rgb and grayscale images
plt.figure(figsize=(10,6))

plt.subplot(1,2,1)
plt.imshow(image_rgb)
plt.axis('off')
plt.title('RGB')
plt.tight_layout()

plt.subplot(1,2,2)
plt.imshow(image_gray, cmap='gray')
plt.axis('off')
plt.title('Gray scale')
plt.tight_layout()

plt.show()

```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_4_0.png)



```python
# Segment seeds using a global automated threshold
global_thresh = threshold_otsu(image_gray)
image_binary = image_gray > global_thresh

```


```python
# Display classified seeds and grayscale threshold
plt.figure(figsize=(12,4))

plt.subplot(1,3,1)
plt.imshow(image_gray, cmap='gray')
plt.axis('off')
plt.title('Original grascale')
plt.tight_layout()

plt.subplot(1,3,2)
plt.hist(image_gray.ravel(), bins=256)
plt.axvline(global_thresh, color='r', linestyle='--')
plt.title('Otsu threshold')
plt.xlabel('Grasycale')
plt.ylabel('Counts')

plt.subplot(1,3,3)
plt.imshow(image_binary, cmap='gray')
plt.axis('off')
plt.title('Binary')
plt.tight_layout()

plt.show()

```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_6_0.png)



```python
# Invert image
image_binary = ~image_binary

```


```python
# Remove small areas (remove noise)
image_binary = area_opening(image_binary, area_threshold=1000, connectivity=2)

```


```python
# Closing (performs a dilation followed by an erosion. Connect small bright patches)
image_binary = binary_closing(image_binary, disk(5))

```


```python
# Display inverted and denoised binary image
plt.figure(figsize=(6,6))

plt.imshow(image_binary, cmap='gray')
plt.axis('off')
plt.title('Binary')
plt.tight_layout()

plt.show()

```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_10_0.png)



```python
# Identify seed boundaries
contours = find_contours(image_binary, 0)

# Print number of seeds in image
print('Image contains',len(contours),'seeds')

```

    Image contains 36 seeds



```python
# Plot seed contours
plt.figure(figsize=(6,6))
plt.imshow(image_binary, cmap='gray')
plt.axis('off')
plt.tight_layout()

for contour in contours:
    plt.plot(contour[:, 1], contour[:, 0], '-r', linewidth=1.5)
    
```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_12_0.png)



```python
# Label image regions
label_image = label(image_binary)
image_label_overlay = label2rgb(label_image, image=image_rgb)

```


```python
# Display image regions on top of original image
plt.figure(figsize=(6, 6))
plt.imshow(image_label_overlay)
plt.tight_layout()
plt.axis('off')
plt.show()

```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_14_0.png)



```python
# Display contour for a single seed
plt.figure(figsize=(12, 8))

for seed in range(36):
    plt.subplot(6,6,seed+1)
    plt.plot(contours[seed][:, 1], contours[seed][:, 0], '-r', linewidth=2)
    plt.tight_layout()
plt.show()

```


![png](image_analysis_count_seeds_files/image_analysis_count_seeds_15_0.png)

