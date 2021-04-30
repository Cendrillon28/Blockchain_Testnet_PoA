# Running a Proof of Authority Blockchain

"Proof of Authority (PoA) is a reputation-based consensus algorithm that is typically used for private blockchain networks...The consensus PoA algorithm uses the value of identifiers, which means that the block validators do not create coin stakes, but instead have their own reputation. Consequently, PoA blockchains are protected by validation nodes that are considered to be trustworthy. The PoA model is based on a limited number of block validators. Blocks and transactions are checked by pre-approved participants who act as moderators of the system." source:  https://changelly.com/blog/what-is-proof-of-authority-poa/#:~:text=Proof%20of%20Authority%20%28PoA%29%20is%20a%20reputation-based%20consensus,and%20former%20technical%20specialist%20Gavin%20Wood%20in%202017.

Steps:

1. Make sure to install the Go Ethereum Tools, by following these steps:


        Open your browser and navigate to the Go Ethereum Tools download page at https://geth.ethereum.org/downloads/

        Scroll down to the "Stable Releases" section and proceed depending on your operating system.
        
2. In the folder where the geth downloads are stored, do git bash.  Create two new nodes with new account addresses that will serve as our pre-approved sealer addresses.

    * Create accounts for two nodes for the network with a separate `datadir` for each using `geth`.
        * ./geth --datadir node1 account new
        * ./geth --datadir node2 account new
    * Save the addresses:
        Node1 account new:  Public address of the key:   0xb6d0fC4e4278b6666759d462F4BbB6cE25031d76
        Node2 account new:  Public address of the key:   0xEE83E5252471783a9ADc2f1bfb4D0687F2CE8895
        ![create nodes](Screenshots/Nodes1&2.png)
    
2. Generate the PoA genesis block.

    * Run `puppeth`, name your network, and select the option to configure a new genesis block.
         * code: ./puppeth
         * network name: banknet
         * chain id: 555 
    * Choose the `Clique (Proof of Authority)` consensus algorithm.
    ![genesis_block1](Screenshots/Creation_of_Genesis_Block.png)
    
    * Paste both Nodes 1 and 2 account addresses (see step 1) one at a time into the list of accounts to seal.

    * Paste Node 1 and 2 addresses again in the list of accounts to pre-fund. There are no block rewards in PoA, so you'll need to pre-fund.

    * Choose `no` for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei. This keeps the genesis cleaner.

    * Complete the rest of the prompts, and when you are back at the main menu, choose the "Manage existing genesis" option.

    * Export genesis configurations. This will fail to create two of the files, but you only need `networkname.json`. In this case, banknet.json
     ![genesis_block2](Screenshots/Creation_of_Genesis_Block_end.png)
     
     
3. Congratulations you have created the genesis block! Let us now initialize the nodes with the genesis' json file.

    * Using `geth`, initialize each node with the new `networkname.json`. Use banknet.json for this project.
        * ./geth --datadir node1 init networkname.json
        * ./geth --datadir node2 init networkname.json

    ![node_initialization](Screenshots/Initialization_of_Nodes_using_init.png)


4. Now the nodes can be used to begin mining blocks.

    * Run the nodes in separate terminal windows with the commands below. Soon after running the command, note to type your password and hit enter:
        *  ./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock
        note: sealer_one_address = node1 public address (see step 1)
        
        
        *  ./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock
        note: sealer_two_address = node2 public address (see step 1)

![Node2-mine](Screenshots/Node2-mine.mp4)

    * **VERY IMPORTANT:** Type your password and hit enter - even if you can't see it visually!

5. Your private PoA blockchain should now be running!

6. With both nodes up and running, the blockchain can be added to MyCrypto for testing.

    * Open the MyCrypto app, then click `Change Network` at the bottom left:
  
    * Click "Add Custom Node", then add the custom network information that you set in the genesis.

    * Make sure that you scroll down to choose `Custom` in the "Network" column to reveal more options like `Chain ID`:

    * Type `ETH` in the Currency box.
    
    * In the Chain ID box, type the chain id you generated during genesis creation.

    * In the URL box type: `http://127.0.0.1:8545`.  This points to the default RPC port on your local machine.

    * Finally, click `Save & Use Custom Node`. 

    ![change network](Screenshots/Creation_of_custom_banknet_MyCrypto.png)

7. After connecting to the custom network in MyCrypto, it can be tested by sending money between accounts.

    * Select the `View & Send` option from the left menu pane, then click `Keystore file`.

     * On the next screen, click `Select Wallet File`, then navigate to the keystore directory inside your Node1 directory, select the file located there, provide your password when prompted and then click `Unlock`.

    * This will open your account wallet inside MyCrypto. 
    
    * Looks like we're filthy rich! This is the balance that was pre-funded for this account in the genesis configuration; however, these millions of ETH tokens are just for testing purposes.   

    ![keystore_unlock](Screenshots/MyCrypto_AccountBalance_in_Keystore.png)

    * In the `To Address` box, type the account address from Node2, then fill in an arbitrary amount of ETH:

     * Confirm the transaction by clicking "Send Transaction", and the "Send" button in the pop-up window.  

    * Click the `Check TX Status` when the green message pops up, confirm the logout:

    ![check tx](Screenshots/Successful_Transaction_MyCrypto.png)

    * You should see the transaction go from `Pending` to `Successful` in around the same blocktime you set in the genesis.

    * You can click the `Check TX Status` button to update the status.

    ![successful transaction](Screenshots/Transfer_MyCrypto_Success.png)

Congratulations, the blockchain is up and running.