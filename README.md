# Private blockchain using Geth &amp; Tools
####  	Pre-installation setup:

   *Before you get started, you’ll want to make sure you have a file for storing notes ready on your local machine. Keeping notes of certain values is absolutely essential for success.* 

####    Installing dependencies and environment configuration: 

* Download Go Ethereum Tools (Geth & Tools) from the website: https://geth.ethereum.org/downloads/. 

* Decompress the archive in the location of your preference; you will use this software to create blockchain.

   In this manual I’ll be using **MyCrypto** wallet to connect it to our blockchain and make a transaction, it can be downloaded here: https://download.mycrypto.com/.

####    Framework of Blockchain creation:

1. Create accounts nodes
2. Set up and generate genesis block
3. Initialize and launch nodes
4. Make a test transaction

#### 	Create accounts nodes:

  *All nodes on a blockchain are connected to each other and they constantly exchange the latest blockchain data with each other*.

- Through a terminal window navigate to the folder where Geth & Tools are kept; then run following command:

```
./geth account new --datadir node1
```

- You have to come up with a password to lock created account.

   This will create account for a node in the network in a ***separate directory***.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/1.png?raw=true)

- *Create as many nodes you want to test but make sure to keep them in separate directories*.

- Make a note of "Public address of the key" for later use.

  ​	After this step your notes should look similar to this: 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/2.png?raw=true)

####    Generating the first block: 

  *The genesis block is almost always hardcoded into the software of the applications that utilize its block chain. So this will be our next step!*

- While in folder where your tools are kept, run this command to access Ethereum private network manager:

```
./puppeth
```

#####    This should execute with the following output:

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/3.png?raw=true)

- Type in a name for your network (I chose ***privatechain***) and execute to move forward in the wizard.
- Type 2 to pick “Configure new genesis” option, then 1 to “Create new genesis from scratch.
- Type 2 to choose Proof of Authority consensus engine and continue; press enter again to set time required to mine a block to default.

#####    Next you will be asked to enter accounts that are allowed to seal (*sealer node without a vote of the network is allowed to mine/generate new blocks*): 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/4.png?raw=true)

- Copy and paste at least two account addresses you saved into your notes previously, without the 0x prefix. 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/5.png?raw=true)

- Execute again to proceed with wizard. 
- Paste same addresses again in the list of accounts to pre-fund. There are no block rewards in PoA, so you'll need to pre-fund.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/6.png?raw=true)

- Execute again to proceed with wizard.
- You can choose no for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei. This keeps the genesis cleaner.
- Type in a specific number as your chain/network ID (***I chose 375***).

#####    Configured new genesis block!

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/7.png?raw=true)

#####    What is your next step? Now you will have to export genesis configurations. 

* When you’re back at the main menu, choose “Manage existing genesis” option: 

- Type 2 to access sub-menu, then type 2 again to “Export genesis configurations”.

  This will fail to create two of the files, but ***you only need privatechain.json***, so you can delete the privatechain-harmony.json file.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/8.png?raw=true) 

#####    Now, it's time to initialize and tell the nodes to use your genesis block!

- Initialize the first node, replacing privatechain.json with your own:

```
./geth —datadir node1 init privatechain.json
```

#####    You should see this success message: 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/9.png?raw=true)

#####    At this stage your blockchain is only a couple steps away from being brought to life! 

  Now you will run the first node, **unlock the accoun**t, enable mining, **and the RPC flag**. *Only one node needs RPC enabled*.

- Launch the first node into mining mode with the following command:

```
./geth --datadir node1 --networkid <your network id> --unlock <account public address>  --rpc --mine --minerthreads 1 --allow-insecure-unlock
```

* The **—unlock** flag allows to unlock the account, then launch and sync node with the network, so you'll be asked to provide a password you've previously created. 

- The **—mine** flag tells node to mine new blocks; the **—minerthreads** flag tells get how many CPU “workers” to use.
- The **—rpc** flag enables us to talk to other nodes.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/10.png?raw=true)

- Make sure to take a note of your first node enode as we will need this address to tell the second node where to find the first node.(highlighted on screenshot above). 

   ##### You should see the node "Committing new mining work": 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/11.png?raw=true)

#####    	Now you will launch the second node.

- Open another terminal window and navigate to the same directory again. 
- To successfully launch the second node you will have to **change sync port** and pass **enode:// address**  by running following command: 

```
./geth --datadir node2  --networdid 375 —unlock <account public address> --port 30304 --bootnodes “enode://<enode address> —allow-insecure-unlock
```



- The **--bootnodes** flag allows you to pass the network info needed to find other nodes in the blockchain. This will allow us to connect both of our nodes.
- *Make a note of new account enode address*. 

##### The output in the prompt should be a message indicating completion of synchronization. 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/12.png?raw=true)

### Sending test transaction

#####    Use the MyCrypto GUI wallet to connect to the node with the exposed RPC port.

- Navigate to MyWallet application and choose **“Change network”** in the menu.
- You will need to use a c**ustom network**, and include the **chain ID**, and use **ETH** as the currency.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/14.png?raw=true)

#####    Import the keystore file from the node1/keystore directory into MyCrypto. This will import the private key.

- Choose **“Keystore File”** as an option to access your wallet, then select wallet file from it’s directory and unlock account with the password.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/15.png?raw=true)

   The private key for this account is kept in **“Wallet info”** tab. 

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/16.png?raw=true)

   To send a transaction go to **“Send Ether & Tokens”** tab.

- Provide public key address for your second node from previously saved notes.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/17.png?raw=true)

- Confirm the transaction and click **“TX Status”** in the popup to view transaction.

![](https://github.com/karlmunchaussen/privateblockchain/blob/master/Images/18.png?raw=true)

### 	You have succesfully created private testnet blockchain and sent a test transaction! 

