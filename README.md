# How does the features of a sentence influences its popularity

## Link for the data story 
https://kevinfaustini.github.io/pop-sentences/

## Abstract 

People often share their thoughts or opinions to the medias. Some of them, like politicians, do it in order to spread their views and spread influence, therefore their goal is to maximize the amount of people who will hear or read what they say. Our research aims to determine what language-related mechanisms influence a sentence's reach. For this, we use Quotebank's data and explore the influence of simple factors on quote's frequency in the media.


## Research Questions
- Does the basic intrinsic features of a sentence influence its popularity ?
- If so, which features are most related to its popularity?
- Are there any non-sentence related features which may confound our results?


## Additional datasets
We used the following datasets :


### Google Trends 
 
Used to get words/speaker popularity. It gives a 0-100 score for a given search topic, in a given period of time and region. Higher number represents higher percentile among all searches. We are looking to account for speaker's popularity, as it is likely to affect quote's popularity. 

We use Google Trends specifically for the analysis of popular quotes, as such we take a sub-sample of the most popular quote, where we define "popular" as "has over 1000 citations". From this we get 290 unique speakers but note that this is by not exhaustive, indeed we encountered a few issues with the dataset : as expected, because the number of API calls per hour is limited, we couldn't obtain as much data as we hoped, so the number of unique speakers is as high as we could retrieve in 2 days using 3 proxies. For correct inference, we would need unique an identifier for each person, or regroup under one ID all references to the same person, however considering Quotebank have different entities for the same person (such as President Donald Trump and Donald Trump), we leave this issue for further research. On top of this, Google Trends has really strict request requirements and doesn't process non-ASCII characters, which makes things difficult for non-english speakers. 

We use the monthly popularity of the speakers as well as the average popularity from 2012 to the quotation date as this is a period over which Google Trends data is stable but still relevant.

## Methods

### Parameters

Here are the parameters we use as well as the reason why : 

- `numOccurrences` the number of times a quotation appears in our dataset
- `numWords` the number of words in the quotation
- `averageWordLength` the average word length
- `largestWordLength` the longest word length 
- `numOfPunctuation` the number of punctuation signs
- `numRepeatedWords` the number of repeated words
- `numNumbers` how many times a literal number appears in the quotation (however considering how highly insignificant this feature is, we omit it)
 We pick these parameters because they seem to be the most obvious and straightforward metrics we can compute and compare.
  
Additionally:

- we try all second order interaction terms
- `SpeakerPop` the speaker popularity according to Google Trends



### Data analysis
#### Initial Data Analysis (M2)
For purpose of primary analysis, we have taken a subsample of our data. The 100000 observations subsample of the data are available in the repo. We believe that the main trends and peculiarities of this subsample are likely to persist in the full data as this subsample is already very large, and taking a subsample allows us to speed up. Below we summarize the main points of interest in the data subsample.

1. Histograms.

The first step in data analysis is a glance on each variable distribution by plotting histograms. The first thing to notice is that there are ouliers in each variable, and this makes the historgams hard to read. We can clearly see, however, that all the variables have a right-skewed distribution (fig. LogOcc.jpg), except the the average word length with comparatively symmetrical distribution (fig. WordLength.jpg). Commonly, skewness is dealt with by using log transforms.

2. Descriptive statistics

To investigate the variables distribution further, we look at the table with the descriptive statistics (table Summary.csv). It is obvious that for each variable the maximum value is much larger than the 75th and even 99th percentiles. There are several ways to determine outliers and influential observations, including dffits and added-variable plot, but we utilise the simple rule that any observations that are more than 1.5 interdecile range below 10th percentile or more than 1.5 interdecile range above 90th percentile are outliers, as it is more feasible given the large data we have (table Summary_noout.csv).

3. Correlation 

The aim of our study is to analyse the determinants of the popularity of a quotation. Here we measure popularity with the number of occurences of a quotation. First, if we look at the correlation of the number of occurences with each parameter (without outliers), we spot the following (table Corr_noout.csv):

- there is a negative correlation between the number of words and the number of occurences, so it may be the case that shorter quotations tend to be more popular;
- there is a positive correlation between the average word length and the number of occurences, so it may be the case that quotations with longer words tend to be more popular;
- there is a negative correlation between the number of punctuation signs and the number of occurences, so it may be the case that the quotations with more simple yet not too short sentences tend to be more popular;
- there is a negative correlation between the number of repeated words and the number of occurences, so it may be the case that the quotations with more diverse words tend to be more popular.
	
Second, when the distribution looks like many instances of small values and several instances of large values, it is useful to do a log-transformation of variables. When we look at the log-transformed variables correlation (without outliers), the sign of the correlation is preserved as described above. Note that even though the absolute value of the correlation is relatively close to zero, it is still high enough for such a large sample, which paves the way for our future research.

The scatter plot for the number of occurences vs number of words (fig. OccWords.jpg) and the number of occurences vs the number of punctuation signs (fig. OccPunctuation.jpg) suggest that there is a non-linear relationship. Concluding note is that our sample is large, so correlations are likely to lead to statistically significant variables.

4. Different subsamples. 

We have checked that if we look at the instances of very high (more than 99th percentile) number of occurences (including outliers) and compare its correlations with other variables (table Corr_large_occ.csv) with that for the subsample with removed outliers (table Corr_noout.csv), we see that the signs of several correlations differ:

- there is a positive correlation between the average word length and the number of occurences in the sample without outliers, but it is negative for the sample with highly popular quotations only, so there may be the non-linearity in this relationship;
- there is a very small correlation coefficient for the no-outliers subsample between the largest word length and the number of occurences, but in the high-popularity subsample this correlation coefficient is large by the absolute value and negative, suggesting that it may important not to use too long words if you want your quotation ti become extremely popular.

Note that at this stage of our analysis, we did not make the very detailed analysis of what happens when we have extremely large values of our variables, because it deserves a profound research and modelling, which is out of scope now and be done for the next project milestone.

#### In depth data analysis (M3)

1. Preliminary remarks
   - Because the number of occurrences of a quotation increases over time, we should consider old data, however because trends tend to change and because we want to gain insights and results meaningful in today's context, we should rather consider recent data. As such, we decide to use data from 2017 which seems to be a good compromise. Note that we only use one year for two reasons, the first one being the fact that this is enough data for the purpose of our analysis and the second one being that we are greatly limited by our equipment or technology at our hands (we don't have powerful machines and Colab doesn't allow us much resources).
   - Our analysis really focus on large scale data as well as on the effect of _simple, intrinsic_ features (the ones we detailed earlier). Future research could include some more complex features, like the ones described [here](https://www.mdpi.com/2076-3417/10/20/7285/pdf) for example.
   - We remove outliers as well as quotations which occur only once because it means Quotebank only has the original (i.e. this quote hasn't been quoted anywhere else as far as Quotebank can tell us). 
   - Because we really want to focus on explanation rather than prediction, we do not make use of cross-validation. Of course, with further research on classification and more complex features, we would definitely cross-validate our results.
   - Despite using several more or less complex models, we get low R² values everywhere, which means none of our models explain the data well based on our features selection. Hence a future interesting research could be to develop our own model to describe quotations complexity, similar to what is done [here](https://aclanthology.org/2021.semeval-1.76.pdf).

2. Usage of the whole sample 
As said earlier, we first pre-process the data in a classic fashion and then describe it to observe an expected result : the data is heavily skewed, very few quotations are cited more than hundreds of time while a huge amount are quoted less than 5 times. Indeed, only 10% of our sample has more than 14 references and the mean is equal to 10 while the median is 3. Only 1% of our sample has more than 110 citations. This already indicate it might be hard to find meaningful results.
As suggested by the data outlook, we use a log scale and perform the regression again. We don't get significantly different results.

3. Restriction to popular quotes only
The data is heavily skewed which makes our analysis difficult, hence we decide to restrict our analysis to popular quotes only. Here we defined 'popular' as quotes which have more than 100 occurrences. We observe a much smaller (more than half) MSE compared to the full sample (0.4 vs 0.9).

4. Feature engineering
As mentioned in the parameters section, we also make use of second order interactions. We first decide to try all of them, and while some of these features are significant, they don't contribute that much : the MSE we get is 0.001 lower than the one before. Then we try with another model, from the XGBoost library which we will detail later. Note this model is usually better suited for classification however it's still quite good for regression purposes. We obtain a MSE which is 0.01 lower than previously (0.373), that is, slightly better than with least squares but not enough for us to conclude that more complex nonlinear models explain the data well enough. We kept the second order interactions for our classification task with XGBoost so that we're consistent in our analysis but also to address possible cases of complex nonlinear relationships. Note also that we can have more degrees of freedom without stability problems because of our large sample size.

5. Classification
Using the same definition of popular as before, we take a sample with as many "popular" quotes as "unpopular" ones. We build the classifier and try to predict "popular" quotes with it. Next, we follow the classification approach. If our goal is to predict whether the quote will be popular or not, we can consider binary classification, and respective models. We take quotes which have more than 100 occurences as Popular, and try to predict the probability of being popular. First, we use classic logit approach. To avoid problems of sample balance, we take all Popular quotes and same number of others. Results are no different from before - we don't really predict well enough. XGB is one of the best classifiers in the field, but performs only marginally better.

6. Google Trend data addition
Popularity of a speaker might be an important feature. To access it, we use Google Trends - via pytrends library which uses Google API to access popularity of a search term. We are strongly restricted on number of calls, but manage to obtain a sample of about 300 people. We add this feature to our analysis, and repeat it. Even though our perfomance improved and the popularity is highly significant, we do not see any significant breakthrough, with R^2 still as low as previously.


### Models

For purposes of inference, OLS is a solid and classic choice. It also allows us to easily interpret the results. Hence this is what we use for most of our analysis. We also use XGBoost.
For purposes of prediction, we use [a nonlinear model](https://docs.getml.com/latest/api/getml.predictors.XGBoostRegressor.html) from the XGBoost library which uses a mix of techniques such as Random Forest and Gradient Boosting. This is a [well known and acclaimed](https://en.wikipedia.org/wiki/XGBoost) classifier model which usually outperforms other models in practice. 


## Proposed timeline

(P2 start)
Formulation of research questions
Construction of research plan
Construction of data handling pipeline
Specification of methods
Data exploration
From results, re-iteration of steps above.

(P3 start)
Re-iteration of steps above using feedback from TA
Research of additional semantic features which can be useful, based on other research done in this field.
Collection of additional data, clean and prepare it for research.
Collection of resulting set of features, and build models.
From results of previous step:
   explore anomalies, effects of outliers, etc.
   introduce new features
   reiterate several times
Interpret results, give conclusions
Create a prediction model for quote popularity

Note: according to data description, there are total of 178m quotes. 
We process 5.2m in 2600sec, so we need approximately 25 hours to process everything.


## Organization within the team

K.Faustini - data story  
M.Laval - data handling  
A.Khozhenetc - modelling, organization  
A.Kovrigina - data analysis, research idea  
