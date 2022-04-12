# cw-plus-walktrhough
Walkthrough each contract in the cw-plus repository.

cw-plus work notes

# Resource commands
```
junod tx bank send [from_key_or_address] [to_address] [amount] [flags]
junod query bank balances [address] [flags]

junod tx bank send $(junod keys show unsafe-test -a) juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9 40000000ujuno --generate-only
```
# Denomiations and values
The Metric System prefix u is micro or millionths which is 0.000001 of the base


UNSURE ABOUT THIS PART
We start out with 1000 juno.

1 juno = 1000000 ujunox
10 juno = 10000000 ujunox
100 juno = 1000000000 ujunox
1000 juno = 10000000000 ujunox
           1000000000
           951474818



Contract Interaction Framwork:
    Hypothosis:
    Conclusion:
    
    Store: 
    Instantiate:
    Execute:
    Query:





# CW1-Whitelist:
## Hypothosis: 
        I believe this contract is only used to set admins of the contract.
        By itself this contract can only allow the publisher to set the admins. 
        Those admins can then mayber set an admin if they have high enough rights.
        Admins can be added that cannot be removed and admins can be added that can be removed.

## Conclusion:

## Store: 
```
junod tx wasm store cw1_whitelist.wasm  --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```
#### Code ID: 3
    

## Instantiate:

```
junod tx wasm instantiate 3 '{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y", "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9"], "mutable":true}' --amount 50000ujunox --label "cw1-whitelist" --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```


#### Contract Address: 
```
juno1fzm6gzyccl8jvdv3qq6hp9vs6ylaruervs4m06c7k0ntzn2f8faqxt5lvx - Not mutable
juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t - Mutable
```

 ## Query:
```
junod query wasm contract-state smart juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"admin_list":{}}' --chain-id testing
```

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"update_admins": {"admins":["juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562", "juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8"]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block
```

## Execute:

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
  '{"increase_allowance":{"spender":"<key-B>","amount":{"denom":"ujunox","amount":"2000000"}}}' \
  --from <admin-key-A> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```



## Update admin list to include admin-a
#### juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t \
'{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57", "juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9", "juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}' --from unsafe-test --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```

``
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
 ```
 
 
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

```
'{"execute":{"msgs":[{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}]}}'
```

```
junod tx wasm execute juno18egdakntewpnhr9u4wml6rygyszzanapquefkn4fmywt9uevvwzslx4s5t '{"execute":{"msgs":[{"custom":{"update_admins":{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57","juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9","juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562"]}}}]}}' --from master --chain-id testing --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block 
```

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







```
cw1-subkeys:
    Hypothosis:
        This contract incorporate cw1-whitelist and has all its functionality.
        This contract implements the ability to move the native token `juno` around in the form of allowances.
        Admins can set an allowance to another key. That key, if it is an admin can also set an allowance to another key.

    Conclusion:
```






