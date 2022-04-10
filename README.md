# Detection-of-Shape-and-Color-using-opencv


import numpy as np
import cv2
img = cv2.imread('opencvsample.jpeg')
imgGrey = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
_, thrash = cv2.threshold(imgGrey, 240, 255,cv2.THRESH_BINARY)
contours, _ = cv2.findContours(thrash, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
lst=[]
i=0
def shape_rec(no_points,clr):
  x = no_points.ravel()[0]
  y = no_points.ravel()[1] - 5
  if len(no_points) == 3:
    l=["Triangle",(x,y)]
    lst.append(l)
  elif len(no_points) == 4:
    x1 ,y1, w, h = cv2.boundingRect(no_points)
    aspectRatio = float(w)/h
    if aspectRatio >= 0.95 and aspectRatio <= 1.05:
      l=["Square",(x,y)]
      lst.append(l)
    else:
      l=["Rectangle",(x,y)]
      lst.append(l)  
  elif len(no_points) == 5:
      l=["Pentagon",(x,y)]
      lst.append(l)
  else:
      l=["Circle",(x,y)]
      lst.append(l)
  l.insert(0,clr)   

hsvFrame = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

### Set range for red color and
### define mask

red_lower = np.array([22, 98, 20], np.uint8)
red_upper = np.array([23, 255, 255], np.uint8)
red_mask = cv2.inRange(hsvFrame, red_lower, red_upper)

### Set range for green color and
### define mask

green_lower = np.array([50, 100, 100], np.uint8)
green_upper = np.array([70, 255, 255], np.uint8)
green_mask = cv2.inRange(hsvFrame, green_lower, green_upper)

### Set range for blue color and
### define mask

blue_lower = np.array([94, 80, 2], np.uint8)
blue_upper = np.array([120, 255, 255], np.uint8)
blue_mask = cv2.inRange(hsvFrame, blue_lower, blue_upper)

### Set range for yellow color and
### define mask

yellow_lower = np.array([19, 245, 214], np.uint8)
yellow_upper = np.array([39, 255, 255], np.uint8)
yellow_mask = cv2.inRange(hsvFrame, yellow_lower, yellow_upper)

### Set range for orange color and
### define mask

orange_lower = np.array([6, 245, 215], np.uint8)
orange_upper = np.array([26, 255, 255], np.uint8)
orange_mask = cv2.inRange(hsvFrame, orange_lower, orange_upper)

kernal = np.ones((5, 5), "uint8")

### For red color

red_mask = cv2.dilate(red_mask, kernal)
res_red = cv2.bitwise_and(img, img, mask = red_mask)

### For green color

green_mask = cv2.dilate(green_mask, kernal)
res_green = cv2.bitwise_and(img, img, mask = green_mask)

### For blue color

blue_mask = cv2.dilate(blue_mask, kernal)
res_blue = cv2.bitwise_and(img, img, mask = blue_mask)

### For yellow color

yellow_mask = cv2.dilate(yellow_mask, kernal)
yellow_red = cv2.bitwise_and(img, img, mask = yellow_mask)

### For orange color

orange_mask = cv2.dilate(orange_mask, kernal)
res_orange = cv2.bitwise_and(img, img, mask = orange_mask)
      
### Creating contour to track blue color

contours, hierarchy = cv2.findContours(blue_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    no_points = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    shape_rec(no_points,"Blue")

### Creating contour to track red color

contours, hierarchy = cv2.findContours(red_mask,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE) 
for pic, contour in enumerate(contours):
    no_points = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    shape_rec(no_points,"Red")

### Creating contour to track green color

contours, hierarchy = cv2.findContours(green_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    no_points = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    shape_rec(no_points,"Green")

### Creating contour to track yellow color

contours, hierarchy = cv2.findContours(yellow_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    no_points = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    shape_rec(no_points,"Yellow")

### Creating contour to track Orange color

contours, hierarchy = cv2.findContours(orange_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    no_points = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    shape_rec(no_points,"Orange")   
     
print(lst)
cv2.waitKey(0)
cv2.destroyAllWindows()
