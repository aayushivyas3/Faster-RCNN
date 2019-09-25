# Overview

The data set chosen for this experiment in this Practical Project is Red Blood Cell Data set. The data set is made up of microscopic images of red and white blood cells. Usually for such images it is hard to detect the red and white blood cells with naked eyes, so in an attempt to solve that problem, an object detection model is developed. In this model, the Faster R-CNN algorithm is used to detect red blood cells in the images. There are 490 images in the data set. This is divided into two parts: Train and Test. The Training(290) and Testing(200) data sets are customized using some pre-processing . But we only use 5 images to test the detection model using Faster R-CNN due to time and memory constraints.

It is important to detect the right objects, in this case cells in the object since this model can be used in many bio-technical equipment to automatically detect the cells. The most basic problem while detecting in such images is that usually the cells are packed very near to each other and can sometimes overlap, and then it can be hard to localize and detect a cell. Especially when the structures of the cells are completely identical, it makes it even harder for a human or a machine to detect clearly. These models are actually used in blood tests for blood cell counts and has many other applications.


# Exploration

Faster R-CNN is one of the most of the most popular Neural networks used for object detection. The architecture starts by taking images as input with dimensions  HeightxWidthxDepth. As a part of pre-processing , some of the requirements of the models are fulfilled. Such as creating a list of bounding boxes, labels to assign those bounding boxes, and a probability value for those labels and bounding boxes. In the data set used the annotations used are in .csv files which are then converted to xml files to use them as trecords(tensor records) for the model.

The data set contains two splits: Train and Test, each with annotations files in them. We replace the data set with the customized one. These annotation files are basically list of ground truths for that particular image. These ground truths are manually drawn over the images and fed to the model as a part of training. The model takes these ground truths and their labels and, in the easiest language, learns from them. The annotations take the longest to annotate and so the data set for this project is small. 

These annotations are taken as tfrecords in the model and converted to xml files, where these are read by the pre-trained model and uses them to classify the images.


# Methodology

For the first part of the architecture, transfer learning is used so as to not to implement the algorithm network from scratch. Transfer Learning is basically using a pre-trained model, specifically using its weights and train it on the customized data set. Usually the weights taken are already trained on a bigger data set. The base model for this is the VGG16 network architecture.

![alt text](https://i1.wp.com/appliedmachinelearning.blog/wp-content/uploads/2019/08/vgg16.png?resize=714%2C204&ssl=1)


* Convolution Layers(CONV3) = 12
* Pooling Layers(POOL) = 5
* Fully Connected Layer(FC) = 3


Next, the image goes through this pre-trained CNN and gives a feature map as an output. This is used to extract features for the next layer. The next layer, RPN (Region Proposal Network) uses those features and makes regions(bounding boxes) which might contain the object of interest in them. The output of this architecture are bounding boxes which detect these objects based on the amount of overlapping with the ground truth. The amount of the thresholding can be set by the user and the results will vary based on some configuration settings.

The biggest problem is to set the parameter value for the network. Parameters such as Learning rate, batch-size, rpn_ratios, and IOU_threshold are very important to get better accuracy while training. The RPN is the one layer which is very important in this network. The following image shows how the rpn works:


![alt text] (https://miro.medium.com/max/1126/1*wpyZTK62emDpTE_Wbza9oA.png)


# Evaluation

Faster R-CNN gave great accuracy scores. When experimented with SSD( Single Shot Detector), the overall loss while training was way less and gradually decreased while the training proceeded. Although it takes fair amount of time setting the anchors and bounding box thresholds, ultimately the accuracy scores got high up to 97%. When compared to SSD, the accuracy was better each time we trained. The output of SSD is a prediction map. Each location in this map stores classes confidence and bounding box information as if there is indeed an object of interests at every location. While testing was a completely different story, while first experimenting with Faster R-CNN, while testing the accuracy did not show correct, but as we started the training the model more and used the previously trained weights for the next training, the testing results started increasing too. The following are the tests performed on five images.






