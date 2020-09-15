# RCNN_Object_Detection

The entire project involves the following steps to successfully complete the Region based Convolution Neural Network (RCNN) based object detection to detect garbage:
1. Downloading and Annotating dataset
2. Building Essential tools
3. Building Dataset
4. Model Building and Training
5. Detect garbage using the trained model

## 1. Downloading and Annotating Dataset
Use bulk image downloader application available to download images in bulk, clean the dataset from noises by removing unwanted images. I have used Microsoft Vott annotation toolkit to annotate (i.e) box the area in the image where garbage is present and export the annotated project as Pascal VOC format.

## 2. Building Essential Tools:
To perform this object detection task we need to define few essential functions like Non-Max Supression (NMS) and Intersection over Union (IoU). NMS is used to reduce the number of bounding boxes created due selective segmentation to minimum (say 1) and IoU is used to find which bounding box amoung n number of bounding boxes created has our region of interest (i.e. garbage in it). 

## 3. Building Dataset
Next we build a dataset to train our neural network to classify garbage with great accuracy. Using the annotation file created before we crop the bounding box annotated on the image, augment it in different format (do not enable Horizontal Flip) and save the images to a seperate directory (positive class) and using the IoU and selective segmentation we crop and save regions where IoU value is less than 5% (a decent low value) in another seperate directory (negative class). Now our neural network will have access to a excellent quality dataset.

## 4. Model Building and Training
We will use the well recognised fine tuning method, to convert best in class standard neural architects (in my case MobilNetV2 with imagenet weights) to classify on our custom dataset. To fine tune we import the standard model without the top layer, build our dense layer/top layer over it and train the model by freezing the base layer (Note: If your using multiclass object classification first train by freezing all the layers in the base model and train the dense layer alone, and then unfreeze few layers in the last and train to get excellent accuracy). After training save the model.

## 5. Detect Garbage using the trained Model
With our trained model, we selective segment our test image, classify the bounding boxes as region of interest (roi) predict the bounding boxes using our trained model and getting the results of those with maximum probability. We the filter the outcomes for garbage label alone (positive class) apply non max suppression to the bounding boxes and get final bounding box which detects the class in the given input image successfully. 
