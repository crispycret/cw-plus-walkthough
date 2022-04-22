# CW1 Whitelist

## Description

This contract allows admins of a contract to be set. If the contract variable `mutable=true` then admins can be added or removed. If `mutable=false` then the admins that were set at the instantiation of the contract will be the only admins allowed for this contract and `mutable` cannot be set to `true`

## Store Contract

Storing the contract on the blockchain will result in the output of the contracts id. The `code id` is the index of stored contracts on the blockchain and is important to note down.

### Raw Storing of contract
This method of storing the contract does not auto track the `code id` of the contract and requires you to find the `code id` of the stored the contract from the output.

```
junod tx wasm store cw1_whitelist.wasm  --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```
   

### OR, Store the contracts 'Store' TX Hash and the CODE ID as temporary environment variables
```
cd artifacts

CW1WHITELIST_STORE_TX=$(junod tx wasm store cw1_subkeys.wasm  --from master --chain-id=testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block --output json -y | jq -r '.txhash')

CW1WHITELIST_CODE_ID=$(junod query tx $CW1WHITELIST_STORE_TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')
```
 

## Instantiate Contract
To instantiate the contract we must reference the contracts `code id`. The contract instantiation output will contain inside of it the `contract address`. Again, the first command is the raw method to instantiate the contract while the second method stores the `contract address` as a temporary environment variable.

When instantiating if no admin(s) is designated than the uploader of the contract will become the sole admin of the contract. We want to set the variable `mutable` as true so that after instantiation admins can be removed or added by other admins.

### Raw Instantiation of contract

```
junod tx wasm instantiate $CW1WHITELIST_CODE_ID '{"admins":["<ADMIN_ADDRESS_1>", "<ADMIN_ADDRESS_2>"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

You can etiher directly replace `<ADMIN_ADDRESS_1>` with your admin address or you can use the following commands which bakes in the admin addresses if you have setup the environment variables.

```
junod tx wasm instantiate $CW1WHITELIST_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"], \"mutable\":true}" --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y

CW1WHITELIST_CONTRACT_ADDRESS= $(junod tx wasm instantiate $CW1WHITELIST_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"], \"mutable\":true}" --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y | jq -r '.logs[0].events[2].attributes[0].value')

```

From the output of the above command you can locate the `contract address` or you can copy the `tx hash` and run a query to locate the contract address

```

```

#### Locate Contract Address from query.
```
junod tx query $TX
```
#### Store the Contract Address as Environment Variable
```
CW1WHITELIST_CONTRACT_ADDRESS=<Your Contracts Address>
```

### OR, Store the contracts 'Instantiation' TX Hash and the CODE ID as temporary environment variables
```
Method not yet deveolped!!!

CW1WHITELIST_INSTANTIATE_TX=
CW1WHITELIST_CONTRACT_ADDRESS=
```

This will show us the contract address.

 Contract Addresses: 
 Not mutable Contracts
 juno1fzm6gzyccl8jvdv3qq6hp9vs6ylaruervs4m06c7k0ntzn2f8faqxt5lvx 


 Mutable Contracts
 juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t Mutable





## Query:
```
junod query wasm contract-state smart juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"admin_list":{}}' --chain-id testing
```

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"update_admins": {"admins":["juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562", "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block
```

##Execute:

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
  '{"increase_allowance":{"spender":"<key-B>","amount":{"denom":"ujunox","amount":"2000000"}}}' \
  --from <admin-key-A> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```



# Update admin list to include admin-a
# juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562

`junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
'{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57", "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}' --from unsafe-test --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block `

junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
 


## Instantiate Contract

## Query Contract


## Update Admins
### Execute Contract

### Query Contract






## Next Chapter
#### [Chapter 06-cw1-subkeys](https://github.com/crispycret/cw-plus-walkthrough/blob/main/06-cw1-subkeys.md)






## RAW Notes




CW1-Whitelist:
    Hypothosis: 
        I believe this contract is only used to set admins of the contract.
        By itself this contract can only allow the publisher to set the admins. 
        Those admins can then mayber set an admin if they have high enough rights.
        Admins can be added that cannot be removed and admins can be added that can be removed.

    Conclusion:


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


'{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}'

junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"custom":{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 



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







