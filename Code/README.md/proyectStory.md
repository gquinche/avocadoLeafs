# The origin of this proyect
This proyect was started by Proffesor Joaquin Ramirez and ___ with the a *collab* notebook using one of the tensorflow notebooks [examples](https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/images/classification.ipynb) for a convolutional neural network notebook.
From there on myself (gquinche) and Edinson Serrano had worked on increasing the scope and accuracy of the model, the following are roadblocks and advances of the proyect.

# Advances

## Accuracy correction
The initial notebook reported a high accuracy that we think was not indicative of the performance of the model, from the new begining of the proyect a focus was on reducing leakeage and changing to a three step hold out technique (Train,Validate,Test), arguments for these decisiones can be found online or in the attached PDF document in this folder.

To combat [_leakeage_](https://www.wikiwand.com/en/Leakage_(machine_learning)) and duplicate data (even if they were rotations or symmetries) some scripts were created to detect and erase them, as the data origin (google drive) was separate from the notebooks, this was a slow and frustating process, today we encourage the use of a more robust tool, like [**ROBOFLOW**](https://roboflow.com/), that would detect the duplicates and leakeage automatically.

## Model expansion
As classification of a single leaf is a somewhat easy process, and the Professor was interested in generalizing the model to analyze whole trees, a instance segmentation pathway was choosen, altough harder to implement instance segmentation give us boxes and polygons that it detects as certain class (in this case leaves) and doing an ensembble model we can the classify each box and polygon with a tradition classification network, the pros are that we get a more interpretable model, not only for us researchers but for the user, knowing exactly what leave is having damage can have various platical aplications, more so seeing exactly how a model is estamating damage by a ratio of damaged/ healthy, a user could correct in principle an incorrect classification and estimate the ratio easyly anyway.
## Data augmentation and semi supervised trials
As one of the proposed models to do the instance segmentation was Detectron2, we discovered that poligonal instance segmentation was a very slow process, we tried several techniques then to try and speed up the process

* change to bounding box segmentation
* using the default label assit system of **ROBOFLOW**
* creating a custom label assist model
  
One of the problems we found with box segmentation was that it wasn't very robust when leaves where rotated, we explored an intermediate [alternative](https://blog.roboflow.com/yolov5-for-oriented-object-detection/) that was developed recently.
the dafult label assyst was fine, however only suggested rectangular "straight" bounding boxes
We created a custom label model with the free credits provided by roboflow, however the option to use never emerged, this seems to be because label assist only works with straigh bounding boxes at the moment, the custom model also had a very hard time generalizing, with a 0.3 accuracy, seems sadly that the data augmentation that polygons now follow perfectly (by rotating and staying exactly in place for example) was too much for the model, probably the most problematic aspect was obscuring pixels, or  
