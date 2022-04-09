# Detection-of-Shape-and-Color-using-opencv
`import numpy as np
import cv2

img = cv2.imread('opencvsample.jpeg')
imgGrey = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
_, thrash = cv2.threshold(imgGrey, 240, 255, cv2.THRESH_BINARY)
contours, _ = cv2.findContours(thrash, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
lst=[]
i=0

hsvFrame = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

### Set range for red color and
### define mask
red_lower = np.array([22, 98, 20], np.uint8)
red_upper = np.array([23, 255, 255], np.uint8)
red_mask = cv2.inRange(hsvFrame, red_lower, red_upper)

###Set range for green color and
###define mask
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
    approx = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    x = approx.ravel()[0]
    y = approx.ravel()[1] - 5
    if len(approx) == 3:
        lb=["Triangle",(x,y)]
        lst.append(lb)
    elif len(approx) == 4:
        x1 ,y1, w, h = cv2.boundingRect(approx)
        aspectRatio = float(w)/h
        if aspectRatio >= 0.95 and aspectRatio <= 1.05:
          lb=["Square",(x,y)]
          lst.append(lb)

        else:
          lb=["Rectangle",(x,y)]
          lst.append(lb)  
    elif len(approx) == 5:
        lb=["Pentagon",(x,y)]
        lst.append(lb)
    else:
        lb=["Circle",(x,y)]
        lst.append(lb)    
    lb.insert(0,"Blue")  


### Creating contour to track red color
contours, hierarchy = cv2.findContours(red_mask,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE) 
for pic, contour in enumerate(contours):
  #area = cv2.contourArea(contour)
  #if(area > 600):

    approx = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    x = approx.ravel()[0]
    y = approx.ravel()[1] - 5
    if len(approx) == 3:
        lr=["Triangle",(x,y)]
        lst.append(lr)

    elif len(approx) == 4:
        x1 ,y1, w, h = cv2.boundingRect(approx)
        aspectRatio = float(w)/h
        if aspectRatio >= 0.95 and aspectRatio <= 1.05:
          lr=["Square",(x,y)]
          lst.append(lr)


        else:
          lr=["Rectangle",(x,y)]
          lst.append(lr) 
 
    elif len(approx) == 5:
        lr=["Pentagon",(x,y)]
        lst.append(lr)

    else:
        lr=["Circle",(x,y)]
        lst.append(lr)
    lr.insert(0,"Red")

### Creating contour to track green color
contours, hierarchy = cv2.findContours(green_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    approx = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    x = approx.ravel()[0]
    y = approx.ravel()[1] - 5
    if len(approx) == 3:
        lg=["Triangle",(x,y)]
        lst.append(lb)
    elif len(approx) == 4:
        x1 ,y1, w, h = cv2.boundingRect(approx)
        aspectRatio = float(w)/h
        if aspectRatio >= 0.95 and aspectRatio <= 1.05:
          lg=["Square",(x,y)]
          lst.append(lg)

        else:
          lg=["Rectangle",(x,y)]
          lst.append(lg)  
    elif len(approx) == 5:
        lg=["Pentagon",(x,y)]
        lst.append(lg)
    else:
        lg=["Circle",(x,y)]
        lst.append(lg)    
    lg.insert(0,"Green") 

### Creating contour to track yellow color
contours, hierarchy = cv2.findContours(yellow_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    approx = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    x = approx.ravel()[0]
    y = approx.ravel()[1] - 5
    if len(approx) == 3:
        ly=["Triangle",(x,y)]
        lst.append(ly)
    elif len(approx) == 4:
        x1 ,y1, w, h = cv2.boundingRect(approx)
        aspectRatio = float(w)/h
        if aspectRatio >= 0.95 and aspectRatio <= 1.05:
          ly=["Square",(x,y)]
          lst.append(ly)

        else:
          ly=["Rectangle",(x,y)]
          lst.append(ly)  
    elif len(approx) == 5:
        ly=["Pentagon",(x,y)]
        lst.append(ly)
    else:
        ly=["Circle",(x,y)]
        lst.append(ly)    
    ly.insert(0,"Yellow") 

### Creating contour to track Orange color
contours, hierarchy = cv2.findContours(orange_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)    
for pic, contour in enumerate(contours):
    approx = cv2.approxPolyDP(contour, 0.01* cv2.arcLength(contour, True), True)
    x = approx.ravel()[0]
    y = approx.ravel()[1] - 5
    if len(approx) == 3:
        lo=["Triangle",(x,y)]
        lst.append(lo)
    elif len(approx) == 4:
        x1 ,y1, w, h = cv2.boundingRect(approx)
        aspectRatio = float(w)/h
        if aspectRatio >= 0.95 and aspectRatio <= 1.05:
          lo=["Square",(x,y)]
          lst.append(lo)

        else:
          lo=["Rectangle",(x,y)]
          lst.append(lo)  
    elif len(approx) == 5:
        lo=["Pentagon",(x,y)]
        lst.append(lo)
    else:
        lo=["Circle",(x,y)]
        lst.append(lo)    
    lo.insert(0,"Orange")     
     
print(lst)
cv2.waitKey(0)
cv2.destroyAllWindows()`
