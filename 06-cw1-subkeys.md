# CW1 Subkeys

## Description

This contract incorporartes cw1-whitelist and therefore has all of the same functionality. It expands functionality by allowing allowances of a native token to be set by admins.

## Store contract
```
junod tx wasm store cw1_subkeys.wasm  --from unsafe-test --chain-id testing \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```


## Or Storing while storing TX and Contrant ID Code
```
TX=$(junod tx wasm store cw1_subkeys.wasm  --from test --chain-id=testing --gas auto --output json -y | jq -r '.txhash')
CODE_ID=$(junod query tx $TX --output json | jq -r '.logs[0].events[-1].attributes[0].value')
```

## Instantiate contract
```
junod tx wasm instantiate <code-id> '{"admins":["$master"],"mutable":false}' --amount 50000ujunox --label "CW1 example contract" --from <your-key> --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

```
junod tx wasm instantiate 1 '{"admins":["juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y"],"mutable":false}' --amount 50000ujunox --label "CW1 example contract" --from test --chain-id testing \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block -y
```

## Find contract address
```
junod query wasm list-contract-by-code <code-id>
junod query wasm list-contract-by-code 1
```





## Query commands
```
junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"allowance":{"spender":"juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y"}}' --chain-id testing

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"admin_list":{}}' --chain-id testing

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"all_allowances":{}}' --chain-id testing
```

## Execute commands
```
junod tx wasm execute juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 \
  '{"increase_allowance":{"spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl","amount":{"denom":"ujunox","amount":"2000000"}}}' \
  --from juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y \
  --chain-id testing \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block

junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"allowance":{"spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl"}}' --chain-id testing


junod q bank balances <key-C>


junod tx wasm execute <contract-addr> \
  '{"execute":{"msgs":[{"bank":{"send":{"to_address":"<key-C>","amount":[{"denom":"ujunox","amount":"500"}]}}}]}}' \
  --from <key-B> \
  --chain-id <chain-id> \
  --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -b block


junod q bank balances <key-C>


junod query wasm contract-state smart juno14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9skjuwg8 '{"allowance":{"spender":"juno1rm8eja6cczs0y0y6vwy9tnufe74t785ffu6cfl"}}' --chain-id testing
```
  
  
  
## [Next Chapter - 05 CW1 Whitelist](05-cw1-whitelist.md)


## [Previous Chapter - 07 Fixed Multisig](07-fixed-multisig.md)

