# Tweepy-Spark Twitter
Data Practicum II
Spark is one of the latest technologies being used to quickly and easily handle Big Data
It is an open source project on Apache
It was first released in February 2013 and has increased in popularity due to it’s ease of use and speed
It was created at the AMPLab at UC Berkeley
You can think of Spark as a flexible alternative to MapReduce
Spark can use data stored in a variety of formats
Cassandra
Amaazon Web Services S3
HDFS
And more

## Project:
This project will consist of two phases.  The first phase of our project will consist using Python 3.6 to set up a live data streaming pipeline with Twitter using the tweepy package and Spark.
The tag word we will use is 'Steelers', which was chosen for two reasons:
Originally from a suburb of Pittsburgh, Pennsylvania
The National Football season is getting under way.

The second phase will be to analyze the top 10 tweets.  Once the specified number of tweets have been obtained, we will analyze each individual tweet and attempt to rank the top 10 most popular tweets using the hashtag marker.

A dashboard type visualization will be the final phase, which will consist of a bar plot using the matplotlib and seaborn library. Our dashboard will be updated in real time as the tweets are collected and the top ten 'hashtag' topics will be displayed in a bar plot visualization.  Each item in the top ten list will be displayed in a different color.  The x-axis will displar the total cound and the y-axis will show the hashtag topic.

## Data:
The data will consist of creating a pipeline to live stream tweets that contain the tagword "Boxer puppies". The pipeline will continue to live stream tweets to adequately display atleast ten tweets.

Ultimately, the amount of tweets obtained should be around 10,000 to get a true sample of the mlst popular topics containing our tagword.

