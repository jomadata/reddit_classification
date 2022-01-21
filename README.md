# Project 3, Natural Language Processing.
### by Joomart Achekeev

### Executive Summary

What model performs best during identification of the miscalenious subreddit post to a specific subreddit topic? This particular project is comparing Decision Trees CLassifier, K Nearest Neighbors, Bagging(step up from Decision Tree Clasifier) and second level linear model that uses results from the previous three models to build on those results.

Dataset with the size of 120 thousand rows, has been scrapped using Pushshift API across 78 top subreddits (the data on the 'topness' is taken directly from reddit.com). After data cleaning process, the 9 subreddits have been selected, due to having over 1000 posts.

These 9 subreddit dataframe (further DF) has been used to train the abovementioned models and the best model yeilded the result of ~62 percent accuracy on the text of the post, and ~55 percent accuracy on the title of the post under the bagging model. Further model fine tunning has been neglected, due to the time and machine power constraints, but grid search with different hyperparameters of the bagging model can be used to gain additional accuracy percentages.

### Intro

The goal of the project 3 in the DSIR1011 cohort was to train on Natural Language Processing by scrapping data directly of reddit.com in order to build several binary classification models and compare their results. Thinking that I could build a model that would make multi classification I decided to scrape the data across 78 most popular subreddits, 2000 posts deep in each one of them. This is where i made my main mistake during the project implementation.


### 01 Web scrapping the list of top subreddit topics
When I decided to scrape the top topics off subreddit, i stumbled into the problem of needing to webscrape not just a text on the page but text inside javascript object. Since i try to avoid hardcoding as much as i can, i had to google the solution and ended up with using this one https://stackoverflow.com/questions/63761991/how-to-scrape-a-javascript-website-in-python?rq=1, it requiered me to pip install selenium and chromium drivers. Eventually i was able to get my hands (code) onto the list of top topics as identified by reddit.com, and did some cleaning. Namely deleted the Ask... subreddits, due to them having only the title of the post, and extreamly high reate of removal.

### 02 Web scrapping actual data
As mentioned in the introduction, the idea was to werb scrape through 78 subreddits with 2000 topics deep in each one of them. The written code took ~70 minutes to execute and provided me with 72 thousand rows of data. 

In order to not be blocked by the website i introduced random wait time from 4 to 6 seconds, in addition i have randomized the list of topics for them not to be in the alphabetical order.

Additional code tweaks where introduced in order to monitor the implementation of the web scrapping code, that is elapsed time, which portion of the web scrapping the code is implementing at the moment, wait time during current cycle and total number of errors up to the current time.

### 03 Data cleaning and EDA

Next step was to het rid of the Null values, and the posts that had [deleted] in their text or title. 

After this the database shrinked to ~22 thousand rows, and only 9 subreddits that had over 1000 posts for further analysis.

The 9 selected subreddits are:
1. Battlefield
2. DestinyTheGame
3. DnD
4. EscapefromTarkov
5. Jokes
6. MaliciousCompliance
7. PiratedGames
8. TwoXChromosomes
9. explainlikeimfive
of which 5 are about games. This cleaning process leaves us with just ~12 thousand rows of data across 9 topics.

In order to start the EDA, the text data needed to be vectorized, for which I used following Hyperparameters max_df=0.95, max_features=10000, ngram_range=(1, 2), stop_words='english'. Due to using the title and text of the subreddit post, i had to vectorize the data into two different databases in order to not get thw results mixed up.

Other then that the rest of the EDA quite straight forward with most frequent words across several subreddits, the most frequent words in the whole dataframe.

### 04 Moddeling

DecisionTreeClassifier, K Nearest Neighbors, Bagging (step up from decision tree) and Stacking models have been fed the data from two tables (title and text)

DecisionTreeClassifier has returned the accuracy score of 53.98% on the title data and 57.15% that is significantly better than basic model guessing with the probability of 1/9 (~11.11%)

K Nearest Neighbors returned lower results with 35.55% accuracy for title and 31.37% accuracy for text. Even though the scores are very low, suprisingly the accuracy for title data is higher

GridSearch over KNN model, returned 40.6% accuracy for title on {'n_neighbors': 3, 'weights': 'distance'} parameters, and 33.14% accuracy for text on {'n_neighbors': 5, 'weights': 'distance'} parameters.

Bagging model returned accuracy score of 55.52% for title data and 62.13% for text data.

In Stacking model additionaly to using all three level one models to fit the stacking model, I have used title and text data combined. Surprisingly the returned cross_val_score is 46.77% wich is substantially worse than the bagging model that was used as input for the stacking.

### Conclusion

As a conclusion, in this particular case, the bagging model has yeilded the best result with 62.13% on the text data. There is room for imporovement, each of the models can be thoroughly grid searched with detailed hyperparameters sepparated, different vectorizer can be used, n_gram range can be tweaked. But due to time and processing power constraints this was not implemented in this particular project.
In addition it is needed to be carefull with the text data as only 12 rows can return over 250 thousand columns of data after vectorizing it. Thus, next time there is a need to strictly follow the insturctions, if it says 2 subreddits, then 2 subreddits it is.