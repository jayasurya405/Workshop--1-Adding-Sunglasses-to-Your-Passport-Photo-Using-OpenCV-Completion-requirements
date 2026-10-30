# Workshop--1-Adding-Sunglasses-to-Your-Passport-Photo-Using-OpenCV-Completion-requirements
Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!

## PROGRAM

### Step 1: Import Required Libraries
Import OpenCV, NumPy, and Matplotlib to perform image processing and display the results.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

### Step 2: Read and Display the Face Image
Load the original face image and display it using Matplotlib.

```python
faceImage = cv2.imread('3.jpeg')

plt.imshow(faceImage[:, :, ::-1])
plt.title("Face")
plt.axis('off')
plt.show()

print(faceImage.shape)
```

### Step 3: Read and Display the Sunglasses Image
Load the sunglasses image with its alpha channel to preserve transparency information.

```python
glassPNG = cv2.imread('sunglass.png', -1)

plt.imshow(glassPNG[:, :, :3][:, :, ::-1])
plt.title("Sunglasses")
plt.axis('off')
plt.show()
```

### Step 4: Resize the Sunglasses Image
Resize the sunglasses to fit the eye region of the face.

```python
glassPNG = cv2.resize(glassPNG, (190, 50))

print("Image Dimension =", glassPNG.shape)
```

### Step 5: Separate Color and Alpha Channels
Separate the color information and alpha channel from the sunglasses image.

```python
glassBGR = glassPNG[:, :, 0:3]
glassMask1 = glassPNG[:, :, 3]

plt.figure(figsize=[12, 6])

plt.subplot(121)
plt.imshow(glassBGR[:, :, ::-1])
plt.title("Sunglasses Color Channels")
plt.axis('off')

plt.subplot(122)
plt.imshow(glassMask1, cmap='gray')
plt.title("Sunglasses Alpha Channel")
plt.axis('off')

plt.show()
```

### Step 6: Place Sunglasses on the Face
Copy the face image and place the sunglasses over the selected eye region.

```python
faceWithGlassesNaive = faceImage.copy()

faceWithGlassesNaive[165:215, 110:300] = glassBGR

plt.imshow(faceWithGlassesNaive[:, :, ::-1])
plt.title("Face with Sunglasses")
plt.axis('off')
plt.show()
```

### Step 7: Apply Masking to the Sunglasses
Use the alpha channel to create a mask, remove the corresponding background pixels, and combine the sunglasses with the eye region.

```python
glassMask = cv2.merge(
    (glassMask1, glassMask1, glassMask1)
)

glassMask = np.uint8(glassMask / 255)

faceWithGlassesArithmetic = faceImage.copy()

eyeROI = faceWithGlassesArithmetic[165:215, 110:300]

maskedEye = cv2.multiply(eyeROI, (1 - glassMask))
maskedGlass = cv2.multiply(glassBGR, glassMask)

eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

plt.figure(figsize=[15, 5])

plt.subplot(131)
plt.imshow(maskedEye[:, :, ::-1])
plt.title("Masked Eye Region")
plt.axis('off')

plt.subplot(132)
plt.imshow(maskedGlass[:, :, ::-1])
plt.title("Masked Sunglasses Region")
plt.axis('off')

plt.subplot(133)
plt.imshow(eyeRoiFinal[:, :, ::-1])
plt.title("Augmented Eye and Sunglasses")
plt.axis('off')

plt.show()
```

### Step 8: Display the Final Image
Replace the selected eye region with the combined image and compare the original face with the final result.

```python
faceWithGlassesArithmetic[165:215, 110:300] = eyeRoiFinal

plt.figure(figsize=[12, 6])

plt.subplot(121)
plt.imshow(faceImage[:, :, ::-1])
plt.title("Original Image")
plt.axis('off')

plt.subplot(122)
plt.imshow(faceWithGlassesArithmetic[:, :, ::-1])
plt.title("With Sunglasses")
plt.axis('off')

plt.show()
```

## OUTPUT

The following outputs are obtained:

1. Original face image displayed using OpenCV and Matplotlib.
   
<img width="487" height="540" alt="image" src="https://github.com/user-attachments/assets/55d70147-9789-4dfd-8407-fbba40da4eed" />

2. Sunglasses image loaded and displayed.

<img width="743" height="350" alt="image" src="https://github.com/user-attachments/assets/fe1d5591-1393-4ad6-bad0-24168a60080f" />

3. Color and alpha channels of the sunglasses displayed separately.

<img width="1380" height="230" alt="image" src="https://github.com/user-attachments/assets/728340c0-98aa-4918-9316-cf42b4fcd7d2" />
  
4. Sunglasses placed over the eye region.

<img width="513" height="518" alt="image" src="https://github.com/user-attachments/assets/312dc495-b9a2-4863-a250-2598b7b47cc4" />

5. Combined eye and sunglasses region obtained using masking and arithmetic operations.

 <img width="1372" height="176" alt="image" src="https://github.com/user-attachments/assets/e8eb88b6-b8b3-47ff-b553-4ef10e39f7d7" />

6. Original and modified face images displayed side by side.

<img width="1372" height="748" alt="image" src="https://github.com/user-attachments/assets/925472e4-1b29-4a8a-aa1a-f6c0e873bf71" />


## RESULT

Thus, sunglasses were added to a face image using Python, OpenCV, NumPy, and Matplotlib. The experiment demonstrated image loading, resizing, alpha-channel separation, masking, pixel arithmetic, and image blending to overlay sunglasses onto the eye region.
