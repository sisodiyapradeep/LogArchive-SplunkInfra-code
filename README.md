**S3 SQS Lambda Firehose Method**
This repo is for individuals and organizations looking to get better visibility, observability, and monitoring in their AWS account or AWS accounts.  There are sets of CloudFormation templates here designed to help get data related to AWS accounts and services to Splunk for analysis and alerting. This code will dpeloy all the resources required to send the Logs data to Splunk.

**S3-SQS-Lambda-Firehose Method Overview**

Many services can send logs to S3 buckets, including internal AWS services such as CloudTrail. Using this method of taking objects (files) with events from S3, sending object metadata to SQS, using a Lambda function to retrieve the object (file) in S3, sending the events to Firehose, then sending them to Splunk can be a cost-effective, scalable, serverless, event-driven way to send events to Splunk.

**Architecture**
(<img width="641" alt="Splunk_Logs_Ingestion" src="https://github.com/sisodiyapradeep/LogArchive-SplunkInfra-code/assets/51401756/cea3a8b7-0b1d-4b5f-bf82-c6fb51c84555">
)

**S3-SQS-Lambda-Firehose Method Detailed steps:**

Object is placed into log S3 bucket
SQS message is sent SQS queue from S3 with object (file) metadata
Lambda function polls for the SQS messages
Lambda function parses the SQS messages and for each message (each object placed into the S3 bucket): 4a. Retrieves the object metadata (bucket name and key) 4b. Validates the file type is supported by the function 4c. Downloads the object (file) from the S3 bucket 4d. Uncompresses the downloaded file if it is compressed 4e. Reads the file contents into memory 4f. Breaks up the file contents into separate events 4g. For each event: 4g-1. Gets the timestamp of the event 4g-2. Constructs the record to send to Firehose from the event and timestamp 4g-3. Sends the events to Firehose 4h. Deletes the file with the events
