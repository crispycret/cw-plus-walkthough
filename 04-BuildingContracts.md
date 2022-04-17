

## Building Contracts
 The first thing we must do is build all the contracts. Thankfully we can compile them all at once with the following command.

```
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
```

This will take a while.

## Ready to Store Contracts

Now that all the contracts are compiled we can store them onto the juno blockchain.



