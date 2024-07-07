# Example Subgraph for 5ire

An example to help you get started with The Graph. For more information see the docs on https://thegraph.com/docs/.



## Prerequisites
- yarn

## Steps
### Deploy Smart contract

+ Method 1: Using [5ireIDE](https://ide.5ire.network/#optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.18+commit.87f61d96.js) and [5ire Wallet Extension](https://chrome.google.com/webstore/detail/5irechain-wallet/keenhcnmdmjjhincpilijphpiohdppno?hl=en) to deploy smart contract

**Noted**: Save information about the `contract address` and `block number` at the time of deploying the smart contract



### Interact Smart contract

+ Execute `createGravatar` transaction (Because the `schema.graphql` has been configured for information such as `id`,`owner`, `displayName`, `imageUrl`)

### Create Subgraph 
+ Step 1: `cd example-subgraph`

    Run command 
    ```
    yarn install
    ```

    ```  
    yarn codegen
    ```
+ Step 2: Open  `subgraph.yaml`

    + Change `dataSources` -> `source` -> `address>` → `<your contract address`

    + Add `dataSources` → `source` → `startBlock` → `<your blockNumber>` (smart contract deployed in that block number )

+ Step 3: Create subgraph

    ```  
    graph create --node https://subgraph-api.qa.5ire.network example
    ```

**Noted**: `example` is package name 


### Deploy Subgraph

Run command:

    graph deploy --ipfs https://ipfs-api.qa.5ire.network/ --node https://subgraph-api.qa.5ire.network example
    
### Indexing 
+ Once it is finished, open: https://subgraph.qa.5ire.network/subgraphs/name/example/graphql
 in the browser.

+ Indexing command

```
query MyQuery {
	gravatars {
		id
		imageUrl
		displayName
}
}
```

+ Result 

```
{
  "data": {
    "gravatars": [
      {
        "id": "0x0",
        "imageUrl": "dung5ire",
        "displayName": "dung5ire"
      }
    ]
  }
}

```
# References
https://github.com/graphprotocol/example-subgraph