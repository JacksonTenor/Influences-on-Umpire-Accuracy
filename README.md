# Additional Parameters Influencing Human Strikezones

## Goals

The objective of this project is to determine what factors other than position influence an umpire's accuracy when calling balls and strikes. I plan to look at factors such as inning and batter's handedness that should not influence the data but possibly does as well as factors such as batter's height that should influence ball/strike calls.

## Methods

I have created multiple models based on a pitch's x and y coordinates as they cross the plate in order to set up a baseline for accuracy. I then will predict data using this model and find the subsets that are more or less accurate. Finally, I will add the factors that most directly influence the data as explanatory variables for a final model.

## The data

The pitch data was collected and published by major league baseball. It has been collected into nice csv files on kaggle (https://www.kaggle.com/pschale/mlb-pitch-data-20152018). Pitches included are from the 2015-2018 seasons. Additionally this dataset includes seperate csvs containing data containing information about the atbat (ie inning, batter, pitcher) and game (ie umpire, weather). I have joined the data from the pitch csv with other data to 

## Results

While the initial models predicted balls and strikes with a solid accuracy, I was unable to find additional parameters that significantly improved performance. Batter handedness, inning, and home/away batter had very little difference in model accuracy, and adding them to the models improved performance at best marginally.

Umpires, however, do have varying levels of accuracy. I was able to use the model to find umpires that were better or worse at calling strikes. However, even upon creating a model that added the umpire as an explantory variabele, the accuracy increased only marginally.

Overall, it appears that umpires are, in the end, pretty good at their jobs.

## Baseline Models

Using only the positional data as the ball crosses the plate, I created four models to predict whether the pitch would be called a ball or a strike. The first, a logistic regression, had an underwhelming performance. The second, a nearest neighbors model, performed quite well. This performance was matched by both of the nerual networks I trained. 

After evaluating these four models, I took the confidences of one neural network and the nearest neighbors model and averaged them to create a final model, although this step similarly did not improve accuracy.

Given that many of these models have topped out at around a 91.5% accuracy, I infer that given these explanatory paramters, this level of accuracy is near the limit we can get due to variance in umpire calls.

Each of these models is tested on a retained portion of the data set equal to 20% of the total data to avoid overfitting and ensure accurate results.

### Logistic Regression

#### Process:
I began with a simple logistic regression. It was a simple, although relatively unsuccessful starting point. I first tried giving it px and pz values, but the model just predicted everything to be a ball. However, when giving it distance from the center of the observed values it was able to find success.

#### Results:
In the end I was able to create a moderately successful classifier with 88.7% accuracy by using the distance from center as an explanatory variable.

#### Remaining Questions:
There is no guarentee the mean of the px and pz values is the best center point for this regression. It would be possible to try measuring distance from a variety of points and seeing which would perform the best, but I belive other models will outperform even the most optimized logistic regression for this problem.

### K Nearest Neighbors

#### Process:
As an attempt to create a better model than the logistic regression, I decided to use a K nearest neighbors model. Immediately I was getting success above 90%, but to maximize the model's accuracy I looped through different values for n_neighbors to find the optimal one, which ended up being around 95. I also tried weighting based on distance, which had minimal impact on the model. 

#### Results:
The end models were extremely successful with 91.5% for the unweighted model and 91.2% for the distance weighted model.
This is quite good!

#### Remaining Questions:
I believe if I spent as much time tweaking the distance weighted model as I did the non-weighted model it could perform as well or better. I also would be interested if there are any meaningful differences in the confidences of these two models
The first model is a simple fraction over 95, but I would believe this one could be different.

### Neural Networks

#### Process:
For my neural nets I used relu as the activation function. The documentation for this package reccomended using the 'adam' solver for larger datasets, so I utilized that. I trained a net with two hidden layers with 64 neurons each as well as a larger one with three hidden layers of size 128.

#### Results:
Both of these models peformed similary to eachother and to the KNN model. This leads me to believe that, given this data set and these parameters, the best we may be able to do is around 91.5%.

As one would expect, these take a while to train, so use of the pickle package is highly reccomended here.

## Non-Location parameters and their impact on the models

### Batter Handedness

I predicted batter handedness to be one of the most influential factors. However, to my surprise, the accuracy of our model was not significantly different for lefties or righties and adding handedness to the nearest neighbors model only improved it marginally.

### Home/Away

While I don't expect there to be a significant difference in calls for the home or away team, finding a difference in this area would be problematic, so I decided to check. Fortuantely, based both on the lack of difference in the model's accuracy on home team at bats vs away team at bats as well as the model's lack of improvement when considering the half of the inning indicates that this is not something to worry about.

### Inning Number

I predicted that inning number would influence an umpire's accuracy as umpires would tend to start games strong and progressively get worse as a game goes on. This, however, appears to be false, as there was no signficant difference in accuracy depending on inning and the model did not improve with inning as an additional paramter.

### Umpire

It should not be surprising that the largest influence on the accuracy of balls and strikes is the umpire themselves. The list of umpires, accuracy, and number of pitches called shows more variation in accuracy than any of the models to this point have.

However, in spite of this, adding umpire as an explanatory variable to a neural network failed to significantly improve performance.
