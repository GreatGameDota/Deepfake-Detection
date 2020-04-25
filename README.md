# Deepfake Detection Challenge

My 69th place solution and writeup for the [Deepfake Detection Challenge](https://www.kaggle.com/c/deepfake-detection-challenge) hosted on Kaggle by AWS, Facebook, Microsoft, the Partnership on AI’s Media Integrity Steering Committee and others.

![](https://github.com/GreatGameDota/Deepfake-Detection/blob/master/assets/logo.jpg)

## Initial Thoughts

Once again an amazing Kaggle competition that I had a ton of pleasure participating in! I will like to thank Kaggle and the hosts for putting together this competition and working hard to make sure it went as smooth as it did! This was another great computer vision competition that taught me how to handle imbalanced data aswell as the value of having a good training dataset.

One very unique thing about this competition was the large amount of data which totaled 470+ GB of video files. It was a challenge for me since I don't have anything close to handle that amount of data whether it be locally or in the cloud. However I was able to overcome this challenge by taking advantage of Kaggle's kernals which are able to hold the competition's data easily. I was also able to create my own datasets on Kaggle to hold all the individual images of the deepfake faces. More on this later.

## Overview

My final solution was an ensemble of 7 models: 2x Xception and 5x Efficentnet-B1. All models are pretrained with similar heads. They were all trained in Kaggle kernals using a version of a group of Kaggle datasets made by myself and [Hieu Phung](https://www.kaggle.com/phunghieu) (see below). [Pretrained Mobilenet](https://www.kaggle.com/unkownhihi/mobilenet-face-extractor-helper-code) was used as face extractor (special thanks to [Shangqiu Li](https://www.kaggle.com/unkownhihi) and team for sharing this method!). To balance the data while training I used a simple undersampling technique and I randomly sample 1 frame from every video while training. All models were trained slightly differently (training dataset version, hidden size, LR, augmentation, etc.).

## Model

![](https://github.com/GreatGameDota/Deepfake-Detection/blob/master/assets/model.png)

All models had the above architecture except for one that had a hidden layer size of 256 instead of 512 (See submission.ipynb).

All model's pretrained weights were loaded from [Pytorchcv](https://github.com/osmr/imgclsmob). All models used Binary CrossEntropy loss. One model used mixup/cutmix during training.

A fun fact is that the training method and model architectures are barely modified from my public baseline Kaggle kernal which can be found [here](https://www.kaggle.com/greatgamedota/xception-classifier-w-ffhq-training-lb-555)!

## Input and Augmentation

Input was one of the most difficult challenges in the competition due to the size amount and how the images of the faces should be.

I just wrote out my entire journey dealing with image datasets and realized it was way to long. So long story short I made my own image dataset which can be found [here](https://www.kaggle.com/c/deepfake-detection-challenge/discussion/134420)!

In the dataset I pulled images of people's faces from 10 frames from every video. Then I would randomly sample one of those images from a specific video and train the model based on video ids. All images had a resolution of 150pxx150px.

For augmentation, some models I had HorizontalFlip along with DownScale and JpegCompression (as suggested in a Kaggle forum) and for other models I just had HorizontalFlip.

## Training

Training was simple for this competition and the only problems were the unbalanced data and having an accurate validation set. All models were trained with Adam optimizer with low initial LR, ReduceLROnPlateau, and for 20 epochs. Best model checkpoint was saved based on LogLoss validation. All models were trained in Kaggle kernals.

For validation I did a simple folder-wise split validation where folders 0-40 were used to train and folders 41-49 as validation. (Some of the final models used 40-49 as validation)

## Ensembling/Blending

I ensembled all the models using the geometric mean of their probabilities.

## Final Submission

![](https://github.com/GreatGameDota/Deepfake-Detection/blob/master/assets/scores.png)

## What didn't work

- LRCN (CNN + RNN)
- External data
- Post-processing/Clipping

## Final Thoughts

This was another great Kaggle competition! I'm so happy I achieved such a great performance on a competition that a lot of people just skipped due to the amount of data. I'm also surprised I performed so well with only Kaggle kernals!

I'm so excited to get my second ever competition medal and I'm even more excited because it is my first silver medal! I now graduate to Kaggle competition expert!

My previous competition: [Bengali Character Classification](https://github.com/GreatGameDota/Bengali-Character-Classification)

My next competition: [placeholder]

## Some helpful Links

My baseline kernal: https://www.kaggle.com/greatgamedota/xception-classifier-w-ffhq-training-lb-537  
My submission kernal: https://www.kaggle.com/greatgamedota/69th-place-solution-mobilenet-inference/  
Image dataset kernal: https://www.kaggle.com/greatgamedota/deepfake-detection-full-data  
