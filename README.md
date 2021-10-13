# Training custom yolo-weights

## Sampling data

### Taking pictures
Take at least a couple of hundred images with various background, lighting and also take some pictures where the object is partly covered. You also want to take pictures with varying angles, tilt and distance. The number of images needed will also depend on whether the object you want to detect have large variations in appearence. 

These pictures do not need to be in high resolution as Yolo will automatically scale them down to around 400x400.

### Naming data

To rename all the images in a clean format you can run "photo_renamer.py". Just enter the folder directory in
the folder variable and run.

## Labeling images

```bash
cd labelImg
```

Please read the readme in the labelImg folder for installation and how to run the program.

Add your classes in data/predefined_classes.txt (and remove the pre-existing classes)

## IMPORTANT! BEFORE YOU BEGIN LABELING
Press the button on the left of the program where it says "Pascal/VOC" until it says "YOLO" to have the correct format.

To be efficient in labeling, please use the  following keyboard shortcuts:

| Keyboard Shortcuts 	| Description                    	|
|--------------------	|--------------------------------	|
| CTRL + U           	| Load all images from directory 	|
| CTRL + SHIFT + D   	| Delete the current image       	|
| W                  	| Create a rect box              	|
| D                  	| Next Image                     	|
| A                  	| Previous Image                 	|
| CTRL + S           	| Save image                     	|

## You should now have:

1 folder with all your training images numbered as 1, 2, 3... aswell as a matching .txt document on the format:

```bash
 <Class>                 <Bounding Box>
    0          0.231771 0.913564 0.112351 0.172872
    0          0.321771 0.143564 0.522351 0.653872
```


# Training with Darknet

### Creating train.txt and test.txt

Copy all training images and .txt files in
```
darknet\data\obj
```

Then run create_test_and_train.py after you have set the directory where the images are. Remember to use the full path. You should now have two .txt files: 
```
train.txt and test.txt
```
Here you should see the path of every image in darknet\data\obj. If you get any errors when trying to start training, change the path in train.txt and test.txt to full paths. This can easily be done by selecting everything infront of eg. 1.txt and changing all occurences (CTRL + F2) in VSCode.


1. Find the file yolo-obj.cfg in 
```
darknet/cfg/yolo-obj.cfg
```
and configure based on number of classes. 

max_batches = 2000*number_of_classes, minimum 4000
```
steps = 80%, 90% of max_batches (eg. 3200, 3600)
```
On the last convolutional layer:
```
filters = 5*(number_of_classes + 5) (eg. 30 for classes=1)
```

On the last region layer:
```
classes = number_of_classes (eg. 1)
```

2. Create file obj.names in the directory darknet/data, with classnames - each in new line
eg.

```
spy
robot
```
3. Create file obj.data in the directory darknet/data. It should look like this, where classes = number of classes:

```
classes= 1
train  = data/obj/train.txt
valid  = data/obj/test.txt
names = data/obj.names
backup = backup/
```
backup/ is where the trained weights will appear.

### Now you're ready to start training!

```bash
./darknet detector train cfg/obj.data cfg/yolo-obj.cfg cfg/darknet19.448.conv.23
```

