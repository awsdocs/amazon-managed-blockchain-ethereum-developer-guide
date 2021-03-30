# Using the Ethereum JSON\-RPC API with Amazon Managed Blockchain<a name="ethereum-json-rpc"></a>

This topic provides a listing of Ethereum JSON\-RPC methods that Managed Blockchain supports, followed by code examples that implement JSON\-RPC API calls from clients using either WebSockets or HTTP\.

You use the Ethereum JSON\-RPC API from a client to query smart contract data and submit transactions to an Ethereum node in Managed Blockchain\. The client uses either an HTTP or WebSockets endpoint hosted by the Ethereum node in Managed Blockchain to connect and send the API calls\. You can get the node endpoints using the node details page in the Managed Blockchain console\. For more information, see [Viewing node details](ethereum-nodes.md#ethereum-node-information)\.

Ethereum on Managed Blockchain only supports the `eth_sendRawTransaction` method, which requires that you create and sign the transaction before sending it to the node\. Managed Blockchain does not have any way to sign transactions similar to an Ethereum wallet application\.

All Ethereum JSON\-RPC API calls to an Ethereum node on Managed Blockchain are authenticated using the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. This means that only authorized IAM principals in the AWS account that created the node can interact with it using the API\. AWS credentials \(an access key ID and secret access key\) must be provided with the call\. IAM policies do not apply to API calls and can not be used to allow or deny Ethereum API access to a node\. IAM permissions policies only apply to operations related to node creation, deletion, and management\.

## Supported JSON\-RPC methods<a name="supported-json-rpc"></a>

Ethereum on Managed Blockchain supports the following Ethereum JSON\-RPC API methods\. Methods not listed are not supported\. Each supported API call has a brief description of its utility\. Unique considerations for using the JSON\-RPC method with an Ethereum node in Managed Blockchain are indicated where applicable\.

**Important**  
WebSockets calls have a 512 KB payload limit\. Some calls may exceed this limit and cause a "message response is too large" error\. Use HTTP for these requests instead of WebSockets\. HTTP requests have a 6 MB payload limit\. Responses that exceed the HTTP also result in a "message response is too large" error\.


| Method | Description | Considerations | 
| --- | --- | --- | 
| [eth\_blockNumber](https://eth.wiki/json-rpc/API#eth_blocknumber) | Returns the number of the most recent block\. |  | 
| [eth\_call](https://eth.wiki/json-rpc/API#eth_call) | Immediately executes a new message call without creating a transaction on the blockchain\. | eth\_call consumes 0 gas, but has a gas parameter for messages that require it\. | 
| eth\_chainId | Returns an integer value for the currently configured Chain Id value introduced in [EIP\-155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md)\. Returns None if no Chain Id is available\. |  | 
| [eth\_estimateGas](https://eth.wiki/json-rpc/API#eth_estimategas) | Estimates and returns the gas required for a transaction without adding the transaction to the blockchain\. |  | 
| [eth\_gasPrice](https://eth.wiki/json-rpc/API#eth_gasprice) | Returns the current price per gas in Wei\. |  | 
| [eth\_getBalance](https://eth.wiki/json-rpc/API#eth_getbalance) | Returns the balance of an account for the specified account address and integer position of storage\. |  | 
| [eth\_getBlockByHash](https://eth.wiki/json-rpc/API#eth_getblockbyhash) | Returns information about the block specified using the block hash\. |  | 
| [eth\_getBlockByNumber](https://eth.wiki/json-rpc/API#eth_getblockbynumber) | Returns information about the block specified using the block number\. |  | 
| [eth\_getBlockTransactionCountByHash](https://eth.wiki/json-rpc/API#eth_getblocktransactioncountbyhash) | Returns the number of transactions in the block specified using the block hash\. |  | 
| [eth\_getBlockTransactionCountByNumber](https://eth.wiki/json-rpc/API#eth_getblocktransactioncountbynumber) | Returns the number of transactions in the block specified using the block number\. |  | 
| [eth\_getCode](https://eth.wiki/json-rpc/API#eth_getcode) | Returns the code at the specified account address\. |  | 
| [eth\_getFilterChanges](https://eth.wiki/json-rpc/API#eth_getfilterchanges) | Polls the specified filter ID, retuning an array of logs that occurred since the last poll\. | Filters are ephemeral\. If Managed Blockchain needs to manage or maintain node instances for availability and performance, and an instance is replaced, filters may be deleted\. We recommend that you write your application code to handle the occasional deletion of filters\. | 
| [eth\_getFilterLogs](https://eth.wiki/json-rpc/API#eth_getfilterlogs) | Returns an array of all logs for the specified filter ID\. | Filters are ephemeral\. If Managed Blockchain needs to manage or maintain node instances for availability and performance, and an instance is replaced, filters may be deleted\. We recommend that you write your application code to handle the occasional deletion of filters\. | 
| [eth\_getLogs](https://eth.wiki/json-rpc/API#eth_getlogs) | Returns an array of all logs for a specified filter object\. | Filters are ephemeral\. If Managed Blockchain needs to manage or maintain node instances for availability and performance, and an instance is replaced, filters may be deleted\. We recommend that you write your application code to handle the occasional deletion of filters\. | 
| eth\_getProof | Experimental – Returns the account and storage values of the specified account, including the Merkle proof\. |  | 
| [eth\_getStorageAt](https://eth.wiki/json-rpc/API#eth_getstorageat) | Returns the value of the specified storage position for the specified account address\. |  | 
| [eth\_getTransactionByBlockHashAndIndex](https://eth.wiki/json-rpc/API#eth_gettransactionbyblockhashandindex) | Returns information about a transaction using the specified block hash and transaction index position\. |  | 
| [eth\_getTransactionByBlockNumberAndIndex](https://eth.wiki/json-rpc/API#eth_gettransactionbyblocknumberandindex) | Returns information about a transaction using the specified block number and transaction index position\. |  | 
| [eth\_getTransactionByHash](https://eth.wiki/json-rpc/API#eth_gettransactionbyhash) | Returns information about the transaction with the specified transaction hash\. |  | 
| [eth\_getTransactionCount](https://eth.wiki/json-rpc/API#eth_gettransactioncount) | Returns the number of transactions sent from the specified address\. |  | 
| [eth\_getTransactionReceipt](https://eth.wiki/json-rpc/API#eth_gettransactionreceipt) | Returns the receipt of the transaction using the specified transaction hash\. |  | 
| [eth\_getUncleByBlockHashAndIndex](https://eth.wiki/json-rpc/API#eth_getunclebyblockhashandindex) | Returns information about the uncle block specified using the block hash and uncle index position\. |  | 
| [eth\_getUncleByBlockNumberAndIndex](https://eth.wiki/json-rpc/API#eth_getunclebyblocknumberandindex) | Returns information about the uncle block specified using the block number and uncle index position\. |  | 
| [eth\_getUncleCountByBlockHash](https://eth.wiki/json-rpc/API#eth_getunclecountbyblockhash) | Returns the number of counts in the uncle specified using the uncle hash\. |  | 
| [eth\_getUncleCountByBlockNumber](https://eth.wiki/json-rpc/API#eth_getunclecountbyblocknumber) | Returns the number of counts in the uncle specified using the uncle number\. |  | 
| [eth\_getWork](https://eth.wiki/json-rpc/API#eth_getwork) | Returns the hash of the current block, the seedHash, and the boundary condition \(also called the "target"\) to be met\. |  | 
| [eth\_newBlockFilter](https://eth.wiki/json-rpc/API#eth_newblockfilter) | Creates a filter in the node to notify when a new block arrives\. Use <code>eth\_getFilterChanges</code> to check for state changes\. |  | 
| [eth\_newFilter](https://eth.wiki/json-rpc/API#eth_newfilter) | Creates a filter object with the specified filter options \(such as from block, to block, contract address, or topics\)\. |  | 
| [eth\_newPendingTransactionFilter](https://eth.wiki/json-rpc/API#eth_newpendingtransactionfilter) | Creates a filter in the node to notify when new pending transactions arrive\. Use <code>eth\_getFilterChanges</code> to check for state changes\. |  | 
| [eth\_protocolVersion](https://eth.wiki/json-rpc/API#eth_protocolversion) | Returns the current Ethereum protocol version\. |  | 
| [eth\_sendRawTransaction](https://eth.wiki/json-rpc/API#eth_sendrawtransaction) | Creates a new message call transaction or a contract creation for signed transactions\.  | Managed Blockchain supports raw transactions only\. You must create and sign transactions before sending them\. For more information, see [How to create raw transactions in Ethereum](https://medium.com/blockchain-musings/how-to-create-raw-transactions-in-ethereum-part-1-1df91abdba7c)\. | 
| [eth\_subscribe](https://geth.ethereum.org/docs/rpc/pubsub) | Experimental for publication subscription – Creates a subscription for specified events and returns a subscription ID\.  | Available only when using WebSockets\. Subscriptions are coupled to each connection\. When the connection closes, the subscription is removed\. | 
| [eth\_syncing](https://eth.wiki/json-rpc/API#eth_syncing) | Returns an object with sync status data or false when not syncing\. |  | 
| [eth\_uninstallFilter](https://eth.wiki/json-rpc/API#eth_uninstallfilter) | Uninstalls the filter with the specified filter ID\. |  | 
| [eth\_unsubscribe](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB#cancel-subscription) | Experimental for publication subscription – Cancels the subscription with the specified subscription ID\. |  | 
| [net\_listening](https://eth.wiki/json-rpc/API#net_listening) | Returns true if the client is actively listening for network connections\. |  | 
| [net\_peerCount](https://eth.wiki/json-rpc/API#net_peercount) | Returns the number of peers currently connected to the client\. |  | 
| [net\_version](https://eth.wiki/json-rpc/API#net_version) | Returns the current network ID\. |  | 
| [txpool\_inspect](https://geth.ethereum.org/docs/rpc/ns-txpool#txpool_inspect) | Lists a textual summary of all transactions currently pending inclusion in the next blocks, and those that are queued \(being scheduled for future execution only\)\. |  | 
| [txpool\_status](https://geth.ethereum.org/docs/rpc/ns-txpool#txpool_status) | Provides a count of all transactions currently pending inclusion in the next blocks, and those that are queued \(being scheduled for future execution only\)\. |  | 
| [web3\_clientVersion](https://eth.wiki/json-rpc/API#web3_clientversion) | Returns the current client version\. |  | 
| [web3\_sha3](https://eth.wiki/json-rpc/API#web3_sha3) | Returns Keccak\-256 \(not the standardized SHA3\-256\) of the given data\. |  | 

## Examples – making Ethereum API calls to an Ethereum node in Amazon Managed Blockchain<a name="ethereum-api-examples"></a>

The following examples demonstrate ways to make Ethereum API calls to an Ethereum node on Amazon Managed Blockchain\.

**Topics**
+ [Endpoint format for WebSockets and HTTP](#ethereum-api-endpoints)
+ [Using web3\.js](#web3-examples)
+ [Using the AWS SDK for JavaScript with WebSockets](#connect-to-node-websockets-awsjssdk)
+ [Using awscurl to make JSON\-RPC calls over HTTP](#connect-to-node-http)

**Important**  
The Signature Version 4 signing process requires credentials associated with an AWS account\. Some examples in this section export these sensitive credentials to the shell environment of the client\. Only use these examples on a client running in a trusted context\. Do not use these examples in an untrusted context, such as in a web browser or mobile app\. Client credentials should never be embedded in user\-facing applications\. To expose an Ethereum node on Managed Blockchain to anonymous users visiting from trusted web domains, you can set up a separate endpoint in [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) backed by a Lambda function that forwards requests to your node using the proper IAM credentials\.

### Endpoint format for WebSockets and HTTP<a name="ethereum-api-endpoints"></a>

An Ethereum node created using Ethereum on Managed Blockchain hosts one endpoint for WebSockets connections and another for HTTP connections\. These endpoints conform to the following patterns\.

**Note**  
The node ID is case\-sensitive and must be lowercase where indicated, or a signature mismatch error occurs\.

**WebSockets endpoint format**

```
wss://my-node-id-lowercase.wss.ethereum.managedblockchain.us-east-1.amazonaws.com/
```

For example, `wss://nd-6eaj5va43jggnpxouzp7y47e4y.wss.ethereum.managedblockchain.us-east-1.amazonaws.com/`

**HTTP endpoint format**

```
http://my-node-id-lowercase.ethereum.managedblockchain.us-east-1.amazonaws.com/
```

For example, `https://nd-6eaj5va43jggnpxouzp7y47e4y.ethereum.managedblockchain.us-east-1.amazonaws.com/`

### Using web3\.js<a name="web3-examples"></a>

[Web3\.js](https://web3js.readthedocs.io/en/v1.3.0/) is a popular collection of JavaScript libraries available using the Node package manager \(npm\)\. You can run the following examples to send a JSON\-RPC API call to Ethereum using a Javascript file for Node\.js\. The examples demonstrate an HTTP connection and a WebSockets connection to an Ethereum node\.

Both HTTP and WebSockets connection types rely on a local connection provider library to open the Signature Version 4 authenticated connection to the Ethereum node\. You install the provider for the connection locally by copying the source code to a file on your client\. You then reference the library files in the script that makes the Ethereum API call\.

#### Prerequisites<a name="web3-prerequisites"></a>

Running the example scripts requires the following prerequisites\. Prerequisites for both HTTP and WebSockets connections are included\.

1. You must have node version manager \(nvm\) and Node\.js installed on your machine\. If you are using an Amazon EC2 instance as your Ethereum client, see [Tutorial: Setting Up Node\.js on an Amazon EC2 Instance](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html) for more information\.

1. Type `node --version` and verify that you are using Node version 14 or later\. If necessary, you can use the `nvm install 14` command followed by the `nvm use 14` command to install version 14\.

1. Use node package manager \(npm\) to install the `aws-sdk`, `web3`, and `xhr2` packages as shown in the following examples\.\.

   ```
   npm install aws-sdk
   ```

   ```
   npm install web3
   ```

   ```
   npm install xhr2
   ```

1. The example scripts use ES modules\. To enable ECMAScript \(ES\) module support, add the `"type": "module"` line to your `package.json` file\. The example that follows shows the contents of a simple `package.json` file\.

   ```
   {
     "type": "module",
     "dependencies": {
       "aws-sdk": "^2.809.0",
       "web3": "^1.3.0",
       "xhr2": "^0.2.0"
     }
   }
   ```

1. The environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` must contain credentials associated with the same AWS account that created the node\. The environment variables `AMB_HTTP_ENDPOINT` and `AMB_WS_ENDPOINT` must contain your Ethereum node's HTTP and WebSockets endpoints respectively\.

   Export these variables as strings on your client as shown in the examples that follow\. Replace the values with appropriate values from your IAM user account\.

   ```
   export AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
   ```

   ```
   export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
   ```

   ```
   export AMB_HTTP_ENDPOINT="https://nd-6eaj5va43jggnpxouzp7y47e4y.ethereum.managedblockchain.us-east-1.amazonaws.com/"
   ```

   ```
   export AMB_WS_ENDPOINT="wss://nd-6eaj5va43jggnpxouzp7y47e4y.wss.ethereum.managedblockchain.us-east-1.amazonaws.com/"
   ```

**To make an Ethereum API call using web3\.js over HTTP to your Ethereum node on Managed Blockchain**

1. Copy the contents of the example that follows, and then use your preferred text editor to save it to a file named `aws-http-provider.js` on your client machine in the same directory where you run your script\.

   **Contents of aws\-http\-provider\.js**

   ```
   /////////////////////////////////////////////////////
   // Authored by Carl Youngblood
   // Senior Blockchain Solutions Architect, AWS
   // Adapted from web3 npm package v1.3.0
   // licensed under GNU Lesser General Public License
   // https://github.com/ethereum/web3.js
   /////////////////////////////////////////////////////
   
   import AWS from 'aws-sdk';
   import HttpProvider from 'web3-providers-http';
   import XHR2 from 'xhr2';
   
   export default class AWSHttpProvider extends HttpProvider {
     send(payload, callback) {
       const self = this;
       const request = new XHR2(); // eslint-disable-line
   
       request.timeout = self.timeout;
       request.open('POST', self.host, true);
       request.setRequestHeader('Content-Type', 'application/json');
   
       request.onreadystatechange = () => {
         if (request.readyState === 4 && request.timeout !== 1) {
           let result = request.responseText; // eslint-disable-line
           let error = null; // eslint-disable-line
   
           try {
             result = JSON.parse(result);
           } catch (jsonError) {
             let message;
             if (!!result && !!result.error && !!result.error.message) {
               message = `[aws-ethjs-provider-http] ${result.error.message}`;
             } else  {
               message = `[aws-ethjs-provider-http] Invalid JSON RPC response from host provider ${self.host}: ` +
                 `${JSON.stringify(result, null, 2)}`;
             }
             error = new Error(message);
           }
   
           callback(error, result);
         }
       };
   
       request.ontimeout = () => {
         callback(`[aws-ethjs-provider-http] CONNECTION TIMEOUT: http request timeout after ${self.timeout} ` +
           `ms. (i.e. your connect has timed out for whatever reason, check your provider).`, null);
       };
   
       try {
         const strPayload = JSON.stringify(payload);
         const region = process.env.AWS_DEFAULT_REGION || 'us-east-1';
         const credentials = new AWS.EnvironmentCredentials('AWS');
         const endpoint = new AWS.Endpoint(self.host);
         const req = new AWS.HttpRequest(endpoint, region);
         req.method = request._method;
         req.body = strPayload;
         req.headers['host'] = request._url.host;
         const signer = new AWS.Signers.V4(req, 'managedblockchain');
         signer.addAuthorization(credentials, new Date());
         request.setRequestHeader('Authorization', req.headers['Authorization']);
         request.setRequestHeader('X-Amz-Date', req.headers['X-Amz-Date']);
         request.send(strPayload);
       } catch (error) {
         callback(`[aws-ethjs-provider-http] CONNECTION ERROR: Couldn't connect to node '${self.host}': ` +
           `${JSON.stringify(error, null, 2)}`, null);
       }
     }
   }
   ```

1. Copy the contents of the example that follows, and then use a text editor of your choosing to save it to a file named `web3-example-http.js` in the same directory where you saved the provider from the previous step\. The example script runs the `getNodeInfo` Ethereum method\. You can modify the script to include other methods and their parameters\.

   **Contents of web3\-example\-http\.js**

   ```
   import Web3 from 'web3';
   import AWSHttpProvider from './aws-http-provider.js';
   const endpoint = process.env.AMB_HTTP_ENDPOINT
   const web3 = new Web3(new AWSHttpProvider(endpoint));
   web3.eth.getNodeInfo().then(console.log);
   ```

1. Run the script to call the Ethereum API method over HTTP on your Ethereum node\.

   ```
   node web3-example-http.js
   ```

   You should see output similar to the following\.

   ```
   Geth/v1.9.24-stable-cc05b050/linux-amd64/go1.15.5
   ```

**To make an Ethereum API call using web3\.js over WebSockets to your Ethereum node on Managed Blockchain**

1. Copy the contents of the example that follows, and then use a text editor of your choosing to save it to a file named `aws-websocket-provider.js` in the same directory on your client where you run your script\.

   **Contents of aws\-websocket\-provider\.js**

   ```
   /////////////////////////////////////////////////////
   // Authored by Carl Youngblood
   // Senior Blockchain Solutions Architect, AWS
   // Adapted from web3 npm package v1.3.0
   // licensed under GNU Lesser General Public License
   // https://github.com/ethereum/web3.js
   /////////////////////////////////////////////////////
   
   import AWS from 'aws-sdk';
   import WebsocketProvider  from 'web3-providers-ws';
   import pkg from 'websocket';
   const { w3cwebsocket } = pkg;
   const Ws = w3cwebsocket;
   
   export default class AWSWebsocketProvider extends WebsocketProvider {
     connect() {
       const region = process.env.AWS_DEFAULT_REGION || 'us-east-1';
       const credentials = new AWS.EnvironmentCredentials('AWS');
       const host = new URL(this.url).hostname;
       const endpoint = new AWS.Endpoint(`https://${host}/`);
       const req = new AWS.HttpRequest(endpoint, region);
       req.method = 'GET';
       req.body = '';
       req.headers['host'] = host;
       const signer = new AWS.Signers.V4(req, 'managedblockchain');
       signer.addAuthorization(credentials, new Date());
       const headers = {
         'Authorization': req.headers['Authorization'],
         'X-Amz-Date': req.headers['X-Amz-Date'],
         ...this.headers
       }
       this.connection = new Ws(this.url, this.protocol, undefined, headers, this.requestOptions, this.clientConfig);
       this._addSocketListeners();
     }
   }
   ```

1. Copy the contents of the following, and then use a text editor of your choosing to save it to a file named `web3-example-ws.js` in the same directory where you saved the provider from the previous step\. The example script runs the `getNodeInfo` Ethereum method and then closes the connection\. You can modify the script to include other methods and their parameters\.

   **Contents of web3\-example\-ws\.js**

   ```
   import Web3 from 'web3';
   import AWSWebsocketProvider from "./aws-websocket-provider.js";
   const endpoint = process.env.AMB_WS_ENDPOINT
   const web3 = new Web3(new AWSWebsocketProvider(endpoint));
   web3.eth.getNodeInfo().then(console.log).then(() => {
     web3.currentProvider.connection.close();
   });
   ```

1. Run the script to call the Ethereum API method over WebSockets on your Ethereum node\.

   ```
   node web3-example-ws.js
   ```

   You should see output similar to the example that follows\.

   ```
   Geth/v1.9.24-stable-cc05b050/linux-amd64/go1.15.5
   ```

### Using the AWS SDK for JavaScript with WebSockets<a name="connect-to-node-websockets-awsjssdk"></a>

The example that follows uses a JavaScript file for Node\.js to open a WebSockets connection to the Ethereum node endpoint in Managed Blockchain, and then send an Ethereum JSON\-RPC API call\.

Running the example script requires the following:
+ You must have Node\.js installed on your machine\. If you are using an Amazon EC2 instance, see [Tutorial: Setting Up Node\.js on an Amazon EC2 Instance](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)\.
+ You must use the Node package manager \(npm\) to install the AWS SDK for JavaScript, websocket\-client, and ws packages as shown in the example commands that follow\. The script uses classes from these packages\.

  ```
  npm install websocket-client
  ```

  ```
  npm install ws
  ```

  ```
  npm install aws-sdk
  ```
+ The environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` must contain credentials associated with the same account that created the node\.

**Note**  
If you added the `"type": "module"` line in your `package.json`, which is necessary for the previous web3 example, the script that follows will fail with the error `require is not defined`\. Modify `package.json` to remove or comment out this line before running the script\.

**To make an Ethereum API call over WebSockets to your Ethereum node on Managed Blockchain**

1. Copy the contents of the script that follows and save it to a file on your machine, for example, `ws-ethereum-example.js`\. Use a text editor to replace `my-node-id-lowercase` with the ID of a node in your account, such as `nd-6eaj5va43jggnpxouzp7y47e4y`, and replace `us-east-1` with the AWS Region in which you created your node\.

   The example calls the Ethereum JSON\-RPC method `eth_subscribe` along with the `newHeads` parameter\. You can replace this method and its parameters with any method listed in [Supported JSON\-RPC methods](#supported-json-rpc)\.

   **Contents of ws\-ethereum\-example\.js**

   ```
   const AWS = require('aws-sdk');
   const WebSocket = require('ws')
   const region = 'us-east-1';
   const host = 'my-node-id-lowercase.wss.ethereum.managedblockchain.us-east-1.amazonaws.com';
   const payload = {
     jsonrpc: '2.0',
     method: 'eth_subscribe',
     params: ["newHeads"],
     id: 67
   }
   const credentials = new AWS.EnvironmentCredentials('AWS');
   const endpoint = new AWS.Endpoint(`https://${host}`);
   const request = new AWS.HttpRequest(endpoint, region);
   request.method = 'GET';
   request.body = '';
   request.headers['host'] = host;
   const signer = new AWS.Signers.V4(request, 'managedblockchain');
   signer.addAuthorization(credentials, new Date());
   const ws = new WebSocket(`wss://${host}`, {headers: request.headers});
   ws.onopen = async () => {
     ws.send(JSON.stringify(payload));
     console.log('Sent request');
   }
   ws.onerror = (error) => {
     console.error(`WebSocket error: ${error.message}`)
   }
   ws.onmessage = (e) => {
     console.log(e.data)
   }
   ```

1. Run the script to call the Ethereum API method over WebSockets on your Ethereum node\.

   ```
   node ws-ethereum-example.js
   ```

   The `eth_subscribe` method with the `newHeads` parameter generates a notification each time a new header is appended to the chain\. You should see output similar to the example that follows, with additional subsequent responses\. The WebSockets connection remains open and additional notifications appear until you cancel the command\.

   ```
   sent request
   {"id":67,"jsonrpc":"2.0","result":"0xabcd123456789efg0h123ijk45l6m7n8"}
   ```

### Using awscurl to make JSON\-RPC calls over HTTP<a name="connect-to-node-http"></a>

The example that follows uses [awscurl](https://pypi.org/project/awscurl/0.6/), which sends a signed HTTP request based on the current credentials you have set for the AWS CLI\. If you construct your own HTTP requests, see [Signing AWS requests with Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html) in the *AWS General Reference*\.

Replace `my-node-id-lowercase` with the ID of a node in your account such as `nd-6eaj5va43jggnpxouzp7y47e4y`\. The example calls the `web3_clientVersion` method, which takes an empty parameter block\. You can replace this method and its parameters with any method listed in [Supported JSON\-RPC methods](#supported-json-rpc)\.

```
awscurl --service managedblockchain \
-X POST -d '{"jsonrpc": "2.0", "method": "web3_clientVersion", "params": [], "id": 67}' \
https://my-node-id-lowercase.ethereum.managedblockchain.us-east-1.amazonaws.com
```

The command returns output similar to the following\.

```
{"jsonrpc":"2.0","id":67,"result":"Geth/v1.9.22-stable-c71a7e26/linux-amd64/go1.15.5"}
```