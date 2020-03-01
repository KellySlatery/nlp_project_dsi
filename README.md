# Project 2 - Ames Housing Data and Kaggle Challenge

Kelly Slatery
US-DSI-10
01.31.2020

## Project Directory
```
project-3
|__ code
|   |__ 01_Data_Collection.ipynb  
|   |__ 02_Data_Cleaning_and_exploration.ipynb  
|   |__ 03_NLP_Parsing.ipynb  
|   |__ 04_Preprocessing_and_Modeling.ipynb  
|   |__ 05_Model_Evaluation.ipynb  
|__ data
|   |__ submissions.csv
|   |__ comments.csv
|   |__ clean_beatles_subs.csv
|   |__ clean_beatles_coms.csv
|   |__ clean_queen_subs.csv
|   |__ clean_queen_coms.csv
|   |__ beatles_subs.csv
|   |__ beatles_coms.csv
|   |__ queen_subs.csv
|   |__ queen_coms.csv
|   |__ data_dictionary.csv
|__ project_3_presentation_kellyslatery.pdf
|__ README.md
```


## Problem Statement

The 1960's bore witness to a music revolution like no one had ever seen before--multitrack recording and music video clips started to spread and rock & roll went mainstream, all alongside a rise in national U.S. pop culture. The Beatles broke the charts with the formation fo their legendary group in 1957, while Queen dared to challenge norms after their formation in 1970. The popularity and quality of these two groups is often compared due to their record-breaking album sales, dedication of their fanbases, iconic ground-breaking tracks, and co-membership of the rock music genre. Normally, it is a question of criteria and subjective opinion to determine which of these groups could be considered "the greatest band". However, in this notebook, we will answer the question of who is really the better band with one criterion: love of their fans to this day. Which fanbase loves their band more? And how can we use classification models to determine which band receives more love from their fanbase to end this discussion once and for all?



## Executive Summary

In this project, we explore data collected through Reddit's Pushshift API to classify posts to either the Beatles or Queen subreddit. The data analyzed consists of the most recent 20,000 posts (submissions) from either subreddit as of Tuesday morning January 28th. As the request limit for Reddit's Pushshift API is 200 requests per minute (with 500 posts/submissions per request), all of the data for this notebook was collected without issue in under a minute. 

In order to classify posts, some initial data cleaning and NLP parsing were required before beginning to model using both Count Vectorizers and TFIDF (Term Frequency Inverse Document Frequency) Vectorizers. Seven different classification models were tested using various parameter search methods and all but one with both type of vectorizer to determine which combination of vectorizer, model type, and hyperparameters yielded the most accurate model.

In the end, the Support Vector Machine (SVC), Random Forest, and Logistic Regression yielded the most accurate predictions. However, SVC was inefficient and Random Forest was more overfit, whereas Logistic Regression proveded fairly accurate predictions (accuracy ~= 85.53% ; baseline accuracy ~= 50.6%) and a more interpretable model. Thus, the Logistic Regression model using the TFIDF Vecotrizer were the transformer/model of choice to classify posts/submissions and answer our problem statement.

After running the Logistic Regression model, it was found that amongst the top 20 predictors for both Beatles and Queen, almost all were band member or album names, or important years. Thus, we looked to the next top 20 predictors for either band and found few more interesting words. After looking at all predictors of the Beatles subreddit vs. predictors of the Queen subreddit, a trend appeared: Beatles subreddit predictors tended to include more words associated with success/popularity (e.g. album(s), cover(s), official, Spotify, ranking), while Queen subreddit predictors tended to include more positive words (e.g. amazing, special, quality, love, enjoy).



## Data Dictionary

As described above, the data here comes from two subreddits: [Beatles](https://www.reddit.com/r/beatles/) and [Queen](https://www.reddit.com/r/queen/). In accordance with time limits (200 requests/min at 500 submissions/comments per request), 20,000 submissions and 20,000 comments were pulled from each subreddit. However, only submissions data was used for analysis in this project. Data types and manipulations are described below:

|  	| Column Name 	| Data Type 	| Modifications 	| Description 	|
|----	|--------------------------------	|-----------	|-----------------------------------------------------------------------------------	|---------------------------------------------------------------------	|
| 0 	| author 	| object 	| None 	| Author of post/submission 	|
| 1 	| author_flair_text 	| object 	| None 	| Non-ascii parts of author username 	|
| 2 	| created_utc 	| int64 	| None 	| Time post was created in UTC 	|
| 3 	| score 	| int64 	| None 	| Aggregate sum of upvotes and downvotes (no negatives) 	|
| 4 	| selftext 	| object 	| Nulls replaced with '-' 	| Body of post/submission 	|
| 5 	| subreddit 	| int64 	| Binarized 	| Subreddit to which the post/submission belongs (1=Beatles, 0=Queen) 	|
| 6 	| title 	| object 	| Nulls replaced with '-' 	| Title of post/submission 	|
| 7 	| author_full 	| object 	| None 	| Combined: 'author' and 'author_flair_text' 	|
| 8 	| all_text 	| object 	| None 	| Combined: 'selftext' and 'title' 	|
| 9 	| tokenized_selftext 	| object 	| Text data: tokenized with regular expression 	| Tokenized 'selftext' 	|
| 10 	| tokenized_title 	| object 	| Text data: tokenized with regular expression 	| Tokenized 'title' 	|
| 11 	| tokenized_all_text 	| object 	| Text data: tokenized with regular expression 	| Tokenized 'all_text' 	|
| 12 	| lemmatized_tokenized_selftext 	| object 	| Text data: tokenized with regular expression, lemmatized with WordNetLemmatizer() 	| Lemmatized 'tokenized_selftext' 	|
| 13 	| lemmatized_tokenized_title 	| object 	| Text data: tokenized with regular expression, lemmatized with WordNetLemmatizer() 	| Lemmatized 'tokenized_title' 	|
| 14 	| lemmatized_tokenized_all_text 	| object 	| Text data: tokenized with regular expression, lemmatized with WordNetLemmatizer() 	| Lemmatized 'tokenized_all_text' 	|
| 15 	| stemmatized_tokenized_selftext 	| object 	| Text data: tokenized with regular expression, stemmatized with PorterStemmer() 	| Stemmatized 'tokenized_selftext' 	|
| 16 	| stemmatized_tokenized_title 	| object 	| Text data: tokenized with regular expression, stemmatized with PorterStemmer() 	| Stemmatized 'tokenized_title' 	|
| 17 	| stemmatized_tokenized_all_text 	| object 	| Text data: tokenized with regular expression, stemmatized with PorterStemmer() 	| Stemmatized 'tokenized_all_text' 	|



## Conclusions & Recommendations

In the end, there was an inconclusive winner because the best differentiators (looking at individual predictors of either group's subreddit) would still necessitate choosing criteria and required my subjective analysis of the sematics of the words chosen by the analysis. However, perhaps this project could be improved by performing a form of sentiment analysis on the data in future studies. Lastly, though this model scored only around ~85.43% accuracy on testing data, this model still predicts much better than the baseline (~50.6%) and might be helpful in differentiating betwen other pop culture figures.
