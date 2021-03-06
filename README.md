# **Connect into Private Ethereum Blockchain (eBlocPOW)**

Dashboard: [http://ebloc.cmpe.boun.edu.tr:3001/](http://ebloc.cmpe.boun.edu.tr:3001/)

## **Preinstallations**

### **Installation Instructions for Mac**

#### Pre-requirements

- If you don't have Homebrew, [install it first](https://brew.sh).

- From following link: https://nodejs.org/en/, download `10.10.0 Current`.

```bash
sudo npm install npm pm2 -g
brew install go
```

#### Geth Installation building from source

```
git clone https://github.com/ethereum/go-ethereum
cd go-ethereum
make geth
```

:warning: If something went wrong during building from source, install `go-ethereum` using Homebrew tap. :warning:

Run the following commands to add the tap and install `geth`:

```bash
brew tap ethereum/ethereum
brew install ethereum
```

### **Installation Instructions for Linux**

#### Node.js and Node Package Manager(npm) installation

```bash
sudo apt-get install nodejs
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm install pm2 -g
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

#### **Go-installation (https://github.com/golang/go/wiki/Ubuntu)**

```bash
sudo add-apt-repository ppa:gophers/archive
sudo apt-get update
sudo apt-get install golang-1.10-go
```

- Put this line `export PATH=$PATH:/usr/lib/go-1.10/bin`  into `$HOME/.profile` file and do `source $HOME/.profile`

#### **[Geth](https://github.com/ethereum/go-ethereum) Installation**

##### Pre-requirements

```bash
sudo apt-get install git
sudo apt-get install -y build-essential libgmp3-dev golang
```

##### Building from source

```bash
git clone https://github.com/ethereum/go-ethereum 
cd go-ethereum/
git pull
make geth
```

:warning: If something went wrong during building from source install `go-ethereum` from PPA :warning:

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

------------

#### Do following for both Linux and Mac

After `go-ethereum` is installed, copy `geth` located under `go-ethereum/build/bin` into` /usr/local/bin`:

```bash
$ ls go-ethereum/build/bin
geth
$ sudo cp build/bin/geth /usr/local/bin/
$ which geth
/usr/local/bin/geth
```

Please note that `Geth` version should be greater or equal than `1.8.15`.

```bash
$ geth version|grep "Version: 1"
Version: 1.8.15-stable
```

Now you can jump to [eBloc Setup on Linux and macOS](https://github.com/ebloc/eblocPOA/blob/master/README.md#ebloc-setup-on-linux-and-macos).

##### Please note that to update `geth`, please enter into `go-ethereum` directory and do:

```bash
git pull
make geth
```

if you face with any merging issues please do following:

```bash
latestTag=$(git describe --tags)

git describe --tags #returns most recent tag
git checkout $latestTag
make geth
```

----------------------

### **eBloc Setup on Linux and macOS**

#### Downloading

```bash
cd $HOME
git clone https://github.com/ebloc/eBlocPOW.git

cd eBlocPOW
git clone https://github.com/cubedro/eth-net-intelligence-api

cd eth-net-intelligence-api
npm install
```

#### Initialises a new genesis block and definition for the network

Navigate into `eBlocPOW` directory.

:warning: Do `./initialize.sh` only once. You do not need to do it again :warning:

```bash
./initialize.sh
```

#### Server run (Always run with `sudo`)

```bash
sudo ./server.sh
```

- If you want to kill your server please do: `sudo killall geth`
- You can keep track of output of your `geth-server` by running following: `sudo tail -f gethServer.out`

```
 sudo tail -f gethServer.out
Password:
INFO [02-12|16:22:34] Imported new chain segment   blocks=1  txs=0 mgas=0.000 elapsed=503.882ms mgasps=0.000  number=111203 hash=582a44...6e15dd
INFO [02-12|16:22:49] Imported new chain segment   blocks=1  txs=0 mgas=0.000 elapsed=491.377ms mgasps=0.000  number=111204 hash=b752ec...a0725d

```

#### Client run (geth console)

```bash
./client.sh
```

If you are successfully connected into `eBlocPOW` network inside `geth` console; `peerCount` should return 1 or more, after running `net` command.

-----------------

#### Create your Ethereum Account

**Creating an account:**

```bash
$ cd eBlocPOW
$ eblocPath="$PWD"
$ geth --datadir="$eblocPath" account new
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat passphrase:
Address: {a0a50a64cac0744dea5287d1025b8ef28aeff36e}
```

Your new account is locked with a password. Please give a password. Do not forget this password. Please enter a difficult passphrase for your account. 

You should see your `Keystore File (UTC / JSON)`under `keystore` directory. 

```bash
[~/eBlocPOW]$ ls keystore
UTC--2018-02-14T10-46-54.423218000Z--a0a50a64cac0744dea5287d1025b8ef28aeff36e
```

**On the console, use:**

You can also create your Ethereum account inside your `geth-client`. Here your `Keystore File` will be created with root permission, `eBlocWallet` will not able to unlock it.

```bash
> personal.newAccount()
Passphrase:
Repeat passphrase:
"0x7d334606c71417f944ff8ba5c09e3672066244f8"
```

Now you should see your `Keystore File (UTC / JSON)`under `private/keystore` directory. 

```bash
[~/eBlocPOW]$ ls private/keystore
UTC--2018-02-14T11-00-59.995395000Z--7d334606c71417f944ff8ba5c09e3672066244f8
```

To give open acccess to the keystore file:

```bash
sudo chown -R $(whoami) private/keystore/UTC--...
```

-----------------

#### **How to attach to eBloc Network Status (http://ebloc.cmpe.boun.edu.tr:3001)**

##### To Run

- Please open `stats.sh` file under `eBlocPOW`directory. Write your unique name instead of `mynameis`.

- :warning: Change `DATADIR` variable with path for `eth-net-intelligence-api` directory :warning:

- :warning: `geth-server` should be running on the background :warning:

##### Finally you should run following command

```
./stats.sh
```

- `sudo pm2 show app` should return some output starting with `"status | online`.

Now, you should see your node on http://ebloc.cmpe.boun.edu.tr:3001.

- If you successfully see your name, put this line `./stats.sh` into the last line of `server.sh` file.

-----------------

#### **Access your Ethereum Account using eBlocWallet**

In order to use `eBlocWallet`, first `sudo ./server.sh` should be executed, hence `geth-server` should run on the background.

Later, open (http://ebloc.cmpe.boun.edu.tr:3002). Then on the right top corner press:

`(),` => `Add Custom Node` => `Save & Use Custom Node`.

Now if the read warning message is removed, your eBlocWallet is connected to your `geth-server`.

`Send Ether and Tokes` => Select `Keystore File (UTC / JSON)`=>`SELECT WALLET FILE` (Your wallet is located under `eBlocPOW/keystore` name starting with `UTC`) => `Unlock`

<img width="1100" alt="screen shot 2018-02-12 at 13 29 13" src="https://user-images.githubusercontent.com/18537398/36092854-dfb53d80-0ff9-11e8-9dc5-b2e788d01bd5.png">

Later you should see your account information (balance, account, etc).

<img width="1088" alt="screen shot 2018-02-12 at 13 38 48" src="https://user-images.githubusercontent.com/18537398/36092942-2fa6c0ac-0ffa-11e8-9664-4f9df315f407.png">

-----------------

#### **Helpful commands on geth client**

Please try following commands on your `geth-client` console.

```
Welcome to the Geth JavaScript console!

instance: Geth/v1.7.3-stable/darwin-amd64/go1.9.2
 modules: admin:1.0 clique:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> net
{
  listening: true,
  peerCount: 1,
  version: "23422",
  getListening: function(callback),
  getPeerCount: function(callback),
  getVersion: function(callback)
}

# How to check list of accounts
> eth.accounts
["0x3b027ff2d229dd1c7918910dee32048f5f65b70d", "0x472eea7de6a43b6e55d8be84d5d29879df42a46c"]

> sender=eth.accounts[0]
"0x3b027ff2d229dd1c7918910dee32048f5f65b70d"

> reciever=eth.accounts[1]
"0x472eea7de6a43b6e55d8be84d5d29879df42a46c"

# How to check your balance
> web3.fromWei(eth.getBalance(sender))
100

# How to unlock your Ethereum account
> personal.unlockAccount(sender)
Unlock account 0x3b027ff2d229dd1c7918910dee32048f5f65b70d
Passphrase:
true

# How to send ether to another account
> eth.sendTransaction({from:sender, to:reciever, value: web3.toWei(0.00001, "ether")})
"0xf92c11b6bd80ab12d5d63f7c6909ac7fc45a6b8052c29256dd28bd97b6375f1b"  #This is your transaction receipt.

# How to get receipt of your transaction
> eth.getTransactionReceipt("0xf92c11b6bd80ab12d5d63f7c6909ac7fc45a6b8052c29256dd28bd97b6375f1b")
{
  blockHash: "0x17325837f38ff84c0337db87f13b9496f546645366ebd94c7e78c6a4c0cb5a87",
  blockNumber: 111178,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: "0x3b027ff2d229dd1c7918910dee32048f5f65b70d",
  gasUsed: 21000,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  status: "0x1",
  to: "0x472eea7de6a43b6e55d8be84d5d29879df42a46c",
  transactionHash: "0xf92c11b6bd80ab12d5d63f7c6909ac7fc45a6b8052c29256dd28bd97b6375f1b",
  transactionIndex: 0
}
```

#### **Some helpful links**

- [Managing your accounts](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
- [Sending Ether on geth-client](https://github.com/ethereum/go-ethereum/wiki/Sending-ether)
