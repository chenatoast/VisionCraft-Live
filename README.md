<div align="center">

# 🎨 OpenCV Image Processing Techniques

A collection of real-time image processing techniques using OpenCV and Python

</div>

## 📋 Table of Contents

- [Introduction](#introduction)
- [Setup Requirements](#setup-requirements)
- [Brightness Manipulation](#brightness-manipulation)
  - [Increasing Brightness](#increasing-brightness)
  - [Reducing Brightness](#reducing-brightness)
- [Background Effects](#background-effects)
  - [Blue Background Merging](#blue-background-merging)
  - [Warm Tone Filter](#warm-tone-filter)
- [Image Composition](#image-composition)
  - [Merging Static Images](#merging-static-images)
  - [Logo Overlay](#logo-overlay)
- [Image Masking](#image-masking)
- [Usage Notes](#usage-notes)

## 📝 Introduction

<div style="text-align: justify">
This repository contains a collection of real-time image processing techniques implemented with OpenCV in Python. The code provides various filters and effects that can be applied to webcam feeds or static images, demonstrating fundamental concepts in computer vision and image manipulation.
</div>

## 🛠️ Setup Requirements

<div style="text-align: justify">
To run the code in this repository, you'll need the following libraries installed:
</div>

```python
import cv2 as cv
import numpy as np
import matplotlib.pyplot as plt
```

<div style="text-align: justify">
You'll also need a working webcam for the real-time effects. The code uses OpenCV's VideoCapture to access the webcam (usually index 0).
</div>

## ☀️ Brightness Manipulation

### Increasing Brightness

<div style="text-align: justify">
This technique adds pixel values to increase brightness. Three different intensity levels are demonstrated:
</div>

```python
# Increasing Brightness
pixels = float(10)

cam = cv.VideoCapture(0)

while True:
    _, img = cam.read()
    img = cv.flip(img, 1)
    
    img_1 = img + pixels          # Low brightness increase
    img_1[img_1 <  0 ] = 0
    img_1[img_1 > 255] = 255
    img_1 = img_1.astype(np.uint8)
    
    img_2 = img + (2*pixels)      # Medium brightness increase
    img_2[img_2 <  0 ] = 0
    img_2[img_2 > 255] = 255
    img_2 = img_2.astype(np.uint8)
    
    img_3 = img + (3*pixels)      # High brightness increase
    img_3[img_3 <  0 ] = 0
    img_3[img_3 > 255] = 255
    img_3 = img_3.astype(np.uint8)
    
    # Display all versions
    cv.imshow("Original", img)
    cv.imshow("Filter-1", img_1)
    cv.imshow("Filter-2", img_2)
    cv.imshow("Filter-3", img_3)
    
    if cv.waitKey(1) == 27:       # ESC key to exit
        cam.release()
        break
```

<div style="text-align: justify">
The code ensures that pixel values remain within the valid range of 0-255 after modification.
</div>

### Reducing Brightness

<div style="text-align: justify">
Similar to increasing brightness, this technique subtracts pixel values to create darker images:
</div>

```python
# Reducing Brightness
pixels = float(10)

cam = cv.VideoCapture(0)

while True:
    _, img = cam.read()
    img = cv.flip(img, 1)
    
    img_1 = img - pixels          # Low brightness reduction
    img_1[img_1 <  0 ] = 0
    img_1[img_1 > 255] = 255
    img_1 = img_1.astype(np.uint8)
    
    img_2 = img - (2*pixels)      # Medium brightness reduction
    img_2[img_2 <  0 ] = 0
    img_2[img_2 > 255] = 255
    img_2 = img_2.astype(np.uint8)
    
    img_3 = img - (3*pixels)      # High brightness reduction
    img_3[img_3 <  0 ] = 0
    img_3[img_3 > 255] = 255
    img_3 = img_3.astype(np.uint8)
    
    # Display all versions
    cv.imshow("Original", img)
    cv.imshow("Filter-1", img_1)
    cv.imshow("Filter-2", img_2)
    cv.imshow("Filter-3", img_3)
    
    if cv.waitKey(1) == 27:       # ESC key to exit
        cam.release()
        break
```

## 🎭 Background Effects

### Blue Background Merging

<div style="text-align: justify">
This technique creates a blue background and blends it with the webcam feed:
</div>

```python
# Creating Blue Background
blue = [247, 206, 139]  # BGR format in OpenCV
background = []

for i in range(720):
    temp = []
    for j in range(1280):
        temp.append(blue)
    background.append(temp)
    
background = np.array(background).astype(np.uint8)

# Merging with webcam feed
cam = cv.VideoCapture(0)

while True:
    _, img = cam.read()
    img = cv.flip(img, 1)
    
    img = np.array(img).astype(np.uint8)
    background = cv.resize(background, (img.shape[1], img.shape[0]))
    merged = cv.addWeighted(img, .85, background, .15, 0)
    
    cv.imshow("Original", img)
    cv.imshow("Merged", merged)
    
    if cv.waitKey(1) == 27:
        cam.release()
        break
```

### Warm Tone Filter

<div style="text-align: justify">
This effect adds a warm yellow tone to the webcam feed:
</div>

```python
# Creating Feed with Warmer Tone
yellow = [108, 222, 249]  # BGR format in OpenCV

background = []
for i in range(720):
    temp = []
    for j in range(1280):
        temp.append(yellow)
    background.append(temp)
    
background = np.array(background).astype(np.uint8)

cam = cv.VideoCapture(0)

while True:
    _, img = cam.read()
    img = cv.flip(img, 1)
    
    img = np.array(img).astype(np.uint8)
    
    background = cv.resize(background, (img.shape[1], img.shape[0]))
    merged = cv.addWeighted(img, .90, background, .10, 0)
    
    cv.imshow("Original", img)
    cv.imshow("Merged", merged)
    
    if cv.waitKey(1) == 27:
        cam.release()
        break
```

## 🖼️ Image Composition

### Merging Static Images

<div style="text-align: justify">
This function blends two static images with customizable weights:
</div>

```python
# Merge two static images
def merge(foreground_path, background_path, a, b):
    img = cv.imread(foreground_path)
    background = cv.imread(background_path)
    
    background = cv.resize(background, (img.shape[1], img.shape[0]))
    final = cv.addWeighted(img, a, background, b, 0)

    cv.imshow('Original', img)
    cv.waitKey(0)

    cv.imshow('Processed', final)
    cv.waitKey(0)

# Example usage
merge('img_1.jpg', 'img_2.jpg', .5, .5)
```

### Logo Overlay

<div style="text-align: justify">
This technique overlays a logo or watermark onto the webcam feed at a specific position:
</div>

```python
# Add logo overlay to webcam feed
cam = cv.VideoCapture(0)

logo = cv.imread('download.png')
logo = cv.resize(logo, (50, 50))

while True:
    _, img = cam.read()
    img = cv.flip(img, 1)
    
    # Position the logo at specific coordinates
    img[430:481, 590:641] = logo

    cv.imshow('Frame', img)
     
    if cv.waitKey(1) == 27:
        cam.release()
        break
```

## 🎭 Image Masking

<div style="text-align: justify">
This technique creates a binary mask based on pixel intensity thresholds:
</div>

```python
# Masking an image
camera = cv.VideoCapture(0)

lower = np.array([0, 0, 0])
upper = np.array([150, 150, 150])

while True:
    _, img = camera.read()
    img = cv.flip(img, 1)    
    
    mask = cv.blur(img, (4, 4))
    mask = cv.inRange(mask, lower, upper)
    
    cv.imshow("Frame", img)
    cv.imshow("Mask", mask)
    
    if (cv.waitKey(1) == 27):
        camera.release()
        break
        
camera.release()
```

<div style="text-align: justify">
The mask highlights pixels in the specified range, which can be useful for object detection, background removal, or creating special effects.
</div>

## 📌 Usage Notes

<div style="text-align: justify">

1. Press the `ESC` key (key code 27) to exit any of the real-time processing windows.

2. When working with static images, any key press will close the current window and proceed to the next step.

3. The webcam is accessed using `cv.VideoCapture(0)`, where 0 typically refers to the default camera. If you have multiple cameras, you may need to change this index.

4. Image flipping with `cv.flip(img, 1)` creates a mirror effect, making it more intuitive when viewing yourself in the webcam feed.

5. The BGR color format is used in OpenCV (not RGB), so color definitions may look reversed compared to other imaging libraries.

6. For best performance, ensure your computer has adequate processing power, as running multiple video processing windows simultaneously can be resource-intensive.

</div>

---

<div align="center">
Created with ❤️ for computer vision enthusiasts
</div>
