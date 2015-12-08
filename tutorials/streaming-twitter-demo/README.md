- [Twitter Streaming Demo](#twitter-streaming-demo)
- [Requirements](#requirements)
- [Specifying Tweet Categories](#specifying-tweet-categories)
- [Starting/Stopping the demo] (#to-start-the-demo)
- [Checking the data] (#viewing-the-data)
- [Visualizing the data] (#visualizing-the-data)

# Twitter Streaming Demo

The Twitter Streaming Demo pulls in live Twitter feeds and pushes them out to Apache Kafka. Actian DataFlow picks up these messages from Kafka every few seconds, enriches them, and stores the result in Actian Vector in Hadoop (hereafter Vector-H). The stored data can then be analyzed using various tools.

## Requirements

The demo requires the following software installed on your system:

> NOTE: If you provisioned Actian Vortex through the Actian Management Console
> (AMC) and selected the Twitter demo option at the provision screen,
> all these components (except the Twitter Dev Access Token) are already
> installed.

 - Actian Vector-H, which was installed with Vortex
 - Apache Kafka Server process (also known as a broker) on the Vector-H master node. You can usually check this through your Hadoop provider's management interface (for example, Apache Ambari in the case of Hortonworks Data Platform).
 - zip utility (can be installed through yum install zip as root)
 - Tweepy, a Twitter library for Python (can be installed with pip install tweepy)
 - A Twitter Dev Access Token (see the following section)

There might be some other packages required and the demo start script will make the necessary checks and prompt you in case something else needs to be installed.

### Obtaining and Using a Twitter Dev Access Token

Pulling in live data from Twitter requires a Twitter Dev Access Token, which consists of four required values. To obtain the token, you must have a Twitter account. To obtain the token:
 
 1. Go to https://apps.twitter.com and sign in with your Twitter account. You are logged in. 
 2. Click "Create an app". The Create an application page is displayed.
 3. Fill in the details for the application, including the following:
- Name: Give your application a unique name.
- Description: A short description.
- Web Site: Your web site (you can also use a placeholder).
- Callback URL: Leave blank

 Accept the agreement, and then click "Create your Twitter application".
The application page is displayed. 
 4. Select the "Keys and Access Tokens" tab. 
 5. Scroll down and click "Create my access token".
The token is created. 
 6. Note the following four fields and their values. You will need these values for the demo to run.
 - Consumer Key
 - Consumer Secret
 - Access Token
 - Access Token Secret

The authentication credentials are set up. We now need to update the demo to use the access token. 

### To use the token

1.	Log in to the Vector-H master node as user actian, using the password you chose at installation time.
2.	Switch to the demo directory: 
> cd /opt/Actian/Vortex/demos/streaming_twitter_demo/

3.	Open the twitterAuth.py file with your favorite editor and update it with the four values of the token from your newly created Twitter application. Save the changes and quit the editor. 

You are now ready to start the demo.

## Specifying Tweet Categories

To control the amount of data, tweets from specific categories are pulled in. The default categories are "bigdata" and "hadoop". If you are interested in other categories, you can edit the file topics.txt and add a new line for each topic that you are interested in. 

> NOTE: You can edit this file while the demo is running; the topics will be
> picked up or removed dynamically

## Starting and Stopping the Demo

### To start the demo
Execute the start demo script as user actian: 

> ./start_demo.sh.

The database and tables are created and the necessary processes are started. 

### To stop the demo

Execute the stop demo script as user actian: 

> ./stop_demo.sh.

The demo stops after a few seconds.

## Viewing the data
The processed tweets are stored in a table called tweets in the database stream. 

To quickly check if the data is being stored 

1.	Start the demo.
2.	Source the actian environment: 

> source /home/actian/.ingVHsh

3.	Start the SQL terminal monitor to get a stored tweets summary: 

> sql -udemo stream <
> /opt/Actian/Vortex/demos/streaming_twitter_demo/dataflow_workflows/count.sql

The row count should be steadily increasing.
## Visualizing the Data
Visualizing the data makes it easier to look at the tweets and identify tweet trends. The demo comes with a pre-built Tableau workbook that contains a dashboard to visualize all the stored information.  

To visualize the data

1.	Open the provided Tableau workbook using Tableau Desktop.
Tableau displays a dashboard, which is based on the Twitter data stored in Vector in Hadoop. 
2.	Refresh the dashboard to change the visualization. 
3.	Click a category in the dashboard to filter the other views based on that category.

Tweet something in the category you are monitoring and it should appear on the dashboard in a few seconds!
