# Ensemble Methods

## Introduction

In this lesson, we'll learn about **_ensembles_** and why they're such an effective technique for supervised learning. 

## Objectives

You will be able to:

- Explain what is meant by "ensemble methods"
- Explain the concept of bagging as it applies to ensemble methods 


## What are ensembles?

In Data Science, the term **_ensemble_** refers to an algorithm that makes use of more than one model to make a prediction. Typically, when people talk about ensembles, they are referring to Supervised Learning, although there has been some ongoing research on using ensembles for unsupervised learning tasks. Ensemble methods are typically more effective when compared with single-model results for supervised learning tasks. Most Kaggle competitions are won using ensemble methods, and [much has been written](https://blogs.sas.com/content/subconsciousmusings/2017/05/18/stacked-ensemble-models-win-data-science-competitions/) about why they tend to be so successful for these tasks. 

### Example 

Consider the following scenario -- you are looking to invest in a company, and you want to know if that company's stock will go up or down in the next year. Instead of just asking a single person, you have the following experts available to you:

1. **_Stock Broker_**: This person makes correct predictions 80% of the time  
2. **_Finance Professor_**: This person is correct 65% of the time  
3. **_Investment Expert_**: This person is correct 85% of the time  

If we could only take advice from one person, we would pick the Investment Expert, and we can only be 85% sure that they are right.  

However, if we can use all three, we can combine their knowledge to increase our overall accuracy. If they all agree that the stock is a good investment, what is the overall accuracy of the combined prediction?

We can calculate this by multiplying the chances that each of them are wrong together, which is $ 0.2 * 0.35 * 0.15 = 0.0105\ error\ rate$, which means that our combined accuracy is $1 - 0.0105 = 0.9895$, or **_98.95%_**!  

Obviously, this analogy is a bit of an oversimplification -- we're assuming that each prediction is independent, which is unlikely in the real world since there's likely some overlap between the things each person is using to make their prediction. We also haven't calculated the accuracy percentages for the cases where they disagree. However, the main point of this example is that when we combine predictions, we get better overall results. 


## Resiliency to variance

Ensemble methods are analogous to "Wisdom of the crowd". This phrase refers to the phenomenon that the average estimate of all predictions typically outperforms any single prediction by a statistically significant margin -- often, quite a large one.  A Finance Professor named Jack Treynor once demonstrated this with the classic jar full of jellybeans. Professor Treynor asked all 850 of his students to guess the number of jellybeans in the jar. When he averaged the guesses, he found that of all the guesses in the class, only one student had guessed a better estimate than the group average. 

Think back to what you've learned about sampling, inferential statistics, and the Central Limit theorem. The same magic is at work here. Estimators are rarely perfect. When Professor Treynor asked each student to provide an estimate of the number of jellybeans in the jar, he found that the estimates were normally distributed. This is where "Wisdom of the crowd" kicks in because we can expect the number of people who underestimate the number of jellybeans in the jar to be roughly equal to the number of people who overestimate the number of jellybeans. So we can safely assume the extra variance above and below the average essentially cancel each other out, leaving our average close to the ground truth value! 

Consider the top-right example in this graphic that visually demonstrates high variance:

<img src='https://raw.githubusercontent.com/learn-co-curriculum/dsc-ensemble-methods/master/images/new_bias-and-variance.png' alt="four targets showing low and high variance on the x-axis and lot and high bias on the y-axis" width="600">

Most points miss the bullseye, but they are just as likely to miss in any direction. If we averaged all of these points, we would be extremely close to the bullseye! This is a great analogy for how ensemble methods work so well -- we know that no model is likely to make perfect estimates, so we have many of them make predictions, and average them, knowing that the overestimates and the underestimates will likely cancel out to be very close to the ground truth. The idea that the overestimates and underestimates will (at least partially) cancel each other out is sometimes referred to as **_smoothing_**.  

### Which models are used in ensembles?

For this section, we'll be focusing exclusively on tree-based ensemble methods, such as **_Random forests_** and **_Gradient boosted trees_**. However, we can technically use any models in an ensemble! It's not uncommon to see **_Model stacking_**, also called **_Meta-ensembling_**, where multiple different models are stacked, and their predictions are aggregated. In this case, the more different the models are, the better! This is because the more different the models are, the more likely they have the potential to pick up on different characteristics of the data. It's not uncommon to see ensembles consisting of multiple logistic regressions, Naive Bayes classifiers, Tree-based models (including ensembles such as random forests), and even deep neural networks!  

For a much more in-depth explanation of what model stacking looks like and why it is effective, take a look at this great [article from Kaggle's blog, No Free Hunch!](http://blog.kaggle.com/2016/12/27/a-kagglers-guide-to-model-stacking-in-practice/)


## Bootstrap aggregation

The main concept that makes ensembling possible is **_Bagging_**, which is short for **_Bootstrap Aggregation_**. Bootstrap aggregation is itself a combination of two ideas -- bootstrap resampling and aggregation. You're already familiar with bootstrap resampling from our section on the Central Limit theorem. It refers to the subsets of your dataset by sampling with replacement, much as we did to calculate our sample means when working with the Central Limit theorem. Aggregation is exactly as it sounds -- the practice of combining all the different estimates to arrive at a single estimate -- although the specifics for how we combine them are up to us. A common approach is to treat each classifier in the ensemble's prediction as a "vote" and let our overall prediction be the majority vote.  It's also common to see ensembles that take the arithmetic mean of all predictions, or compute a weighted average. 

The process for training an ensemble through bootstrap aggregation is as follows:

1. Grab a sizable sample from your dataset, with replacement 
2. Train a classifier on this sample  
3. Repeat until all classifiers have been trained on their own sample from the dataset  
4. When making a prediction, have each classifier in the ensemble make a prediction 
5. Aggregate all predictions from all classifiers into a single prediction, using the method of your choice  

<img src='https://raw.githubusercontent.com/learn-co-curriculum/dsc-ensemble-methods/master/images/new_bagging.png' alt="flowchart of input sample being split into several bootstrap samples, then building several decision trees, then aggregation">

Decision trees are often used because they are very sensitive to variance. On their own, this is a weakness. However, when aggregated together into an ensemble, this actually becomes a good thing!

## Summary

In this lesson, we learned about what constitutes an **Ensemble**, and how **Bagging** plays a central role in this. In the next lesson, we'll see how bagging is combined with another important technique to create one of the most effective ensemble algorithms available today -- **_Random forests_**!
