# AWS-Comprehend-Twitter

![1](https://user-images.githubusercontent.com/37382927/96356103-df2bd100-109e-11eb-8230-108d0b215e2a.png)

This is a project that I recently worked on which allows one to stream and analyze the sentiment of live tweets from Twitter based on a certain keyword such as "bitcoin." It involves a working relationship between Twitter, Python and Amazon Web Services. This requires one to have a Twitter Developer account and, of course, an AWS account (Free-Tier works fine).  The flow goes as follow:

  1. Tweets are pulled using a Python code running on AWS Cloud9 and fed into AWS Kinesis Firehose.  
  2. AWS Kinesis Firehose then passes the tweets to an AWS Lambda function. <br>
  3. Lambda, running a Python code, will then pass the tweets into AWS Comprehend.<br>
  4. AWS Comprehend returns the sentiment analysis as Positive, Negative and Neutral.<br>
  5. The sentiment analysis results are also sent into AWS Elastic Search and visualzed on AWS Kibana like the following: 

Positive Sentiment:

![Positive](https://user-images.githubusercontent.com/37382927/96356280-0d121500-10a1-11eb-97b3-f168164cd3d2.png)

Negative Sentiment:

![Negative](https://user-images.githubusercontent.com/37382927/96356288-1ac79a80-10a1-11eb-90d7-9f45346b6585.png)

Netural Sentiment:

![Neutral](https://user-images.githubusercontent.com/37382927/96356284-156a5000-10a1-11eb-8553-de182eb86640.png)

Mixed Sentiment:

![Mixed](https://user-images.githubusercontent.com/37382927/96356289-1c915e00-10a1-11eb-95e2-9ccf1aef3fc8.png)

STEPS:<br>
* Please note that the following steps assume that you have a basic working knowledge of AWS Console and services such as Cloud9, Lambda, Elastic Searh, etc.  Also please note that although this is run on AWS Free Tier Account, leaving services running (such as Elastic Search) will incurr costs so please turn off services after testing. 

1. Sign up for a Twitter Developer account via https://developer.twitter.com/en
    - This step is important in order to get access to the Twitter API (500,000 Tweets allowed per month)
    - Apply for a student version access (answer a few questions regarding your account and usage- in my case it was a student project)
    - Once approved you will be given a Twitter <i>API Key</i> and <i>API Secret Key</i>.
    - You will then have to a create and name your new App ( AWSComprehend-GA) and which will be provided with an <i>Access Token</i> and            <i>Secret Access Token</i>.
    - Update the 'api_auth.cfg' file that I have included in this repository with your:
      <i>API Key, API Secret Key, Access Token</i> and <i>Secret Access Token</i>

<img width="1680" alt="Twitter 1" src="https://user-images.githubusercontent.com/37382927/97384578-3561f680-188d-11eb-931a-95efe2acc64d.png">

<img width="1680" alt="Twitter 2" src="https://user-images.githubusercontent.com/37382927/97384821-c2a54b00-188d-11eb-89d6-6c6564ff35b0.png">

2. Log onto you AWS account - preferebly using an IAM alias
    - Update the attached api_auth.cfg with your AWS <i>Access Key ID</i> and <i>Secret Access Key</i>
    - When I ran the project certain elements could not be executed in Northen California.  Thus I set the region to US West (Oregon)

3. Create a Cloud9 IDE instance (i.e. twitterBot) using:<br>
    - Keep Default options:
      - <i> t2.micro (1 GiB RAM + 1 vCPU)
      Free-tier eligible. Ideal for educational users and exploration.</i>
      - Use your default VPC and VPN
     
    - Load the two files streaming-twitter.py and api_auth.cfg
    
4. Setup AWS Lambda Function which will be used via AWS Kinesis
    - Used attached lambda_function script attached

5. Setup AWS Kinesis Firehose and use Lambda function created in step 4 as its "Data Transformation"

6. Setup AWS Elastic Search

7. Run twitter-streaming.py script on Cloud9
    - Run the code for about 15 minutes to collecte data on the provided keyword
    
8. Launch AWS Kibana and analyze the collected data 
