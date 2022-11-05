# Introduction to SuperFluid Finance

## Materials

* [EthGlobal Tutorials](https://ethglobal.com/guides/introduction-to-superfluid-protocol-be10i#money-streaming-with-solidity)
* [Documentation from Superfluid Finance](https://docs.superfluid.finance/)
* [Foundry Book](https://book.getfoundry.sh/)

## Install Dependencies

```
// init the foundry project
forge

// install dependencies
forge install openzeppelin/openzeppelin-contracts --no-commit
forge install superfluid-finance/ethereum-contracts --no-commit
forge remappings

// remapping dependencies
touch remappings.txt
echo "@openzeppelin/=lib/openzeppelin-contracts/
@superfluid-finance/ethereum-contracts/=lib/ethereum-contracts/packages/ethereum-contracts/
" >> remappings.txt

// build the foundry project
forge build
```

## Deploying Contracts

```
// deploy to the local testnet
// before doing that deploy the host contract and dai contract first
anvil
forge create --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 FlowSender

// deploy to the goerli testnet
export HOST_ADDR=0x22ff293e14F1EC3A09B137e9e06084AFd63adDF9
export FDAIX=0xF2d68898557cCb2Cf4C10c3Ef2B034b2a69DAD00
export GOERIL_RPC=https://ethereum-goerli-rpc.allthatnode.com
forge create --rpc-url $GOERIL_RPC --private-key $PRIVATE_KEY FlowSender --constructor-args $HOST_ADDR $FDAIX
```

## Interact With FlowSender to load it with fDAIx

```
cast estimate 0x233CBfF6f6c5E550da24B9D0e9b800292782c276 "gainDaiX()" --rpc-url $GOERIL_RPC --private-key $PRIVATE_KEY
cast send 0x233CBfF6f6c5E550da24B9D0e9b800292782c276 "gainDaiX()" --rpc-url $GOERIL_RPC --private-key $PRIVATE_KEY --gas-limit 160000
```

## Interact with FlowSender to send someone a stream of fDAIx

* Note that this flow rate is always in wei per second, saying that you wants the contract to stream 10 fDAIx/hour.

```
10 fDAIx/hour

= 10 * (10**18) fDAIx / hour

= 10 * (10**18) fDAIx / ( 60 seconds * 60 minutes )

= 2777777800000000 <- the flow rate number you would provide
```

```
cast estimate 0x233CBfF6f6c5E550da24B9D0e9b800292782c276 "createStream(int96,address)" --rpc-url $GOERIL_RPC --private-key $PRIVATE_KEY 2777777800000000 0x856A1e9914ed608eE14626aC24aB61F1AB7f7A82
cast send 0x233CBfF6f6c5E550da24B9D0e9b800292782c276 "createStream(int96,address)" --rpc-url $GOERIL_RPC --private-key $PRIVATE_KEY 2777777
800000000 0x856A1e9914ed608eE14626aC24aB61F1AB7f7A82 --gas-limit 450000
```
