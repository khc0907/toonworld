## ToonWorld Based On Go Ethereum

Official Golang implementation of the Ethereum protocol.

[![API Reference](
https://camo.githubusercontent.com/915b7be44ada53c290eb157634330494ebe3e30a/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f676f6c616e672f6764646f3f7374617475732e737667
)](https://pkg.go.dev/github.com/ethereum/go-ethereum?tab=doc)
[![Go Report Card](https://goreportcard.com/badge/github.com/ethereum/go-ethereum)](https://goreportcard.com/report/github.com/ethereum/go-ethereum)
[![Travis](https://travis-ci.com/ethereum/go-ethereum.svg?branch=master)](https://travis-ci.com/ethereum/go-ethereum)


Automated builds are available for stable releases and the unstable master branch. Binary
archives are published at https://geth.ethereum.org/downloads/.

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions](https://geth.ethereum.org/docs/install-and-build/installing-geth).

Building `geth` requires both a Go (version 1.14 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
make geth
```

or, to build the full suite of utilities:

```shell
make all
```

#### Defining the private genesis state

First, you'll need to create the genesis state of your networks, which all nodes need to be
aware of and agree upon. This consists of a small JSON file (e.g. call it `toonworld_genesis.json`):

```json
{
   "config": {
      "chainId": 5253,
      "homesteadBlock": 0,
      "eip150Block": 0,
      "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "eip155Block": 0,
      "eip158Block": 0,
      "byzantiumBlock": 0,
      "constantinopleBlock": 0,
      "petersburgBlock": 0,
      "istanbulBlock": 0,
      "clique": {
         "period": 15,
         "epoch": 30000
      }
   },
   "nonce": "0x0",
   "timestamp": "0x60a653ed",
   "extraData": "0x000000000000000000000000000000000000000000000000000000000000000024ea37f2b34809df0f8b8d3071c80e3b966443410000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
   "gasLimit": "0x47b760",
   "difficulty": "0x1",
   "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
   "coinbase": "0x0000000000000000000000000000000000000000",
   "alloc": {
      "24ea37f2b34809df0f8b8d3071c80e3b96644341": {
         "balance": "0xADB53ACFA41AEE12000000"
      }
   },
   "number": "0x0",
   "gasUsed": "0x0",
   "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```


#### Starting up your member nodes

With the bootnode operational and externally reachable (you can try
`telnet <ip> <port>` to ensure it's indeed reachable), start every subsequent `geth`
node pointed to the bootnode for peer discovery via the `--bootnodes` flag. It will
probably also be desirable to keep the data directory of your private network separated, so
do also specify a custom `--datadir` flag.

```shell
$ geth --datadir /home/forge/nodes/node1/ --syncmode 'full' --port 30311 --rpc --rpcaddr '0.0.0.0' --rpcport 8501 --rpccorsdomain '*' --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://fe3009b3365fc935949b1edb8919210772e1254e9a3acb96b33a39bf53ae301485fcbf5ec11434391acfadfb627c961cb9c83451c566a10483f9f8ae9617a4e0@127.0.0.1:0?discport=30310' --networkid 5253 console
```

*Note: Since your network will be completely cut off from the main and test networks, you'll
also need to configure a miner to process transactions and create new blocks for you.*


## License

The go-ethereum library (i.e. all code outside of the `cmd` directory) is licensed under the
[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html),
also included in our repository in the `COPYING.LESSER` file.

The go-ethereum binaries (i.e. all code inside of the `cmd` directory) is licensed under the
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html), also
included in our repository in the `COPYING` file.
