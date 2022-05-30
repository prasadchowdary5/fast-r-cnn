Deep Convolution networks have significantly improved image classification and object detection accuracy. Compared to image classification, object detection is a more challenging task that requires more complex methods to solve. Prior to the arrival of Fast R-CNN, most of the approaches train models in multi-stage pipelines that are slow and inelegant. In this article I will give a detailed review on Fast Rcnn paper by Ross Girshick. We will divide our review to 7 parts:

Drawbacks of previous State of art techniques (R-CNN and SPP-Net)
Fast RCNN Architecture
Training
Detection
Some Further Observations and Evaluations
Comparison with state of art results
Main Results
1. Drawbacks of previous State of art techniques (R-CNN and SPP-Net)
Fast R-CNN proposes a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations. It came as an improvement of R-CNN and SPP-Net.

Some drawbacks of R-CNN were:

Training is multistage pipeline: In R-CNN first a Convnet is fine tuned on object proposals using log loss. Then, it fits SVM to Convnet features by replacing Soft Max. In third stage regressors are trained.
Training is expensive in space and time.
Detection is also very slow.
The main drawback of R-CNN is that is slow. It is mainly because Convnet forward pass of each object proposals. There are about 2000 object peoposals generated from each image. SPP-Net solves this issue as it computes the feature map of entire image and then embedding ROI proposals to these feature maps through ROI projection. SPP-Net accelerates R-CNN by 10 to 100×at test time. Training time is also reduced by 3×due to faster proposal feature extraction.Even then SPP-Net also had several drawbacks.The main drawbacks are:

Training is multistage pipeline.
The fine-tuning algorithm proposed in cannot update the convolution layers that preceds the spatial pyramid pooling.(But possible in R-CNN)
Fast R-CNN was proposed as a new training algorithm that fixes the disadvantages of R-CNN and SPPnet. Some of the notable features are:

Training in single stage pipeline.
Higher detection quality (mAP) than R-CNN, SPPnet
Training can update all layers.
No disk space required for feature catching.
Why Encryption is Critical to Everyday Life? | Data Driven Investor
You type in passwords almost every single day, the most basic form of encryption used in your life. Yet the question…

2. Fast-RCNN Architecture
A fast R-CNN takes a set of object proposals and image as input. The network passes this image through several convolution layers and max pooling layers and forms a feature map. Map the object proposals on feature map using ROI projection.ROI projection is just finding the coordinates of region proposals on feature map corresponding to that in original image.I will explain about it later.

For each object proposal, Region of Interest(ROI) pooling layer will extract a fixed length feature vector and pass it through fully connected layers. These fully connected layers branch into two output layers:

One that produces soft max probability of K+1 classes(K classes and 1 background class)
Other which provides bounding box coordinates for K classes.

Before explaining further its better to explain some concepts which you need to understand.
