# Data protection for Ethereum on Amazon Managed Blockchain<a name="ethereum-data-protection"></a>

Data encryption helps prevent unauthorized users from reading data from a blockchain network and the associated data storage systems\. This includes data saved to persistent media, known as *data at rest*, and data that may be intercepted as it travels the network, known as *data in transit*\.

## Encryption at rest<a name="managed-blockchain-encryption-at-rest"></a>

Amazon Managed Blockchain offers fully managed encryption at rest\. Managed Blockchain encryption at rest provides enhanced security by encrypting all data at rest on Ethereum nodes using Managed Blockchain owned encryption keys in AWS Key Management Service \(AWS KMS\)\. This functionality helps reduce the operational burden and complexity involved in protecting sensitive data\. With encryption at rest, you can build security\-sensitive blockchain applications that meet strict encryption compliance and regulatory requirements\.

Encryption at rest integrates with AWS KMS for managing the encryption key that is used to encrypt your tables\. A Managed Blockchain owned key is used to encrypt data at rest by default at no additional cost\. No configuration is required\. Using an AWS managed encryption key is not supported\. For more information, see [AWS owned keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk) in the *AWS Key Management Service Developer Guide*\.

## Encryption in transit<a name="managed-blockchain-encryption-in-transit"></a>

By default, Managed Blockchain uses an HTTPS/TLS connection to encrypt all data transmitted from a client computer running the AWS CLI to AWS service endpoints\.

You don't need to do anything to enable the use of HTTPS/TLS\. It is always enabled unless you explicitly disable it for an individual AWS CLI command by using the \-\-no\-verify\-ssl command line option\.