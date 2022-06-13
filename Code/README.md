# The origin of this proyect
This project was started by Professor Joaquin Ramirez and ___ with the a *collab* notebook using one of the tensorflow notebooks [examples](https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/images/classification.ipynb) for a convolutional neural network notebook.
From there on myself (gquinche) and Edinson Serrano had worked on increasing the scope and accuracy of the model, the following are roadblocks and advances of the proyect.

# Advances

## Accuracy correction
The initial notebook reported a high accuracy that we think was not indicative of the performance of the model, from the new beginning of the project a focus was on reducing leakage and changing to a three step hold out technique (Train,Validate,Test), arguments for these decisions can be found online or in the attached PDF document in this folder.

To combat [_leakage_](https://www.wikiwand.com/en/Leakage_(machine_learning)) and duplicate data (even if they were rotations or symmetries) some scripts were created to detect and erase them, as the data origin (google drive) was separate from the notebooks, this was a slow and frustrating process, today we encourage the use of a more robust tool, like [**ROBOFLOW**](https://roboflow.com/), that would detect the duplicates and leakage automatically.

##  Expansion with instance segmentation 
As classification of a single leaf is a somewhat easy process, and the Professor was interested in generalizing the model to analyze whole trees, a instance segmentation pathway was chosen at first, although harder to implement instance segmentation give polygons of detections of a certain class (in this case leaves) the planned ensemble model would then take this polygons and  classify them with a regular convolutional network, the pros are that we get a more interpretable model, not only as researchers but for the user, knowing exactly what leave is having damage can have various practical applications, more so seeing exactly how a model is estimating damage by a ratio of damaged/ healthy, a user could correct in principle an incorrect individual classification and estimate the ratio easily anyway.
## Data augmentation and semi supervised trials
As one of the proposed models to do the instance segmentation was Detectron2, we discovered that polygonal instance segmentation was a very slow process, we tried several techniques then to try and speed up the process

* change to *object detection*
* using the default label assist system of **ROBOFLOW**
* creating a custom label assist model
  
One of the cons of object detection (box polygons instances only) was that it wasn't very robust when leaves where rotated, we explored an intermediate [model](https://blog.roboflow.com/yolov5-for-oriented-object-detection/) that was developed recently however we haven't decided on it's use. 

The default label assist was fine and speed up the process, however again it was only applicable to object detection and not instance segmentation 
Finally We tried to create a custom instance segmentation label assist model with the free credits provided by Roboflow, however the option to use it never emerged, this seems to be because label assist only works with straight bounding boxes at the moment, the custom model also had a very hard time generalizing, with a 0.3 accuracy, this was indicative that we had not enough data for the training.
## Simpler models generalize better with less data
As we have stated we had a hard time training and also labeling with the instance segmentation technique, this is a general problem of this technique and why it isn't as common, in general it needs a high cost cost for labeling and a big quantity of data, thus we have to correct this direction. *there wasn't a very strong reason for having precise bounding polygons of our leafs*, we wont blame ourselves too hards as this emerged from disconnected collaboration and trying to tailor a cancer detection model to our problem. We will try to fix the first problem with a tool like Trello and happily know that a detectron model can work with bounding boxes too.

## Data set cleaning 

After another inspection to the we confirmed that not only there was a lot of duplicates in the original data set passing from 768 images to 616, but there was also a lot of rotations so we got to a final 533 with this in mind we got a more critical eye about possible leakage between classes and also data semi duplicates, the problems is if this little variations can be memorized in the training set but are also present in the validation and test set, we will get a very overfitted model.

# Next Steps

* Switch from the instance segmentation model to box detection and test it on the complete data set
* use open cv or such to extract a given box segment from an image and run classification on it
* continue on consolidation of the notebook, data and roadmap
* clean more the dataset