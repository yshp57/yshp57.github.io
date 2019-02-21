---
layout: post
title:      "Project 1 - Feature Selection"
date:       2019-02-21 04:43:27 +0000
permalink:  project_1_-_feature_selection
---

When doing my first Data Science project, I was having some trouble with selecting features for my model. After scrubbing my data and getting dummy variables for my categorical features, I was left with 91 (!) variables. Modeling with all of them would have way overfitted my model, allowing me to predict my data accurately, but not any unknown. To deal with this, I would have to impliment some kind of Feature Selection process to pick the more important variables, allowing my model to ignore the other information and only calculate based on the variables that account for meaningful relationships.

### Attempt 1 - RFE (Recursive Feature Elimination)

I first tried to use RFE to select my features. RFE is a function in sklearn that trains our model on all of our data and determines which feature is the least important, based on the coeficient given for the given feature in the resulting linear model. It then drops that feature and trains the model on the remaining features, again dropping the feature with the lowest coeficient. It continues doing this until we are left with only the n most important features, n being a parameter we give the function. To decide how large n should be, we can loop RFE through the total range of functions and look at how the R Squared value changes as we increase the number of features. At a certain number of features, the R Squared value begins to slow it's rise, and it is that many features that we want for our model. Any more than that and we risk overfitting our model!

However, one drawback to using RFE is that we can only accurately compare coefficients when each feature is scaled. Otherwise we risk deeming one feature more important than the other, simply because it's units are higher. At first, this wasn't a problem as I had min-max scaled my data. However, as I continued with the project, I decided that I wanted to prioritize the interpretability of my model over it's accuracy, and therefore decided against min-max scaling, which would have made interpreting my results more complex. I instead had to find another function to help with selecting my features.

### Attempt 2 - Stepwise Selection 

I settled on a stepwise selection for my final model. In the first step of a Stepwise Selection, the first "forward step," the function selects the feature with the lowest p-value in relation to our target and, if it is lower than a threshold in  parameter, adds it to the list of selected features. After each forward step, the function performs a "backward step" in which all features that have been selected are assessed as to whether their p-value as part of the model is higher that a set threshold out parameter of the function. The function continues with alternating forward and backward steps until there are no more parameters that have a p-value lower than the threshold in paramter that are not in the function, and no features with a p value above our threshold out parameter left in our function. Again, we can  iterate through the range of the number of selected features to identify where the R Squared value slows its increase to determine how many features we should use in our model. This worked despite not being scaled, because unlike the coefficient, the p-value of a feature is independant of it's scale. 

My hope is that by using this technique, I was able to select the features that I needed without  sacrificing the interpretability of my model. 
