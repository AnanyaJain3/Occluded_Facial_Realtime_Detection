# Designing and implementing a low-power multi-camera-based human detection system on low-power edge devices that addresses the challenges created by occlusions and rapid changes in positions during human detection.

*Dataset and Models - https://drive.google.com/drive/folders/1_0GHVNCEpMjbGgt04sXqjHvaPakmxHZP?usp=share_link*

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

Deep CNN

There are two different architectures described in the FaceNet paper:
Deep CNN Based on Zeiler and Fergus Network Architecture
Inception Model Architecture Based on GoogLeNet
The two architectures differ in the number of parameters used and the Floating Point Operations Per Second (FLOPS). FLOPS is a standard measure of computer performance that requires floating-point computations. The model accuracy is higher with larger FLOPS. A network with lower FLOPS runs faster and consumes less memory, but results in lower accuracy.

**NN1: Deep CNN Based on Zeiler and Fergus Network Architecture**

The Zeiler and Fergus CNN architecture consists of 22 layers and trains on 140 million parameters at 1.6 billion FLOPS per image.

<img src="https://github.com/AnanyaJain3/Occluded_Facial_Realtime_Detection/blob/main/image/yyQEgwc-2.png" alt="lpastar" width="400"/>

**NN2: Inception Model Architecture Based on GoogleNet**

This model has 20× fewer parameters (around 6.6 million to 7.5 million) and 5× fewer FLOPS (around 500 million to 1.6 billion).

<img src="https://github.com/AnanyaJain3/Occluded_Facial_Realtime_Detection/blob/main/image/rD5QyDZ.png" alt="lpastar" width="400"/>

**NN4**

This network has an input size of 96x96 and requires only 285 million FLOPS. It's suitable for mobile devices.

<img src="https://github.com/AnanyaJain3/Occluded_Facial_Realtime_Detection/blob/main/image/3i9oznA.png" alt="lpastar" width="400"/>

## *Face Embedding*

The face embeddings of sizes 1×1×128 are generated from the L2 normalization layer of the deep CNN. This embedding is used in face verification and face clustering

<img src="https://github.com/AnanyaJain3/Occluded_Facial_Realtime_Detection/blob/main/image/O3Spw9G.png" alt="lpastar" width="400"/>

## **The Triplet Loss**

The embeddings of the same faces are called positives, and the embeddings of different faces are called negatives. The face being analyzed is called the anchor. To calculate the loss, a triplet consisting of an anchor, a positive, and a negative embedding is formed, and their Euclidean distances are analyzed. The learning objective of FaceNet is to minimize the distance between an anchor and a positive, and maximize the distance between the anchor and a negative. A training triplet contains three samples: anchor, positive, and negative (A, P, N). Any triplet loss embedding network objective is to learn an embedding such that

<img src="https://github.com/AnanyaJain3/Spacecraft-Trajectory-Optimization/blob/main/animations/Astar.gif" alt="lpastar" width="400"/>

## **Triplet Selection**

Choosing the correct image pairs is extremely important as there will be a lot of image pairs that will satisfy this condition. The model won't learn much from them and will also converge slowly because of that. To ensure fast convergence, it is crucial to select triplets that violate the triplet constraint.

<img src="https://github.com/AnanyaJain3/Spacecraft-Trajectory-Optimization/blob/main/animations/Astar.gif" alt="lpastar" width="400"/>

Eq(1) means that given an anchor image of person A, we want to find a positive image of A such that the distance between those two images is largest. Eq(2) means that given an anchor image A, we want to find a negative image such that the distance between those two images is smallest So, we are just selecting the hard positives and hard negatives here. This approach helps us in speeding convergence as our model learns useful representations.
The inventors of FaceNet used mini-batches consisting of 40 positives and randomly selected negative embeddings. The minimum and maximum distances were calculated for each mini-batch to create triplets.

## **Face Verification**

Face verification compares the facial embeddings of all trained images with the given image to find matches. Finding whether two images belong to the same person is 1:1 mapping.

## **Face Clustering**
Face clustering is the process of grouping images of the same person together for albums. It answers the question, "Are there similar faces?" The embeddings of faces can be extracted, and a clustering algorithm such as K-means can be used to group the faces of the same person together
