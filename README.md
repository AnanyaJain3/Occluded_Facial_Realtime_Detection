# Designing and implementing a low-power multi-camera-based human detection system on low-power edge devices that addresses the challenges created by occlusions and rapid changes in positions during human detection.

## *Face Detection*

Face detection is similar to object detection. It is the process of automatically detecting the location of faces and localizing by drawing a bounding box in the image. Top, left, bottom, and right coordinates of the face are returned by the face detection algorithm MTCNN (Multi-Task Cascaded Convolutional Neural Network)

## *Facial Landmark Detection*

Facial landmarks are the spatial points in a human face. The number of spatial points chosen may vary from five to 78 points depending on annotation. Facial landmarks are also referred to as fiducial-points, facial key points, or face pose.

After identifying facial keypoints, the distance between these points is measured. This value is called facial features. These features are used to classify a face.

## *Face Alignment*

The faces can be aligned using fiducial points. This is done to make face images clicked from different angle face straight forward. Then the features extracted are matched with a template. The aligned faces can be used for comparison. The aligned face is the output of MTCNN, and is given as input to FaceNet

## *Face Recognition Using FaceNet Model*

Face recognition is the process of identifying a person from a digital image or a video. This is a 1:K matching problem. A use case for this could be marking employee attendance when an employee enters the building by looking up their face encodings in the database. The FaceNet model expects a 160x160x3 size face image as input, and it outputs a face embedding vector with a length of 128. This face embedding contains information that describes a face's significant characteristics. Then, FaceNet finds the class label of the training face embedding that has the minimum L2 distance with the target face embedding. It is the shortest distance between two points in an N dimensional space also known as Euclidean space
The Convolutional Neural Network model uses the Triplet Loss function. This function returns a smaller value for similar images and larger value for disimilar images. The components of a FaceNet network are described in the following sections.

INPUT IMAGES

The training set consists of thumbnails of faces cropped to a 160x160 size from the images. Other than translation and scaling, no other alignments to the face crops are needed.




