# CW1 Whitelist

## Description

This contract allows admins of a contract to be set. If the contract variable `mutable=true` then admins can be added or removed. If `mutable=false` then the admins that were set at the instantiation of the contract will be the only admins allowed for this contract and `mutable` cannot be set to `true`

# Store Contract

Storing the contract on the blockchain will result in the output of the contracts id. The `code id` is the index of stored contracts on the blockchain and is important to note down.

### Raw Storing of contract
This method of storing the contract does not auto track the `code id` of the contract and requires you to find the `code id` of the stored the contract from the output.

```
junod tx wasm store cw1_whitelist.wasm  --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```
   

### OR, Store the contracts 'Store' TX Hash and the CODE ID as temporary environment variables
```
cd artifacts

CW1_WHITELIST_STORE_TX=$(junod tx wasm store cw1_whitelist.wasm  --from master --chain-id=testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block --output json -y | jq -r '.txhash')

CW1_WHITELIST_CODE_ID=$(junod query tx $CW1_WHITELIST_STORE_TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')
```
 
 

# Instantiate Contract
To instantiate the contract we must reference the contracts `code id`. The contract instantiation output will contain inside of it the `contract address`. Again, the first command is the raw method to instantiate the contract while the second method stores the `contract address` as a temporary environment variable.

When instantiating if no admin(s) is designated than the uploader of the contract will become the sole admin of the contract. We want to set the variable `mutable` as true so that after instantiation admins can be removed or added by other admins.

### Raw Instantiation of contract

```
junod tx wasm instantiate $CW1_WHITELIST_CODE_ID '{"admins":["<ADMIN_ADDRESS_1>", "<ADMIN_ADDRESS_2>"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

You can etiher directly replace `<ADMIN_ADDRESS_1>, <Admin_Address_2>` with your admin addresses or you can use the following commands which bakes in the admin addresses if you have setup the environment variables.

```
## Bakes Admin A and Admin B into the contract by escaping double quotes .

junod tx wasm instantiate $CW1_WHITELIST_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"], \"mutable\":true}" --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y


# Does the same as the above but stores the contract address in the variable CW1WHITELIST_CONTRACT_ADDRESS

CW1_WHITELIST_CONTRACT_ADDRESS=$(junod tx wasm instantiate $CW1_WHITELIST_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"], \"mutable\":true}" --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y --output json | jq -r '.logs[0].events[2].attributes[0].value')
```

#### Store the Contract Address as Environment Variable

Add these lines to the bottom of `~/.profile`.  

```
export CW1_WHITELIST_CODE_ID=<Your Code ID>
export CW1_WHITELIST_CONTRACT_ADDRESS=<Your Contracts Address>
```




# Query:
```
junod query wasm contract-state smart juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"admin_list":{}}' --chain-id testing
```


# Execute:

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"update_admins": {"admins":["juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562", "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block
```

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
  '{"increase_allowance":{"spender":"<key-B>","amount":{"denom":"ujunox","amount":"2000000"}}}' \
  --from <admin-key-A> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```


# Update admin list to include admin-a
# juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562
```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
'{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57", "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}' --from unsafe-test --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 

junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```
 

# Next Chapter
#### [Chapter 06-cw1-subkeys](06-cw1-subkeys.md)


# Previous Chapter
#### [04-BuildingContracts.md](04-BuildingContracts.md)

.

.

.

.

.

.



# RAW Notes (IGNORE)

### CW1-Whitelist:
##### Hypothosis: 

I believe this contract is only used to set admins of the contract.
By itself this contract can only allow the publisher to set the admins. 
Those admins can then mayber set an admin if they have high enough rights.
Admins can be added that cannot be removed and admins can be added that can be removed.

##### Conclusion:
<Explain What I know Here>


#### Readable JSON Of an Execute Message
```
{
  execute: {
    msgs: [{
      bank: {
        send: {
          to_address: "<key-C>",
          amount: [{
            denom: "ujunox",
            amount: "500"
          }]
        }
      }
    }]
  }
};
```




#### Serialize Contract Message As Proper JSON
```
'{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}'
```

#### Execute the Message  
```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"custom":{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```


#### Extended Contract Message
```
{
    execute: {
        msgs: [
            {

                update_admins: {
                    admins: [
                        "juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57", 
                        "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", 
                        "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"
                    ],
                },
            },
        ]
    }
};
{
    execute: {
        msgs: [{
            custom: {
                update_admins: {
                    admins: [
                        "juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57", 
                        "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", 
                        "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"
                    ],
                }
            }
        }]
    }
};
```



