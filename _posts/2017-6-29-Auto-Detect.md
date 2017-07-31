---
layout: post
title: AutoDetect - Detecting cars with Deep Learning
---

<img src='{{ site.url }}/images/AD_1.png' align='middle' />

Let’s say you are looking to buy a new car in the near future. You go on platforms like eBay Motors or AutoTrader with a huge amount of car offers and you filter by condition, mileage, transmission, location, etc. Imagine you have a platform where you can immediately access used car offers very conveniently by taking a picture of a car on the street and get all the relevant offers with detailed information about it.

<img src='{{ site.url }}/images/ebayMotors1.png' align='middle' />

So for my final three weeks project, I was able to combine my little experience in iOS development with my knowledge in machine learning I acquired over the last 12 weeks. My goal for this project was to create a real world application that makes it easy to find out the make, model and eventually individual information about the car like average price of a used car or individual damages.

Here’s a quick overview over the whole process:
<img src='{{ site.url }}/images/AD_3.png' align='middle' />

<b>Data Acquisition</b>

First of all, I wanted to acquire enough images to make it the modeling process as easy as possible. One of the biggest platforms for used cars is eBay Motors with over 60,000 cars. Since most of the offers have up to ten images I had to decide how many images I need. For 96 out of 100 car offers, the first three images are images of the exterior and also show the car in convenient angles.

<img src='{{ site.url }}/images/ebayMotors2.png' align='middle' />

Via eBay’s API I downloaded the first three images of each car which amounted to about 170,000 car images in total. In the beginning, I had 2,700 different car models partly because many car model names were not consistent. Also, with having only limited time for this project I had to reduce the amount of classes to realize a viable image classification model.
As a guideline, it is recommended to have at least 30 images for each class in a image classification model. After removing car models with less than 30 images and normalizing car model names like ‚Audi TT 2.0‘ and ‚Audi TT TDI‘, I ended up with 468 classes.

<b>Modeling</b>

For building the underlying technology, I used a method within the Deep Learning space called transfer learning. A common task is to first train an image classification model with the ImageNet Challenge data set, and then transfer this model’s knowledge to a distinct task like classifying car models.
After preparing, processing and cleaning my data I used Keras with TensorFlow as the backend to build my image classification model. I chose to use the pre-trained neural network InceptionV3 as a base model, extend it with my own neural network and fine tune it on all the 170,000 car images. Instead of using all of the Inception classes (1,000 objects classes like trees, people, animals, etc.), my own neural network’s output are probabilities for only 468 classes.

<img src='{{ site.url }}/images/AD_2.png' align='middle' />

The way transfer learning works is that you first fit and train your model only with the extended layers by freezing all the convolutional layers of the InceptionV3. After the top layers are well trained, we can start fine-tuning some of the convolutional layers of the InceptionV3. Here, we only trained the model with the last 50 layers.
In general, a neural network tries to detect edges in the earlier layers, shapes in the middle layers and high level data specific features in the later layers.
In my case, I had enough images for the model to learn how a car looks like, hence, I only needed the later layers to extract the very specific features of my car images. Each image gets resized to 224x224 pixels and turned into a matrix of numbers.
Additionally, I used image augmentation to improve the model’s performance by rescaling, zooming in, ZCA whitening and horizontal flipping each image. This step in combination with balancing out the number of images of some car models helped me to prevent my model from overfitting.
<b>Performance</b>

After struggling with class imbalance, over- and underfitting, I am pretty happy with the results AutoDetect achieves. Over 468 classes, the right car model is 93% of the times in the top three predictions (Accuracy: 90% on training set &  80% on validation set). I think this really shows how powerful deep learning can be if you have enough labeled data.

<img src='{{ site.url }}/images/AD_4.png' align='middle' />

To put this technology to use I created an iOS 11 app with Swift and Apple’s only recently released machine learning library, CoreML (released at the beginning of June), which makes it easy to convert Keras models and implement them in the iOS environment.

<b>Demo of the app</b>

<iframe width="560" height="315" src="https://www.youtube.com/embed/_CNRriH_qtA" frameborder="0" allowfullscreen></iframe>

<b>Future works</b>

Since I didn’t have enough time to finish my idea of a mobile platform for used cars, there are definitely a couple of things I want to work on:
- Show location-based offers
- Include price averages of these offers
- Improve the interface of the app
- Release the app in the next iOS 11 release this fall


<img src='{{ site.url }}/images/AD_5.png' align='middle' />

<b>Code</b>
If you are interested in the code visit my <a href="http://www.github.com/Flowinger/AutoDetect">GitHub repo</a>.

