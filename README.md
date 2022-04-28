# Using Yolo in smart parking

YaCui,https://github.com/cuiya01/smart-parking-in-deep-learning.git

## Introduction

- Maintaining empty parking spot count using YOLO real-time vehicle detection. Code readily runnable in google colab. Due to occlusions (coming due to the presence of mirror in the middle of camera and parking lot which slightly reflects nearby people passing through), low resolution of video and positioning of cars at different angles in the parking lot and limitations of yolo, it cannot detect every car in all the frames and hence the count fluctuates. Please don't mind the wrong number of empty spots, I didn't count them before running the program since my focus was to check whether the count fluctuates or not.
- Last semester I built a small smart parking with an ultrasonic sensor dht-22, but this seemed too expensive for most large car parks, as well as not being easy to maintain. After that assignment I often thought about how to improve and optimise this project. It wasn't until this semester, when I studied deep learning, that I discovered that I could use one or more cameras instead of a sensor, and at the same time ensure a great deal of accuracy.

## Research Question

As society grows and more and more families own cars, road traffic becomes more congested and it becomes more difficult to find a suitable parking space, often entering a car park that is no longer available and having to leave, which can take a lot of time. In car parks where there are only a few empty spaces, users also spend a lot of time looking for a parking space, which can lead to traffic congestion in the car park.

This is why the real-time detection of the number of empty parking spaces in a car park will be a great solution to many of the worries of people's lives.

## Application Overview

#### The entire system consists of：

1, a network of top view cameras whose views when combined will cover the entire area car park.

2, A cloud server and database to hold information such as the occupancy status of all parking spaces parked in the car park and the time stamps of each vehicle entering and exiting

3, a web application that will display all relevant and necessary information. A detailed description of each module of the system is given below.

#### Top view camera network

The top view cameras provide a bird's eye view of the entire car park. The view from all these cameras combined will cover the entire car park. rpi cameras are also selected for the top view, but they fit into a wide angle lens. The information from these cameras is used to identify the occupancy status of each slot, perform feature extraction from the top view and track vehicles until they are parked.

## Data

There is no AI (Artificial Intelligence) without IA (In-formation Architecture) . Having a good all-round data set is very important for building good AI solutions. In our proposed solution we have performed object detection as well as object classification and hence two different types of datasets are required. They are discussed in the section below.

An open source dataset is available for parking lots , called PKLot consisting of 12, 417 images of parking lots and 695, 899 images of parking spaces segmented and perspective transformed. This database contains 12,417 images (1280X720) captured from two different parking lots (parking1 and parking2) in sunny, cloudy and rainy days. The first parking lot has two different capture angles (parking1a and parking 1b).The images are organised into three directories (parking1a, parking1b and parking2). Each directory contains three subdirectories for different weather conditions (cloudy, rainy and sunny). Inside of each subdirectory the images are organised by acquisition date.

Secondly, in order to better train the model for video detection, we searched for videos of car parks on google and also selected a few good quality videos to train the model on.

Describe what data sources you have used and any cleaning, wrangling or organising you have done. Including some examples of the data helps others understand what you have been working with.

When doing yolo annotations, sometimes we need to eliminate annotations that are incorrect or exceed a certain range.

For example, if we want to exclude annotation boxes where the centre point is distance from the edge of the image, I have created a file called data cleaning to do c b hu li for this case.

## Model

I chose yolo as my model architecture，YOLOv3 (You Only Look Once, Version 3) is a real-time object detection algorithm that identifies specific objects in videos, live feeds, or images. YOLO uses features learned by a [deep convolutional neural network](https://viso.ai/deep-learning/deep-neural-network-three-popular-types/) to detect an object. 

- Compared to other models, yolo loses some of its accuracy while guaranteeing speed, but since I only need to detect whether the site is empty or not, I don't need too much accuracy.

![image-20220428135236987](/Users/yacui/Library/Application Support/typora-user-images/image-20220428135236987.png)

Compared to other models, yolo is more suitable for my project because yolo.

- YOLO detects objects very quickly. Because there is no complicated detection process, just feed the image to the neural network and get the detection result, YOLO can complete the object detection task very fast. 
- YOLO is very good at avoiding background errors that produce false positives. 
- YOLO can learn the generalised features of the object.

I choose to run my code on top of colab because google colab doesn't need to be installed, which saves me a lot of time. Again, there are many other benefits to using colab, such as

- The provision of a free Jupyter notebook environment.
- Comes with pre-installed packages.
- being fully hosted on the Google Cloud.
- no need for the user to set up on a server or workstation.
- the Notebook is automatically saved in the user's Google Drive.
- providing a browser-based Jupyter notebook.

## Experiments

I chose PKLot Dataset as my dataset to train on the data. I compared the learning rate of 1e-2 with the learning rate of 1e-3, where mainly gloU,Objectness,Classification,Precision ,Recall,mAP.

With deep network training, the learning rate is one of the most important influencing factors, so I chose to adjust its learning rate to see how it affects the results. By adjusting this learning rate, I found that these two learning rates did not have a particularly significant effect on the final results.

![5171651078842_.pic](/Users/yacui/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/b3c5f942a09578fa6d1181152ebf7dad/Message/MessageTemp/79612bee89ee4499d780ee956633ea85/Image/5171651078842_.pic.jpg)

<center>1e-3</center>

![image-20220428141634274](/Users/yacui/Library/Application Support/typora-user-images/image-20220428141634274.png)

<center>1e-2</center>

## Results and Observations

Throughout the project, I found that the training data was particularly important in helping the model to improve its accuracy, and that the learning rate was rather less important in comparison. This is because by tuning the learning rate, I did not find any significant impact on the resultant output.

In the future, I may try to connect the Raspberry Pi camera to get real-time data and test the model again. It is also important to store the data in the cloud, like mqtt for example. This will help us visualise the data and produce data that will make it easier to manage the car park. For example, the number of cars entering and leaving the car park each day and how long different spaces are occupied and how much they are used.

## Bibliography

*If you added any references then add them in here using this format:*

1. A. Thakallapelli, S. Ghosh and S. Kamalasadan, "Real-time frequency based reduced order modeling of large power grid," 2016 IEEE Power and Energy Society General Meeting (PESGM), 2016, pp. 1-5, doi: 10.1109/PESGM.2016.7741877.
2. Bura, H., Lin, N., Kumar, N., Malekar, S., Nagaraj, S. and Liu, K. (2018). An Edge Based Smart Parking Solution Using Camera Networks and Deep Learning. [online] IEEE Xplore. Available at: https://ieeexplore.ieee.org/abstract/document/8457691?casa_token=RJx-VcpnydQAAAAA:jwE91ykSzBPCBynoKUvF_4HLqbPhad5d39sCMdetZcy2oeoV95ItCcRtDXQsq8j40Mgn26lPVA [Accessed 28 Apr. 2022].
3. Levy, J.M., Irvin-Erickson, Y. and La Vigne, N. (2017a). A case study of bicycle theft on the Washington DC Metrorail system using a Routine Activities and Crime Pattern theory framework. Security Journal, 31(1), pp.226–246.

----

## Declaration of Authorship

I, Ya Cui, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

*Digitally Sign by typing your name here*

![image-20220428150741282](/Users/yacui/Library/Application Support/typora-user-images/image-20220428150741282.png)

ASSESSMENT DATE
