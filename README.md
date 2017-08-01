# Live streaming twitter data using pyspark and tweepy.
## Data Practicum II
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
Introduction to Spark Streaming

Spark has pretty well known Streaming Capabilities, if streaming is something you've found yourself needing at work then you are probably familiar with some of these concepts already, in which case you may find it more useful to jump straight to the official documentation here:

http://spark.apache.org/docs/latest/streaming-programming-guide.html#spark-streaming-programming-guide

It is really a great guide, but keep in mind some of the features are restricted to Scala at this time (Spark 2.1), hopefully they will be expanded to the Python API in the future!

For those of you new to Spark Streaming, let's get started with a classic example, streaming Twitter! Twitter is a great source for streaming because its something most people already have an intuitive understanding of, you can visit the site yourself, and a lot of streaming technology has come out of Twitter as a company. You don't access to the entire "firehose" of twitter without paying for it, but that would be too much for us to handle anyway, so we'll be more than fine with the freely available API access.

Let's discuss SparkStreaming!

Spark Streaming is an extension of the core Spark API that enables scalable, high-throughput, fault-tolerant stream processing of live data streams. Data can be ingested from many sources like Kafka, Flume, Kinesis, or TCP sockets, and can be processed using complex algorithms expressed with high-level functions like map, reduce, join and window. Finally, processed data can be pushed out to filesystems, databases, and live dashboards. In fact, you can apply Spark’s machine learning and graph processing algorithms on data streams.

Keep in mind that a few of these Streamiing Capabilities are limited when it comes to Python, you'll need to reference the documentation for the most up to date information. Also the streaming contexts tend to follow more along with the older RDD syntax, so a few things might seem different than what we are used to seeing, keep that in mind, you'll definitely want to have a good understanding of lambda expressions before continuing with this!

There are SparkSQL modules for streaming:

http://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=streaming#module-pyspark.sql.streaming

But they are all still listed as experimental, so instead of showing you somethign that might break in the future, we'll stick to the RDD methods (which is what the documentation also currently shows for streaming).

Internally, it works as follows. Spark Streaming receives live input data streams and divides the data into batches, which are then processed by the Spark engine to generate the final stream of results in batches.

## Project:
This project will consist of multiple phases.  The first phase of our project will consist using Python 3.6 to set up a live data streaming pipeline with Twitter using the tweepy package and Spark.
The tag word we will use is 'Donald Trunp'.
Our President is on the news a lot and it would be interesting to see what hashtag topics are be attached to tweets that contain his name

The second phase will be to analyze the top 10 tweets.  Once the specified number of tweets have been obtained, we will analyze each individual tweet and attempt to rank the top 10 most popular tweets using the hashtag marker.

A dashboard type visualization will be displayed, which will consist of a bar plot using the matplotlib and seaborn library. Our dashboard will be updated in real time as the tweets are collected and the top ten 'hashtag' topics will be displayed in a bar plot visualization.  Each item in the top ten list will be displayed in a different color.  The x-axis will displar the total cound and the y-axis will show the hashtag topic.

The final part of the project will be to caputure a significant number of tweets (2,000+) and complete a sentiment analysis on what will be a representation of the tweets streamed.

## Data:
The data will consist of creating a pipeline to live stream tweets that contain the tagword "Donald Trump". The pipeline will continue to live stream tweets to adequately display the ten most popular.

Ultimately, the amount of tweets obtained should be around 10,000 to get a true sample of the most popular topics containing our tagword.

## Preparation
A virtual machine running Ubuntu 16.0.4 was used, along withPython 3.5.2, which comes installed on the VM machine.  Jupyter notebook was also utilized in creating python scripts.
The following packages were used and installed via pip3:
pyspark,
 matplotlib,
 seaborn,
 IPython,
 time,
 tweepy,
 socket,
 json,
 pandas.

In order to run the .py and .ipynb scripts, two terminal windows are needed.  
