# How structure of a sentence influences its popularity
## Abstract 
*(143 words)*

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

### Wikimedia 

By using their REST API, we are able to retrieve the number of visits on a given page, to use it as estimate of our speakers popularity, as it could be a confounding factor for ones sentence popularity.

### [Corpus of Contemporary American English (COCA)](https://www.english-corpora.org/coca/)
Only in the 2015-2020 time frame, it would be used during our sentence anylsis of words.


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


### Models

Once we choose the first set of parameters that we want to use (which might change depending on our first analysis results), we need to choose a model to use.
For purposes of inference, OLS is a solid and classic choice. It also allows us to easily interpret the results.
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


## Organization within the team

K. Faustini - report writing
M. Laval - data handling
A. Khozhenetc - organization
A. Kovrigina - analysis, research idea


## Questions for TAs

1. What model is preferable for inference? OLS is a classic choice, but what would be the best choice, given that we want to interpret the results as well?
2. Is it even useful to create a predictive model? 
3. What data can we use to determine metadata on words, such as whether they are academic or common, or their use frequency?
