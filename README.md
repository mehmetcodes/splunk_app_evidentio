#Evident.io app for Splunk

Evident.io App for Splunk Version 1.2

This Splunk App is for people who have an account with Evident.io (http://www.evident.io) and want to gather their alerts into their on-premis Splunk or Splunk Cloud instance. Unlike the previous version, you will no longer need to have the App for AWS installed in order to pull the data from Evident. This solution will now use an AWS lambda function and the Splunk HTTP Event Collector (HEC). Here is what you will need in order to get this solution up and running in your AWS environment:
	
	- A Configured Evident.io account that is sending data to an SNS topic in AWS
	- Access to creating a Lambda function in your AWS account
	- Splunk Token from the HTTP Event Collector (See below for links to documentation on HTTP Event Collector)

Reference Material :

	- http://dev.splunk.com/view/event-collector/SP-CAAAE7T - Video on setting up a Lambda function with Splunk.
	- http://docs.splunk.com/Documentation/Splunk/6.4.1/Data/UsetheHTTPEventCollector - Setup Splunk HTTP Event Collector 
	- https://gist.github.com/glennblock/0d5e6384d93449d3e7c6 - Information on how to properly setup props.conf

Here's a list of files that are added to your $SPLUNK_HOME/etc/apps/splunk_app_evidentio folder:

	- default/props.conf

##Splunk Cloud Customers 
Contact Support to have the Evident.io App for Splunk installed on your environment.  You will also need to specify in the case that you need to enable the HTTP Event Collector.  They will send you the URL and token that you will need for the following steps. Make sure to specify the sourcetype=aws-evidentio and the index=main.)

Steps to configure your deployment :
- Step 1 - Download and install the Evident.io App for Splunk from Splunkbase (https://splunkbase.splunk.com/app/3204/)
- Step 2 - Create HTTP Event Collector (HEC) Token Creation

	- Log into your Splunk instance and click on Settings -> Data Inputs -> HTTP Event Collector.  Click on Create New Token. Name the input "Evident.io Input" then click Next. 
	
	- Select the correct source type "aws:evidentio", leave the Default Index as "Default." Click Review, then Submit. 
	- Copy the Token Value and keep it ready. 
	- Finally, enable the token by clicking on "Global Settings" and enable the tokens.

		- Now create a new input and generate the associated token.  Once you have this token, copy it and have it available for the next step. 

- Step 2 - Create the AWS Lambda Function
	- Log into your AWS Console and navigate to the Lambda Function service. 
	- Click on "Create a Lambda function"
	- In the Filter box, type in "splunk-logging" Click on the result.
		- Give your Lambda function a name, I would suggest using "GetDataFromEvidentio"		
	- With the lambda function setup, now go to your SNS topics and subscribe your newly created lambda function to the topic collecting data from Evident.io.
	
- Step 3 - Testing 
	- Log into your Evident.io account and send out test alerts by using the alerting feature in Evident.io. 
