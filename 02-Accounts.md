



## Create Juno Accounts

Now that we have the unsafe key names `unsafe-test` we will create a few more juno accounts to store, instatiate and interact with the contracts.

* We want to create a `master` key that will be used for storing and instatiating contracts.
* Then we want to create 3 more accounts that will act as `admins` for the contracts.
* Then we want to create 3 more accounts that will act as `users` of the contracts.

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

master: juno1p8g3ds77grvvzgkga8jyejcse9ugfm5cp3087v 
twin start nut impulse exercise hold vicious arrow cry silly wild drum meadow soap boy derive food toe oxygen margin blade inherit indicate shuffle

reserve: juno1raqvz3j2aystuq627h674v2x4s2alsyqkwescm
ignore pottery critic edge wonder wasp grief opera duty usual nice twenty scatter sort team switch alter over gasp middle broccoli leaf matrix stick


# Admins
admin-a: juno1z5jvxjgayudwad7keejcqkv5fq2xsyctjn9fz2
viable proof cushion engine wrap pistol average obtain ladder stick seat force health budget team degree flee inch purpose pistol ski detect caution mention

admin-b: juno120mtrgpem5r4lefrxnlgr6kf8g7lgekjrqsum6
bomb clog soldier settle normal aware uniform proof tongue sunset carry involve field push dawn mean swear abstract usage host shoulder fly mirror hurry

admin-c: juno1ezfkye82kwxdxp6qczvdvqtrxu5uw84hln3l0d
river patrol mushroom arch license toy repair culture fish home dirt unknown boss radar electric law derive access assist inhale rail hour decade want

# Users
user-a: juno14z5gmxggpff5lpmftwf4n7s2s6vsu3tc4r2wpl
grape seed purpose pigeon cluster diary number engine elbow olympic skate toward gas maximum frequent crucial venue offer witness quality shoot glue viable grape

user-b: juno1w8g0lsmgvtdhrl6tuw7rcrruma0gzpav3ydvkm
dream intact buddy alcohol mobile access tell enable leaf session glove dinosaur room crisp wash february credit leave chronic powder arch embody slush quick

user-c: juno1r9ztg5rxkyekrgmt760qscum5hatvpfy6gf3s5
vault elbow neck twin garden waste first ridge warfare license split anger swim vague one stool worry oval egg omit supreme child wedding pottery
```

If you want to take some extra time to store the public keys as environment variables then you should put something similar to the below at the bottom of the `~/.profile` file.

```
# JUNO Vars
export CHAIN_ID="testing"
export MONIKER_NAME="ValidatorName"

# JUNO Accounts
export UNSAFE_TEST=juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y
export MASTER=juno1n2r0urfl5yezwcnfsllst0qsakmfqsy7rf3jdr
export RESERVE=juno1gl9njgz9s3y82cas7f0rv72qjydm7jx93kf8h6
export ADMIN_A=juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh
export ADMIN_B=juno1d3pdywztwek0v5sxqa8t6kwq7fkqfrl8y94tqw
export ADMIN_C=juno1mt5we8478ud82auklx5mcj8mjxjxsh808kqgtm
export USER_A=juno1f7qvu4gzjc3c7vzxl845shtzwewqdd8hdm7ql7
export USER_B=juno14dnyyawm4c3pr88fscnqu5ppt5dp0cq7tum5xy
export USER_C=juno1pz65jurpjjfmhwusqhv8d9mruyyty4845wm767

# Juno Contracts
...
...
```


# Next Chapter
##### [03-BuildingContracts.md](03-Funding.md)


# Previous Chapter
#### [01-GettingStarted.md](01-GettingStarted.md)

