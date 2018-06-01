# WeRateDogs Data Wrangling Project

### Introduction

This project is a data wrangling project, which mainly focus on fixing the data quality and tidiness issues using python. The dataset that I am wrangling is the tweet archive of Twitter user @dog_rates, also known as WeRateDogs. WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. These ratings almost always have a denominator of 10. And the numerators almost always greater than 10, because "they're good dogs Brent." The tweet archive records using in this project contains basic tweet data (tweet ID, timestamp, text, etc.) for all 2356 of their tweets as they stood on August 1, 2017. 

This project includes steps as follows:
- Data wrangling, which consists of: gathering data, assessing data, and cleaning data 
- Storing, analyzing, and visualizing your wrangled data

### Data Gathering

1. `twitter_archive`: The WeRateDogs Twitter archive, which is provides by the Udacity Course and I use pd.read_csv() to import them into dataframe.
2. `image_predictions`: The tweet image predictions, i.e., what breed of dog (or other objects, animal, etc.) is present in each tweet according to a neural network. This file ('image_predictions.tsv') is hosted on Udacity's servers and downloaded programmatically using the requests library and the provided url.
3. `tweet_data`: Using the tweet IDs in the WeRateDogs Twitter archive, query the Twitter API for each tweet's JSON data using Python's Tweepy library and store each tweet's entire set of JSON data in a file called 'tweet_json.txt' file. Each tweet's JSON data is written to its own line.


### Data Assessing

Inspecting data set for two things: data quality issues and lack of tidiness

•	Quality Issues means content issues like missing, duplicate, or incorrect data 
•	Untidy Data has specific structural issues 

In addition, four dimensions of data quality assessment help me guide the thought process while assessing the data. For example, Completeness: are there any missing data in specific rows or columns? Validity: are there any records not correct due to any reason? Accuracy: are there any extreme data or unusual data? Consistency: are they keep the consistence of scale standard or data type?

#### Tidiness Issues

1.	Columns 'doggo', 'floofer', 'pupper', 'puppo' in twitter_archive should belong to one colomn – stage
2.	The tweet_data table need to merge into the twitter_archive table.

#### Quality Issues

`twitter_archive `

1.	Some columns have huge amount of missing values, for example, "in_reply_to_status_id", "in_reply_to_user_id", "retweeted_status_id", "in_reply_to_user_id", "retweeted_status_id", "retweeted_status_user_id", "retweeted_status_timestamp". Since I don't need in_reply and retweet data in this project, I prefer to delete those columns directly.
2.	The variable "expanded_urls" also has few missing values, which means some records had no images. Any ratings without images should not be taking into account.
3.	The datatype of "timestamp" is not correct.
4.	Optimize the source contents for human reading habit; change the long URL links to certain words.
5.	The standard for "rating_denominator" is 10, but it includes some other numbers, which could be the mis-parse.
6.	The "rating_numerator" also has some incorrect values.
7.	Some dog names are incorrect.
8.	The dog names are sometimes first letter capital but sometimes not. Keep the name format consistent.

`image_predictions `

1.	The columns’ names are not clear and straightforward such as p1, p2.
2.	The prediction dog breeds involve both uppercase and lowercase for the first letter.


### Data Cleaning


-	Tidiness Issue 1:  Create a new variable – ‘stage’ to show the four dog stages, drop the four columns, and fill the empty with NaN.

-	Tidiness Issue 2: Merge the tweet_data into the twitter_archive using inner join.

-	Quality Issue 1: Remove all the unnecessary columns directly ('retweeted_status_id', 'retweeted_status_user_id', 'retweeted_status_timestamp', 'in_reply_to_status_id', 'in_reply_to_user_id', 'in_reply_to_user_id).

-	Quality Issue 2: Remove the records with no images information (‘expanded_urls’ is NaN).

-	Quality Issue 3: Change the datatype of 'timestamp' to datetime.

-	Quality Issue 4: Optimize the source content by ‘Twitter for iphone’, ‘Vine - Make a Scene’, ‘Twitter Web Client’, and ‘TweetDeck’.

-	Quality Issue 5: 10 is the default value of ‘rating_denominator’, then correct the wrong values based on the corresponding text information.

-	Quality Issue 6: Correct the ‘rating_numerator’ values from the text information. 

-	Quality Issue 7: Change the frequent incorrect dog name to None. 

-	Quality Issue 8: Capitalize the first letter of dog name for consistence.

-	Quality Issue 9:  Change the column names for better readability in image_predictions.

-	Quality Issue 10:  Capitalize the first letter of first prediction in image_predictions (I could do that for all the predictions, but I decide to only apply to the first prediction since this variable is the important one).

Finally, I conduct a final test for the datasets and store the twitter_archive_clean to the file ‘twitter_archive_master.csv’.



### Data Analyzing and Conclusion

1.	The Distribution of Source

This plot above shows the distribution of source. We can see that the dominate source of tweets is from iPhone twitter app, which is 94% in the total. That means the twitter app is the main channel for people using to tweet, retweet, post, and others, while the TweetDesk is pretty rare (less than 1%). 

2.	The Distribution of Dog Stages

Similarly, I check the distribution of dog stages. It shows that ‘pupper’ (a small doggo, usually younger) is the most popular dog stage, followed by ‘doggo’ and ‘puppo’. It could be due to the young and unmatured dog is usually cuter than the adult dog. It should also be noticed that there’s huge amount missing data in dog stages, thus the distribution may not reflect the truth. 

3.	Retweet_count and favorite_count Correlation

A reasonable hypothesis is that most popular tweets usually get a large number of retweets and favorite counts. I test the correlation between ‘retweet_count’ and ‘favorite_count’. The pearson r^2 is 0.759, that is a high value showing a strong positive relationship between ‘retweet_count’ and ‘favorite_count’. The plot below confirms this hypothesis. 

4.	The Classification of Dogs Results Analysis

The ‘image_predictions’ table stores the result of a classification of dog breeds through a neural network. I am curious about the how this model works? What’s the accuracy of this model? Therefore, I analyze and visualize the results in below. 

These breeds above are the top 10 dog breeds this model predicted. Golden retriever and Labrador retriever are top 2 and both over 100 predictions. It could be because those two are most common breeds in U.S. We have more image data on those breeds, and thus trained a better result. 

The first plot above shows the prediction success rate of whether or not first prediction is a breed of dog. The pie chart indicates almost 2/3 situations the predictions are correct, even though this result is not good enough for a deep learning model. The second plot shows how confident the algorithm is in its first prediction. We can see 100% is the most cases, however the amounts of 0.1 to 0.8 dominate the entire distribution. That also could suggest that the model is not good enough. 


### Reference

Twitter API Guide: 
<https://www.slickremix.com/docs/how-to-get-api-keys-and-tokens-for-twitter/>
Tweepy documentation: <https://media.readthedocs.org/pdf/tweepy/latest/tweepy.pdf>
WeRateDogsTwitter: <https://twitter.com/dog_rates?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor>

