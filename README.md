# AWS-Comprehend-Twitter

![1](https://user-images.githubusercontent.com/37382927/96356103-df2bd100-109e-11eb-8230-108d0b215e2a.png)

This is a project that I recently worked on which allows one to stream and analyze the sentiment of live tweets from Twitter based on a certain keyword such as "bitcoin." It involves a working relationship between Twitter, Python and Amazon Web Services. The flow goes as follow:

-Tweets are pulled using a Python code and fed into AWS Kinesis Firehose.  
-AWS Kinesis Firehose then passes the tweets to an AWS Lambda function. 

-Lambda, running a Python code, will then pass the tweets into AWS Comprehend.

-AWS Comprehend returns the sentiment analysis as Positive, Negative and Neture.

-The sentiment analysis results are also sent into AWS Elastic Search and visualzed on AWS Kibana like the following: 

Positive Sentiment:

![Positive](https://user-images.githubusercontent.com/37382927/96356280-0d121500-10a1-11eb-97b3-f168164cd3d2.png)

Negative Sentiment:

![Negative](https://user-images.githubusercontent.com/37382927/96356288-1ac79a80-10a1-11eb-90d7-9f45346b6585.png)

Netural Sentiment:

![Neutral](https://user-images.githubusercontent.com/37382927/96356284-156a5000-10a1-11eb-8553-de182eb86640.png)

Mixed Sentiment:

![Mixed](https://user-images.githubusercontent.com/37382927/96356289-1c915e00-10a1-11eb-95e2-9ccf1aef3fc8.png)

STEPS:
1- Sign up for a Twitter Developer account via https://developer.twitter.com/en
  - Create a Project App (answer a few questions regarding your app- in my case it was student project)
  - Make sure to save your API Key and API Secret Key in a seperate file

2- Log
