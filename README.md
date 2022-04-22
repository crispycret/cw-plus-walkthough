# cw-plus-walktrhough
Walkthrough each contract in the cw-plus repository. See `01-GettingStarted` for instructions on getting a testnet up and running. 


# Denomiations and values
The Metric System prefix u is micro or millionths which is 0.000001 of the base

### UNSURE ABOUT THIS PART
We start out with 1000 juno.

```
1 juno = 1000000 ujunox
10 juno = 10000000 ujunox
100 juno = 1000000000 ujunox
1000 juno = 10000000000 ujunox
```


# Resource commands
```
junod tx bank send [from_key_or_address] [to_address] [amount] [flags]
junod query bank balances [address] [flags]

```


# Build All Contracts
```
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
```



# CW1 Whitelist:

### Descripion:

This contract allows admins of a contract to be set. If the contract variable `mutable=true` then admins can be added or removed. If `mutable=false` then the admins that were set at the instantiation of the contract will be the only admins allowed for this contract and `mutable` cannot be set to `true`

### Store
### Instantiate
### Query
### Execute



# CW1 Subkeys:

### Descripion:

This contract incorporartes cw1-whitelist and therefore has all of the same functionality. It expands functionality by allowing allowances of a native token to be set by admins.

### Store

### Instantiate
```
junod tx wasm instantiate $CW1SUBKEYS_CODE '{"admins":["$admin_a", "$admin_b"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y

cd artifacts
TX=$(junod tx wasm store cw1_subkeys.wasm  --from <your-key> --chain-id=<chain-id> --gas auto --output json -y | jq -r '.txhash')
CODE_ID=$(junod query tx $TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')
```

### Query

### Execute





#CW3 Fixed Multisig

### Descripion:

### Store
### Instantiate
### Query
### Execute





#CW3 Flex Multisig

### Descripion:

### Store
### Instantiate
### Query
### Execute

