
# Download the contracts

You can download the `cw-plus` contracts from the [offical github repoistory](https://github.com/CosmWasm/cw-plus). You should look through these docs for more information.

```
git clone https://github.com/CosmWasm/cw-plus.git

# OR
git clone git@github.com:CosmWasm/cw-plus.git
```


# Building Contracts
We could build each contract separtely but it is easier to build them all at once. 

```
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
```

This will take a while.


## Ready to Use Contracts

Now that all the contracts are compiled we can start to store them on the local juno node.


## [Next Chapter - 05 CW1 Whitelist](05-cw1-whitelist.md)


## [Previous Chapter - 03 Funding](03-Funding.md)



