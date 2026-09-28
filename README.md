# Computer Vision Projects
1. The Anatomy of Vision: From Pixels to Gradients
Before a machine can recognize complex objects, it must perceive structure. It does this by calculating spatial intensity gradients—detecting sharp changes in brightness that signify borders, surfaces, and shadows.

<img width="250" height="271" alt="image" src="https://github.com/user-attachments/assets/c9db1506-7da3-4de6-b5bc-ce1e766a10e3" />

Feature Extraction via Intensity Gradients
Using mathematical convolutions, such as the Sobel operator, the vision pipeline calculates partial derivatives across pixel coordinates (x,y) to determine gradient magnitude G:
G= 
G 
x
2
​	
 +G 
y
2
​	
 

​	
 
Python
import cv2
import numpy as np

# 1. Load raw pixel grid in grayscale
image = cv2.imread('scene.jpg', cv2.IMREAD_GRAYSCALE)

# 2. Compute spatial derivatives along X and Y axes
sobel_x = cv2.Sobel(image, cv2.CV_64F, 1, 0, ksize=3)
sobel_y = cv2.Sobel(image, cv2.CV_64F, 0, 1, ksize=3)

# 3. Calculate edge strength (gradient magnitude)
magnitude = cv2.magnitude(sobel_x, sobel_y)
edge_map = np.uint8(np.absolute(magnitude))

cv2.imwrite('edges.jpg', edge_map)
2. High-Level Cognition: Neural Attention & Spatial Mapping
Modern architectures—such as Vision Transformers (ViTs) and real-time detection models like Ultralytics YOLO—group edge features into hierarchical representations. The vision system constructs 3D bounding volumes, tracks movement vectors across frames, and extracts spatial depth maps.

Object Detection & Bounding Box Prediction
The neural network processes image patches through self-attention layers to predict target classes alongside normalized bounding box coordinates [x 
center
​	
 ,y 
center
​	
 ,width,height]:
Python
import torch
import cv2

# Load pre-trained vision backbone (e.g., YOLO or Vision Transformer)
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', pretrained=True)

# Run inference on incoming video frame
frame = cv2.imread('city_street.jpg')
results = model(frame)

# Parse detected objects, confidence scores, and bounding boxes
for detection in results.xyxy[0]:
    x1, y1, x2, y2, conf, cls_id = detection
    if conf > 0.5:
        label = f"{model.names[int(cls_id)]}: {conf:.2f}"
        cv2.rectangle(frame, (int(x1), int(y1)), (int(x2), int(y2)), (0, 255, 0), 2)
        cv2.putText(frame, label, (int(x1), int(y1) - 10), 
                    cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

cv2.imshow('Computer Vision Perception', frame)
The Computer Vision Processing Hierarchy
Layer	Input Data	Operation	Output / Meaning
Low-Level	H×W×3 RGB Matrix	Gaussian Blur, Sobel Convolutions	Edges, Corners, Textures
Mid-Level	Edge & Texture Feature Maps	Region Proposals, Contour Grouping	Shapes, Surfaces, Part Alignments
High-Level	Deep Attention Weights	Transformer / CNN Classification	Bounding Boxes, Mask Profiles, 3D Mesh
Action	Class Probabilities & Tracking Vectors	Motion Estimation, Actuation Rules	Steering Commands, Cursor Trigger, Alert Signals
