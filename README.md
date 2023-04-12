# Vape-Detection
Made as a part of the Computational Media Lab

## Process
The following diagram is a nutshell of our evaluation procedure. Corresponding datasets and scripts are attached in this repository with more detailed instructions below.

![Process Diagram](figures/process_diagram.png)


## Data Collection

We collected data over several days using the [TikTokAPI](https://github.com/drawrowfly/tiktok-scraper). 

As shown in the paper and the figure above, we get 300 total videos from which we take screenshots. I have another repository where I maintain some helper codes, you can find code to take screenshots [there](https://github.com/tanviaanand/TikTok-Helper-Codes).

Exact data collected will depend on current TikTok Terms of Service, maintenance of the above library, and the ever changing TikTok recommendation algorithm. You can open an issue here or contact us if you would like to receive a copy of our dataset. We'll need to verify your use case and intentions before hand, additional restrictions may apply but otherwise we are happy to see the research community progress!

## Data Preparation

### Annotation
We used labelIMG, an open source library, to create image annotations, AKA draw bounding boxes around the objects of interest (in our case: vape, hand, smoke). Detailed download and usage instructions can be found [here](https://github.com/heartexlabs/labelImg). A human being will have to carry this step out!

### YOLO Requirements
Create the corresponding annotations of your training and validations set in the YOLO format.

Images must be stored in yolov7/data/train/images
Annotations must be stored in yolov7/data/train/labels

## Code

### Training

We used the YOLOv7 implementation given [here](https://github.com/WongKinYiu/yolov7). You can clone the repository for your specific use, or use it from our [yolov7](yolov7) folder.

The following files are used for custom implementation:

1. **yolov7/data/custom_hyperparameters.yaml**: This file is used to set hyperparameters and data augmentation settings.
2. **yolov7/data/custom_data.yaml**: Mention the location of train and validation folder  as follows
`train: ./data/train 
val: ./data/val
nc: 3  # number of classes
names: [ 'hand','smoke','vape' ]  # names of classes you are using` 
3. **cfg/training/yolov7_custom.yaml**: Download the model weights given in the  repo to your local system

Next, training is done using the following command (run this on the terminal of your choice):

`!python train_.py --device 0 --batch-size 16 --data data/custom_data.yaml --img 640 640 --cfg cfg/training/yolov7_custom.yaml --weights yolov7_training.pt --name model_1 --hyp data/custom_hyperparameters.yaml --epochs 700`

You will find the weights of the model on yolov7/runs/train/model_1 

You can now use these weights to run inference as follows (run this on the terminal of your choice):

`!python detect.py --weights runs/train/yoloV7_3_2/weights/best.pt --conf 0.6 --img-size 640 --source ./data/val/images --name inference1`

Alternatively, if you'd like to use python notebooks to make your life easier, you can find the training implemented in [notebooks/resource-allocator-1.ipynb](notebooks/resource-allocator-1.ipynb) and inference done from [notebooks/run-inference.ipynb](notebooks/run-inference.ipynb). 

If you'd like to run inference or train on your own model and dataset, you can simply change the file paths in the commands.

Thank you for referring to our project and feel free to open issues in case of any questions!
