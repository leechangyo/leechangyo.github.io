---
layout: post
title: 4. Detect Lane in Camera images
category: Python
tag: Python
---

# CODE TO SELECT WHITE PIXELS OUT OF THE IMAGE

```python
import matplotlib.image as mpimg
import matplotlib.pyplot as plt
import numpy as np
import cv2
```
```python
image_color = mpimg.imread('image_lane_c.jpg')
image_color.shape
# (540, 960, 3)
```

```python
plt.imshow(image_color)
```
<a href="https://postimg.cc/Tprt5GV5"><img src="https://i.postimg.cc/c4z2jL6h/1.png" width="700px" title="source: imgur.com" /><a>

```python
image_copy = np.copy(image_color)
image_copy.shape
#(540, 960, 3)
```

```python
image_copy[ (image_copy[:,:,0] < 200) | (image_copy[:,:,1] < 200) | (image_copy[:,:,2] < 200) ] = 0  # any value that is not white colour
```

```python
# Display the image                 
plt.imshow(image_copy, cmap = 'gray')
plt.show()
```
<a href="https://postimg.cc/w7TkMJjZ"><img src="https://i.postimg.cc/vH52PtFY/4213213.png" width="700px" title="source: imgur.com" /><a>

```python
plt.imshow(image_color)
```

# CODE TO PERFORM COLOR SELECTION

```python
import matplotlib.image as mpimg
import matplotlib.pyplot as plt
import numpy as np
import cv2
```
```python
image_color = mpimg.imread('image_lane_c.jpg')
image_color.shape
# (540, 960, 3)
```

```python
plt.imshow(image_color)
```
<a href="https://postimg.cc/Tprt5GV5"><img src="https://i.postimg.cc/c4z2jL6h/1.png" width="700px" title="source: imgur.com" /><a>

```python
image_gray = cv2.cvtColor(image_color, cv2.COLOR_BGR2GRAY)
plt.imshow(image_gray, cmap = 'gray')
image_gray.shape
# (540, 960)
```

<a href="https://postimg.cc/f3ZyW7w0"><img src="https://i.postimg.cc/tgnx5DwD/41232.png" width="700px" title="source: imgur.com" /><a>

```python
image_copy = np.copy(image_gray) #copy image_gray
image_copy.shape
# (540, 960)
```

```python
image_copy
```

```
array([[191, 191, 192, ..., 191, 191, 191],
       [191, 191, 191, ..., 191, 191, 191],
       [191, 191, 191, ..., 191, 191, 191],
       ...,
       [161, 163, 160, ..., 190, 191, 191],
       [170, 168, 160, ..., 185, 186, 187],
       [164, 156, 145, ..., 172, 173, 173]], dtype=uint8)
```

```python
image_copy[ (image_copy[:,:] < 250) ] = 0  # any value that is not white colour
# any of the pixel value less than 250, 0
# it store new array from [(image_copy[:,:])]
```

```python
# Display the image                 
plt.imshow(image_copy, cmap = 'gray')
plt.show()
```
<a href="https://postimg.cc/MX8SCZpv"><img src="https://i.postimg.cc/J7szBGwc/421321.png" width="700px" title="source: imgur.com" /><a>

# CODE TO PERFORM REGION OF INTEREST (ROI) SELECTION

```python
import cv2
import numpy as np
import matplotlib.image as mpimg
#from matplotlib import pyplot as plt #plotting fram work.
import matplotlib.pyplot as plt # it is same as from matplotlib import pyplot as plt
%matplotlib inline
# magic function in python, it help to see picture immediately in notebook jupiter

image_color = cv2.imread('image_lane_c.jpg')
#image_color = mpimg.imread('image_lane_c.jpg') # import image as RGB instead of BGR

cv2.imshow('Original Image', image_color) # opencv open with RGB as real picture
cv2.waitKey()
cv2.destroyAllWindows()
image_color.shape
# (540, 960, 3)
```

```python
height, width = image_color.shape[:2] # give to valeu height, width by list
# height 540
# width 960
plt.imshow(image_color) # plt. fucntion open with BGR color by matplotlib
# that's why the cv.imshow and plt.imshow picture difference
```
<a href="https://postimg.cc/yDRnCKd8"><img src="https://i.postimg.cc/vZqK9Hwf/3.png" width="700px" title="source: imgur.com" /><a>

```python
image_color.shape
# (540, 960, 3)
image_gray = cv2.cvtColor(image_color, cv2.COLOR_BGR2GRAY)
plt.imshow(image_gray, cmap = 'gray')
```
<a href="https://postimg.cc/RW1fTZhK"><img src="https://i.postimg.cc/L8wT2qMC/32.png" width="700px" title="source: imgur.com" /><a>

```python
image_gray.shape # when it change to gray, the number of color is eare
# (540, 960)
```


```python
# Select points of the region of interest (ROI)
ROI = np.array([[(0, height),(400, 330), (550, 330), (width, height)]], dtype=np.int32)    
# first point, second point, third point.
# (0,height) means we gonna stop when height is reached ( corner point)
# (width,height) means we gonna stop when width and height is reached ( corner point)

# define a blank image with all zeros (ie: black)
blank = np.zeros_like(image_gray)   
blank.shape
#(540, 960)
```

```python

# Fill the Region of interest with white color (ie: 255)!
mask = cv2.fillPoly(blank, ROI, 255)
#polyynomial specifying any of it our image which is our blank image our ROI
#255 because we want to fill that rigion of interest with once(white)
# Perform bitwise AND operation to select only the region of interest
masked_image = cv2.bitwise_and(image_gray, mask)
# take our image_gray and take mask.
# like it all together.
# bitwise means that put together
masked_image.shape
#(540, 960)
plt.imshow(mask,'gray')
```
<a href="https://postimg.cc/wtmBkW0P"><img src="https://i.postimg.cc/tgrVpwPX/5.png" width="700px" title="source: imgur.com" /><a>

```python

plt.imshow(masked_image, cmap = 'gray')
```

<a href="https://postimg.cc/4myH91RC"><img src="https://i.postimg.cc/SN76HZZK/12.png" width="700px" title="source: imgur.com" /><a>
