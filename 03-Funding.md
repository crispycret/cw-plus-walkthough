
# Funding Accounts
Now that we have all of our accounts created lets drop them some juno. First make sure that the `unsafe-test` key has been populated with some juno.

You can query the balances of an address by providing the keyname, or by environment variable, or by manually providing the account address. 

I will mostly be using environment variables for these commands. If you have your juno accounts file you can just copy paste your addresses. For now I will do the same command using each method but expect for me to be using only the environment variables soon. If there are any major changes to commands due to using environment variables I will provide the offical way to execute the command. 

I recommed using the environemnt variables while learning to save time.

## Checking Account Balances

### Method 1

```
junod q bank balances juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y

# OR

junod q bank balances $(junod keys show unsafe-test -a)
```

### Method 2

Otherwise if set environment variables you can do the following with the same result

```
junod q bank balances $UNSAFE_TEST
```


## Sending Tokens

Now that we can see that the `unsafe-test` key has juno lets send some to the `master` account.


```
junod tx bank send <sender-keyname> <reciever-addr> <amount> <chain>

junod tx bank send unsafe-test $MASTER 10000000ujunox --chain-id testing
```

Checking the `master` accounts balance we can see that the transaction was a success.

```
junod query bank balances $MASTER
```

## Funding Accounts for later use

Now let's fund the rest of the accounts. We will add funds to the `reserve` key first. From the `reserve` key we will send funds to `admin-a`. Then from `admin-a` we send funds to `admin-b` and from `admin-b` to `admin-c`.
We do it this way just so we can familarize ourselves with the juno cli a little better.

```
junod tx bank send unsafe-test $RESERVE 20000000ujunox --chain-id testing

junod tx bank send reserve $ADMIN_A 15000000ujunox --chain-id testing
junod tx bank send admin-a $ADMIN_B 10000000ujunox --chain-id testing
junod tx bank send admin-b $ADMIN_C 5000000ujunox --chain-id testing
```

Now that the admin accounts each have 5 juno lets send some to the user accounts and top off the `reserve` account one more time.

```
junod tx bank send reserve $(junod keys show user-a -a) 1000000ujunox --chain-id testing
junod tx bank send reserve $(junod keys show user-b -a) 1000000ujunox --chain-id testing
junod tx bank send reserve $USER_C 1000000ujunox --chain-id testing

junod tx bank send unsafe-test $(junod keys show reserve -a) 18000000ujunox --chain-id testing
```


### Checking balances

Lets check the balances of our accounts.

```
junod query bank balances juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y

junod query bank balances $(junod keys show master -a)
junod query bank balances $RESERVE

junod query bank balances $ADMIN_A
junod query bank balances $ADMIN_B
junod query bank balances $ADMIN_C

junod query bank balances $USER_A
junod query bank balances $USER_B
junod query bank balances $USER_C
```

Now that we have accounts funded with juno let's start interacting with the contracts.


# [Next Chapter - 04 Building Contracts](04-BuildingContracts.md)


# [Previous Chapter - 02 Accounts](02-Accounts.md)
