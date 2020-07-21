# EOSIO Getting Started#1 (Development Environment)

## Prebuilt Binaries
Prebuilt EOSIO software packages are available for the operating systems below. Find and follow the instructions for your OS:

### Mac OS X Brew Install

```bash
brew tap eosio/eosio
brew install eosio
```

### Mac OS X Brew Uninstall

```bash
brew remove eosio
```

## Build EOSIO

Download EOSIO source from GitHub to build it. Create an executable EOSIO Binary Files.

### Getting eos

```bash
mkdir ${eos_folder}
cd ${eos_folder}
git clone https://github.com/EOSIO/eos --recursive
```

make your custom eos_folder and move. 

git clone from eos GitHub, 'git clone [https://github.com/EOSIO/eos](https://github.com/EOSIO/eos) --recursive'

### EOSIO Build

```bash
$ cd eos
$ ./script/eosio_build.sh
```

### EOSIO Install

```bash
$ sudo ./scirpt/eosio_build.sh
```

### EOSIO Binary File

```bash
cd ./build/bin
ls
cleos		eosio-blocklog	keosd		nodeos		trace_api_util
```

the environment variable to run the binary file above. vim .zshrc or .bashrc etc

## Create Development Wallet

Wallets are repositories of public-private key pairs. Private keys are needed to sign operations performed on the blockchain. Wallets are accessed using cleos.

### Step 1: Create a wallet

```bash
cleos wallet create --to-console
```

`cleos` will return a password, save this password somewhere as you will likely need it later in the tutorial.

```bash
Creating wallet: default
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5Kewn9L76X8Fpd....................t42S9XCw2"
```

### Step 2: Open the Wallet

Wallets are closed by default when starting a keosd instance, to begin, run the following

```bash
cleos wallet open
```

Run the following to return a list of wallets.

```bash
cleos wallet list
```

and it will return

```bash
Wallets:
[
  "default"
]
```

### Step 3 : Unlock it

The `keosd` wallet(s) have been opened, but is still locked. Moments ago you were provided a password, you're going to need that now.

```bash
cleos wallet unlock
```

You will be prompted for your password, paste it and press enter.

Now run the following command

```bash
cleos wallet list
```

It should now return

```bash
Wallets:
[ 
  "default *"
]
```

Pay special attention to the asterisk (*). This means that the wallet is currently **unlocked**

### Step 4: Import keys into your wallet

Generate a private key, `cleos` has a helper function for this, just run the following.

```bash
cleos wallet create_key
```

It will return something like..

```bash
Created new private key with a public key of: "EOS8PEJ5FM42xLpHK...X6PymQu97KrGDJQY5Y"
```

### Step 5: Import the Development Key

```bash
cleos wallet import
```

You'll be prompted for a private key, enter the `eosio` development key provided below

```bash
5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
```

## Start KEOSD and NODEOS

### Step 1 : Boot Wallet and Node

#### Step 1.1 : Start keosd

```bash
keosd &
```

#### Step 1.2: Start nodeos

```bash
% start.sh
--------------
nodeos -e -p eosio \
--plugin eosio::producer_plugin \
--plugin eosio::producer_api_plugin \
--plugin eosio::chain_api_plugin \
--plugin eosio::http_plugin \
--plugin eosio::history_plugin \
--plugin eosio::history_api_plugin \
--filter-on="*" \
--access-control-allow-origin='*' \
--contracts-console \
--http-validate-host=false \
--verbose-http-errors >> nodeos.log 2>&1 &
--------------
```

```bash
## default nodeos config file path
# linux : 
~/.local/share/eosio/nodeos/config/config.ini
~/.local/share/eosio/nodeos/data
# macosx :
~/Library/Application\ Support/eosio/nodeos/config/config.ini
~/Library/Application\ Support/eosio/nodoes/data
```

### Step 2 : Check the installation

#### Step 2:1 : Check that Nodeos is Producing Blocks

```bash
tail -f ./nodeos.log
```

#### Step 2.2: Check the Wallet

```bash
cleos wallet list
```

You should see a response with an list of wallets: (If the wallet is empty, see the section 'Create Development Wallet')

```bash
Wallets:
[
  "default *"
]
```

#### Step 2.3 : Check Nodeos endpoints

This will check that the RPC API is working correctly, pick one.

1. Check the `get_info` endpoint provided by the `chain_api_plugin` in your browser: [http://localhost:8888/v1/chain/get_info](http://localhost:8888/v1/chain/get_info)
2. Check the same thing, but in the console on your **host machine**

```bash
curl http://localhost:8888/v1/chain/get_info
```

github action test
