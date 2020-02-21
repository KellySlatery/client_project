# Project 5: Optimizing Evacuation Routes using Real-Time Traffic Information

Kelly Slatery, Song May, Michael Daugherty | 02.21.2020 | US-DSI-10

## Problem Statement

While there are many GPS and navigation systems available for our daily commutes and errands, these systems rely on predictable traffic flow and user input, and thus break down during disasters. Up-to-date traffic information during disasters and evacuations comes from other real-time sources, such as news sources, FEMA, and Twitter.

The goal of this project was to use text gathered from official Twitter feeds in the state of Texas to optimize evacuation routes during Hurricane Harvey (no official evacuation order was ever issued). We chose to focus on locating information about road closures in tweets, and plotting the information visually on an interactive map.

## Table of Contents
*Code is presented in the order in which it was written and run; data may be viewed in any order*

### Code

#### Data Collection
[1: 01_Project_Data_Collection](https://github.com/KellySlatery/client_project/blob/master/01_Project_Data_Collection.ipynb)\
[2: 02_Extra_Data_Collection](https://github.com/KellySlatery/client_project/blob/master/02_Extra_Data_Collection.ipynb)\
[3: 03_Police_Data_Collection](https://github.com/KellySlatery/client_project/blob/master/03_Police_Data_Collection.ipynb)\
[4: 04_Data_Concatenation](https://github.com/KellySlatery/client_project/blob/master/04_Data_Concatenation.ipynb)

#### Data Cleaning & EDA
[1: 05_EDA_and_Cleaning](https://github.com/KellySlatery/client_project/blob/master/05_EDA_and_Data_Cleaning.ipynb)\
[2: 06_Prepare_Train_Data](https://github.com/KellySlatery/client_project/blob/master/06_Prepare_Train_Data.ipynb)

#### Modeling & Predictions
[1: 07_Model_Data_1](https://github.com/KellySlatery/client_project/blob/master/07_Model_Data_1.ipynb)\
[2: 08_Model_Data_2](https://github.com/KellySlatery/client_project/blob/master/08_Model_Data_2.ipynb)\
[3: 09_Model_Data_3](https://github.com/KellySlatery/client_project/blob/master/09_Model_Data_3.ipynb)\
[4: 10_Model_Predictions](https://github.com/KellySlatery/client_project/blob/master/10_Model_Predictions.ipynb)

#### Mapping 
[1: 11_Get_Locations.ipynb](https://github.com/KellySlatery/client_project/blob/master/11_Get_Locations.ipynb)\
[2: 12_Get_Coordinates](https://github.com/KellySlatery/client_project/blob/master/12_Get_Coordinates.ipynb)\
[3: 13_Get_Lat_Lng](https://github.com/KellySlatery/client_project/blob/master/13_Get_Lat_Lng.ipynb)\

#### Data
[All Data](https://github.com/KellySlatery/client_project/tree/master/data) → 
[Training](https://github.com/KellySlatery/client_project/tree/master/data/train_data) | [Test](https://github.com/KellySlatery/client_project/tree/master/data/test_data)\
[Predictions](https://github.com/KellySlatery/client_project/blob/master/data/final_data_with_predictions.csv)

### [Executive Summary](https://github.com/KellySlatery/client_project#executive-summary)


## Description of Data

All data were gathered in the form of tweets from transportation authorities and police departments across Texas. More than 100,000 tweets were collected using the [GetOldTweets 3](https://github.com/Dawars/GetOldTweets3) Python library. A range of dates from 2016 to 2020 was specified for the library's scraping function to search, as well as Twitter usernames and maximum number of tweets to collect from each username. The tweets were saved in a pandas DataFrame, exported to a .csv file, then cleaned in [a separate Jupyter notebook](https://github.com/KellySlatery/client_project/blob/master/05_EDA_and_Data_Cleaning.ipynb).

## Required Software

#### Python libraries:
- [pandas](https://pandas.pydata.org/)
- [numpy](https://numpy.org/)
- [GetOldTweets3](https://github.com/Dawars/GetOldTweets3)
- [time](https://docs.python.org/3.7/library/time.html)
- [re](https://docs.python.org/3.5/library/re.html)

#### 

## Executive Summary

When a natural disaster occurs and evacuation of an area becomes necessary, it is imperative that the people in that area have quick and easy access to information about which routes to use and which to avoid. Often, this information is scattered across the internet and can be difficult to find. It must therefore be aggregated and converted into a more usable form. This project is an attempt to accomplish that goal.

The first step of the process was data collection. We focused on Twitter for its large volume of publicly available posts and the simple methods available for accessing them in bulk. We used the Python library [GetOldTweets 3](https://github.com/Dawars/GetOldTweets3) to scrape from 69 Twitter feeds in Texas, including 39 departments of transportation and 30 police departments, dating back to 2016.

This large collection was then searched for words such as "road," "highway", and "closed." Those tweets which contained such words were labeled pertinent (1), and those which didn't were labeled nonpertinent (0). We then separated the tweets into those from before or after Hurricane Harvey (25 August - 2 September 2017), and those which were tweeted during the event. 

The first group of tweets (pre- and post-hurrican) were separated into a training set and a test set; we used the training set to train several classification models and the test set to make predictions of pertinent or nonpertinent. The model with the most accurate predictions on the test set was then used to classify the tweets made during the hurricane.

With a collection of around 100 tweets made during Hurricane Harvey which our model had classified as related to road closures, we used regular expressions to extract the intersections of those roads where the closures occurred. We then used a geocaching application to convert the intersections into latitudes and longitudes. Finally, we used Tableau to produce a map with a marker at each location.

## Data Collection

We collected data using the [GetOldTweets3](#https://pypi.org/project/GetOldTweets3/) (GOT3) Python library for accessing tweets. While the Official Twitter API is robust, it limits data collection to the past week, so GOT3 provided access to much more data while also allowing for search by date range (and more, described below in [Results and Recommendations](https://github.com/KellySlatery/client_project#Results-and-Recommendations).

Train data consists of all tweets from 56 total accounts over the past 3 years (2016-01-01 to 2020-02-10), excluding the time period when Hurricane Harvey was in Texas (2017-08-25 to 2017-09-02). Of the 56 Twitter accounts, 52 were official Texas Police Department (PD) accounts or official Texas Department of Transportation (TxDOT) accounts, with 4 unofficial transportation accounts recommend by TxDOT on their [website](https://www.txdot.gov/driver/weather/txdot-twitter-feeds.html).

Test data includes tweets pulled from all 30 of the same official Texas PD accounts, along with 12 official or recommended TxDOT accounts from locations near where Hurricane Harvey hit. This is the data that was unseen by our model during model training, and then was used to extract the location information visible on our [public Tableau map](https://public.tableau.com/shared/WCGKZ9D3M?:display_count=y&:origin=viz_share_link).


## EDA and Cleaning
 
The final data we gathered had 5 columns and 173,977 rows. Knowing the fact that our project could help people find a safe and quick escape route, we realized that our data cleaning must be very thorough and precise.
 
### Routine EDA
 
Routine EDA involved nothing special other than checking null values and making sure data types were correct. The column that had the most null values was the hashtag columns. We thought about keeping it and thought that hashtags could be useful. However, the fact of nearly 70% of the values from the hashtag column was missing, and that the hashtags themselves did not seem to contain much traffic information, led to the decision of dropping the hashtag column.
 
For the ‘tweet’ column, we deleted all the characters that came with website links, as well as all the punctuations. That made our ‘tweet’ column contain purely words. We also deleted rows where the ‘tweet’ cell was null because these most likely represent deleted tweets and contained no useful information.
 
### Filtering
 
Using Word Vectors, we were able to pick out the most related words for use to use in our filters. In our first layer, we chose words like ‘st’, ‘ln’, ‘ave’, ‘ramp’ and ‘close’ etc. With these words in the filter, we could filter out the tweets that were not related to traffic or street situations. In our second layer, we used the words that were used in some much more generic situations such as ‘open’, ‘school’, ’parking’, and ‘StateFairOfTX’ etc. We were able to filter out most of the tweets that had the word “closed’ in it but had nothing to do with traffic.
 
### Assign Categories
 
After two layers of filtering, we had a positive dataset. Using reverse filtering, we also got our negative dataset. So we concatenated both after assigning a new column ‘category’ to each of the two datasets where ‘1’ meaning useful tweets and ‘0’ meaning not useful tweets. Exporting this final labeled dataframe as a CSV file concluded our EDA.

## Modeling and Predictions

After labeling the data, we set up 7 GridSearches to evaluate parameters for Logistic Regression, Random Forest, ADABoost Classifier, and Support Vector Classifier with CountVectorizer or TfidfVectorizer. With a baseline score of 94.9%, we were looking for a model with incredibly high accuracy, low variance, and an optimization of false negatives (we want to find as many useful tweets as possible).

Of the 7 GridSearches, all models performed quite well, however most were also overfit. Logistic Regression with a CountVectorizer and l1 regularization received the same accuracy score for training cross-validation and testing, at 99.7%. While the Random Forest received a 98.8% on testing data, it received 100% on the training data, and thus we deemed it well overfit.

Proceeding with the logistic regression, we used the [pickle module](https://docs.python.org/3/library/pickle.html) to export the model (serialize the model by converting it into a byte stream) for future use. Then, we made predictions on the raw test data by importing (deserializing / unpickling) our model of choice, and added those predicted classes ({1: ‘useful’, 0: ‘not useful’}) to the dataframe for coordinate fetching.


## Results and Recommendations

Our results are shown on this [public Tableau map](https://public.tableau.com/shared/WCGKZ9D3M?:display_count=y&:origin=viz_share_link). While the contents of this repository cover the basic breadth of steps necessary to accomplish the problem statement goal, this is a complex project with many moving features. With more time and resources, we hope we could expand on this notebook to accomplish the following:

- Data Collection: Besides official Twitter accounts, we could also use search for tweets by geo coordinates and search query terms to find live updates on traffic information from everyday users. Additionally, we could build a web scraper to get information from other sites that provide live disaster updates, such as FEMA or local news sources. Last, data would need to be collected in real-time in the event of a disaster, and thus we would like to upload our data to AWS and create functions to automatically scrape sources and run our notebooks to provide the user with a real-time map of road blockages near them.

- Data Cleaning: Our cleaning consisted of a combination of masks to include specific words and exclude other words. We should expand upon this list to ensure all interstate highway names and other road names are included (Ex. In Texas, “FM” = “Farm-to-market road”). In some tweets, interstate names were referenced only by their numbers, and thus were excluded from useful tweet data.
Modeling: While we ran 7 GridSearches over 4 different types of high-performing classification models, we could attempt a Gradient Boost Classifier as well, though 99.7% is hard to beat.

- Locations: The Regex strings we used to filter locations out of tweets is preliminary and meant to serve as a prototype for what is possible with Regex. The Regex has to be adapted to include more highway/street names, intersections, and more, and is flexible and adaptable for other localities, as well as generalizable. However, in addition to further filtering with Regex, we also recommend official Twitter accounts to standardize location provision. Latitude and longitude coordinates would be ideal, but otherwise easily searchable locations (such as an exact address) and norms for capitalization and punctuation would make this process more efficient and provide even more accurate information to the people.

- Embed Tableau: While our Tableau map is interactive, we would like to take it a step further and embed it into a dashboard accessible on a website or app (or both). We have looked into using [Flask](#https://palletsprojects.com/p/flask/), a web application framework. This would provide the most accessibility to our service, though we would need a more robust system to handle the high simultaneous demand that we would expect from an app providing a disaster emergency service.
