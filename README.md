# How should you structure your sentences in order to increase their reach?
*(Is it too precise?)*
## Abstract 
*(143 words)*

Prominent persons often want to share their thoughts or opinions to the medias, with the goal of maximizing the amount of people who will hear or read what they say. Open any newspaper or news website and you will find quotations from some prominent persone and often this is done with a clear desinre to convince or rally people to their cause. This analysis aims to determine what language-related mechanisms seem to improve ones sentence's reach. More specifically, it investigates how the popularity of a quotation is related to its structure and some specific content. In this context, popularity is defined as the number of times a given quotation is referred in a given dataset.  
To achieve such goal, we're going to analyse the Quotebank dataset from 2015 to 2020 and extract structure related features that can be quantatively compared. 

## Research Questions
- Are sentences intrinsic features related to their popularity and if so, can we measure their effect ?
- Are sentences extrinsic features related to their popularity and if so, can we measure their effect ?
- Should one focus on *what* they say more than on *how* they say it ? (extrinsic vs intrinsic)
- Can we maximize our sentence's reach by optimizing its structure ?
- Are the answers to these questions different based on the theme of the sentences ?
- 

## Proposed additional datasets
We're going to use the following datasets :

### Wikidata 

We want to get the speakers gender, field of occupation, age and net worth, mainly to enrich our speakers' data but also so that we can then split our analysis on these categories and try to derive causal or correlation links. 

### Google Trends 
 
Used to get words/speaker popularity on some parameters (date, region...). It uses a 0-100 metric and the format looks like `speaker = {date:metric, date:metric}`. We keep in mind however that there are some limitations enforced by google : limited number of search, probably unavailable data for some speakers present in the QuoteBank dataset.

### Wikimedia 

By using their REST API, we are going to retrieve the number of visits on a given page, mainly we will use it to have a rough estimation of our speakers popularity as it could be a confounding factor for ones sentence popularity.

### [Corpus of Contemporary American English (COCA)](https://www.english-corpora.org/coca/)
Only in the 2015-2020 time frame, it would be used during our sentence anylsis of words.

### Other datasets 

These datasets are noth exhaustive yet and \[...]

## Methods

We first derive a list of paramaters we want to use for our analysis as well as why we want to use these parameters. We placed them in two categories : easy and not easy, based on if we can get these parameters simply by manipulating the quotation itself or if we need some external tools or data. Some proposed parameters for now are listed below :
- **Easy** 
  - the humber of words in the quotation, removing connectors words such as conjunctions, determinants and others
  - the average word length
  - the longest word length 
  - the number and type of punctuation signs
  - the number of repeated words
  - the presence of litteral numbers  
 We picked these parameters because they seem to be the most obvious and straightforwrard metrics we can compute and compare.
    
- **Not easy**
  - metadata on our speakers (mentioned above, could be used for statistics or to limit confounding)
  - keywords or popular words in the sentence (to infer the effects of given subjects)
  - does the speaker address the public directly ? (e.g. "People of America \[...]", "My dear fans \[...]", this could possibly increase the scope of reach)
  - does the quotation mention other people (could possibly increase the scope of reach)
  - general frequencies of the words used in the quotation (................note: i'm not sure i remember why this would be useful for our project)
  - the language level : formal/informal, this seems quite hard as it might require some NLP however we could try a "sub-problem" where we would try to detect swear words or scientific words

Once we choose the first set of parameters that we want to use (which might change depending on our first analysis results), we need to choose a model to use, we have a few ideas but we're not sure what to use - more on this in the "Questions for TAs" rubric.
We're going to use either \[...]

## Proposed timeline

\[...]

## Organization within the team

\[...]


## Questions for TAs

1. We're not totally sure about what model we should, OLS sound nice and easy but given the number of parameters perhaps a Neural Net would be better ?


*(791 words as of this commit)*
