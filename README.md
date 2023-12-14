# Predicting Recipe Calories
This is project 5 from DSC80 course by Javier Ponce
## Problem Identification

In this project I will create a model that predicts the calories of a recipe using other features like (number of steps, rating , etc.). This would be done through liner regression since most of my features are numerical and continous it made sense to use linear regression instead of trees. Since we want to predict calories, the response variable would be calories, and I will extractthat information from the nutrition column of the original recipes dataframe. I specifically chose calories because I think it is one of the variables on the recipe data frame that people care the most, a lot of people are interested in controlling their diet and calories is the most common way to keep track of the things you eat. The metric I will be using for this project is RMSE thi is because my model is a linear regression and because contrary to the R squared the RMSE has units that we can interpret.

## Baseline Model	

For my baseline model, I created a polynomial model and I searched for the best degree of polynomial. I used three features. the first one is the number steps on a recipe, my reasoning for including this feature was that more steps could mean that you were cooking more food, This is a quantitative varible and for this model I left it as it came in the dataframe. Then as my second variable I used number of ingridients, under the  logic that each ingridient should add some amount of calories and therefore more ingridients should lead to more calories, this variable is also quantitative. Lastly, my third feature was average rating for this one I had to merge the group ratings by recipe id to the recipe data frame, my resoning was that normally (or at least I) we enjoy more food that are somewhat unhealthy and therefore I reasoned that higher calories should lead to higher ratings.

After creating and fitting my model I don't think this model is the best one because the RMSE seems really high (around 600 calories), remember that the units of the rmse are calories and therefore we can interpreo them and to me 600 seems a big number of calories.

## Final Model	

To improve my baseline model and to find my final model I started by making some scatter plots of the relation between my features and the response variable. Here I realized a few things. The steps X calories scatter plot resembled the sahpe of a curved L and I remembered that in lectured that shape what realted to using log tranformation. Additionally I Transformed my rating column to standard units, this made sense to me because using the regular ratings would assume that a rating of 2 was two times less good that a rating of 4 which is not really true because ratings are subjective to each individual. 

After standarizing rating and getting the logs of steps column I plugged this new features in a pipeline to make a new model. Note that since I had different transformation to different columns I had to use a Columntransformer. Then after fitting my model I make an K fold cross validation to compare the performance of my original polynomial model and this new one. As result of my cross validation my new model resulted better than the baseline one but the RSME is still around the same values (600) only a few units less than the baseline model.

## Fairness Analysis	

Lastly, the fairness analysis. 
For this part of my project I wanted to know if my model was better predicting higher or lower caloric recipes, for this I used the binarizer trasnformed to split my calories column into Recipes with an amount of calories above the mean (high caloric recipes) and recipes with fewer calories than the mean of calories. after this I used my predictior to calculated the RSME of both groups and I discovered that my model is not even closed to fair.

My model had a RMSE of 229 for the low caloric recipes compared to a RSME of 1059 for the high caloric recipe. this by itself does not tells us much, but I decided to make a permutation test to see if this RMSE where something normal. I decided to use a significance level of 0.05 and a null hypothesis of : My Model is fair, against the alternative of : my model has lower RSME for lower caloric recipes. After 1000 permutations none of the simulations was close to the observed RSME getting me a p value of 0.0 and helping me reject the null.


