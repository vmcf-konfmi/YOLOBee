[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.12919127.svg)](https://doi.org/10.5281/zenodo.12919127)
# YOLOBee

## Introduction

Bees are essential insects for biodiversity, as they pollinate plants. Pollination is a vital natural phenomenon for plants, enabling them to reproduce. This is the main reason why bees are necessary on the planet. However, bee numbers are declining drastically every year. According to an article in the newspaper LesEchos written by Charlotte Meyer in 2022, "In Europe, around 40% of bee colonies have disappeared in less than ten years."  This is a huge number and one that has a direct impact on us. Indeed, the article also explains that "75% of the world's food production depends on pollinating insects." This is why we need to change things and encourage the development of bees. 

The project I'm working on involves learning about bee's trajectories to understand whether certain flowers are more attractive to bees than others. Thanks to this, we will understand better bees' behavior and their needs. The experimentation involves 20 3D-printed flowers arranged in lines of 5. The flowers are 3D-printed to enable the control of all the parameters: for the flowers to be the same color and shape but diversify their height. 

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

*videos* got three videos which are use for the detection step.
 
 ## Dependencies as Python version and package versions used (from Watermark)
Python implementation: CPython

Python version       : 3.10.12

IPython version      : 7.34.0

albumentations        : 1.4.8

opencv-python-headless: not installed (but needed for SciAugment)

imgaug                : 0.4.0

cv2                   : 4.8.0

yolov5                : 7.0.13 (based on PIP)

torch                 : 2.3.0+cu121


## Links to other/reused/relevant projects - YOLOv5, Ultralytics, SciCount, SciAugment, Watermark, ...
During this project, a lot of documentation and algorithms were used.

Yolov5: https://github.com/ultralytics/yolov5

augmentations: https://github.com/martinschatz-cz/SciAugment

Ultralytics: https://github.com/ultralytics/ultralytics

## Workflows
To train the neural network for recognizing bees, some steps need to be followed.
### Data Preparation
Firstly, for the neural network to learn, it needs a dataset with pictures of the bees. For it to work correctly, it needs at least 30 frames in input. The best dataset will be composed of different frames with bees buzzing in the flowers or bees flying. It will enable the algorithm to recognize a bee in any condition. If you want to train the neural network based on a video, you will need to cut frames directly from the video. For this, you can use the document called `cut_frames` (which is in the folder notebook), which will pick some of the frames inside the video you have entered in input. You can change the number of frames you need inside the algorithm, but the default parameters will cut the video into 30 frames

Then, the frames need to be annotated. Indeed, to learn, the neural network needs to know what corresponds to a bee. For this step, you can use the software named [makesense.ai](https://www.makesense.ai/). It lets you annotate your frames in a very simple way. If you do not want to create your dataset, you can use the file named `frames_04+frames_2.zip` in the Yolov5 folder. This file includes the different frames and the YOLO annotations. It can be used, by default, in the notebook `beesdetection.ipynb`, which is located in the folder notebook. If you have created your data set you will need to change the path of the data set directory in the notebook. Then, the augmentation will generate new images, based on the frames of the data set. Having more images will allow the neural network to have more images to train. In the notebook, only the Default augmentation is done.

### Training
The next step is the training. This will train the neural network to recognize bees on images. 
To launch a training it is better to be connected to a GPU, because it could not work or be very long using only the RAM of the system. The training is launched only by one code line (beginning with `!python train.py` ...). In this line of code, a lot of parameters can be changed if the user wants something specific (you can find them all on the Ultralytics website). However, some parameters need to remain, like img, batch, epochs, data, and weights. 

- **img** is the size of the different images used for the training. Before going into the model, the images are sized to the dimension that has been put in parameters. If little objects want to be spotted, a higher resolution is necessary, so a bigger img.

- **batch** is a parameter that defines the number of samples to work through before updating the model's weights. Using batch -1 will allow the biggest size of batch that your hardware can support.

- **epochs** is a parameter that will define the number of times the neural network will go through the entire dataset. In the notebook, it is initialized as 100, but it won't be enough. If you want to have good results, you will need to put around 800 epochs.

- **data** is a parameter that will need a yaml file. This file consists of writing the number of classes that the network should recognize, but also the path to the validation and training data. In our case, the file is named class1.yaml in the Yolov5 folder.

- **weights** are the initial weights parameters of the model. If you have already done at least one training, you can put the weights of your last training, but if it's your first training, you need to choose between the different models, yolov5s, yolov5m, yolov5x... Those are different sizes of neural networks. Yolov5s is the small one, m is the medium, and x is the large one. Larger models like YOLOv5x and YOLOv5x6 will produce better results in nearly all cases, but they have more parameters, so they require more CUDA memory to train, and are slower to run. So choosing a model really depends on the usage. 

Other parameters exist like device (to specify the number of GPUs used for training), time (which defines the maximum training time in hours), patience, and cache ...

After the training is over, you can launch the validation phase to know how well it worked. The algorithm will do the detection on the dataset and give you the result, so you can see if it can recognize the bees and with which probability. It also gives you a confusion matrix and different graphs to analyze.

### Detecting
The last step is the detection. You can put the path of a video or of different frames that you want to detect. The parameters save-txt and save-conf create for each frame a text file that is composed of all the objects detected. They all respect the same format which is  [class] [x_coordinate] [y_coordinate] [width] [height] [confidence]. With those files, you can launch a new training using the detection, by correcting the mistakes of the algorithm.

 ## Results/Comparison (separate issue)
All the data we will analyze are in the folder validation of each training. In those folders, we will be interested, especially in the confusion matrix and the mAP50-90. The confusion matrix is a table that allows us to know the performance of an algorithm in machine learning. It classifies the output data into four categories: the true positive, the false positive, the true negative, and the false negative. We can see examples of confusion in the matrix below.

  Training_00              |  Training_02              | Training_03 and Training_04
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/vmcf-konfmi/YOLOBee/blob/main/training_00/validation/confusion_matrix.png)   |  ![image](https://github.com/vmcf-konfmi/YOLOBee/blob/main/training_02/validation/confusion_matrix.png) | ![image](https://github.com/vmcf-konfmi/YOLOBee/blob/main/training_03/validation/confusion_matrix.png)

The first figure shows the confusion matrix for different Training. Training 03 and Training 04 have the exact confusion matrix. Training 00 has 94% true positives which is a good result for a first training. This high result can be explained because the dataset were consisted of easy frames. Easy frames contain bees on flowers but not a lot of bees flying. The second training was initialized with the weights of the last training but with a new set of data that only has bees flying. So the results were much more complicated because the neural network had difficulties detecting flying bees. Also, because the algorithm has a new set of data, it forgets the previous dataset. This explains why the second training only has 80%  true positives. Training 03 was a mix of the dataset used in the first training and the second training. This explains the better result. Training 04 had more frames than Training 03 but it did not increase the number of true positives.

To understand the interest of Training 04 compared to Training 03, we can observe the mAP50 and mAP50-95 between the two trainings. The mAP50 corresponds to the mean average precision calculated at an intersection over union (IoU) threshold of 0.50. In the same way, map50-95 corresponds to the average of the mean average precision calculated at varying IoU thresholds, ranging from 0.50 to 0.95. This means that for different thresholds (going from 50 to 95 generally by 0.05 step), the algorithm calculates the mean average precision. To obtain the real value of map50-95, the average of the different thresholds is calculated. map50-95 gives a better vision of the algorithm and its performance.

We can observe the performances of Training_03 and Training_04:
|               | Training_03   | Training_04   |
| ------------- | ------------- | ------------- |
| mAP50         | 0.968         | 0.974         |
| mAP50-95      | 0.661         | 0.709         |

We can observe that Training 04 has better mAP values than training_03. The values are pretty good and the difference is not big enough to see a difference in the confusion matrix. However, thanks to the mAP values, we can see that the different frames added for the training_04 were useful. 

Then, we can concetrate on the comparision of the diffenrent yolov5 models. Indeed, the training_05 and 06 are comparing the model s, m and x. Both training 05 and 06 owns the same frames at the beginning. The difference between the two training is the augmentation. Training 06 have more augmentations, so a bigger dataset for the training step. The result of the validation are exposed below. The training ran until an early stop was applied. You can find specific number of epochs for each model size in the Jupyter Notebooks.  

|               | Training_05_s   | Training_05_m   | Training_05_x   | Training_06_s   | Training_06_m   | Training_06_x   |
| ------------- | --------------- | --------------- | --------------- | --------------- | --------------- | --------------- |
| mAP50         | 0.974           | 0.975           | 0.974           | 0.98            | 0.981           | 0.981           |
| mAP50-95      | 0.569           | 0.615           | 0.638           | 0.727           | 0.745           | 0.729           |

Thanks to this value, we can see a huge improvement between training 05 and training 06. Indeed, between Training_05_s and Training_06_s, there is a 0,158 difference, which is a lot for the exact frames at the beginning of the algorithm. We can conclude that having a larger dataset helps bee tracking because the algorithms have more data to train on, but also, one of the augmentations that was added for this training is BrightContrast. For the bee's detection, this augmentation is essential because the frames have shadows and the bees are dark, so training the neural network to detect bees where contrast is missing is necessary.  We can see that training 06 does not always have better results than the other training. Indeed, it could be that the database is too small to change all the parameters. So maybe increasing my database could be an option for getting better results than training m and s. 

To run a training, a connection to the GPU is necessary because the CPU is not powerful enough. Indeed, the RAM of the system in Google Collab is 12,7 Giga Byte and is under the minimum resources for the training. This is why a GPU is necessary. While comparing the different models, we noticed a big time difference during the training, between the size of the model used and the size of the dataset. For training x, it is better to use two GPUs to ensure the end of the training and that it will not take too much time. 

To continue, we can observe the difference between the yolov5 and yolov8 models. The latest version of Ultralytics is the Yolov8, which went out in 2023, while Yolov5 came out in 2020. Between the two versions, the results are practically the same, even if Yolov8 seems a little less efficient for the m and x models. But the results are still pretty good. Yolov8 is a larger neural network, so maybe the dataset is not big enough. It is supposed to be better but slower because of the size of the neural network. You can find more details in the Jupyter Notebooks contained in the YOLOv5 and YOLOv8 folders.

After training the neural network, we wanted to observe the trajectory of the bees. Thanks to the detection, we could retrace the position of each bee in each frame. In the images below, we can see all the positions taken by the bees in the different videos located in the folder *videos*.

  video 1              |  video 2              | video 3
:---------------------:|:---------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/d33dac18-4c5b-4525-9610-81d813bae68b)|  ![image](https://github.com/user-attachments/assets/771122b1-28ac-4b1d-98ec-c2b79c173290) | ![image](https://github.com/user-attachments/assets/5520dd19-b6ea-4b06-94d3-3bcd2efbeb38)

With those pictures, we can see that the tracking seems to work. But we can not conclude anything with these three short videos. Now the detection is working, it could be launched on a longer video to have realistic results. 


 ## Tracking with Trackmate
After training the neural network to recognize bees, the objective is to retrace their movement. To do this step, we used a software called trackmate. This software needs for each frame, a mask to know where the bees are. All the masks are created with the file `masks_creation` in the folder notebook. This file contains the coordinates of the bounding boxes created in the detection step. This file is separated into two parts, tracing the center on a frame and mask creation. The first part can draw on an image of the movement of the bees on a video. The second is creating for each frame, a mask where the center of the bees is white on a black background. We also needed the time between two consecutive frames. This data was calculated in the `cut_frames` file, located in the same folder as `masks_detection`. The first result was not satisfying because the bee's detection was not good enough to track the movement of each bee. We modified the detection of the video `video_2_bees_extract_1.mov` by lowering the confidence threshold from 0.5 (by default) to 0.25. Decreasing the threshold allowed the detection of more bees. The video created by the detection is below.

https://github.com/user-attachments/assets/4846c61f-3bca-4d2d-a753-282aa09ea5f9

We can see in this video that the bees are pretty well detected even if sometimes the algorithm does not recognize them. This could be because of the shadow or the flower sick. Indeed, both decrease the contrast between bees and the background, making the detection more complicated. This could be interesting either to change the color of the sick to a brighter color or to increase the dataset with more frames of bees in shadow or in front of a flower stick.
With this detection, we did the tracking step again and the result was better. We can see the result below.

https://github.com/user-attachments/assets/5a769c93-d469-4d00-8f1d-b32abe34b3a1

The bees correspond to the white square and the line corresponds to their movement. The color of the line corresponds to the speed of the bee. The Warmer the color is, the faster the bees will move. This tracking can let us see the movement of each bee, to better understand on which flower bees are stopping. Unfortunately, the algorithm is not completely efficient because we can see at 7 seconds of the detection video that 2 bees are switching their flowers. However, we can't see it in the second video. This is because the algorithm has difficulties detecting the bees which are close to each other.

 
## Zenodo Repository

Jeannin, C., & Schätz, M. (2024). vmcf-konfmi/YOLOBee: YOLOBee 1.0.0 (v1.0.0). Zenodo. [https://doi.org/10.5281/zenodo.12919127](https://doi.org/10.5281/zenodo.12919127)

```
@misc{https://doi.org/10.5281/zenodo.12919127,
  doi = {10.5281/ZENODO.12919127},
  url = {https://zenodo.org/doi/10.5281/zenodo.12919127},
  author = {Jeannin,  Chloé and Sch\"{a}tz,  Martin},
  language = {en},
  title = {vmcf-konfmi/YOLOBee: YOLOBee 1.0.0},
  publisher = {Zenodo},
  year = {2024},
  copyright = {Affero General Public License v1.0 or later}
}
```

## Acknowledgements

*Computational resources and consultations were provided by the Vinicna Microscopy Core Facility co-financed by the Czech-BioImaging large RI project  LM2023050. Additional computational resources were provided by the e-INFRA CZ project (ID:90254), supported by the Ministry of Education, Youth and Sports of the Czech Republic.*
