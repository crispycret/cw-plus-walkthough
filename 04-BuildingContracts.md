

# Building Contracts

To interact with smart contracts on the blockchain we must first build/compile all the contracts. We can compile them all at once with the following command.

```
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
```

This will take a while.


## Ready to Use Contracts

Now that all the contracts are compiled we can start to store them onto our local juno testing blockchain.


## [Next Chapter - 05 CW1 Whitelist](05-cw1-whitelist.md)


## [Previous Chapter - 03 Funding](03-Funding.md)



