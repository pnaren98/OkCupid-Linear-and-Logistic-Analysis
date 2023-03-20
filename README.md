# OkCupid-Linear-and-Logistic-Analysis
Uses linear and logistic regressions to find and examine trends in the characteristics of OkCupid users.
# Let’s get started!
From the start, this sample is biased due to it only having data on Okcupid users, and so it cannot be used to make generalizations about Americans as a whole. The bias will increase as the analysis goes on. I will try to list as many of the obvious biases as I can at the end of this Readme.
# 1-3
We start with choosing the proper subset of the data. The variables this analysis will primarily focus on are:
 - relationship status
 - sex
 - occupation
 - body type
 - education
 - height
 - age
 - income

To make the data processable we remove every row with incomplete values in any of the above characteristics, which includes rows where income is listed as “-1” (a substitute for NA). Unfortunately, this means reducing the length of the sample from 59946 to 10032, leading to even more bias (for instance, people with low incomes or characteristics they believe are embarrassing may choose not to list them.)

Calling a description of the numeric data reveals some significant outliers, which we remove by limiting the sample to rows where age, income, and height are within 3 standard deviations of the original mean. This cuts the sample size further to 9323.
# 4-6
Next we set up binary variables. The one perhaps most intriguing for a sample from a dating site is whether or not the person in question has been successful in finding a partner (whether or not he is taken). The unique entries in the ‘status’ column include single, seeing someone, available, married, and unknown. We set the “taken” dummy to 1 if the entry is ‘seeing someone’ or ‘married.’ This doesn’t necessarily mean ‘attractive,’ since someone with the ‘available’ option might have had many partners, but simply doesn’t have one at the moment, and some single people might just not have spent enough time on the site.

After this, we set up binary variables for the unique values in ‘job’ as well as sexual orientation, body type (coerced into thin, large, average, and athletic), and whether the person in question has graduated from a four year college at least. Finally we set up a binary for male (as opposed to female) and divide the sample into two subsets for each sex. The subsets will be further divided into train and test sets.
# 7-12
First we start with some simple linear regressions. The first set uses the male subset, and uses income as an independent variable and the taken status as a dependent variable- to see if income makes it more likely for a man to find a partner. The initial regression suggests exactly the opposite, with the likelihood of finding a partner decreasing with income. However, a closer look at the model’s accuracy suggests that it should be taken with a grain of salt, with a near zero R2 as well as graphs of the intersection of predictions and actual results suggesting almost no correlation.

We try another linear regression between two continuous variables: age and income (do older people on OkCupid tend to have higher incomes?) This model has a higher R2, but not by much. For the rest of the analysis we will abandon linear regressions in favor of logistic ones.
# 13-17
Logistic regressions are “classifiers,” meant to forecast the results of binary variables, so they could be more useful in this case. Once again, a simple logistic regression predicts that income reduces the odds of finding a partner. But a closer look at the regressions’ performance metrics does not do the model any favors. According to the confusion matrix, the model never predicts an affirmative result for the taken variable- only successfully predicting a negative result. This is the case for both men and women. It appears that according to regression models at least income has a very low correlation with finding a partner. 

The next three cells create functions to find the differences between the overall sample and a sample of only people with partners. There is too much bias to detect any trends that can be applied to real life, so this is just to be used as an exercise.

# 18-23
The rest of the analysis tries to identify trends between a different set of variables, with athletic as the dependent dummy variable. Supposedly, people with higher incomes would have more time and money to afford to train and condition. But initially logistic  regressions do not yield anything useful. The confusion matrices show that two out of the three regressions in cell 18 only predict affirmative results (and most of the time inaccurately), while the other only predicts negative. 

Adding more variables improves the analysis. When analyzing both sexes, it appears that with an above 50% accuracy the chances of an OkCupid user being athletic increases with income and decreases with age and height. This trend is the same for both men and women exclusively.

Next, we try normalizing the numeric data, or transforming it into a “standard scale” from around -1 to around 1, so that larger numbers such as income does not skew the regression results. The same logistic regressions after scaling the data are all as accurate, if not slightly more, but yield somewhat different coefficients. Now athleticism trends upwards with height, and trends upwards with age as well for women.

# Closing notes
The bias in this analysis are too many to count. Several obvious ones include:
 - The base sample consisting only of OkCupid users rather than something more general like Census data.
 - The sample being limited to mostly complete entries without numeric outliers.
 - The statistic of “seeing someone” or “married” being mistaken for meaning “attractive” or “able to find a partner.”
 - The age, height, and income variables being endogenous (influencing each other before influencing the dependent variable.)

Overall, this analysis is meant to be used as more of a demonstration of machine learning techniques on a public dataset. I would not advise any readers to draw conclusions from the results of the regressions in this code.

Source: https://www.kaggle.com/datasets/yashsrivastava51213/okcupid-profiles-dataset
