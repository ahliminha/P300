# P300

## Introduction

[P300](https://en.wikipedia.org/wiki/P300_(neuroscience)) is a signal measured in the Brain that occours after a stimulus. The signal can be measured by an Electroencephalography Device and analyzed by classification algorithms. In this study you will find a **Neural Network Approach** to this problem.

The signal in one electrode is something like this below:
![The signal may seem like this:](/images/indice.jpg)

## Donchin Machine

[Emanuel Donchin](https://www.researchgate.net/profile/Emanuel_Donchin) is a famous researcher in the field of P300 analysis, focusing his work in extract neurofeedback from this signal. One that became very famous is about a speller machine. The concept is:
- A screen with a virtual keyboard is displayed
- A EEG device is set on the user
- A computer is connected to both devices
- Is asked too the user to think about one letter and look to the screen
- Blink randomly rows and columns
- Measure the tension signal in the occipital lobe (since its a visual signal)
- Pre-process the signal
- Predict the positive P300 signal 
- Find the desired letter


![Speller keyboard](/images/speller.png)

## How does te detecction process works?

Usually, people around the world try to use ensemble od Linear Methods, such as Support Vector Machines, to Classify the signal, almost always using some kind of feature extraction. 

Especially in this work I've tried something different. As the signal is acquisited in 8 different electrodes, there is some spatial information in the signal and I wanted to use this. So, my approach was:
- Get the timeseries of potential from each simuli in each electrode
- Put them all side by side in a 351x8 array
- Pass the signal over a 2-D Convolutional Network (like the image ones)
- Output a binary classification 

### Why this approach?
In Data Driven systems the last thing we want to do is to throw data away, so, in my research as Undergraduate me and my teacher, [Romis Attux](http://www.dca.fee.unicamp.br/~attux/) tried to not to waste the spatial information in the analysis. Such as the Feature Extraction used to do, selecting the "best" electrode

## Data

The data used is from [a Kaggle Dataset](https://www.kaggle.com/rramele/p300samplingdataset). The data contains:
- All the raw data from the tension measured in the electrodes
- Rows that a simuli is started
- Time of the simuli duration
- Label
- Row/Column Lighted

Each one of these for 35 letters from 8 individuals separately.

## Pre-Processing

All the pre-processing must fit some criteria. First, we don't want to vanish the signal using filters. As you may know, the raw data comes with other brain signals composed, and it's not from our interest to analyze them in our network. So, as P300 is a time-landed signal, with low frequence, a low bandpass filter is a suitable choice for filtering. It was used a [butterwhorth filter](https://en.wikipedia.org/wiki/Butterworth_filter). After this, data must be split into time-series accordingly to the potential. P300 signals are evident 300ms after the stimulus, besides that, the "fall" and the "pre" peak are so important as the main peak. A window of 1 second, here it's 352 timestamps, is suitable for the analysis.

## Training

Putting all the data in the model to train, validate and test becomes after. In each commit available in the git you can see all the different architectures I've used so far, check it out! The idea here was simple:

We want to know if there are any advantages in using [transfer learning](https://en.wikipedia.org/wiki/Transfer_learning) to one individual. The TL process here is:
- Get data from all individuals minus one and train the network
- Split the data of the last and train intending to fine tune parameters
- Analyze the performance of the network in the last individual
- Compare results with the network trained in all individuals data

### Why Transfer Learning?

There is one problem when it comes to P300 based Computer-Brain Interfaces (BCI) that is training the classifier. If we get the same performance with less data in a fine tunning process, the final user gets less bored trying to finish the process started in the factory. Improving user's experience.

Other point here is the capability of generalyze Brain Information. If the transfer learning process goes well, we cen affirm that, in some way, we can model this signal for any situation and just fine tune this to each person instead of overfit the classifier to one person alone. 

## Results

Results were exciting. In P300 usual approaches, mean over a lot of signals are done intending to get better data. Another way to do more reliable measures is to repeat the process to achieve a higher accuracy comparing a set of "trials", in BCI world a trial is a repeat of all rows/columns exciting to find the desired letter.

Here we got an accuracy over ~75% to each time-series individually, without any means or repeat. Of course, this is a initial result, must be done further study in an [OpenBCI machine](https://openbci.com/).

![OpenBCI](/images/openbci-1280x720.jpg)

## Requirements
You may need:
- Mat4py
- Pandas
- Numpy
- Keras
- Matplotlib

## Enviornment

I strongly advice you to run this notebook on [Google Colab](https://colab.research.google.com/notebooks/intro.ipynb#recent=true). It will erase you a lot of time setting everything up. Just remind to ut your data somewhere there or in your drive to use it.

You can find my final report of the research in this repository (in Brazilian Portuguese)

Feel free to reach me out in my [gmail](lucascosta816@gmail.com) about any mistakes or improvements. 