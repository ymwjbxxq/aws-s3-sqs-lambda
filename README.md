# FROM S3 To SQS To Lambda #

Thanks to events, you can concatenate many serverless services together with minimal effort.
Many patterns are presented [here](https://serverlessland.com/patterns), and today, I want to play with two of them:

* [S3 to SQS](https://serverlessland.com/patterns/s3-sqs) 
* [SQS to Lambda](https://serverlessland.com/patterns/sqs-lambda)

![picture](https://bitbucket.org/DanBranch/s3-sqs-lambda/downloads/s3_to_sqs_to_lambda.png)

### Why this repository? ###

Usually, we work with multiple accounts and environments like test/stage/prod and need to deploy from CI like GitHub or GitLab or similar. 
Perhaps you need to deploy only a change on a specific service and avoid deploying all, so this could be the reason for a split.
For example, I like to split the .yml files based on resources, in this case, S3, SQS and Lambda.

I show how these two patterns will look like in multiple files in this repository. 

Under the resources directory, you will find a .yml file for each service. 
I have added extra information like:

* Tags
* Mappings
* Policies

I tried to make it more "real", but it is what you find on the serverlessland.com patterns page if you strip it all.

The message inside the SQS body looks like this:

```javaScript
{
    "eventVersion": "2.1",
    "eventSource": "aws:s3",
    "awsRegion": "eu-central-1",
    "eventTime": "2021-05-08T09:31:57.896Z",
    "eventName": "ObjectRemoved:Delete",
    "userIdentity": {
        "principalId": "AWS:xxxxxxx:xxxxx"
    },
    "requestParameters": {
        "sourceIPAddress": "xxxxxxxxxxx"
    },
    "responseElements": {
        "x-amz-request-id": "xxxxxx",
        "x-amz-id-2": "xxxxxxxx"
    },
    "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "xxxxxxxxxx",
        "bucket": {
            "name": "mybucket-test",
            "ownerIdentity": {
                "principalId": "xxxxxxxx"
            },
            "arn": "arn:aws:s3:::mybucket-test"
        },
        "object": {
            "key": "dir/filename.png",
            "sequencer": "xxxxx"
        }
    }
}
```