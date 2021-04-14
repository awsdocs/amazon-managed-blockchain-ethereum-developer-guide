# Logging Amazon Managed Blockchain API calls using AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Managed Blockchain is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Managed Blockchain\. CloudTrail captures all API calls for Managed Blockchain as events\. The calls captured include calls from the Managed Blockchain console and code calls to the Managed Blockchain API operations\.

If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Managed Blockchain\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Managed Blockchain, the IP address from which the request was made, who made the request, when it was made, and additional details\.

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)\.

## Managed Blockchain information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Managed Blockchain, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing events with CloudTrail Event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\.

For an ongoing record of events in your AWS account, including events for Managed Blockchain, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following:
+ Overview for [Creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail supported services and integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html)
+ [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/configure-sns-notifications-for-cloudtrail.html)
+ [Receiving CloudTrail log files from multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All Managed Blockchain actions are logged as management events by CloudTrail and are documented in the [Amazon Managed Blockchain API Reference](https://docs.aws.amazon.com/managed-blockchain/latest/APIReference/)\. For example, calls to the `CreateNode`, `GetNode` and `DeleteNetwork` actions generate entries in the CloudTrail log files\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following:
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Understanding Managed Blockchain log file entries<a name="understanding-managed-blockchain-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. Managed Blockchain supports logging management events\. For more information, see [Logging management events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-events-with-cloudtrail.html) in the *AWS CloudTrail User Guide*\. Managed Blockchain also supports logging data events for Ethereum JSON\-RPC calls over HTTP or WebSockets\. For more information, see [Using CloudTrail to track Ethereum JSON\-RPC calls](#ethereum-jsonrpc-logging)\.

CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\. 

**Example – Management event log entry**  
The following example shows a CloudTrail management event log entry that demonstrates the `GetNode` action\.  

```
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "ABCD1EF23G4EXAMPLE56:carlossalazar",
    "arn": "arn:aws:sts::111122223333:assumed-role/Admin/carlossalazar",
    "accountId": "111122223333",
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "webIdFederationData": {},
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "2020-12-10T05:36:38Z"
      }
    }
  },
  "eventTime": "2020-12-10T05:50:48Z",
  "eventSource": "managedblockchain.amazonaws.com",
  "eventName": "GetNode",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "198.51.100.1",
  "userAgent": "aws-cli/2.0.7 Python/3.7.3 Linux/5.4.58-37.125.amzn2int.x86_64 botocore/2.0.0dev11",
  "requestParameters": {
    "networkId": "n-ethereum-ropsten",
    "nodeId": "nd-6EAJ5VA43JGGNPXOUZP7Y47E4Y"
  },
  "responseElements": null,
  "requestID": "1e2xa3m4-56p7-8l9e-0ex1-23456a78m90p",
  "eventID": "ex12345a-m678-901p-23e4-567ex8a9mple",
  "readOnly": true,
  "eventType": "AwsApiCall",
  "recipientAccountId": "111122223333"
}
```

## Using CloudTrail to track Ethereum JSON\-RPC calls<a name="ethereum-jsonrpc-logging"></a>

You can track JSON\-RPC as *data events * using CloudTrail\. Data events are not logged by default when you create a trail\. To record Ethereum JSON\-RPC calls as CloudTrail data events, you must explicitly add the supported resources or resource types for which you want to collect activity to a trail\. Managed Blockchain supports adding data events using the AWS CLI\. For more information, see [Log events by using advanced selectors](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html#creating-data-event-selectors-advanced) in the *AWS CloudTrail User Guide*\.

To log data events for a trail, run the [put\-event\-selectors](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/put-event-selectors.html) command after you create the trail\. Use the `--advanced-event-selectors` option to specify the data events to log\. The following example demonstrates a `put-event-selectors` command that logs all Ethereum JSON\-RPC calls for a trail named `my-ethereum-trail` in the `us-east-1` Region\.

```
aws cloudtrail put-event-selectors \
--region us-east-1 \
--trail-name my-ethereum-trail \
--advanced-event-selectors '[{
    "Name": "MyDataEventSelectorForEtherumJsonRpcCalls",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::ManagedBlockchain::Node"] } ]}]'
```

**Example – Data event log entry**  
The following example demonstrates a CloudTrail data event log entry for an Ethereum JSON\-RPC call, `web3_clientVersion`, from a client to a node in Managed Blockchain\.  

```
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "ABCD1EF23G4EXAMPLE56:carlossalazar",
    "arn": "arn:aws:sts::111122223333:assumed-role/Admin/carlossalazar",
    "accountId": "111122223333",
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "webIdFederationData": {},
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "2020-12-11T16:51:12Z"
      }
    }
  },
  "eventTime": "2020-12-11T19:56:36Z",
  "eventSource": "managedblockchain.amazonaws.com",
  "eventName": "web3_clientVersion",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "198.51.100.1",
  "userAgent": "python-requests/2.23.0",
  "requestParameters": {
    "id": 67,
    "jsonrpc": "2.0",
    "method": "web3_clientVersion",
    "params": []
  },
  "responseElements": {
    "result": "Geth/v1.9.24-stable-cc05b050/linux-amd64/go1.15.5",
    "id": 67,
    "jsonrpc": "2.0"
  },
  "requestID": "1e2xa3m4-56p7-8l9e-0ex1-23456a78m90p",
  "eventID": "ex12345a-m678-901p-23e4-567ex8a9mple",
  "readOnly": false,
  "eventType": "AwsApiCall",
  "recipientAccountId": "111122223333"
}
```