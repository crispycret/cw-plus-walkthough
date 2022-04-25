# CW3 Fixed Multsig




# STORE:

```
BASE_OPTIONS="--chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y"

# Manually look for Code ID value
junod tx wasm store cw3_fixed_multisig.wasm --from master $BASE_OPTIONS --output json | jq -r ''

# OR

CW3_FIXED_MULTISIG_CODE_ID=$(\
  junod tx wasm store cw3_fixed_multisig.wasm --from master $BASE_OPTIONS \
  --output json | jq -r '.logs[0].events[-1].attributes[0].value')
  
echo $CW3_FIXED_MULTISIG_CODE_ID
```



# INSTANTIATE:
### Building a query using the instantiate schema - `cw3-fixed-multisg/schema/instantiate_msg.json`
```
'{
    "max_voting_period": {
        "time": 500000
    },
    
    "required_weight": 2,
    
    "voters": [
        {
            "addr": "<addr>",
            "weight": 1
        }, 
        {
            "addr": "<addr>",
            "weight": 1
        }, 
        {
            "addr": "<addr>",
            "weight": 1
        }
    ]
}'
```

### Format the instantiate message into JSON
There are 3 voters each with a weight of 1 and the vote requires a weight of 2 two pass. Two of the Three voters must vote Yes.
```
'{"max_voting_period":{"time":500000},"required_weight":2,"voters":[{"addr":"<address>","weight":1},{"addr":"<address>","weight":1},{"addr":"<address>","weight":1}]}'
```

### Incorporate your addresses into the message
```
echo $ADMIN_A
echo $ADMIN_B
echo $ADMIN_C

# output
ADMIN_A = juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh
ADMIN_B = juno1d3pdywztwek0v5sxqa8t6kwq7fkqfrl8y94tqw
ADMIN_C = juno1mt5we8478ud82auklx5mcj8mjxjxsh808kqgtm

# JSON of my 3 admins having the same voting power.
'{"max_voting_period":{"time":500000},"required_weight":2,"voters":[{"addr":"juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh","weight":1},{"addr":"juno1d3pdywztwek0v5sxqa8t6kwq7fkqfrl8y94tqw","weight":1},{"addr":"juno1mt5we8478ud82auklx5mcj8mjxjxsh808kqgtm","weight":1}]}'
```

# Store JSON message as a variable and instantiate the contract
```
INSTANTIATE_MSG='{"max_voting_period":{"time":500000},"required_weight":2,"voters":[{"addr":"juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh","weight":1},{"addr":"juno1d3pdywztwek0v5sxqa8t6kwq7fkqfrl8y94tqw","weight":1},{"addr":"juno1mt5we8478ud82auklx5mcj8mjxjxsh808kqgtm","weight":1}]}'

CW3_FIXED_MULTISIG_CONTRACT_ADDRESS=$(junod tx wasm instantiate $CW3_FIXED_MULTISIG_CODE_ID $INSTANTIATE_MSG --label "cw3 fixed multisig" --from admin-a $BASE_OPTIONS)
```


# QUERY:
```
```

# EXECUTE:
```
```





## [Next Chapter - 08 CW3 Flex Multisig](08-cw3-flex-multisig.md)


## [Previous Chapter - 06 CW1 Subkeys.md](06-cw1-subkeys.md)
