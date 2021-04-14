# Key concepts: Ethereum on Amazon Managed Blockchain<a name="ethereum-concepts"></a>

You can use Ethereum on Amazon Managed Blockchain to quickly provision [Ethereum nodes](https://ethereum.org/en/developers/docs/nodes-and-clients/) and join them to the public Ethereum *mainnet* or popular public *testnets*\. Ethereum nodes on a network collectively store the state of the Ethereum blockchain, verify transactions, and participate in consensus to change the state of the blockchain\.

An Ethereum node allows you to develop and use decentralized applications \(dapps\) that interact with an Ethereum blockchain\. The "back\-end" of a dapp is a *smart contract* that runs in a decentralized way across all the nodes joined to an Ethereum network\. Anyone joined to the network can develop and deploy a smart contract that adds functionality\.

The "front end" of a dapp uses [methods in the Ethereum JSON\-RPC API](https://eth.wiki/json-rpc/API#json-rpc-methods) to interact with the Ethereum network through your Ethereum node in Managed Blockchain, reading data and writing transactions\. With Ethereum on Managed Blockchain, your front\-end app can use an HTTP or WebSockets connection to make API calls\. Only users in the AWS account that owns the node can make API calls\. Calls over HTTP and WebSockets are authenticated using the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.

**Important**  
Amazon Managed Blockchain helps customers provision Ethereum nodes\. You are responsible for the creation, maintenance, and use of your Ethereum Accounts\. You are also responsible for the contents of your Ethereum Accounts, which includes, but is not limited to, Ether \(ETH\) and smart contracts\. AWS is not responsible for any of your smart contracts tested, compiled, deployed or called using Ethereum nodes with Amazon Managed Blockchain\.

This guide assumes that you're familiar with the concepts essential to Ethereum, such as nodes, dapps, transactions, gas, Ether, and others\. Before you deploy a node using Ethereum on Managed Blockchain and develop dapps, we recommend that you review the [Ethereum Development Documentation](https://ethereum.org/en/developers/docs/) and [Mastering Ethereum](https://cypherpunks-core.github.io/ethereumbook/01what-is.html)\.

## Considerations and limitations for Ethereum on Managed Blockchain<a name="ethereum-considerations"></a>

When using Ethereum on Managed Blockchain to host a node on an Ethereum network, consider the following\.
+ **Supported networks**

  Ethereum has a public *mainnet* and several public *testnets* used for development, testing, and proof of concept\. Managed Blockchain supports the following public networks\. Private networks are not supported\.
  + **Mainnet** – This is the primary public Ethereum production blockchain proof\-of\-work network\. Transactions on mainnet have actual value \(they have real costs\) and occur on the distributed ledger\.
  + **Rinkeby** – This is a public proof\-of\-authority testnet for Go Ethereum \(Geth\) clients\. Ether on this network has no real monetary value\.
  + **Ropsten** – This is a public proof\-of\-work testnet, so operations on this network closely resemble those on mainnet\. Ether on this network has no real monetary value\.
+ **Mining not supported**

  Ethereum nodes created using Managed Blockchain do not support mining\.
+ **Different endpoints for WebSockets and HTTP**

  Ethereum on Managed Blockchain supports the Ethereum JSON\-RPC API over HTTP and WebSockets\. Each Ethereum node in Managed Blockchain hosts different endpoints for HTTP and WebSockets\.
+ **Payload limit for API calls**

  WebSockets calls have a 512 KB payload limit\. Some calls may exceed this limit and cause a "message response is too large" error\. Use HTTP for these requests instead of WebSockets\. HTTP requests have a 6 MB payload limit\. Responses that exceed the HTTP also result in a "message response is too large" error\.
+ **Signature Version 4 signing of API calls**

  All Ethereum JSON\-RPC API calls to an Ethereum node on Managed Blockchain are authenticated using the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. This means that only authorized IAM principals in the AWS account that created the node can interact with it using the API\. AWS credentials \(an access key ID and secret access key\) must be provided with the call\. IAM policies do not apply to API calls and can not be used to allow or deny Ethereum API access to a node\. IAM permissions policies only apply to operations related to node creation, deletion, and management\.

  Client credentials should never be embedded in user\-facing applications\. To expose an Ethereum node on Managed Blockchain to anonymous users visiting from trusted web domains, you can set up a separate endpoint in [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) backed by a Lambda function that forwards requests to your node using the proper IAM credentials\.
+ **Only raw transactions are supported**

  Managed Blockchain only supports the use of the `eth_sendRawTransaction` method to submit transactions that update the Ethereum blockchain state\. These transactions must be created and signed using Ethereum private keys outside Managed Blockchain before they are sent\. In other words, Managed Blockchain cannot be used as an Ethereum wallet\. Ethereum transactions and private keys must be generated and stored externally\.
+ **Node limit per account**

  Managed Blockchain supports a maximum of 50 Ethereum nodes per account\.