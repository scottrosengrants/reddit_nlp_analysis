# Reddit NLP Analysis through Classification
#### Authored by: Scott Rosengrants

Analysis of various Natural Language Processing models using subreddits scraped from reddit using the Pushshift Reddit API.

### Problem Statements/ Objectives: 

1. Can a model be built to predict one subreddit post from another? What is the optimal model? 
2. Can a model be built to determine several subreddits from one another?  


### Data Dictionary: 

Subreddits used: 

| Subreddit            | ID | url                                          |
|----------------------|----|----------------------------------------------|
| r/Cooking            | 0  | https://www.reddit.com/r/Cooking/            |
| r/Keto               | 1  | https://www.reddit.com/r/keto/               |
| r/EatCheapAndHealthy | 2  | https://www.reddit.com/r/EatCheapAndHealthy/ |
| r/DIY                | 3  | https://www.reddit.com/r/DIY/                |
| r/DataScience        | 4  | https://www.reddit.com/r/datascience/        |

Features: <br>
'selftext' : This is the body of a reddit submission <br>
'title' : This is the title of a reddit submission

Target: <br>
'subreddit': This is the subreddit a particular submission belongs to. 


### Repo Structure: 
Included in this specific repo
- code folder
  - Jupyter notebooks
    - data_collection.ipynb (web scraping)
    - eda.ipynb (data cleaning and feature exploration)
    - modeling.ipynb (multiple categorical model itterations)
- data folder (contains all clean datasets in csv format)
- results folder (contains a csv for each model)
- presentation slides (pdf format) for Reddit stakeholders
    
### Executive Summary: 

In order to approve upon reddit's classification system for which subreddit a submission should be assigned to an analysis of several machine learning models was completed. In this analysis the objective was to first build a model that accurately could determine one subreddit from another. The second objective was determine if an model could then be built to determine what subreddit a submission belonged to out of five subreddits. 

The analysis began with web scraping 100,000 submissions from reddit using the Pushshift Reddit API. 20,000 submissions were collected from Cooking, Keto, EatCheapAndHealthy, DIY, and Datascience each. 

This data was then cleaned. All null values were removed along with all meta data associated with each submission. After exploring the data it was determined that the title and body of the submissions would make up the predictive feature of the model and the subreddit would be the target. 

After cleaning there was roughly 63,000 entirely complete rows of data, aproximately 12,200 per subreddit. 

Modeling then begun. The TFIDF Vectorizer was chosen to vectorize the NLP data because of its unique ability to build a library of terms based on the proportion of that term in the entirety of the dataset. This greatly reduces the amount of features that are created while retaining the valuable strongly indicative terms. 

The TFIDF Vectorizer was then paired with Logistic Regression and Gaussian Naive Bayes models. The subreddits used in the initial testing were designed to be noticibly different. The subreddit EatCheapAndHealthy and Datascience were chosen. Multiple gridsearches were conducted over various parameters to then optimize each model. Two primary factors were considered here, test score accuracy and the time it takes to fit the model. 

The optimal model was discovered to be a Logistic Regression paired with the TFIDF Vectorizer with the folling parameters:     tfidf__max_features = 2000, tfidf__stop_words = english, tfidf__ngram_range = (1,1), tfidf__max_df = .70 , lr__solver = saga, lr__penalty =l2, lr__C = 1

This optimally tuned regression model was then applied to subreddits that were much more closely related, Cooking and EatCheapAndHealthy. 

Finally all five subreddits were tested using the prediction model Support Vector Classifier in conjuncion with the TFIDF Vectorizer. 
  
    
### Findings/Conclusions:

To answer the initial objective of whether or not a model can be built to determine which subreddit to assign a particular submission to, the answer is yes. The optimal Logistic Regression model had an accuracy test score of 98.4% and a fit time of 0.42 seconds. This model was very efficient at classifiying submissions to subreddits that differed from one another greatly. When this model was applied to subreddits more closely related it had a accuracy test score of 84.1% and a fit time of 0.38 seconds. Quite a drop in accuracy however still performing well overall. 

The second ojective of whether or not a multiple subreddit classifier could be successfully built was also found to be true. The Support Vectorizer Classifier was successful in determining the corrrect subbreddit a submission belonged to. It did have a decline in accuracy when determing between the two closely related subreddits Cooking and EatCheapAndHealthy much like the Logistec Regression did. 

### Recommendations/Further Steps:

These two models could continue to be improved upon to increase their predictive accuracy and grow with the scale of the subreddits in question. More research on closely related subreddits is needed to increase the unique signals each subreddit contains. The hyperparameters of the Support Vector model could be gridsearched over to improve overall performance.

It is also recommended to research into the vectorizer dictionaries of each optimized model to understand what words or phrases are most significant to each subreddit. And finally, it would be benificial to automate this process for scale to be used with Reddit’s moderator bots. 

