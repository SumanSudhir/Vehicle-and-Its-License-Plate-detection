# Objective
To train a model which will detect vehicle(Motorcycle, Auto-rikshaw, Bus, Car, Truck) present in frame along with its license plate

# Solution Approach
The YOLO implementations are amazing tools that can be used to start detecting common objects in images and videos. However there are many cases,when the object which we want to detect are not part of the popular dataset. In such cases we need to create our own training set and execute our own training. Vehicle and its License plate detection are one such cases. I have used tiny-yolov3 to detect the desired classes and have collected around 1700+ images for training and validation.

# Dataset
I have collected the image of vehicle of desired class from Google Image,Kaggle and by clicking some picture of vehicle from the roads so that the license plate of vehicle is visible. I have collected around 1700+ images and manually annotated all the 
desired classes in image. Each class consist of atleast 350 images in training dataset.
Later converted the cooridnate of annotated object in yolo format that is:

`<object-class> <x_center> <y_center> <width> <height>`

  Where: 
  * `<object-class>` - integer object number from `0` to `(classes-1)`
  * `<x_center> <y_center> <width> <height>` - float values **relative** to width and height of image, it can be equal from `(0.0 to 1.0]`
  * for example: `<x> = <absolute_x> / <image_width>` or `<height> = <absolute_height> / <image_height>`
  * atention: `<x_center> <y_center>` - are center of rectangle (are not top-left corner)
 

| Vehicle       | Label Id      |
| ------------- |:-------------:| 
| Car           |       0       | 
| Truck         |       1       | 
| Bus           |       2       | 
| Motorcycle    |       3       |
| Auto          |       4       |
| CarLP         |       5       |
| TruckLP       |       6       |
| BusLP         |       7       |
| MotorcycleLP  |       8       |
| AutoLP        |       9       |
*License Plate(LP)

Later splitted all the dataset in training and validation set and stored the path of all the images in file named train.txt and valid.txt

# Configuring Files
Yolov3 needs certain specific files to know how and what to train.
1. obj.data
2. obj.names
3. obj.cfg

## obj.data
This basically says that we are training 10 classes, what the train and validation files are and which file contains the name of object we want to detect.During training save the weight in backup folder.
```
classes = 10
train = train.txt
valid = test.txt
names = obj.names
backup = backup
```
## obj.names
Every new cateogry must be in new line and its category number be same what we have used at the time of annotating data.
```
Car
Truck
Bus
Motorcycle
Auto
CarLP
TruckLP
BusLP
MotorcycleLP
AutoLP
```
## obj.cfg
Just copied the tiny-yolov3.cfg files and made few changes in it.
* In line 3, set `batch=24` to use 24 images for every training step.
* In line 4, set `subdivisions=8` to subdivide the batch by 8 to speed up the training process.
* In line 127, set `filters=(classes + 5)*3`, e.g `filter=45`.
* In line 135, set `classes=10`, number of custom classes.
* In line 171, set `filters=(classes + 5)*3`, e.g `filter=45`.
* In line 177, set `classes=10`, number of custom classes.

then,save the file
# Training 
I have trained tiny-yolov3 for about 40,000 iterations and get the minimum total loss 0.15 with 0.001 learning rate, 0.9 Momentum and 0.0005 decay.

Training Loss Plot:

![Loss Plot](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/lossPlot.png)

# Testing
For testing open the terminal and clone the repository by command 
```
git clone https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection.git
cd Vehicle-and-Its-License-Plate-detection
make
```  
Download weights from the link http://storage.googleapis.com/sudhir_storage/Yolov3/obj_40000.weights in darknet directory.
Move one of your images in the testing group to the directory of Darknet and rename it as `test.jpg`
Next, open Terminal in darknet directory and run
```.
./darknet detector test obj.data cfg/obj.cfg obj_40000.weights test.jpeg
```
In terminal you will see the class of object detected and resulting image will be stored in darknet directory with name prediction.jpg

# Some Results
![Truck](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/results/truck.jpg)
![Bike](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/results/bike.jpg)
![Car](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/results/car.jpg)
![Bus](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/results/bus.jpg)
![Auto](https://github.com/SumanSudhir/Vehicle-and-Its-License-Plate-detection/blob/master/results/auto.jpg)



