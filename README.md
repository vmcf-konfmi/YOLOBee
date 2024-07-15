# YOLOBee

## Introduction
	Les abeilles sont des insectes très importants pour la biodiversité car elles permettent la pollinisation des plantes. La pollinisation est un phénomène naturel très important pour les plantes car elle permet leur reproduction. C’est la raison majeur de l’importance des abeilles sur la planète. Cependant, le nombre d’abeilles diminue drastiquement chaque année. D’après un article sur le journal LesEchos écrit par Charlotte Meyer en 2022, « En Europe, environ 40 % des colonies d’abeilles ont été décimées en moins de dix ans. »  C’est un chiffre très important qui nous impact directement. En effet, l’article explique également que « 75 % de la production mondiale de nourriture dépend des insectes pollinisateurs. » C’est la raison pour laquelle nous nous devons de changer les choses et de favoriser le développement des abeilles. 

	Le projet sur lequel je travaille consiste à connaître avec la trajectoire des abeilles pour comprendre si certaines fleures intéressent plus les abeilles que d’autres, avec un but de pouvoir planter, plus tard des fleurs possédant les caractéristiques favorisant le développement des abeilles.
 
 ## Explanation which folder is for what
 ## Dependencies as Python version and package versions (from Watermark)
 ## Links to other/reused/relevant projects - YOLOv5, Ultralytics, SciCount, SciAugment, Watermark, ...
 ## Workflow (separate issue)
 ## Results/Comparison (separate issue)
 ## Tracking with Trackmate (separate issue)
 ## Publish to Zenodo at the end (separate issue)
## Workflows
To train the neural network for recognizing bees, some steps need to be follow.
### Data Preparation
To begin, for the neural network to learn, you need to have a dataset with some pictures of the bees. Starting from a video, you need to cut some frames to select about 30 frames. The best dataset will be composed of different frames with bees buzzing in the flowers or bees flying. It will enable the algorithm to recognize a bee in any conditions. 

So for getting the frames, you can use the notebook called cut_frames, which will pick some of the frames inside the video you have enter in input. You can change the number of frames you want to have inside the algorithm but the default one will pick for you 30 frames. 

Then, you need to annotate them. Indeed, to learn, the neural network needs to know what is a bee. For this step you can use the software named makesense.ai. This can allow you to annotate you frames in a really easy way. If you do not want to create your own dataset, you can use the file which is name frames_04+frames_2.zip in the Yolov5 folder. This file include the different frames and the YOLO annotations. It can be used by default in the notebook beesdetection.ipynb located in the folder notebook. If you have creating your own dataset you will need to change the path of the dataset directory in the notebook. Then the augmentation will generate new images which are based on the frames of the dataset. This will allow the neural network to have more images to train. In the notebook, only the Default augmentation are done.

### Training
The next step is the training. This will train the neural network to recognize bees on images. 
To lunch a training it is better to be connected to a GPU, because it could not work or be very long using only the RAM. The training is launch only by one code line (begining by !python train.py ...). In this line there are a lot of parameters that you can change if you want someting specific (you can find them all on the ultralytics website). But there is some parameters that you need to have to make it work. You need to have at least img, batch, epochs, data and weights. 

- **img** is the size of the different images used for the training. Before going into the model, the images are sized to the dimension that has been put in parameters. If you want to spot little object, like bees, you will need a higher resolution.

- **batch** is a parameter wich will defines the number of samples to work though before updating the models weights. Using batch -1 will allow the biggest size of batch that your hardware can support.

- **epochs** is a parameter wich will define the number of times you will go through the entire dataset. In the notebook it is set a 100 but it wont be enough. If you want to have good result, you will need to put around 800 epochs.

- **data** is a parameter that will need a yaml file. This file consist of writing the number of classes that the network should recognise, but also the path to the validation and training data. In our case the file is named class1.yaml in the Yolov5 folder.

- **weights** is the inital weights parameters of the model.If you have already done a training you can put the weights of your last training, but if it's your first training you need to choose between the different models,
yolov5s, yolov5m, yolov5x... This are different size of neural network, Yolov5s is the small one, m the medium and x the large one. Larger models like YOLOv5x and YOLOv5x6 will produce better results in nearly all cases, but they have more parameters, so they require more CUDA memory to train, and are slower to run. So the choice of the the model really depend of the usage. 

Other parameters exist like device (to specify the number of GPU used for training), time (which define the maximum training time in hours), patience, cache ...

After the training is finished, you can launch the validation phase to know how well it worked. The algorithm will do the detection on the dataset and give you the result, so you can see if it can recognise the bees and with which probability. It also gives you a confusion matrix and different graph to analyse.

### Detecting
The last step is the detection. You can put the path of a video or of different frames that you want to detect. The parameters save-txt and save-conf create for each frame a text file which is composed of all the object detected. They all respect the same format which is  [class] [x_coordinate] [y_coordinate] [width] [height] [confidence]. With those files, you can launch a new training using the detection, by correcting the mistakes of the algorithm.

### Tracking (optionally)
