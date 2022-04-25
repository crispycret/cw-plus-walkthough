# CW1 Subkeys

## Description

This contract incorporartes cw1-whitelist and therefore has all of the same functionality. It expands functionality by allowing allowances of a native token to be set by admins.

## Store contract
```
junod tx wasm store cw1_subkeys.wasm  --from $MASTER --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```


## Or Storing while storing TX and Contrant ID Code
```
CW1_SUBKEYS_STORE_TX=$(junod tx wasm store cw1_subkeys.wasm --from master $BASE_OPTIONS --output json | jq -r '.txhash')
CW1_SUBKEYS_CODE_ID=$(junod query tx $CW1_SUBKEYS_STORE_TX --output json | jq -r '.code')

# OR

CW1_SUBKEYS_CODE_ID=$(junod tx wasm store cw1_subkeys.wasm --from master $BASE_OPTIONS --output json | jq -r '.logs[0].events[-1].attributes[0].value')


echo $CW1_SUBKEYS_CODE_ID
```

## Instantiate contract

#### Command Structure
Example using an unspecified `admin, code-id, sender, chain-id`
```
junod tx wasm instantiate <code-id> '{"admins":["<address>"],"mutable":true}' \
  --amount 50000ujunox --label "CW1 example contract" --from <your-key> --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

### Method 1
Example using `unsafe-test` address as admin with a `code-id, sender, chain-id`
```
junod tx wasm instantiate $CW1_SUBKEYS_CODE_ID '{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y"],"mutable":true}' \
  --amount 50000ujunox --label "CW1 example contract" --from master $BASE_OPTIONS
```

### Method 2
Example baking `admin-a, admin-b` addresses as admin with a `code-id, sender, chain-id`
```
junod tx wasm instantiate $CW1_SUBKEYS_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"],\"mutable\":true}" \
  --amount 50000ujunox --label "CW1 example contract" --from master $BASE_OPTIONS
```

### Method 3
Example baking `admin-a, admin-b` addresses as admin with a `code-id, sender, chain-id` while storing the contract address as an environment variable.
```
CW1_SUBKEYS_CONTRACT_ADDRESS=$(junod tx wasm instantiate $CW1_SUBKEYS_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"],\"mutable\":false}" \
  --amount 50000ujunox --label "CW1 example contract" --from master $BASE_OPTIONS \
  --output json | jq -r '.logs[0].events[2].attributes[0].value')
```






## Query commands

### Command / Query Structure
Query commands for cw1-subkeys contract with unspecified contract address 

```
junod query wasm contract-state smart <contract-address> '{"admin_list":{}}' --chain-id testing

junod query wasm contract-state smart <contract-address> '{"all_allowances":{}}' --chain-id testing

junod query wasm contract-state smart <contract-address> '{"allowance":{"spender":"<account-address>"}}' --chain-id testing
```

### Method 1
Query commands for cw1-subkeys contract with hard-coded contract address 

```

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"admin_list":{}}' --chain-id testing

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"all_allowances":{}}' --chain-id testing

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"allowance":{"spender":"juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y"}}' --chain-id testing

```

### Method 2
Query commands for cw1-subkeys contract with variable contract address and baked account address

```

junod query wasm contract-state smart $CW1_SUBKEYS_CONTRACT_ADDRESS '{"admin_list":{}}' --chain-id testing

junod query wasm contract-state smart $CW1_SUBKEYS_CONTRACT_ADDRESS '{"all_allowances":{}}' --chain-id testing

junod query wasm contract-state smart $CW1_SUBKEYS_CONTRACT_ADDRESS "{\"allowance\":{\"spender\":\"$ADMIN_A\"}}" --chain-id testing

```



## Execute commands
#### Execute Command
```
junod tx wasm execute <contract-address> <execute-msg> --from <key> --chain-id <chain-id> [options]
```

#### Execute Message Structure

```
'{
    "<action>": {
        "<param>":"<value>",
        "<param>": {
            "<param>":"<value>",
            "<param>":"<value>"
        }
    }
}'

'{"<action>":{"<param>":"<value>","<param>": {"<param>":"<value>","<param>":"<value>"}}}'
```

#### Execute Message as Readable JSON (INVALID)
```
'{
    "increase_allowance": {
        "spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl",
        "amount": {
            "denom":"<value>",
            "amount":"2000000"
        }
    }
}'
```

#### Execute Message

```
'{"increase_allowance":{"spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl","amount":{"denom":"ujunox","amount":"2000000"}}}' 
```


### Execute Command
```
echo $ADMIN_A 

# Use the value of `$ADMIN_A` as the `spender` address
EXECUTE_MSG='{"increase_allowance":{"spender":"<address>","amount":{"denom":"ujunox","amount":"2000000"}}}'

# OR USE
EXECUTE_MSG="{\"increase_allowance\":{\"spender\":\"$ADMIN_A\",\"amount\":{\"denom\":\"ujunox\",\"amount\":\"2000000\"}}}"


junod tx wasm execute $CW1_SUBKEYS_CONTRACT_ADDRESS $EXECUTE_MSG --from admin-b $BASE_OPTIONS
```

### Execute Command: Allocating `ucosm` instead of `ujunox`

```
EXECUTE_MSG="{\"increase_allowance\":{\"spender\":\"$ADMIN_A\",\"amount\":{\"denom\":\"ucosm\",\"amount\":\"2000000\"}}}"


junod tx wasm execute $CW1_SUBKEYS_CONTRACT_ADDRESS $EXECUTE_MSG --from admin-b $BASE_OPTIONS
```


# EXTRA NOTES (IGNORE)
```
junod query wasm contract-state smart $CW1_SUBKEYS_CONTRACT_ADDRESS '{"allowance":{"spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl"}}' --chain-id testing

# OR

QUERY_MSG="{\"allowance\":{\"spender\":\"$ADMIN_A\"}}"
junod query wasm contract-state smart $CW1_SUBKEYS_CONTRACT_ADDRESS $QUERY_MSG --chain-id testing




junod tx wasm execute <contract-addr> \
  '{"execute":{"msgs":[{"bank":{"send":{"to_address":"<key-C>","amount":[{"denom":"ujunox","amount":"500"}]}}}]}}' \
  --from <key-B> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block

```
  
  
  
## [Next Chapter - 07 CW3 Fixed Multisig](07-cw3-fixed-multisig.md)


## [Previous Chapter - 05 CW1 Whitelist](05-cw1-whitelist.md)

