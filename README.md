# YOLOBee

## Introduction

Bees are essential insects for biodiversity, as they pollinate plants. Pollination is a vital natural phenomenon for plants, enabling them to reproduce. This is the main reason why bees are necessary on the planet. However, bee numbers are declining drastically every year. According to an article in the newspaper LesEchos written by Charlotte Meyer in 2022, "In Europe, around 40% of bee colonies have disappeared in less than ten years."  This is a huge number and one that has a direct impact on us. Indeed, the article also explains that "75% of the world's food production depends on pollinating insects." This is why we need to change things and encourage the development of bees. 

The project I'm working on involves learning about bee's trajectories to understand whether certain flowers are more attractive to bees than others. Thanks to this, we will understand better bees' behavior and their needs. The experimentation involves 20 3D-printed flowers arranged in lines of 5. The flowers are 3D-printed for us to control all the parameters, the color of each flower and their petals are exactly the same, only the height is changing. 

 ## Explanation of the different folders
The different folders were used to improve the recognition of the neural network, either by modifying the dataset or by changing the augmentations. They are all using the Yolov5 model. Results improve as the file number increases. Each folder contains the result of the training, the validation, and the detection on three little videos (except   *training_01*). The weights calculated at the end of the training are not included in the folders because they were too heavy. Until *Training 04* the objective was only to improve bee recognition using yolov5m.

 - *Training 00* is the first training realized, which didn't go through to the end.
 - *Training 01* corresponds to the end of Training 00
 - *Training 02* continues the training of the neural network, keeping the last weights calculated at training 01 but with a new data set consisting of different images.
 - *Training 03* continues the training of the neural network, keeping the last weights calculated at training 02 but with a mix of the frames from the first dataset and the second
 - *training 04* continues the training with the weights of 03 but with more pictures of flying bees because it was what the neural network had more difficulties recognizing.

The training 05 were created to compare the results between the different models of YOLOv5:
- Yolov5s is the small model of Yolov5 neural network. It is composed of 214 layers and 7 022 326 parameters.
- Yolov5m is the medium model of Yolov5 neural network. It is composed of 291 layers and 20 871 318 parameters.
- Yolov5x is the large model of Yolov5 neural network. It is composed of 445 layers and 86 217 814 parameters.

Usually, the more parameters the model has, the better the result will be but longest the training will take.
The training has also augmented his data set with frames coming from a new video where the flowers are yellow. The dataset is mixing frames from the different videos.

The *training 06* is doing the same as training 05 but with different augmentation. The aim was to see if using more augmentation, especially The brightest Contrast augmentation, would give better results. 

The *YOLOv8* folder is, in the same way as *training 06* and *training 05*, a comparison of the three models, yolov8s, yolov8m, and yolov8x. Yolov8 is the latest algorithm of the Yolo families. 

The *notebook* folder contains different Google Colab notebooks which was necessary for the project. It also contains an example of notebook *beesdetection* that can be directly opened by clicking on the google collab labels in the readme. This notebook is ready to be run and will do the augmentation, training, validation, and detection. The algorithm will use the files located in the *Yolov5* folder.

*notes* is a folder that was used to identify the different tasks.
 
 ## Dependencies as Python version and package versions used (from Watermark)
Python implementation: CPython

Python version       : 3.10.12

IPython version      : 7.34.0

albumentations        : 1.4.8

opencv-python-headless: not installed

imgaug                : 0.4.0

cv2                   : 4.8.0

yolov5                : unknown

torch                 : 2.3.0+cu121

## Links to other/reused/relevant projects - YOLOv5, Ultralytics, SciCount, SciAugment, Watermark, ...
During this project, a lot of documentation and algorithms were used.

Yolov5: https://github.com/ultralytics/yolov5
augmentations: https://github.com/martinschatz-cz/SciAugment
Ultralytics: https://github.com/ultralytics/ultralytics

## Workflows
To train the neural network for recognizing bees, some steps need to be followed.
### Data Preparation
Firstly, for the neural network to learn, it needs a dataset with pictures of the bees. For it to work correctly, it needs at least 30 frames in input. The best dataset will be composed of different frames with bees buzzing in the flowers or bees flying. It will enable the algorithm to recognize a bee in any condition. If you want to train the neural network based on a video, you will need to cut frames directly from the video. For this, you can use the document called cut_frames (which is in the folder notebook), which will pick some of the frames inside the video you have entered in input. You can change the number of frames you need inside the algorithm, but the default parameters will cut the video into 30 frames

Then, the frames need to be annotated. Indeed, to learn, the neural network needs to know what corresponds to a bee. For this step, you can use the software named makesense.ai. It lets you annotate your frames in a very simple way. If you do not want to create your dataset, you can use the file named frames_04+frames_2.zip in the Yolov5 folder. This file includes the different frames and the YOLO annotations. It can be used, by default, in the notebook beesdetection.ipynb, which is located in the folder notebook. If you have created your data set you will need to change the path of the data set directory in the notebook. Then, the augmentation will generate new images, based on the frames of the data set. Having more images will allow the neural network to have more images to train. In the notebook, only the Default augmentation is done.

### Training
The next step is the training. This will train the neural network to recognize bees on images. 
To launch a training it is better to be connected to a GPU, because it could not work or be very long using only the RAM. The training is launched only by one code line (beginning with *!python train.py* ...). In this line of code, a lot of parameters can be changed if the user wants something specific (you can find them all on the Ultralytics website). However, some parameters need to remain, like img, batch, epochs, data, and weights. 

- **img** is the size of the different images used for the training. Before going into the model, the images are sized to the dimension that has been put in parameters. If little objects want to be spotted, a higher resolution is necessary, so a bigger img.

- **batch** is a parameter that defines the number of samples to work through before updating the model's weights. Using batch -1 will allow the biggest size of batch that your hardware can support.

- **epochs** is a parameter that will define the number of times the neural network will go through the entire dataset. In the notebook, it is initialized as 100, but it won't be enough. If you want to have good results, you will need to put around 800 epochs.

- **data** is a parameter that will need a yaml file. This file consists of writing the number of classes that the network should recognize, but also the path to the validation and training data. In our case, the file is named class1.yaml in the Yolov5 folder.

- **weights** are the initial weights parameters of the model. If you have already done at least one training, you can put the weights of your last training, but if it's your first training, you need to choose between the different models, yolov5s, yolov5m, yolov5x... Those are different sizes of neural networks. Yolov5s is the small one, m is the medium, and x is the large one. Larger models like YOLOv5x and YOLOv5x6 will produce better results in nearly all cases, but they have more parameters, so they require more CUDA memory to train, and are slower to run. So choosing a model really depends on the usage. 

Other parameters exist like device (to specify the number of GPUs used for training), time (which defines the maximum training time in hours), patience, and cache ...

After the training is over, you can launch the validation phase to know how well it worked. The algorithm will do the detection on the dataset and give you the result, so you can see if it can recognize the bees and with which probability. It also gives you a confusion matrix and different graphs to analyze.

### Detecting
The last step is the detection. You can put the path of a video or of different frames that you want to detect. The parameters save-txt and save-conf create for each frame a text file that is composed of all the objects detected. They all respect the same format which is  [class] [x_coordinate] [y_coordinate] [width] [height] [confidence]. With those files, you can launch a new training using the detection, by correcting the mistakes of the algorithm.

### Tracking (optionally)

 ## Results/Comparison (separate issue)
 ## Tracking with Trackmate (separate issue)
 ## Publish to Zenodo at the end (separate issue)

## Acknowledgements

*Computational resources and consultations were provided by the the Vinicna Microscopy Core Facility co-financed by the Czech-BioImaging large RI project  LM2023050. Additional computational resources were provided by the e-INFRA CZ project (ID:90254), supported by the Ministry of Education, Youth and Sports of the Czech Republic.*
