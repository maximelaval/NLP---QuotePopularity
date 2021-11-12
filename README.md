# How structure of a sentence influences its popularity
## Abstract 

People often share their thoughts or opinions to the medias. Some of them, like politicians, do it in order to spread their views and spread influence, therefore their goal is to maximize the amount of people who will hear or read what they say. Our research aims to determine what language-related mechanisms influence sentence's reach. For this, we use Quotebank's data and explore influence of factors on quote's frequency in the media.


## Research Questions
- How features of sentence are related to its popularity?
- Are there any non-sentence related features which may confound our results?


## Proposed additional datasets
We're going to use the following datasets :

### Wikidata 

We want to get the speakers gender, field of occupation, age and net worth. We use this information mainly to include confounding variables directly and remove omitted variable bias from our research.

### Google Trends 
 
Used to get words/speaker popularity. It gives a 0-100 score for a given search topic, in a given period of time and region. Higher number represents higher percentile among all searches. We are looking to account for speaker's popularity, as it is likely to affect quote's popularity.

There are several possible problems we might encounter:
- Number of API calls per hour is limited, so using it on every speaker or quote is not feasible.
- For least popular topics, results are highly volatile for shorter periods of time. 
- Score is comparative, not absolute, so it might be problematic to compare results over time.

Our team has experience with GT, so we are aware of uses and risks. You can see monthly popularity of "Donald Trump" search over several years in (fig. TrumpGT.jpg)


### Wikimedia 

By using their REST API, we are able to retrieve the number of visits on a given page, to use it as estimate of our speakers popularity, as it could be a confounding factor for ones sentence popularity.


## Methods


### Parameters

We first derive a list of paramaters we want to use for our analysis as well as why we want to use these parameters. Some proposed parameters for now are listed below :
  - the humber of words in the quotation, possibly removing connectors words such as conjunctions, determinants and others
  - the average word length
  - the longest word length 
  - the number and type of punctuation signs
  - the number of repeated words
  - the presence of literal numbers  
 We picked these parameters because they seem to be the most obvious and straightforwrard metrics we can compute and compare.
  
 Additionally:
- metadata on our speakers (as mentioned above, used to avoid confounding effects of these)
- keywords or popular words in the sentence (to infer the effects of given subjects)
- does the speaker address the public directly ? (e.g. "People of America \[...]", "My dear fans \[...]", this could possibly increase the scope of reach)
- does the quotation mention other people (could possibly increase the scope of reach)
- general frequencies of the words used in the quotation (................note: i'm not sure i remember why this would be useful for our project)
- the language level : formal/informal, this seems quite hard as it might require some NLP however we could try a "sub-problem" where we would try to detect swear words or scientific words



### Data analysis

For purpose of primary analysis, we have taken a subsample of our data. The 100000 observations subsample of the data are available in the repo. We believe that the main trends and pecularities of this subsample are likely to persist in the full data as this subsample is already very large, and taking a subsample allows us to speed up. Below we summarize the main points of interest in the data subsample.

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

### Models

Once we choose the first set of parameters that we want to use (which might change depending on our first analysis results), we need to choose a model to use.
For purposes of inference, OLS is a solid and classic choice. It also allows us to easily interpret the results. Alternative is using decision tree, as it is also easily interpreted.
For purposes of prediction, nonlinear models such as Neural Networks or Random Forests usually work better. Being able to predict quote's popularity is a useful side result, and these models are likely to be the best.

We will determine best model depending on the results of models in question.

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

K. Faustini - report writing
M. Laval - data handling
A. Khozhenetc - organization
A. Kovrigina - analysis, research idea


## Questions for TAs

1. What model is preferable for inference? OLS is a classic choice, but what would be the best choice, given that we want to interpret the results as well?
2. Is it even useful to create a predictive model? 
3. What data can we use to determine metadata on words, such as whether they are academic or common, or their use frequency?
