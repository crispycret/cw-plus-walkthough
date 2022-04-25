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
```
INSTANTIATE_MSG=""

CW3_FIXED_MULTISIG_CONTRACT_ADDRESS=$()
```


# QUERY:

# EXECUTE:






## [Next Chapter - 08 CW3 Flex Multisig](08-cw3-flex-multisig.md)


## [Previous Chapter - 06 CW1 Subkeys.md](06-cw1-subkeys.md)
