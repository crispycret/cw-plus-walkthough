
## Funding Accounts
Now that we have all of our accounts created lets drop them some juno.
First make sure that the `unsafe-test` key has been populated with some juno.

You can query the balances of an address by providing the address manually or you can retrieve the address using the keyname like I do below. 

I will be using the retrivial method on these commands simply to show which account I am using. If you have your juno accounts file you can just copy paste your recieving address key.

If you did not set enviornment variables for you address keynames then a way to perform queries without copy/pasting your address is to query for your address by keyname.

```
junod q bank balances $(junod keys show unsafe-test -a)
```

Otherwise if set environment variables you can do the following with the same result
```
junod q bank balances $unsafe_test
```

Now that we can see that the `unsafe-test` key has juno lets send some to the `master` account.


```
junod tx bank send unsafe-test $master 10000000ujunox --chain-id testing
```

Checking the `master` accounts balance we can see that the transaction was a success.

```
junod query bank balances $master
```

Now let's fund the rest of the accounts. We will add funds to the `reserve` key first. From the `reserve` key we will send funds to `admin-a`. Then from `admin-a` we send funds to `admin-b` and from `admin-b` to `admin-c`.
We do it this way just so we can familarize ourselves with the juno cli a little better.

```
junod tx bank send unsafe-test $reserve 20000000ujunox --chain-id testing

junod tx bank send reserve $admin_a 15000000ujunox --chain-id testing
junod tx bank send admin-a $admin_b 10000000ujunox --chain-id testing
junod tx bank send admin-b $admin_c 5000000ujunox --chain-id testing
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

