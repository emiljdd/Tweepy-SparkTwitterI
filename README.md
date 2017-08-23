# Live streaming twitter data, including sentiment analysis using tweepy, pyspark, and textblob.
## Data Practicum II
Spark is one of the latest technologies being used to quickly and easily handle Big Data. It is an open source project on Apache. It was first released in February 2013 and has increased in popularity due to its ease of use and speed.  Created at the AMPLab at UC Berkeley and is a flexible alternative to MapReduce Spark that can use data stored in a various formats such as: Cassandra, Amazon Web Services, S3, HDFS, and more.

Spark is well known Streaming Capabilities and if you are probably familiar with some of these concepts already, you may find it more useful to jump straight to the official documentation here:

http://spark.apache.org/docs/latest/streaming-programming-guide.html#spark-streaming-programming-guide

For those of you new to Spark Streaming, let's get started with a classic example, streaming Twitter! Twitter is a great source for streaming because it's something most people already have an intuitive understanding of, you can visit the site yourself, and a lot of streaming technology has come out of Twitter as a company. You don't access to the entire "firehose" of Twitter without paying for it, but that would be a lot to handle anyway, so we'll be more than fine with the freely available API access.

Let's discuss SparkStreaming!

Spark Streaming is an annex of the core Spark API that enables high scalability, high-throughput, and best of all fault tolerant to live data streaming. Data can be consumed from many sources like Kafka, Flume, Kinesis, or TCP sockets, and can be processed using sophisticated innovations expressed with high-level functions like map, reduce, join and window. Finally, prepared data can be propelled out to filesystems, databases, and live dashboards. In fact, you can apply Spark’s ML library and graph processing algorithms on data streams.

Keep in mind that a few of these Streaming Capabilities are limited when it comes to Python, you'll need to reference the documentation for the most up to date information. Also, the streaming contexts tend to follow more along with the older RDD syntax, so a few things might seem different than what is typically seen, keep that in mind, you'll want to have a good understanding of lambda expressions before continuing with this!

There are SparkSQL modules for streaming:

http://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=streaming#module-pyspark.sql.streaming

As of this project they are all still listed as experimental, so instead of showing you something that might break in the future, I'll stick to the better known RDD methods (which is what the documentation also currently shows for streaming).

Internally, it works as follows. Spark Streaming collects current input data streams and partitions the data into parcels, which are then prepared by the Spark engine to generate the final flow of results in batches.

## Project:
This project will consist of three phases.  The first phase of our project will consist using Python 3.6 to set up a live data streaming pipeline with Twitter using the tweepy package and Spark.The tag word we will use is 'Donald Trunp'.
Our President is on the news a lot and it would be interesting to see what hashtag topics are be attached to tweets that contain his name

The second phase will be to analyze the top 10 tweets.  Once the specified number of tweets have been obtained, we will analyze each individual tweet and attempt to rank the top 10 most popular tweets using the hashtag marker.

A dashboard type visualization will be displayed, which will consist of a bar plot using the matplotlib and seaborn library. Our dashboard will be updated in real time as the tweets are collected and the top ten 'hashtag' topics will be displayed in a bar plot visualization.  Each item in the top ten list will be displayed in a different color.  The x-axis will displar the total cound and the y-axis will show the hashtag topic.

The third and final phase of the project will be to caputure a significant number of tweets (2,000+) and complete a sentiment analysis, which will include the polarity and subjectivity of the tweet using the TextBlob package

## Data:
The data will consist of creating a pipeline to live stream tweets that contain the tagword "Donald Trump". The pipeline will continue to live stream tweets to adequately display the ten most popular.

Ultimately, the amount of tweets obtained should be around 10,000 to get a true sample of the most popular topics containing our tagword.

## Methodology:
A virtual machine running Ubuntu 16.0.4 was used, along withPython 3.5.2, which comes installed on the VM machine.  Jupyter notebook was also utilized in creating python scripts.
The following packages were used and installed via pip3:

### Tools and libraries

* Python
    * pyspark - pyspark.streaming import Streaming Context
                          Main entry point for Spark Streaming functionality.
                     pyspark.sql import SQLContext 
                          Main entry point for Data Frame and SQL functionality.
                     pyspark.sql.functions import desc
                          SQL function to allow listing in descending order.
    * matplotlib - Create a bar plot to display the top ten hash tag topics
    * seaborn - Provides an interface with matlplotlib to create interactive graphs
    * IPython - Allows for the display of graphs in the jupyter notebook
    * time - Small, minimalistic, Python library for dealing with time conversions between universal time and arbitrary time zones
    * tweepy - Allows python to connect to the twitter API.
    * socket - Enables a line of communication from twitter to our local machine.
    * json - The json library can parse JSON from strings or files. The library parses JSON into a Python dictionary or list. 
             It can also convert Python dictionaries or lists into JSON strings.

    * pandas -
    * textblob - Sentiment analysis

In order to run the .py and .ipynb scripts, two terminal windows are needed.  

# PHASE I
### Step 1

Set up an Ubuntu VM on my local machine.
Ubuntu version 16.0.4 was created.

### Step 2: Confirm Python 3 is loaded

        python3
        Python 3.5.2 (default, Nov 17 2016, 17:05:23) 
        [GCC 5.4.0 20160609] on linux
        Type "help", "copyright", "credits" or "license" for more information.
        >>>

### Step 3: Load necessary packages using:

         pip3 install <package_name>
         
## TWEETREAD.PY         
### Step 4: TweetRead.py

A .py script was created called TweetRead.  From the tweepy package we install 'OAuthHandler'Stream to handle the authorization credientials that we will enter.  Also, from tweepy we will import 'Stream' and 'StreamHandler' to allow us to log and capture tweets.
The credentials that we obtained fromm the twitter api will also be entered and saved as objects.  

         from tweepy import OAuthHandler
         from tweepy import Stream
         from tweepy.streaming import StreamListener
         
         # Set up your credentials
         consumer_key='<CONSUMER_KEY>'
         consumer_secret='<CONSUMER_SECRET>'
         access_token ='<ACCESS_TOKEN>'
         access_secret='<ACCESS_SECRET>'
         
Next we create a class called tweetListener that will listen for to tweets.

         class TweetsListener(StreamListener): # Create a class that will listen to tweets from Streamlistener

We will now set some user defined functions that will be used to handle the data:

         def on_data(self, data):
            try:
                msg = json.loads( data ) # Create a message from json file
                print( msg['text'].encode('utf-8') ) # Print the message and UTF-8 coding will eliminate emojis
                self.client_socket.send( msg['text'].encode('utf-8') )
                return True
            except BaseException as e:
                print("Error on_data: %s" % str(e))
            return True
            
...and to handle errors that are returned:

         def on_error(self, status): # If an error occurs
             print(status)
             return True
             
Next, we will create a client connection and send the tweets to the local IP address and the defined socket.  Our tag word will be defined, along with the socket.

Our tag word can be edited to pull all tweets that contain it.

One issue that does occur when running the program multiple times is that an error may occur indicating the 'address is invalid' or 'socket in use'.  At this point, to correct the issue, a new socket number = original socket - 1, must be entered.

         def sendData(c_socket): # Send the data to client socket, setting up connection
             auth = OAuthHandler(consumer_key, consumer_secret)
             auth.set_access_token(access_token, access_secret)

            twitter_stream = Stream(auth, TweetsListener(c_socket)) # Passes the tweets into the client socket
            twitter_stream.filter(track=['Donald Trump'])

         if __name__ == "__main__":
             s = socket.socket()         # Create a socket object
             host = "127.0.0.1"          # Get local machine name
             port = 9992                 # Reserve a port for your connection service.
             s.bind((host, port))        # Bind to the port, create tuple

             print("Listening on port: %s" % str(port))

             s.listen(5)                 # Now wait for client connection.
             c, addr = s.accept()        # Establish connection with client.

             print( "Received request from: " + str( addr ) )

             sendData(c)
             
At this point we have completed out authentication and connection script to Twitter and named our tweet tag word using tweepy.  

# PHASE II
## PYSPARK

### Step 1

Let's open another terminal window and start our jupyter notebook to create the following script.
We will initiate the findspark script to locate pyspark from our original directory that it was downloaded too.

               import findspark
               findspark.init('/home/myspark/spark-2.1.0-bin-hadoop2.7')
               import pyspark
               
We must first load the necessary parts of pyspark that will allow us to create a SparkContext, which is the initial state to allow Spark functionality.
Along with that we will iniate Spark Streaming, which will allow us to collect live streaming data.  Finally, we will initiate pyspark.sql to allow SQL queries when we are retrieving the tweets for visualization.

               from pyspark import SparkContext
               from pyspark.streaming import StreamingContext
               from pyspark.sql import SQLContext
               from pyspark.sql.functions import desc
               
### Step 2

Initiate the SparkContext funtionality.  When doing so, we can only initiate once or we must restart our kernel to do so a second time.

               sc = SparkContext()
               
### Step 3

The SparkStreaming object will be created and we will set the update argument to 10 seconds.  This translate to our bar plot being updated every 10 seconds.
Our SQLContext object will be created with the using the argument (sc).  This will allow for SQL queries on the data.
A socketStream object will be created using our local IP address and the socket we used in the TweetRead.py script.  Make sure these variables are the same in both scripts.

               ssc = StreamingContext(sc, 10 )
               sqlContext = SQLContext(sc)
               
               socket_stream = ssc.socketTextStream("127.0.0.1", 9991)
               
### Step 4

Create a tuple that will be made into a list, check for hashtags, sets everything to lowercase, reduces by the predetermined key, stores the object as a tweet object, stores the tweets in descending order (since we are gioing to retrieve the top ten tweets) and registers every ten tweets to a table for later referencing.
            
               ( lines.flatMap( lambda text: text.split( " " ) ) 
                  .filter( lambda word: word.lower().startswith("#") ) 
                  .map( lambda word: ( word.lower(), 1 ) ) 
                  .reduceByKey( lambda a, b: a + b ) 
                  .map( lambda rec: Tweet( rec[0], rec[1] ) ) 
                  .foreachRDD( lambda rdd: rdd.toDF().sort( desc("count")                 
                  .limit(10).registerTempTable("tweets") ) ) 

### Step 5

At this point open a second terminal window and go the directory that contains the TweetRead.py file and type:

                  python3 TweetRead.py > tweet_data2.txt

This will start listening on the defined port and output the collected tweets to a text file called tweet_data.txt.
Once TweetRead.py is started enter the next command in the Jupyter notebook to start the SparkContext session.

                  ssc.start() 
                  
At this point tweets are being read and collected into the output file .

### Step 6

Here we will enable the ability to display the visualization in the jupyter notebook and will only work for the jupyter notebook.
               
                  import time
                  from IPython import display # Enables us to show stuff in the notebook
                  import matplotlib.pyplot as plt #Visualization library
                  import seaborn as sns # Visualization library
                  %matplotlib inline
                  
 ### Step 7
 
Here we will set the time to 3 seconds before we get are first update.  The top ten tweets object will be created using sql.context and a dataframe will be created using pandas.
The next graph clear the previous, if one exists and will set the display parameters using seaborn, which will include our x and y axis, and finally display the graph.

                  count = 0
                  while count < 10:
    
                     time.sleep( 3 )
                     top_10_tweets = sqlContext.sql( 'Select tag, count from tweets' )
                     top_10_df = top_10_tweets.toPandas() # Dataframe library
                     display.clear_output(wait=True) #Clears the output, if a plot exists.
                     sns.plt.figure( figsize = ( 10, 8 ) )
                     sns.barplot( x="count", y="tag", data=top_10_df)
                     sns.plt.show()
                     count = count + 1
                     
https://user-images.githubusercontent.com/7649609/29252628-0be38f4a-8028-11e7-893c-7854a24e12e3.png                     
                     
# PHASE III                     
### Step 1

Our final phase of the project will be to run a sentiment analysis on the output file we created that holds all of our tweets.
Cleaning the tweet data was done so using Microsoft Excel.  Hashtags (#), http(s) address' were removed.  Any Retweeted(RT) indicator was removed from, along with any duplicated tweets.  Another point of contention was making sure blank rows were removed as this proved to be troublesome with 'IndexOutofRange' errors.

Our sentiment analysis will display

    Polarity - a measure of the negativity, the neutralness, or the positivity of the text
    Classification - either pos or neg indicating if the text is positive or negative

To calculate the overall sentiment, we look at the polarity score:

    Positive – from .01 to 1
    Neutral – 0
    Negative – from –.01 to -1

     
The output data collected was imported into Microsoft Excel for data cleaning.
The collected tweets contained various marker strings that needed to be removed befor we could run the sentiment analyzer.
This was accomplished by using the 'Find and Replace' function in Excel.  

Examples of what is will be removed and a screenshot link are listed below.

         *  'b', which was at the beginning of each tweet.
         *  RT, which stood for retweeted was removed.
         *  Removal of the @ sign from each tweet.
         *  http and https, along with any url address that started with '://'.
         *  Or any other '/' followed by text.

https://user-images.githubusercontent.com/7649609/29252625-fd9a728c-8027-11e7-8cb8-e70c1fe4d3af.png
            
Once the tweet data file is cleaned it looks like the following:

https://user-images.githubusercontent.com/7649609/29252722-9cd6186e-8029-11e7-8406-2a01c1841dfd.png

With the data cleaned and ready for sentiment analysis using textblob the following script will be run from the jupyter notebook:

                  import csv
                  from textblob import TextBlob
                  tweetdata = '/home/myspark/tweet_data2.csv'
                  with open(tweetdata, 'r') as csvfile:
                     rows = csv.reader(csvfile)
                     for row in rows:
                        sentence = row[0]
                        blob = TextBlob(sentence)
                        print (sentence)
                        print (blob.sentiment.polarity, blob.sentiment.subjectivity)
                        
The above code borrowed from https://stackoverflow.com/questions/35559199/textblob-sentiment-analysis-on-a-csv-file with some modifcation and syntax error corrections.                        

We now have a file that contains our tweet data along a polarity and subjectivity score attached to each.                    

https://user-images.githubusercontent.com/7649609/29252892-f677c2c0-802c-11e7-91e0-f4fa18434dea.png

# PHASE IV
## Naive Bayes
The textblob package will be used once again for our text classification. Textblob will allow us to incorporate a naive bayes classifier very simply and efficiently.
Since we have our data set cleaned and labled all we have to do is split the data into train and test sets using a 70/30 split.
                     
                     import pandas as pd
                     import numpy as np
                     df = pd.read_csv('tweetdata4.csv')
                     df['split'] = np.random.randn(df.shape[0], 1       
                     msk = np.random.rand(len(df)) <= 0.7
                     train = df[msk]
                     test = df[~msk]
                     
 Once we have our train and test sets we can implement the code provided at:
 http://textblob.readthedocs.io/en/dev/classifiers.html
                     
 We obtained an .7873 accuracy rate with a very simple classification piece of code that was provided for us.
 Not bad!!  With a little more tweaking and using a more powerful natural language classification algorithm we could probably obtain a very respectable accuracy rate.

# Conclusion

Sentiment analysis is quite interesting and if time allowed, further analysis would have been included.  Future anaylise would be to include a regression model try and understand what, if any, relationship exists between:
           * Tweets and geographic location.
           * Which tweets, negative or positive, occur at which time of day?
          
The project was an evolution in learning for me, which allowed me understand the live streaming process and the powerful tools that are available to do so, along with gaining knowledge of the textblob package.
Thank you for your time and patience!
           

Sources:

            *   http://docs.tweepy.org/en/v3.5.0/auth_tutorial.html
            *   http://textblob.readthedocs.io/en/dev/quickstart.html#sentiment
            *   http://www.geeksforgeeks.org/twitter-sentiment-analysis-using-python/
            *   http://tech.thejoestory.com/2015/01/python-textblob-sentiment-analysis.html
            *   http://www.awesomestats.in/
            *   https://stackoverflow.com/questions/35559199/textblob-sentiment-analysis-on-a-csv-file
            *   https://github.com/praritlamba/Mining-Twitter-Data-for-Sentiment-Analysis
