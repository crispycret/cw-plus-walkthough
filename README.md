# cw-plus-walktrhough
Walkthrough each contract in the cw-plus repository.


# Resource commands
```
junod tx bank send [from_key_or_address] [to_address] [amount] [flags]
junod query bank balances [address] [flags]


```
# Denomiations and values
The Metric System prefix u is micro or millionths which is 0.000001 of the base

UNSURE ABOUT THIS PART
We start out with 1000 juno.

1 juno = 1000000 ujunox
10 juno = 10000000 ujunox
100 juno = 1000000000 ujunox
1000 juno = 10000000000 ujunox



Contract Interaction Framework:
    Hypothosis:
    Conclusion:
    
    Store: 
    Instantiate:
    Execute:
    Query:





# cw1-whitelist:
## Hypothosis: 
        I believe this contract is only used to set admins of the contract.
        By itself this contract can only allow the publisher to set the admins. 
        Those admins can then mayber set an admin if they have high enough rights.
        Admins can be added that cannot be removed and admins can be added that can be removed.

## Conclusion:


# cw1-subkeys:
## Hypothosis:
        This contract incorporate cw1-whitelist and has all its functionality.
        This contract implements the ability to move the native token `juno` around in the form of allowances.
        Admins can set an allowance to another key. That key, if it is an admin can also set an allowance to another key.

## Conclusion:






