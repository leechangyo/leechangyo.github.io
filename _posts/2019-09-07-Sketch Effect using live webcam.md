---
layout: post
title: 7. Sketch Effect using live webcam
category: Python
tag: Python
---



```Python
import matplotlib.image as mpimg # to protect unlimit loading when we use cv2.
import matplotlib.pyplot as plt #ploting tool
import numpy as np
import cv2
```
```python
def Cartoon(image_color):

    output_image = cv2.stylization(image_color, sigma_s=100, sigma_r=0.3)
    #Sigma_s 사진 부드러운 정도
    #sigma_r 모서리 부분 날카라운 부분

    return output_image
```

```Python
def LiveCamEdgeDetection_canny(image_color):

    threshold_1 = 30
    threshold_2 = 80
    image_gray = cv2.cvtColor(image_color, cv2.COLOR_BGR2GRAY)
    canny = cv2.Canny(image_gray, threshold_1, threshold_2) #canny one of method to draawing gray and white.
    return canny
```


```Python
cap = cv2.VideoCapture(0) #  capture of our webcam

while True:
    ret, frame = cap.read() # Cap.read() returns a ret bool to indicate success.
    # cv2.imshow('Live Edge Detection', Cartoon(frame))
    # ret(return) get TRUE OR FALSE
    # if true , frame is working(actual image) from videocapture.

    cv2.imshow('Live Edge Detection', LiveCamEdgeDetection_canny(frame))
    cv2.imshow('Webcam Video', frame)
    if cv2.waitKey(1) == 13: #13 Enter Key
        break

cap.release() # camera release
cv2.destroyAllWindows()      
```
