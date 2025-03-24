# Akrati-
    img = cv2.imread('fruit.jpg')
    img1 = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

i used imread to read the image named fruit and converted it from BGR to HSV because we have to seprate a color and we will do it by providing it's range which is done using HSV.

    lower_purple = np.array([125,50,50])
    upper_purple = np.array([160,255,255])

then i provided the upper and lower HSV range of the first color we have to identify that was purple.

    mask1 = cv2.inRange(img1, lower_purple, upper_purple)

then i used inRange for binary mask which will convert all the purple color region to white and other region to black

    kernel = np.ones((5,5),np.uint8)  
    opening1 = cv2.morphologyEx(mask1, cv2.MORPH_OPEN, kernel)

i created a kernel of 5*5 because i did opening(MORPHOLOGICAL OPERATION) to remove noise which will first do erosion and then dilation in the mask.

    ret, threshold1 = cv2.threshold(opening1,140,255,cv2.THRESH_BINARY)

then i had to perform contour on the image for that i convert the opening1 to a binary image.

    contours, hierarchy = cv2.findContours(threshold1, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    for contour in contours:
        x,y,w,h= cv2.boundingRect(contour) 

        cv2.rectangle(img, (x,y), (x+w, y+h),(0,255,0), 2)
        font = cv2.FONT_HERSHEY_SIMPLEX
        cv2.putText(img, 'PURPLE', (x,y),font,0.8,(0,255,0),2,cv2.LINE_AA)

I did contour on threshold1 to find the boundary across the purple color by using cv2.findContour where i provided threshold1, mode is cv2.RETR_EXTERNAL and method is cv2.CHAIN_APPROX_SIMPLE. Then i started a loop using for and used cv2.boundingRect to draw a boundary around the purple color. 
Now to draw a rectangular box around the puple region i used cv2.rectangle and provided the image and color of the box.
To wite text on img i used cv2.putText and provided PURPLE as text with font type, size,color and used cv2.LINE_AA to make the edge smoother.

    res1 = cv2.bitwise_and(img, img, mask=threshold1)
then used bitwise_and to extract the color from the original image using mask = threshold1

    lower_red = np.array([0,120,70])
    upper_red = np.array([10,255,255])

    mask2 = cv2.inRange(img1, lower_red, upper_red)

provided color range for red and created mask for red color.

    kernel = np.ones((5,5),np.uint8)
    dilation = cv2.dilate(mask2, kernel, iterations = 1)

i created a kernel of 5*5 because i did dilation(MORPHOLOGICAL OPERATION) on mask2 which will fill the noises in red region.

    MaxArea = 162

    ret, threshold2 = cv2.threshold(dilation,140,255,cv2.THRESH_BINARY) 
    contours, hierarchy = cv2.findContours(threshold2, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    for contour in contours:
        area = cv2.contourArea(contour)

        if area < MaxArea:
            x,y,w,h = cv2.boundingRect(contour)

            cv2.rectangle(img, (x,y), (x+w, y+h),(0,0,255), 2)
            font = cv2.FONT_HERSHEY_SIMPLEX
            cv2.putText(img, 'RED', (x,y),font,0.8,(0,0,255),2,cv2.LINE_AA)

To identify only the smaller i used cv2.countourArea and if the area was less than a certain area value then only it identified that red color part. Other steps are similar to purple color.

    cv2.imshow('image',img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

used cv2.imshow to show the original image with all the identified region marked, cv2.waitKey to close the window when a key is pressed and cv2.destroyAllWindows() to close all openCV windows.

OUTPUT IMAGE : 
![OUTPUT ](https://github.com/user-attachments/assets/2634b8c9-5e3f-4bba-bb7c-90362ec1a7ef)
