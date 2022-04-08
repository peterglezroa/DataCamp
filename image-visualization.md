# Image Visualization

## Functions
### Load image
```python
madrid_image = plt.imread("./madrid.jpeg")
```

### Show image with matplot
```python
def show_image(image, title="Image", cmap_type="gray"):
  plt.imshow(image, cmap=cmap_type)
  plt.title(title)
  plt.axis("off")
  plt.show()
```

### Comparing plots
```python
def plot_comparison(original, filtered, title_filtered="filtered"):
  fig, (ax1, ax2) = plt.subplots(ncols = 2, figsize=(8,6), shared=True,
    sharey=True)
  ax1.imshow(original, cmap=plt.cm.gray)
  ax1.set_title("original")
  ax1.axis("off")
  ax2.imshow(filtered, cmap=plt.cm.gray)
  ax2.set_title(title_filtered)
  ax2.axis("off")
```
## Convert rgb to gray
```python
from skimage import color

grayscale = color.rgb2gray(original)
rgb = color.gray2rgb(grayscale)
```

## Thresholding
Partitioning an image into a foreground and background.
* If the pixel is less than a val => turn it black
* If the pixel is more => turn it white

First convert it to grayscale and then apply:
```python
thresh=127
binary = image > thresh
```

### Categories
1. Global or histogram based: good for uniform backgrounds
```python
from skimage.filters import threshold_otsu

thresh = threshold_otsu(image) # Optimal threshold
binary = image > thresh
```
2. Local or adaptive: uneven background illumination
```python
from skimage.filters import threshold_local
block_size = 35 # Neighbors
local_thresh = threshold_local(text_image, block_size, offset=10)
binary = image >local_thresh 
```

### Trying all thresholds
```python
from skimage.filters import try_all_threshold

fix, ax = try_all_threshold(image, verbose=False)
show_plot(fig, ax)
```

## Contrast
Difference between de min value and the max value of a pixel.

### Histogram image
An histogram can help us see how much contrast an image has.
```python
red = image[:, :, 0]
green = image[:, :, 1]
blue = image[:, :, 2]

# Ravel makes an continous array of values.
# So it adds a cero to the values that are missing
plt.hist(blue.ravel(), bins=256)
plt.title("Blue Histogram")
plt.show()
```
![contrast]

### Enhance contrast
* Contrast streching
* Histogram equilization
    * Spreads out the most frequent intensity values.

```python
from skimage import exposure
image_eq = exposure.equalize_hist(image)
```
* Adaptive histogram equalization
    * Computes different histograms to different neighborhoods of the image and
    uses them to distribute the values.
```python
from skimage import exposure
# Clip_limit (0-1: more contrast)
image_eq = exposure.equalize_adapthist(image, clip_limit=0.03)
```
* Contrast Limited Adaptive Histogram Equalization (CLAHE)
![contrast enhancement]

## Filters
```python
# Edge detection
from skimage.filters import sobel
edge_sobel = sobel(image_coins)

# Gaussian Blur
from skimage.filters import gaussian
gaussian_image = gaussian(image, multichannel=True)
```

## Transformations
* Used for preparing images for classification Machine Learning models
* Optimization and image compression
* Standarize image proportions

### Rotating
```python
from skimage.transform import rotate

# Rotate the image 90 degrees clockwise
image_rotated = rotate(image, -90)
```

### Rescaling
```python
from skimage.transform import rescale

# Rescale the image to be 4 times smaller
image_rescaled = rescale(image, 1/4, anti_aliasing=True, multichannel=True)
```

### Resizing
```python
from skimage.transform import resize

# Specific resize
height = 400
width = 500

# Resize proportionally
height = image.shape[0]/4
width = image.shape[1]/4

image_resized = resize(image, (height, width), anti_aliasing=True)
```

## Morphology

### Structuring element
Element used for the morphology operations
![structuring element]
![structuring element shapes]


### Dilation
Adds pixels to boundaries of objects in an image.
```python
from skimage import morphology

dilated_image = morphology.binary_dilation(image_horse)
```
![horse dilation]

### Erosion
Removes pixels on image boundaries.
```python
from skimage import morphology

selem = rectangle(12, 6)
eroded_image = morphology.binary_erosion(image_horse, selem=selem)
```
![horse erosion]

## Image resconstruction
### Inpainting
Reconstructing lost parts of images by looking at the non-damaged regions.
These lost regions have to be defined in a mask element.

![mask]

```python
from skimage.restoration import inpaint

mask = get_mask(defect_image)
restored_image = inpaint.inpaint_biharmonic(defect_image, mask,
  multichannel=True)
```

But also it can be used to remove an object if the mask is set on top of saide
element.

```python
# Initialize the mask
mask = np.zeros(image_with_logo.shape[:-1])

# Set the pixels where the logo is to 1
mask[210:290, 360:425] = 1

# Apply inpainting to remove the logo
image_logo_removed = inpaint.inpaint_biharmonic(image_with_logo,
                                  mask,
                                  multichannel=True)

# Show the original and logo removed images
show_image(image_with_logo, 'Image with logo')
show_image(image_logo_removed, 'Image with logo removed')
```

[contrast]: ./imgs/image-visualization-contrast.png
[contrast enhancement]: ./imgs/image-visualization-contrast_enhancement.png
[structuring element]: ./imgs/image-visualization-structuring_element.png
[structuring element shapes]: ./imgs/image-visualization-structuring_element_shape.png
[horse dilation]: ./imgs/image-visualization-horse_dilation.png
[horse erosion]: ./imgs/image-visualization-horse_erosion.png
[mask]: ./imgs/image-visualization-mask.png
