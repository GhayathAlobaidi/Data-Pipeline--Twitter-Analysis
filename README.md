# AWS-Comprehend-Twitter

![1](https://user-images.githubusercontent.com/37382927/96356103-df2bd100-109e-11eb-8230-108d0b215e2a.png)

This is a project that I recently worked on which enables one to stream and analyze the sentiment of live tweets from Twitter based on a certain keyword such as "bitcoin." It involves a working relationship between Twitter, Python and Amazon Web Services. This requires one to have a Twitter Developer account and, of course, an AWS account (Free-Tier). The flow of the project is:

  1. Tweets are pulled from the Twitter API after verifying credentials using a Python code running on AWS Cloud9.
  2. The code feeds the stream of tweets into AWS Kinesis Firehose. <br>
  2. AWS Kinesis Firehose then passes the tweets to an AWS Lambda function. <br>
  3. Lambda, also running a Python code, will then pass the tweets into AWS Comprehend.<br>
  4. AWS Comprehend analyzes the tweets and returns the sentiment analysis as Positive, Negative, Mixed and Neutral.<br>
  5. The sentiment analysis results are also sent into AWS Elastic Search and visualzed on AWS Kibana as the following: 

Positive Sentiment:

![Positive](https://user-images.githubusercontent.com/37382927/96356280-0d121500-10a1-11eb-97b3-f168164cd3d2.png)

Negative Sentiment:

![Negative](https://user-images.githubusercontent.com/37382927/96356288-1ac79a80-10a1-11eb-90d7-9f45346b6585.png)

Netural Sentiment:

![Neutral](https://user-images.githubusercontent.com/37382927/96356284-156a5000-10a1-11eb-8553-de182eb86640.png)

Mixed Sentiment:

![Mixed](https://user-images.githubusercontent.com/37382927/96356289-1c915e00-10a1-11eb-95e2-9ccf1aef3fc8.png)

STEPS:<br>
* Please note that the following steps assume that you have a basic working knowledge of AWS Console and services such as Cloud9, Lambda and Elastic Search.  Also, please note that although this project is run on AWS Free Tier Account, leaving services running (such as Elastic Search) will incurr costs. Thus, it's important to turn off services after testing to avoid charges. 

1. Sign up for a Twitter Developer account via https://developer.twitter.com/en
    - This step is important in order to get access to the Twitter API (500,000 Tweets allowed per month).
    - Apply for a student version access (answer a few questions regarding your account and usage- in my case it was a student project).
    - Once approved, you will be given a Twitter <i>API Key</i> and <i>API Secret Key</i>.
    - You will then have to a create and name your new App (in this example: AWSComprehend-GA) which will be provided with an <i>Access Token</i> and <i>Secret Access Token</i>.
    - Update the 'api_auth.cfg' file included in this repository with your:
      <i>API Key, API Secret Key, Access Token</i> and <i>Secret Access Token</i>

<img width="1680" alt="Twitter 1" src="https://user-images.githubusercontent.com/37382927/97384578-3561f680-188d-11eb-931a-95efe2acc64d.png">

<img width="1680" alt="Twitter 2" src="https://user-images.githubusercontent.com/37382927/97384821-c2a54b00-188d-11eb-89d6-6c6564ff35b0.png">

2. Log onto you AWS account using an account that has admin access (preferebly an IAM alias and not your root account).<br>
    - Update the attached <i>api_auth.cfg</i> with your AWS <i>Access Key ID</i> and <i>Secret Access Key</i>. <br>
    - In order to run this project, certain AWS services could not be executed in Northen California.  Thus, I set the region to US West (Oregon).<br>

3. Create a Cloud9 IDE instance (in this example: twitterBot) using default options:<br>
    - t2.micro (1 GiB RAM + 1 vCPU)<br>
      Free-tier eligible. Ideal for educational users and exploration.<br>
    - Amazon Linux<br>
    - Use your default VPC and subnet. <br>
 
<img width="1680" alt="Cloud9 1 copy" src="https://user-images.githubusercontent.com/37382927/97386063-42ccb000-1890-11eb-83d8-2d306d0bb84d.png">

  - Create a new Python script in the Cloud9 environment (File -> New File).
      - Copy and paste the code from the <i>twitter-streaming.py</i> included in this repository.
      - You can rename the file but please remember the name as it will matter when AWS services call on it in the next steps.
      - Note that the variable 'DeliveryStreamName' points to 'twitter-stream' which is the Kinesis Firehose that will be created in Step 4.
      
<img width="1680" alt="Cloud9 2" src="https://user-images.githubusercontent.com/37382927/97385730-8ffc5200-188f-11eb-84cb-4cc75751965e.png">  

  - Create <i>api_auth.cfg</i> file which we have updated in steps 1 and 2.
      - This file holds the credentials that twitter-streaming.py will use to access the Twitter API and AWS services.
      
<img width="1680" alt="Cloud9 3" src="https://user-images.githubusercontent.com/37382927/97385733-912d7f00-188f-11eb-8925-9fee67a833db.png">

  - Use pip to install the modules needed to run twitter-streaming.py:  <br>
      - Install AWS SDK boto3: <i>sudo pip install boto3</i> <br>
      - Install configparser: <i>sudo pip install configparser</i> <br>
      - Install Tweepy <br>
          - Close and open a new bash terminal in Cloud9 in order for this to pip to work <br>
          - <pip install --user tweepy <br>
   
<img width="1680" alt="Cloud9 4" src="https://user-images.githubusercontent.com/37382927/97385735-925eac00-188f-11eb-8063-ea2092fee1fb.png">

<img width="1680" alt="Cloud9 5" src="https://user-images.githubusercontent.com/37382927/97387521-634a3980-1893-11eb-9b67-5d5e198de5a9.png">

4. Create a new Lambda Function which will receive the stream of tweets via Kinesis Firehose and send to Comprehend. <br>
   Use the following configuration: <br>
    - Choose the option: <i>Author from Scratch</i>
    - Name the function the same as the Cloud9 instance name set up in Step 3 (in this example: twitterbot).
    - <i>Runtime: Python 3.8</i>
    - Download <i>lambda_function.py</i> attached in this repository and enter as the code for your new Lambda function.<br>
    
    - After the function is created, Go to Permissions -> edit Role name
          - Edit the Lambda's policy JSON so it will have access to Comprehend
 

5. Setup AWS Kinesis Firehose and use Lambda function created in step 4 as its "Data Transformation." <br>
    - 

6. Setup AWS Elastic Search domain & Kibana (in this example: "twitter") which will receive the sentiment analysis from twitterbot Lambda function. 
   Use the following configuration: <br>
    - t2small instance <br>
    - For the sake of simplicity in this project, we will setup a public domain access policy: <i>iPv4 address :  * </i> and
      we will uncheck "Require HTTPs for all traffic to the domain"
          - Note: This is not recommended for future setups but once again we're doing this for a quick test in this project.
    - Once active, we will modify twitter's access policy so it can access Kibana that we'll setup in the next steps. JSON data
    - Please remember: leaving this service running will eat up your Free-Tier usage and you will incurr costs.  Stop instances and services after running this test. <br>

8. Run twitter-streaming.py script on Cloud9.
    - Run the code for about 15 minutes to collecte data on the provided keyword.
    
7. Launch AWS Kibana and analyze the collected data. <br>
