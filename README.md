# Example Subgraph for 5ire

An example to help you get started with The Graph. For more information see the docs on https://thegraph.com/docs/.


## Prerequisites
- yarn
- graph-cli

## Steps
### On-chain Data Indexing 
In this example, we deployed ERC20 contract , executed some `Transfer` and `Approve` functions

https://5irescan.io/contract/evm/0x78E1EAe74f63C444bdB63390B9e0bA0138dFb4A2

-> Our target: Indexing `Transfer` event and `Approval` Event 

### Atonomy of Subgraph 

#### Manifest File (subgraph.yaml)

This is the main file that defines the data sources, events and mappings to process that data
1. Configuring `network` is `5ire` 
2. Configuring deployed contract : `address:0x78E1EAe74f63C444bdB63390B9e0bA0138dFb4A2`
3. Configuring block number that we deployed contract: `startBlock: 268730`
4. Adding Entities that we define in `schema.graphql` file (`entities`)
5. Adding correct abis file (`abis`)
6. Adding eventHandlers (Mapping files) (`eventHandlers`)
7. Mapping events to entity (`file`)

Noted: I have configured every steps , just running 

#### GraphQL Schema  (schema.graphql)
Defines the types of data (entities) that the Subgraph will index and make queryable

At this case, we defines 2 entities `Approval` and `Transfer` 

```
type Approval @entity(immutable: true) {
  id: Bytes!
  owner: Bytes! # address
  spender: Bytes! # address
  value: BigInt! # uint256
  blockNumber: BigInt!
  blockTimestamp: BigInt!
  transactionHash: Bytes!
}

type Transfer @entity(immutable: true) {
  id: Bytes!
  from: Bytes! # address
  to: Bytes! # address
  value: BigInt! # uint256
  blockNumber: BigInt!
  blockTimestamp: BigInt!
  transactionHash: Bytes!
}
```

#### Mapping File (src/nyx-token.ts)

Contains transfer and approval handlers that process data when events are triggered. These functions define how the data is stored in the database



### Create Subgraph 
+ Step 1: `cd subgraph-template`

    Run command 
    ```
    yarn install
    ```

    ```  
    yarn codegen && yarn build
    ```
+ Step 2: Configure subgraph (we configured)

+ Step 3: Create subgraph

    ```  
    graph create --node https://api.subgraph.5ire.network example
    ```

**Noted**: `example` is package name 


### Deploy Subgraph

Run command:

    graph deploy --ipfs https://api.ipfs.5ire.network --node https://api.subgraph.5ire.network example
    
### Indexing 
+ Once it is finished, open: https://api.graph.5ire.network/subgraphs/name/example/graphql
 in the browser.

+ Query command

```
query MyQuery {
  transfers(first: 10) {
    transactionHash
    from
    to
    value
  }
}
```

+ Result 

```
{
  "data": {
    "transfers": [
      {
        "transactionHash": "0x150df93faa21da4c7f79ffd23bde033fc48862c01d923568d54e209142dc78c1",
        "from": "0x783fc27915754512e72b5811599504eca458e4c5",
        "to": "0xd8b0cb47fc838e4637e26c0a25a6ef8b705472ff",
        "value": "10000000000000000000"
      },
      {
        "transactionHash": "0x974f75e84ea804752a4aa7a1cfc0aed244a11c7959481dbb21278c3603f17be3",
        "from": "0x0000000000000000000000000000000000000000",
        "to": "0x783fc27915754512e72b5811599504eca458e4c5",
        "value": "10000000000000000000000"
      }
    ]
  }
}
```
# References
https://github.com/graphprotocol/example-subgraph
