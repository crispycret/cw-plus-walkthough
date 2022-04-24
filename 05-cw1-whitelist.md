# CW1 Whitelist


## Description

This contract allows admins of a contract to be set. If the contract variable `mutable=true` then admins can be added or removed. If `mutable=false` then the admins that were set at the instantiation of the contract will be the only admins allowed for this contract and once `mutable=false` it never again be equal to `true`.


### Format

For commands that output important information there will be two versions of doing the same thing. The first method will be the offical way to store, instantiate, and intreact with contracts while the second version of the same command incorporates `bash` to find and store important values such as the `transaction hash, code id, and contract address` as temporary environment variables which can be set to persist through new shell sessions if more steps are taken. 






# Store Contract

Storing the contract on the blockchain will result in the output of the contracts id. The `code id` is the index of stored contracts on the blockchain and is important to note down.


### Method 1

This method of storing the contract on the blockchain does not track the `code id` of the contract and requires you to find the `code id` from the output.

```
junod tx wasm store cw1_whitelist.wasm  --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```
   

### Method 2 

Store the contracts TX Hash and the CODE ID as temporary environment variables

```
cd artifacts

CW1_WHITELIST_STORE_TX=$(junod tx wasm store cw1_whitelist.wasm  --from master --chain-id=testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block --output json -y | jq -r '.txhash')

CW1_WHITELIST_CODE_ID=$(junod query tx $CW1_WHITELIST_STORE_TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')

echo $CW1_WHITELIST_CODE_ID
```

### Store the Contract Code ID as an Environment Variable For Persistence between sessions (OPTIONAL)

Add this line to the bottom of `~/.profile` and add your code id. 

```
export CW1_WHITELIST_CODE_ID=<CodeID>
```

 
 
 

# Instantiate Contract

To instantiate the contract we must reference the contracts `code id`. 
The contract instantiation output will contain inside of it the `contract address`. 

When instantiating if no admin(s) is designated than the uploader of the contract will become the sole admin of the contract. We want to set the variable `mutable` as true so that after instantiation admins can be removed or added by other admins.

### Method 1: 

If you did not use `Method 2` to store the contract replace `$CW1_WHITELIST_CODE_ID` with your contract `code id` you found from the output.

You can etiher directly replace `<ADMIN_ADDRESS_1>, <Admin_Address_2>` with your admin addresses or you can use `Method 2` which bakes in the admin addresses if you have setup the environment variables.

```
junod tx wasm instantiate $CW1_WHITELIST_CODE_ID '{"admins":["<ADMIN_ADDRESS_1>", "<ADMIN_ADDRESS_2>"], "mutable":true}' \
   --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing \
   --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

### Method 2:

Bake Admin A and Admin B into the contract and stores the CONTRACT ADDRESS in a temporary variable.

```
CW1_WHITELIST_CONTRACT_ADDRESS=$(junod tx wasm instantiate $CW1_WHITELIST_CODE_ID "{\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\"], \"mutable\":true}" \
   --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing \
   --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y \
   --output json | jq -r '.logs[0].events[2].attributes[0].value')

echo $CW1_WHITELIST_CONTRACT_ADDRESS
```


### Store the Contract Address as an Environment Variable For Persistence between sessions (OPTIONAL)

Add this line to the bottom of `~/.profile` and add your contract address.  

```
export CW1_WHITELIST_CONTRACT_ADDRESS=<ContractAddress>
```





# Query:

Before we start inspecting the smart contract lets first examine the blockchain to see our contract.


### Examine Blockchain
```
# Show number of contracts stored on-chain
junod q wasm list-code

# Show the number of instantiations of our contract
junod q wasm list-contract-by-code $CW1_WHITELIST_CODE_ID
```

### Examine the Contract

Display the admins of this contract.

```
junod query wasm contract-state smart $CW1_WHITELIST_CONTRACT_ADDRESS '{"admin_list":{}}' --chain-id testing
```






# Execute:

We have stored, initalized and queried the contract. Now it is time to make alterations with to the state of the contract.

The functionality of `CW1 Whitelist` is pretty limited. Let's update the admins of the contract. If you want to add an admin you must also include all current admins when updating the contract otherwise that account will no longer be an admin.

Messages as passed as JSON. For us to be able to bake in environment variables to the JSON string we must use double quotes everywhere, which means we must escape double quotes as well. This is sloppy and not perferred. I will show exmplaes of messages in readable JSON, normal JSON, and baked JSON.

### Execute Command Structure
```
junod tx wasm execute <contract-address> <execute_msg> --from <key> --chain-id <chain-id>  [options]
```

## Execute Message Structure

```
'{"<action>": {"<parameter>": "<value>" }}' 
```

The `<value>` field can be a `string, integer, array, or an object` defined by the type of the parameter located in the contracts schema for this message as well as being a valid json type.


## Update Admins
The only execute message the `cw1-whitelist` contract defines is that of the `update_admins` message which updates the state of the contracts admin list.


### Update Admins - Execute Message Structure
We want to message about updating the admins list: `<action> -> update_admins`.

The message`update_admins` has a single `<parameter> -> admins` which is a list of addresses the contract should use as admins.

#### Readable JSON is NOT VALID
```
'{"update_admins": {
      "admins":[
         "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", 
         "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562",
         "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"
      ]
   }
}'
```

#### Valid JSON
```
'{"update_admins": {"admins":["juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562", "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"]}}'
```

#### Valid JSON baked using environment variables
```
"{\"update_admins\": {\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\", \"$ADMIN_C\"]}}"
```



### Execute Command With Normal JSON
```
junod tx wasm execute $CW1_WHITELIST_CONTRACT_ADDRESS \
   '{"update_admins": {"admins":["juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562",  "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"]}}' \
   --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block
```

### Execute Command With Baked JSON
```
junod tx wasm execute $CW1_WHITELIST_CONTRACT_ADDRESS \
   "{\"update_admins\": {\"admins\":[\"$ADMIN_A\", \"$ADMIN_B\", \"$ADMIN_C\"]}}" \
   --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block
```






## [Next Chapter - 06 CW1 Subkeys](06-cw1-subkeys.md)


## [Previous Chapter - 04 Building Contracts](04-BuildingContracts.md)



.

.

.

.

.

.



# RAW Notes (IGNORE)


```
junod tx wasm execute $CW1_WHITELIST_CONTRACT_ADDRESS \
  '{"increase_allowance":{"spender":"<Address-B>","amount":{"denom":"ujunox","amount":"2000000"}}}' \
  --from <admin-key-A> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```




### CW1-Whitelist:
##### Hypothosis: 

I believe this contract is only used to set admins of the contract.
By itself this contract can only allow the publisher to set the admins. 
Those admins can then mayber set an admin if they have high enough rights.
Admins can be added that cannot be removed and admins can be added that can be removed.

##### Conclusion:
<Explain What I know Here>


#### Execute Message as ReadableJson
##
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



