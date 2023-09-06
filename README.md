S3 SQS Lambda Firehose Method
S3-SQS-Lambda-Firehose Method Overview
Many services can send logs to S3 buckets, including internal AWS services such as CloudTrail. Using this method of taking objects (files) with events from S3, sending object metadata to SQS, using a Lambda function to retrieve the object (file) in S3, sending the events to Firehose, then sending them to Splunk can be a cost-effective, scalable, serverless, event-driven way to send events to Splunk.

Visual overview: 
S3-SQS-Lambda-Firehose Method Detailed steps:
Detailed understanding of how this method works isn't necessary to implement it.

Object is placed into log S3 bucket
SQS message is sent SQS queue from S3 with object (file) metadata
Lambda function polls for the SQS messages
Lambda function parses the SQS messages and for each message (each object placed into the S3 bucket): 4a. Retrieves the object metadata (bucket name and key) 4b. Validates the file type is supported by the function 4c. Downloads the object (file) from the S3 bucket 4d. Uncompresses the downloaded file if it is compressed 4e. Reads the file contents into memory 4f. Breaks up the file contents into separate events 4g. For each event: 4g-1. Gets the timestamp of the event 4g-2. Constructs the record to send to Firehose from the event and timestamp 4g-3. Sends the events to Firehose 4h. Deletes the file with the events
