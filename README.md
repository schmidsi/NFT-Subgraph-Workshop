![Top Slide](./slide.jpg)

- Contract to index: https://explorer.celo.org/token/0xaB12Cd14E43dbc5F7F3f5571B449BFfa14F444cC/token-transfers
- [Google Slides for NFT Subgraph Development Workshop](https://docs.google.com/presentation/d/1v34HTuHp9mcGPuiy3FifujhgGgFbEWvEuHfFLj6HOSo/edit?usp=sharing)

# NFT Subgraph on Celo POAP contracts

## Prerequisites

- Install graph-cli: `yarn global add @graphprotocol/graph-cli`

## First Steps

- [Find the contract on Celo Block Explorer](https://explorer.celo.org/token/0xaB12Cd14E43dbc5F7F3f5571B449BFfa14F444cC/token-transfers)
- [Find the contract creation transaction for startBlock](https://explorer.celo.org/tx/0xd0bc372be9ea48fb569116493f639df8a9fcc8a19419aa57b1c6ef76ea7ad1fd/internal-transactions)
- Get a generic ERC721 ABI


- Run this command to initialise the subgraph with events:

```bash
graph init 
    --product hosted-service \
    --protocol ethereum \
    --from-contract 0xaB12Cd14E43dbc5F7F3f5571B449BFfa14F444cC \
    --index-events \
    --contract-name CeloPOAP \
    --network celo \
    --abi ./abis/ERC721.json \
    schmidsi/celo-poap-subgraph celo-poap-subgraph
```

- Inspect the source
– Create new subgraph on [The Graph Hosted Service](https://thegraph.com/hosted-service/) named "Celo POAP Subgraph"
- `graph auth --product hosted-service ...`
- `yarn deploy`

## Remove unused Entities and Events

```graphql
# schema.graphql
type Transfer @entity {
  id: ID!
  from: Bytes! # address
  to: Bytes! # address
  tokenId: BigInt! # uint256
}
```

```yaml
# subgraph.yaml
    mapping:
      kind: ethereum/events
      apiVersion: 0.0.5
      language: wasm/assemblyscript
      entities:
        - Transfer
      abis:
        - name: CeloPOAP
          file: ./abis/CeloPOAP.json
      eventHandlers:
        - event: Transfer(indexed address,indexed address,indexed uint256)
          handler: handleTransfer
      file: ./src/mapping.ts

```


## Other resources

- https://github.com/Developer-DAO/resources
- https://dev.to/dabit3/the-complete-guide-to-full-stack-ethereum-development-3j13
- https://github.com/itsjerryokolo/CryptoPunks
- https://github.com/dabit3/building-a-subgraph-workshop
- https://thegraph.com/docs/developer/quick-start
- https://thegraph.com/discord
