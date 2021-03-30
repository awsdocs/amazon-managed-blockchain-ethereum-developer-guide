# Setting up for Ethereum on Managed Blockchain<a name="ethereum-setting-up"></a>

## Sign up for AWS<a name="sign-up-for-aws"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all AWS services, including Amazon Managed Blockchain\. You are charged only for the services that you use\.

With Ethereum on Managed Blockchain, you pay for the node, the storage you use, and the number of requests between the node and the network\. 

If you have an AWS account already, move on to the next step\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Create an IAM user with appropriate permissions<a name="create-an-iam-user"></a>

To create and work with Ethereum resources in Managed Blockchain, you need an AWS Identity and Access Management \(IAM\) principal \(user or group\) with permissions that allow necessary Managed Blockchain actions on those resources, such as creating or deleting nodes\.

An IAM principal is also required to make Ethereum JSON\-RPC API calls\. All Ethereum JSON\-RPC API calls to an Ethereum node on Managed Blockchain are authenticated using the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. This means that only authorized IAM principals in the AWS account that created the node can interact with it using the API\. AWS credentials \(an access key ID and secret access key\) must be provided with the call\. IAM policies do not apply to API calls and can not be used to allow or deny Ethereum API access to a node\. IAM permissions policies only apply to operations related to node creation, deletion, and management\.

For information about creating an IAM user, see [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\. For more information about attaching a permissions policy to a user, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html)\. For an example of a permissions policy that you can use to give a user permission to work with Ethereum on Managed Blockchain resources, see [Performing all available actions for Ethereum on Managed Blockchain](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-example)\.