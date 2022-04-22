# cw-plus-walktrhough
Walkthrough each contract in the cw-plus repository. See `01-GettingStarted` for instructions on getting a testnet up and running. 



# Table Of Contents

### Setup
#### [01-GettingStarted.md](01-GettingStarted.md)
#### [02-Accounts.md](01-GettingStarted.md)
#### [03-Funding.md](03-Funding.md)

### Cw-Plus Contracts
#### [04-BuildingContracts.md](04-BuildingContracts.md)
#### [05-cw1-whitelist.md](05-cw1-whitelist.md)
#### [06-cw1-subkeys.md](06-cw1-subkeys.md)
#### [07-fixed-multisig.md](07-fixed-multisig.md)
#### [08-flex-multisig.md](08-flex-multisig.md)



# Things to Know:


## Denomiations and values
The Metric System prefix u is micro or millionths which is 0.000001 of the base

#### UNSURE ABOUT THIS PART

We start with 1000000000 ujunox or 100 juno.

```
1 juno = 1000000 ujunox
10 juno = 10000000 ujunox
100 juno = 1000000000 ujunox
```


## Resource commands
```
junod tx bank send [from_key_or_address] [to_address] [amount] [flags]
junod query bank balances [address] [flags]
```

#### Example
```

junod tx bank send unsafe-test $MASTER 10000000ujunox
junod query bank balances $UNSAFE_TEST 
junod query bank balances $MASTER

```


# Build All Contracts
Navigate to the root folder of the cw-plus folder and run the following command to build all contracts at once.

```
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
```



# Walkthough Summary:


# CW1 Whitelist:

### Descripion:

This contract allows admins of a contract to be set. If the contract variable `mutable=true` then admins can be added or removed. If `mutable=false` then the admins that were set at the instantiation of the contract will be the only admins allowed for this contract and `mutable` cannot be set to `true`

### Store
```
```

### Instantiate
```
```

### Query
```
```

### Execute
```
```



# CW1 Subkeys:

### Descripion:

This contract incorporartes cw1-whitelist and therefore has all of the same functionality. It expands functionality by allowing allowances of a native token to be set by admins.

### Store
```
```

### Instantiate
```
junod tx wasm instantiate $CW1SUBKEYS_CODE '{"admins":["$admin_a", "$admin_b"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y

cd artifacts
TX=$(junod tx wasm store cw1_subkeys.wasm  --from <your-key> --chain-id=<chain-id> --gas auto --output json -y | jq -r '.txhash')
CODE_ID=$(junod query tx $TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')
```

### Query
```
```

### Execute
```
```



# CW3 Fixed Multisig

### Descripion:
Description of CW2 Fixed Multisig

### Store
```
```

### Instantiate
```
```

### Query
```
```

### Execute
```
```






# CW3 Flex Multisig

### Descripion:
Description of CW2 Fixed Multisig

### Store
```
```

### Instantiate
```
```

### Query
```
```

### Execute
```
```



