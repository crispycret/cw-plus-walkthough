# Getting Started


Before starting this series we must do the following.

* install junod

* Set the `moniker name` (Optional) and `chain-id`.
  The moniker name can be anything and for the chain-id we will use `testing` to connect to the localhost node.

Run this juno docker image. Do not modify the default address in the docker command as we will be using it.

```
docker run -it \
  -p 26656:26656 \
  -p 26657:26657 \
  -e STAKE_TOKEN=ujunox \
  -e UNSAFE_CORS=true \
  ghcr.io/cosmoscontracts/juno:v2.1.0 \
  ./setup_and_run.sh juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y
```

After the juno daemon is running we can import juno's unsafe key that we will use to allocate funds to a different address.

## Import The Unsafe Key

Run the below command and import the seed phrase to recover the account.

```
junod keys add unsafe-test --recover
```

Seed Phrase: 
```
clip hire initial neck maid actor venue client foam budget lock catalog sweet steak waste crater broccoli pipe steak sister coyote moment obvious choose
```

Now that we have the unsafe key names `unsafe-test` we will create a few more juno accounts to store, instatiate and interact with the contracts.


## Create Juno Accounts

We want to create a `master` key that will be used for storing and instatiating contracts.
Then we want to create a `reserve` key that will be used to fund all other accounts.
Then we want to create 3 more accounts that will act as `admins` for the contracts.
Then we want to create 3 more accounts that will act as `users` of the contracts.

After the creation of each new key, store it's address and seed phrase in a file.
This is insecure, but since this is for localhost testing and not production we don't need to worry.
When creating a new account you must provide an 8 character password. I just use password for ease of use in testing.

Here are the commands and the resulting file of account information.

```
junod keys add master

juno keys add reserve

juno keys add admin-a
juno keys add admin-b
juno keys add admin-c

juno keys add user-a
juno keys add user-b
juno keys add user-c
```

## Juno Account Information File

After copying and stripping away irrelevant information this is what my accounts file looks like.

```
# Top
unsafe-test: juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y

master: juno1uzaa2sexws4gatetng5ke0lrqpfy89khd990u9
water remember cushion abuse banner rice sheriff image army siege pony wink strong cart knee vocal used cheese choose during foam mango erode lamp

reserve: juno1vrau67dz6xej65kzjvqw24jpxy27uvhs8t06hj
opera supply sunny add actor mercy bus camera stage mind spare win code clap critic know athlete vibrant endless access circle sunny ozone taxi



# Admins
admin-a: juno10pfa9a5l8sy0czqjy7tlquyhrmjn90yhr50562
fall three item coconut lizard climb liar escape midnight subway trial million father upper subject develop view hand sauce radar coast deliver water joke

admin-b: juno1hry3jzq5vca4uufl79cvqqmjqe53qfaxkxuc3u
bus umbrella rose follow gallery brain inflict solar ceiling picnic monitor announce merge remain foot brass panic accuse dilemma taste goddess share warm inspire

admin-c: juno1ayw38tapu8wd3l57fwdhwekcymhcueh59p2pa8
civil nasty shallow tank base pony exact fatigue thought subject water nest hobby way result bind sport film daughter lawsuit silk rally shiver market


# Users
user-a: juno1f7qvu4gzjc3c7vzxl845shtzwewqdd8hdm7ql7
sun uphold butter city coin ski guitar mention yellow broom beyond assume satoshi dragon monster whale announce bulk hawk drink scale resemble render desert

user-b: juno14dnyyawm4c3pr88fscnqu5ppt5dp0cq7tum5xy
actual science notice elder unit reject erosion clinic artefact rose renew mystery swamp frame moon raven saddle eye helmet fatal mansion must six crumble

user-c: juno1pz65jurpjjfmhwusqhv8d9mruyyty4845wm767
dune shy loop water knife leaf baby toe clean lobster general wine joke fantasy leaf used century lens midnight enjoy ball similar athlete radio

```


## Funding Accounts
Now that we have all of our accounts created lets drop them some juno.
First make sure that the `unsafe-test` key has been populated with some juno.

You can query the balances of an address by providing the address manually or you can retrieve the address using the keyname like I do below. 

I will be using the retrivial method on these commands simply to show which account I am using. If you have your juno accounts file you can just copy paste your recieving address key.

```
junod query bank balances $(junod keys show unsafe-test -a)
```

Now that we can see that the `unsafe-test` key has juno lets send some to the `master` account.


```
junod tx bank send unsafe-test $(junod keys show master -a) 10000000ujunox --chain-id testing
```

Checking the `master` accounts balance we can see that the transaction was a success.

```
junod query bank balances $(junod keys show master -a)
```

Now let's fund the rest of the accounts. We will add funds to the `reserve` key first. From the `reserve` key we will send funds to `admin-a`. Then from `admin-a` we send funds to `admin-b` and from `admin-b` to `admin-c`.
We do it this way just so we can familarize ourselves with the juno cli a little better.

```
junod tx bank send unsafe-test $(junod keys show reserve -a) 20000000ujunox --chain-id testing

junod tx bank send reserve $(junod keys show admin-a -a) 15000000ujunox --chain-id testing
junod tx bank send admin-a $(junod keys show admin-b -a) 10000000ujunox --chain-id testing
junod tx bank send admin-b $(junod keys show admin-c -a) 5000000ujunox --chain-id testing
```

Now that the admin accounts each have 5 juno lets send some to the user accounts and top off the `reserve` account one more time.

```
junod tx bank send reserve $(junod keys show user-a -a) 1000000ujunox --chain-id testing
junod tx bank send reserve $(junod keys show user-b -a) 1000000ujunox --chain-id testing
junod tx bank send reserve $(junod keys show user-c -a) 1000000ujunox --chain-id testing

junod tx bank send unsafe-test $(junod keys show reserve -a) 18000000ujunox --chain-id testing
```


### Checking balances

Lets check the balances of our accounts.

```
junod query bank balances $(junod keys show unsafe-test -a)

junod query bank balances $(junod keys show master -a)
junod query bank balances $(junod keys show reserve -a)

junod query bank balances $(junod keys show admin-a -a)
junod query bank balances $(junod keys show admin-b -a)
junod query bank balances $(junod keys show admin-c -a)

junod query bank balances $(junod keys show user-a -a)
junod query bank balances $(junod keys show user-b -a)
junod query bank balances $(junod keys show user-c -a)
```

Now that we have accounts funded with juno let's start interacting with the contracts.


## Building Contracts
 The first thing we must do is build all the contracts. Thankfully we can compile them all at once with the following command.

'''
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.11.3
'''

This will take a while.

## Ready to Store Contracts

Now that all the contracts are compiled we can store them onto the juno blockchain.



