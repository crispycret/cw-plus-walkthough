# CW1-Whitelist



## Store Contract

Storing the contract on the blockchain will result in the output of the contracts id. The `code id` is the index of stored contracts on the blockchain.  

`junod tx wasm store cw1_whitelist.wasm  --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y`

Code ID: 1
    

## Instantiate Contract

Using the `code id` we can instantiate the contract to create an instance of the contract on-chain.

The result of instantiating the contract will display to us the contract address if you look around good enough for it and a `tx hash` that we can use to locate the contract address directly. 

 We will instantiate the contract by providing two admins (admin-a and admin-b) and we allow admins modification (add/remove) by setting `mutable` to true.

```
junod tx wasm instantiate 3 '{"admins":["$admin_a", "$admin_b"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```


From the output of the above command you can locate the `contract address` or you can copy the `tx hash` and run a query to locate the contract address



```
$TX=

junod tx query $TX
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








## Notes




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







