# CosmWasmResourceBook

Blockchain technology has obviously captured the attention of a substantial number of people, some of whom are aspiring developers, a group that I belong to. But, at least for me, the learning curve is equivalent to the Mongells trying to overcome great wall of China. 
 
A little background first. I fit in the category of coders who are self-taught and as a self-taught coder there is a large void in my understanding. As an example, I have always known that development required the use of an array of interconnected frameworks that make a system function. However, what I did not know is that this was called a tech stack... 
 
So, this is a resource dedicated to these people, my people, the people like me. The large swaths of aimless souls crawling on the surface of true development. The reason for building this resource is to anchor myself. To act as a starting point for my development career, which to this point has been non-existent. 
 
Enough story telling lets get into it. 
 
Obviously the first thing is to setup the environment, lets start there. 


## Environment Setup

I will assume that because it's 2022 your setup is Windows 10/11 runing `WSL2`.
If you don't know what I'm talking about or simply wish to following along, check out the resource below before moving on.

Cooming Soon. A few resources I need to compile to make this as straight forward as possible.
* WSL2 







### WSL2 Installation
As of now the `WSL2 Installation` steps aren't tested. They are built from resources and memory. I will take some time to walk through these steps myself on a fresh machine to test and make revisions where nessecary.

https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/

CURRENT RESOURCE LIST (Will be complied into this section
https://stackoverflow.com/questions/46413149/share-folder-between-windows-and-wsl




#### Basic Dependencies for install juno


Run the following commands to finish he basic dependencies.
```
# update the local package list and install any available upgrades
sudo apt-get update && sudo apt upgrade -y

# install toolchain and ensure accurate time synchronization
sudo apt-get install make build-essential gcc git jq chrony -y
```





##### SSH Setup



##### Install Docker
For WSL2 Install the Docker Desltop Application on windows.



##### Install Rust

First, install rustup. 

 `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

Once installed, make sure you have the wasm32 target:

```
rustup default stable
cargo version

# If this is lower than 1.50.0+, update
rustup update stable

rustup target list --installed
rustup target add wasm32-unknown-unknown
```




##### Install Go

Download GO source
```
wget https://golang.org/dl/go1.17.1.linux-amd64.tar.gz
```

Remove any previous GO installation (You may need to run the command with sudo).
```
sudo rm -rf /usr/local/go
```

Install GO
```
sudo tar -C /usr/local -xzf go1.17.linux-amd64.tar.gz
```

After that, open your users .profile file `nano ~/.profile` and add the following lines at the bottom of the file.
```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

After saving those changes run `source ~/.profile` for the changes to take place, or restart your shell.


##### Install Git

###### Configure git to use ssh.
Always remember when cloning a repo to reformat the url to git@github.com:user/the-repository.git.


### Install Juno

This is taken directly from the Juno Docs with some steps ommitted and added.
https://docs.junonetwork.io/smart-contracts-and-junod-development/getting-started

#### Download
Clone the Juno repository. Fetch. Checkout the current `testnet` version.

```
git clone git@github.com:CosmosContracts/juno.git

cd juno
git fetch
git checkout <version-tag>
```

#### Determining what version tag to use.

As of writing the `<version-tag>` we will use is `v2.1.0`. 
Make sure to look at the `Current Github version tag` in the Juno docs.
https://docs.junonetwork.io/validators/joining-the-testnets#current-testnets

The resulting command then is. 
```
git checkout v2.1.0
```


#### Building
Using this version of the repo we can now build. Make sure you are still in the juno directory.
'''
make install
'''

To confirm that the installation has succeeded and to check the version, you can run:

```
which junod

junod version
```
You can see that the github `version-tag` we used for checkout is the version that has been installed.




#### Configuring the environment.

The following section in the offical docs sets these system variables only for the current shell session. They do not persist when the shell is closed. I will assume that you will forget that you even set system variables when you come back later on. So, we will add these system variables in the `~/.profile` file so that they persist. Afterwards, you won't need to worry about these variables again.

So open the `.profile` file by running `nano ~/.profile` and we will add the following system variables at the bottom of the file.

First, we choose a testnet by specifying the `chain-id`. We want to use the localhost so the chain id will be `testing`


```
export CHAIN_ID=testing
export MONIKER_NAME="Test Validator"
```

Save the file and run `source ~/.profile` for these changes to take place or restart your shell.

Check that these changes did take place by checking the variables.

```
echo $CHAIN_ID
echo $MONIKER_NAME
```


#### Setting the minimum gas price.
You should have no problems not running this but since it's covered in the docs we will do it anyways.
```
# note testnet denom
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025ujunox\"/" ~/.juno/config/app.toml
```


### Setting up the Node
Were almost done with the setup, just a few more steps.
Note: We are only setting up a node not a validator. See https://docs.junonetwork.io/validators/joining-the-testnets#setting-up-the-node for how to upgrade your node to a validator.

#### Initialize the chain
```
junod init "$MONIKER_NAME --chain-id $CHAIN_ID
```

This will generate the following files in `~/.juno/config/`
* genesis.json
* node_key.json
* priv_validator_key.json


#### Download the genesis file
```
curl https://raw.githubusercontent.com/CosmosContracts/testnets/main/$CHAIN_ID/genesis.json > ~/.juno/config/genesis.json
curl https://raw.githubusercontent.com/CosmosContracts/testnets/main/uni-2/genesis.json > ~/.juno/config/genesis.json
```

This will replace the `genesis.json` file created using the `junod init` command with a genesis file that preloads an account with `ujunox`.

#### Import Unsafe Key
The `genesis.json` file allocates juno to the public unsafe test key which we will import now

```
junod keys add unsafe-test --recover
```

Seed Phrase: 
```
clip hire initial neck maid actor venue client foam budget lock catalog sweet steak waste crater broccoli pipe steak sister coyote moment obvious choose
```


## Start the Juno Testnet using Dockls
lser.
Run this juno docker image. Do not modify command as we will be using the address baked into the command.

```
docker run -it \
  -p 26656:26656 \
  -p 26657:26657 \
  -e STAKE_TOKEN=ujunox \
  -e UNSAFE_CORS=true \
  ghcr.io/cosmoscontracts/juno:v2.1.0 \
  ./setup_and_run.sh juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y
```


If your using WSL2 then you will notice that the docker desktop application has a new container inside that is running.
You can exit the terminal running the juno docker node using `crtl-c`. You will notice that after closing the juno node the docker container in Docker Desktop has stopped running. To start the juno node from now on all we need to do is click the play button on the container in the docker desktop application.




## Check Balance

Make sure that the account you imported has some juno.

```
junod q bank balances juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y

# or 

juno q bank balances $(junod keys show unsafe-test -a)

```


You should recieve an output similar to the following.

```
balances:
- amount: "1000000000"
  denom: ucosm
- amount: "1000000000"
  denom: ujunox
pagination:
  next_key: null
  total: "0"
```





Crediting Sources:
* https://docs.junonetwork.io/juno/readme
 
 


 
