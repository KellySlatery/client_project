# Project 5: Optimizing Evacuation Routes using Real-Time Traffic Information

Song May, Michael Daugherty, Kelly Slatery | 02.21.2020 | US-DSI-10

## Problem Statement

For our fifth project in General Assembly Data Science Immersive course, we tackled optimizng 

- lots of maps out there for traffic, use user input and satellite (?)
- don't work so well during disasters
- information during disasters often comes in the form of: instant social media updates (Twitter)
- many official accounts

- as the state that gets hit by the widest variety and one of the largest plethora of natural disasters, we chose to focus on Texas
- Puerto Rico if we had had more time to account for Spanish language resources
- In 2017, Texas saw one of the most ___ disasters in its history--Hurricane Harvey
- for the purposes of this project, we narrowed our scope to looking at live Twitter data from official accounts (DOT, police, ...?) to identify up-to-date road condition and other travel information

- created vigorous models with ___ accuracy to determine which tweets contained relevant information, and additionally which tweets contained coordinates or an otherwise usable location reference (such as an address, intersection, or direction/exit on a highway)
- process

- from here, we could use geo.py to find coordinates of location references
- then use an interface such as the Here.com API or Google Maps API to plot road conditions in real-time
- data files would be compiled into scripts that would automatically search twitter for relevant data (specific location & search query terms) whenever a user entered their location, and would reload a map locating any relevant information in real-time
- last step would be to use an algorithm like ______ to plot evacuation routes, utilizing mapping attributes/methods of the API

- Our model allows us to utilize the vast resource of Twitter data in the event of a disaster when our usual technologies fail us
- This is the bridge that was missing in getting information to people in the most accessible, quick-to-use, easy-tounderstand form in the case of emergencies
- we hope it can be put into practice someday!!

## Table of Contents
*Code is presented in the order in which it was written and run; data may be viewed in any order*

### Code

#### Data Collection
[1](https://github.com/KellySlatery/client_project/blob/master/01_Project_Data_Collection.ipynb)\
[2](https://github.com/KellySlatery/client_project/blob/master/02_Extra_Data_Collection.ipynb)\
[3](https://github.com/KellySlatery/client_project/blob/master/03_Police_Data_Collection.ipynb)\
[4](https://github.com/KellySlatery/client_project/blob/master/04_Data_Concatenation.ipynb)

#### Data Cleaning & EDA
[1](https://github.com/KellySlatery/client_project/blob/master/05_EDA_and_Data_Cleaning.ipynb)\
[2](https://github.com/KellySlatery/client_project/blob/master/06_Prepare_Train_Data.ipynb)

#### Modeling & Predictions
[1](https://github.com/KellySlatery/client_project/blob/master/07_Model_Data_1.ipynb)\
[2](https://github.com/KellySlatery/client_project/blob/master/08_Model_Data_2.ipynb)\
[3](https://github.com/KellySlatery/client_project/blob/master/09_Model_Data_3.ipynb)\
[4](https://github.com/KellySlatery/client_project/blob/master/10_Model_Predictions.ipynb)

#### Data
[All](https://github.com/KellySlatery/client_project/tree/master/data) → [Training](https://github.com/KellySlatery/client_project/tree/master/data/train_data) | [Test](https://github.com/KellySlatery/client_project/tree/master/data/test_data)\
[Predictions](https://github.com/KellySlatery/client_project/blob/master/data/final_data_with_predictions.csv)

#### [Executive Summary](https://github.com/KellySlatery/client_project#executive-summary)

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

Having now a collection of around 100 tweets made during Hurricane Harvey which our model had classified as related to road closures, we used regular expressions to extract the intersections of those roads where the closures occurred. We then used a geocaching application to convert these intersections into latitudes and longitudes. Finally, we used Tableau to produce of map with a marker at each location.


## Data Dictionary



## Contents




