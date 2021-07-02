# Working with Ethereum nodes using Managed Blockchain<a name="ethereum-nodes"></a>

## Creating a node<a name="ethereum-create-node"></a>

When you create an Ethereum node, you select the network that the node joins and the configuration details such as the instance type and the Ethereum node type\. Creating an Ethereum node in Amazon Managed Blockchain creates a full Geth node on the selected Ethereum network\. The IAM principal \(user or group\) that you use must have permissions to create nodes and view node information\. For more information, see [Performing all available actions for Ethereum on Managed Blockchain](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-example)\.

Select the following characteristics when you create an Ethereum node:
+ **Blockchain network** – Managed Blockchain supports the following public Ethereum networks:
  + **Mainnet** – This is the primary public Ethereum production blockchain proof\-of\-work network\. Transactions on mainnet have actual value \(they have real costs\) and occur on the distributed ledger\.
  + **Rinkeby** – This is a public proof\-of\-authority testnet for Go Ethereum \(Geth\) clients\. Ether on this network has no real monetary value\.
  + **Ropsten** – This is a public proof\-of\-work testnet, so operations on this network closely resemble those on mainnet\. Ether on this network has no real monetary value\.
+ **Blockchain instance type** – This determines the computational and memory capacity allocated to this node for the blockchain workload\. You can choose more CPU and RAM if you anticipate a more demanding workload for each node\. For example, your nodes may need to process a higher rate of transactions\. Different instance types are subject to different pricing\.
+ **Ethereum node type** – The only node type that is currently supported is **Full node \(Geth\)**\. For more information about node types and Go Ethereum \(Geth\), see [Nodes and clients](https://ethereum.org/en/developers/docs/nodes-and-clients/#advantages-of-different-implementations) in the Ethereum developer documentation\.
+ **Availability Zone** – You can select the Availability Zone to launch the Ethereum node in\. The ability to distribute nodes across different Availability Zones lets you design your blockchain application for resiliency\. For more information, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.

After you create the node, the **Node** details page displays the endpoints that you use to make Ethereum JSON\-RPC API calls from code on a client\. There are separate endpoints for HTTP connections and WebSockets connections\. For more information about sending API calls to an Ethereum node on Managed Blockchain to interact with smart contracts, see [Using the Ethereum JSON\-RPC API with Amazon Managed Blockchain](ethereum-json-rpc.md)\.

### To create an Ethereum node using the AWS Management Console<a name="ethereum-create-node.con"></a>

1. Open the Managed Blockchain console at [https://console\.aws\.amazon\.com/managedblockchain/](https://console.aws.amazon.com/managedblockchain/)\.

1. Choose **Join public network**\.

1. Choose the **Blockchain network** for the node to join according to the preceding guidelines\.

1. Choose a **Blockchain instance type** suitable for your application\. In general, choose an instance type with more CPU and RAM if your nodes need to process a higher rate of transactions more efficiently\.

1. For **Ethereum node type**, choose **Full node \(Geth\)**\.

1. Choose **Create node**\.

   Managed Blockchain provisions and configures the node for you\. The length of this process depends on many variables\. It may take a few minutes for nodes on testnets, and up to an hour or more for nodes on mainnet\.

### To create an Ethereum node using the AWS CLI<a name="ethereum-create-node.cli"></a>

Use the `create-node` command, as shown in the following example\. Replace the value of `--network-id`, `InstanceType`, and `AvailabilityZone` as appropriate\.

```
aws managedblockchain create-node \
  --node-configuration '{"InstanceType":"bc.t3.large","AvailabilityZone":"us-east-1a"}' \
  --network-id n-ethereum-rinkeby
```

Ethereum public networks have the following network IDs:
+ `n-ethereum-mainnet`
+ `n-ethereum-rinkeby`
+ `n-ethereum-ropsten`

The command returns the node ID, as shown in the following example\.

```
{
    "NodeId": "nd-RG3GM4U7HFFHHHGJHHU7UNPCLU"
}
```

## Viewing node details<a name="ethereum-node-information"></a>

After you create a node, you can view administrative properties for each node that your AWS account owns, such as the endpoints to use for Ethereum JSON\-RPC API calls for WebSockets and HTTP, the node status, and important performance metrics for the node\. The IAM principal \(user or group\) that you use must have permissions to list and get node information\. For more information, see [Identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

Basic information such as the Managed Blockchain instance type, Availability Zone, and creation date, are available for the node, along with the following important properties:
+ **Status**
  + **Creating**

    Managed Blockchain is provisioning and configuring the Managed Blockchain instance for the node\. Creation time depends on many factors\. Nodes on testnets typically take a few minutes\. Nodes on mainnet may take an hour or more\.
  + **Available**

    The node is running and available on the network\.
  + **Unhealthy**

    Managed Blockchain detected a problem and is automatically replacing the blockchain instance on which the node runs\. Nodes in an unhealthy state typically return to an available state in approximately five minutes\.
  + **Failed**

    The node has an issue that has caused Managed Blockchain to add it to the deny list on the network\. This usually indicates that the node has reached memory or storage capacity\. As a first step, we recommend that you delete the instance and provision an instance type with more capability\.
  + **Create Failed**

    The node could not be created with the Managed Blockchain instance type and the Availability Zone specified\. We recommend trying another availability zone, a different instance type, or both\.
  + **Deleting**

    The node is being deleted\.
  + **Deleted**

    The node has been deleted\. See the previous item for possible reasons\.
+ **Endpoints**

  Endpoints are used to make Ethereum JSON\-RPC API calls to the node\. Managed Blockchain assigns unique endpoints when it creates the node\. Nodes support connection over HTTP and WebSockets\. You use a different endpoint for each connection\. For more information, see [API call examples](ethereum-json-rpc.md#ethereum-api-examples)\.

### To view Ethereum node information using the AWS Management Console<a name="ethereum-node-information.con"></a>

1. Open the Managed Blockchain console at [https://console\.aws\.amazon\.com/managedblockchain/](https://console.aws.amazon.com/managedblockchain/)\.

1. If the console doesn't open to the **Networks** list, choose **Networks** from the navigation pane\.

1. Choose the **Name** of the Ethereum network that the node belongs to from the list\.

1. On the network details page, under **Nodes**, choose the **Node ID**\.

1. The node details page displays key properties and metrics for the node, as shown in the example that follows\.  
![\[Ethereum node details in the Managed Blockchain console.\]](http://docs.aws.amazon.com/managed-blockchain/latest/ethereum-dev/images/ethereum-node-details.png)

### To view Ethereum node information using the AWS CLI<a name="ethereum-node-information.cli"></a>

Use the `get-node` command to view Ethereum node information, as shown in the following example\. Replace the value of `--network-id` and `--node-id` as appropriate\.

```
aws managedblockchain get-node \
  --network-id n-ethereum-rinkeby \
  --node-id nd-RG3GM4U7HFFHHHGJHHU7UNPCLU
```

The command returns output that includes the node's `HttpEndpoint`, `WebSocketEndpoint`, and other key properties, as shown in the following example\.

```
{
    "Node": {
        "NetworkId": "n-ethereum-rinkeby",
        "Id": "nd-RG3GM4U7HFFHHHGJHHU7UNPCLU",
        "InstanceType": "bc.t3.large",
        "AvailabilityZone": "us-east-1a",
        "FrameworkAttributes": {
            "Ethereum": {
                "HttpEndpoint": "nd-rg3gm4u7hffhhhgjhhu7unpclu.ethereum.managedblockchain.us-east-1.amazonaws.com",
                "WebSocketEndpoint": "nd-rg3gm4u7hffhhhgjhhu7unpclu.wss.ethereum.managedblockchain.us-east-1.amazonaws.com"
            }
        },
        "Status": "CREATING",
        "CreationDate": "2021-06-25T20:10:18.555000+00:00",
        "Tags": {},
        "Arn": "arn:aws:managedblockchain:us-east-1:111122223333:nodes/nd-RG3GM4U7HFFHHHGJHHU7UNPCLU"
    }
}
```

## Deleting a node<a name="ethereum-delete-node"></a>

When you delete an Ethereum node from Managed Blockchain, all resources stored on that node are immediately deleted\. The IAM principal \(user or group\) that you use must have permissions to delete nodes\. For more information, see [Performing all available actions for Ethereum on Managed Blockchain](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-example)\.

### To delete an Ethereum node using the AWS Management Console<a name="ethereum-delete-node.con"></a>

1. Open the Managed Blockchain console at [https://console\.aws\.amazon\.com/managedblockchain/](https://console.aws.amazon.com/managedblockchain/)\.

1. If the console doesn't open to the **Networks** list, choose **Networks** from the navigation pane\.

1. Choose the **Name** of the Ethereum network that the node belongs to from the list\.

1. On the network details page, under **Nodes**, select the **Node ID** and then choose **Delete**\.

### To delete an Ethereum node using the AWS CLI<a name="ethereum-delete-node.cli"></a>

Use the `delete-node` command to delete an Ethereum node, as shown in the following example\. Replace the value of `--network-id` and `--node-id` as appropriate\.

```
aws managedblockchain delete-node \
  --network-id n-ethereum-rinkeby \
  --node-id nd-RG3GM4U7HFFHHHGJHHU7UNPCLU
```