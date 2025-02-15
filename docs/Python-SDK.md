---
id: Python_SDK
title: Python SDK
sidebar_label: Python SDK
---



## Description

client-sdk-python is a python sdk serving the Platon underlying chain. Interact with the underlying chain through the web3 object. At the bottom level, it communicates with local nodes through RPC calls. client-sdk-python can connect to any PlatON node that exposes the RPC interface.

The main functions are used to obtain block data, send transactions, interact with smart contracts, and other applications.

[client-sdk-python download link](https://github.com/PlatONnetwork/client-sdk-python)

## Quick start

### One part: installation

#### **1** Python environment requirements

 Support Python 3.6+ version

#### **2** You can use pip to install directly:

     pip install client-sdk-python


 Or download the code and use it in the python editor. git bash pull the source code, the following operations

    git clone -b 0.15.1-develop https://github.com/PlatONnetwork/client-sdk-python.git

> Where `0.15.1-develop` is the currently used branch.


### Second part: use

#### **1** Web3 mode

 Communication between Platon nodes through P2PMessage. The built-in Web3 mode is used between the node and the python sdk to send JSON-RPC requests, and connect to the node through HTTP, websocket, IPC and other methods.

#### **2** Node connection

- Take HTTP connection as an example, connect to a Platon node

  ```python
  w3 = Web3(HTTPProvider("http://localhost:6789"))
  platon = PlatON(w3)
  print(w3.isConnected())
  ```

  Where localhost:6789 is a node Url of Platon, please enter the Url of the accessible Platon node.

  platon is an instance of the platON class.

- Take Websocket connection as an example

  code show as below:

  ```python
  w3 = Web3(WebsocketProvider("ws://localhost:6790"))
  platon = PlatON(w3)
  print(w3.isConnected())
  ```

- Take IPC connection as an example

  code show as below:

  ```python
  w3 = Web3(IPCProvider("./platon.ipc"))
  platon = PlatON(w3)
  print(w3.isConnected())
  ```




#### 3 Basic API

##### Basic type encoding and decoding

- Web3.toBytes()
    Convert the input parameters to Bytes
    transfer:
    
    ```python
    >>> Web3.toBytes(0)
    b'\x00'
    >>> Web3.toBytes(0x000F)
    b'\x0f'
    ```

- Web3.toHex()

    Convert the input parameter to hexadecimal
  
    transfer:
  
    ```
    >>> Web3.toHex(b'\x00\x0F')
    '0x000f'
    >>> Web3.toHex(False)
    '0x0'
    ```

- Web3.toInt()

    Convert the input parameter to an integer
  
    transfer:
  
    ```python
    >>> Web3.toInt(0x000F)
    15
    >>> Web3.toInt(b'\x00\x0F')
    15
    ```

- Web3.toJSON()

    Convert the input parameters to json format
  
    transfer
  
    ```python
    >>> Web3.toJSON(3)
    '3'
    >>> Web3.toJSON({'one': 1})
    '{"one": 1}'
    ```

- Web3.toText()

    Convert input parameters to string format
  
    transfer
  
    ```python
    >>> Web3.toText(b'cowm\xc3\xb6')
    'cowmö'
    >>> Web3.toText(hexstr='0x636f776dc3b6')
    'cowmö'
    ```
##### Address detection

- Web3.isAddress()

    Check whether the input parameter is an approved address form
    
    transfer
    
    ```python
    >>> Web3.isAddress('0xd3CdA913deB6f67967B99D67aCDFa1712C293601')
    True
    ```

- Web3.isChecksumAddress()

    Check the checksum of the specified address, and return false for non-checksum addresses
    
    ```python
    >>> Web3.isChecksumAddress('0xd3CdA913deB6f67967B99D67aCDFa1712C293601')
    True
    >>> Web3.isChecksumAddress('0xd3cda913deb6f67967b99d67acdfa1712c293601')
    False
    ```

##### Cryptographic hash

- - Web3.sha3()

    Compile input parameters to Keccak-256
    
    transfer:
    
    ```python
    >>> Web3.sha3(0x678901)
    HexBytes('0x77cf3b4c68ccdb65991397e7b93111e0f7d863df3b26ebb053d0857e26486e6a')
    >>> Web3.sha3(text='txt')
    HexBytes('0xd7278090a36507640ea6b7a0034b69b0d240766fa3f98e3722be93c613b29d2e')
    ```

- Web3.soliditySha3()

    Compile the input abi_type and value to Keccak-256
    
    parameter:
    
    - value: true value.
    - abi_type: a list of strings in solidity format equal to value.
    
    transfer
    
    ```python
    >>> Web3.solidityKeccak(['uint8[]'], [[97, 98, 99]])
    HexBytes("0x233002c671295529bcc50b76a2ef2b0de2dac2d93945fca745255de1a9e4017e")
    
    >>> Web3.solidityKeccak(['address'], ["0x49EdDD3769c0712032808D86597B84ac5c2F5614"])
    HexBytes("0x2ff37b5607484cd4eecf6d13292e22bd6e5401eaffcc07e279583bc742c68882")
    ```


#### **4** On-chain query api

After successfully connecting with the nodes on the Platon chain, you can query the relevant information of the nodes on the chain through the api in platon

##### (1) platon.blockNumber

  Returns the current block number.

 Return value:

 An AttributeDict object whose parsed value is the number of the most recent block, Number type.



##### (2) platon.syncing

  Used to check whether the node is currently synchronized with the network.

  return value:

  An AttributeDict object with a resolved value of `Object` or `Bool`. If the node has not been synchronized with the network,
  It returns false, otherwise it returns a synchronization object with the following properties:

  - startingBlock-Number: Synchronization starting block number.

  - currentBlock-Number: currently synchronized block number.

  - highestBlock-Number: Estimated target synchronization block number.

  - knownStates-Number: Estimated states to be downloaded.

  - pulledStates-Number: downloaded states.


##### (3) platon.gasPrice

  Used to get the current gas price, which is determined by the median gas price of several recent blocks.

 Return value:

 An AttributeDict object whose parsed value is a string representing the current gas price, in VON.


##### (4) platon.accounts

  The method returns the list of accounts controlled by the current node.

  return value

  An AttributeDict object whose resolved value is an array of account addresses.


##### (5) platon.evidences

  Returns the storage content at the specified location of the account address.

  return value

  An AttributeDict object whose resolution value is the stored content of the account address.


##### (6) platon.consensusStatus

  Returns the consensus status information of the block tree where the current node is located.

  return value

  An AttributeDict object whose value is the public status information of all the blocks in the block tree.


##### (7) platon.getBalance(address)

  Used to get the balance of a specific account address in a specified block.

  parameter:

  - `address`: String-the account address to check the balance, bech32 address format, the beginning of lax is the testnet, the beginning of lat is the mainnet

  return value:

  An AttributeDict object whose parsed value is the balance string of the specified account address, in units of VON

Some code examples:

```python
from client_sdk_python import Web3, HTTPProvider
from client_sdk_python.eth import PlatON
from hexbytes import HexBytes

# get blockNumber syncing gasPrice accounts evidences consensusStatus
w3 = Web3(HTTPProvider("http://localhost:6789"))
platon = PlatON(w3)
block_number = platon.blockNumber
print(block_number)
print(platon.syncing)
print(platon.gasPrice)
print(platon.accounts)
print(platon.evidences)
print(platon.consensusStatus)

# get Balance
address ='lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja'
balance = platon.getBalance(address)
print(balance)

#Output
385274
False
100000000
['lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja','lax1qqqjkfwu854vf3ze2dpy5gctmxy3gdgzsngj66']
{}
AttributeDict({'blockTree': AttributeDict({'root': AttributeDict({'viewNumber': 22,'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d','blockNumber': 392226,'receiveTime': 05'receiveTime': 58.440430021+08:00','qc': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d','blockNumber': 392 5226,'blockIndex': 392 5226, '0x83a517492b9052de25d3b88413f070becff1e995167094b33805bfaf640b8d1499304844f08478f0525c44e7eb9ec70000000000000000000000000000000000', 'validatorSet': '_xxx'}), 'parentHash': '0x0000000000000000000000000000000000000000000000000000000000000000', 'childrenHash': [ '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90']}), 'blocks': AttributeDict ({' 392226 ': AttributeDict ({' 0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d ': AttributeDict({'viewNumber': 22,'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674 d','blockNumber': 392226,'receiveTime': '2020-09-15T18:05:58.440430021+08:00','qc': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash ':' 0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d ',' blockNumber ': 392226,' blockIndex ': 5,' signature ':' 0x83a517492b9052de25d3b88413f070becff1e995167094b33805bfaf640b8d1499304844f08478f0525c44e7eb9ec70000000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' parentHash ':' 0x0000000000000000000000000000000000000000000000000000000000000000 ',' childrenHash ': [' 0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90 ']})}),' 392227 ': AttributeDict ({' 0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90 ': AttributeDict ({' viewNumber ': 22,' blockHash ':' 0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90 ',' blockNumber ': 392227,' receiveTime ':' 2020-09 -15T18:05:59.542161134+08:00','qc': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae7 9433abc1749e4fa1ec4082dcb0e90 ',' blockNumber ': 392227,' blockIndex ': 6,' signature ':' 0x50bcc710ac0182f311065dfcb4b1c8b378788bcfe53217d6f5b29e3178508bec5557eaf7e9d360930c69c43a091e5f1800000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' parentHash ':' 0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d ',' childrenHash ': [' 0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294 '] }))), '392228': AttributeDict({'0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294': AttributeDict({'viewNumber': 22,'blockHash': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294': AttributeDict({'viewNumber': 22,'blockHash': '0x314adeb1cd8294228' 06:00.642317619+08:00','qc': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294','signatureNumber': 392 7,228,'blockIndex ': '0x505b4a9e0ed0cb3e26b2b327013bcde06350c7b2926126bbaa02417e386b0ecddc31201ec66e8a29e3c86714d669899000 000000000000000000000000000000','validatorSet':'_xxx'}),'parentHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90','childrenHash': []}))))))','StateDictview(AttributeDict') 'epoch': 1569,'viewNumber': 22,'executing': AttributeDict({'blockIndex': 7,'finish': True}),'viewchange': AttributeDict({'viewchanges': AttributeDict({})} ),'lastViewchange': None,'hadSendPrepareVote': AttributeDict({'votes': [AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x201c9308eb4e0af12b98d64694837db7a5ab430blockaa71'857db90392' blockIndex': 0,'validatorIndex': 1,'parentQC': AttributeDict(('epoch': 1569,'viewNumber': 21,'blockHash': '0x3cc758aa857621036e55cede20943ffd1a3b7e1ebc38dffcc690d4804f706dd0','blockNumber': 392220,'blockNumber': 392220, 'signature': '0xb3a89b39e9345d37ea7db2985132a3adb55abb906cd975f137eb608f55be7ea7657b0ecc0ccfa99d3caebb25a088018d00000000000000000000000000000000','validatorSet':'_xxx')),'s ignature ':' 0x599205f55ce4a191405fb5c7ad4a3961bb95a93206525b8218777023f47114a611146a40921f537478339375844cb10b00000000000000000000000000000000 '}), AttributeDict ({' epoch ': 1569,' viewNumber ': 22,' blockHash ':' 0xf6fff5626fc1fd235bb224a4d1b3f68ccd1f9f56ad4d6dff428d33b4b10cff7f ',' blockNumber ': 392222,' blockIndex ': 1,' validatorIndex ': 1, 'parentQC': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x201c9308eb4e0af12b98d64694837db7a59780ab430aa71ccfdb90741d41f857', 'blockNumber': 392221, 'blockIndex': 0, 'signature': '0xfbb64c0d0bd6f531f4d9b6303e9b3ede59deb1877c7a31bd95c0403dcfc32dec7b8060574616d500735a2d734d21e90300000000000000000000000000000000', 'validatorSet' : '_xxx'}), 'signature': '0x422a227b80667cd3f38ff794018ad3f634c0aafdbcc08a9dddff2f46b3d66f829c217dca47add729817a72dae169040000000000000000000000000000000000'}), AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163ea807898913', 'blockNumber': 392223, 'b lockIndex': 2,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0xf6fff5626fc1fd235bb224a4d1b3f68ccd1f9f56ad4d6dff428d33b4b10cff7f','Numberblock'Index: 392 1, 'signature': '0x5dc7f6461e0078b7e474badc1f7b3e0716b52f5f2f2ddcd144bd738e74246d1e5ee3780e0d4f7771b3afd5b10936019500000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0xaedbc8885b066d99905c7813302ce699b895761a1b4ab83efda28dc3b5901b585ada31df67932b68e52edd663f651e9200000000000000000000000000000000'}), AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x45f49937c4ea54b08d36fc1223fd819da11f709b3827217225c235a0ef9f89a8' ,'blockNumber': 392224,'blockIndex': 3,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163eablockNumber'913:', 392223,'blockIndex': 2,'signature': '0xd3a08cfd6c3a166e1d31dd1e58bb9eb1960bebcfc38ebad0eb0880ab30b2e46112c5 14980050159ad235615381ac820d00000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' signature ':' 0xb52fb84d8ea31918023e695fe3619ba14a1335f6455e6ab42559d7e38c9bcef8105d7c33d433f6fb7eb8d524b1afb58b00000000000000000000000000000000 '}), AttributeDict ({' epoch ': 1569,' viewNumber ': 22,' blockHash ':' 0x0341dcfab2ee1ec61b5cd380bd8445ab0a17819cc4f633ee0b3a4ca211c717f9 ',' blockNumber ': 392225,'blockIndex': 4,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x45f49937c4ea54b08d36fc1223fd819da11f709b3827217225c235a0ef9f89'blockIndex: 392224,'blockNumber : 3, 'signature': '0xd837920786bce9ea276ff3a6505dc8f48db18e9f7f4272b66dff966c904bbb83adb70f425af9c99a30957a8eae3cf30b00000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0x63dd1a1f8331320b2be8bd04abb2bae53f67fe9c14f70863723a86ccf329d4d95c9446aae360081935df5a7f2ac8681300000000000000000000000000000000'}), AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x9 2e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d','blockNumber': 392226,'blockIndex': 5,'validatorIndex': 1,'parentQC': Dict(('epochAttribute': 1569,'viewNumber': 22,'blockHec7, 11, 37, 14b,, and ': 392225,' blockIndex ': 4,' signature ':' 0x442a6e1c25857ac5ba84b16ab38918fba3c75681dcf623eacf23d5dbafd293cd0d3d617472e60dccd343c9444c4d428500000000000000000000000000000000 ',' validatorSet ':' x_xx '}),' signature ':' 0xf885e975ff4828d1f10be4dd61b7cd4e362d0f4a0b79af9d7a2ac34209341ee79d76281ec7b205f834e5c2c00c0c398800000000000000000000000000000000 '}), AttributeDict ({' epoch ': 1569,' viewNumber ' : 22,'blockHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90','blockNumber': 392227,'blockIndex': 6,'validatorIndex': 1,'parentQC': AttributeDict({'epochNumberHash': 22 ': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d','blockNumber': 392226,'blockInde x ': 5,' signature ':' 0x83a517492b9052de25d3b88413f070becff1e995167094b33805bfaf640b8d1499304844f08478f0525c44e7eb9ec70000000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' signature ':' 0xbe0d77590be62ec7cb0f5fd2dd5071aa6d97e8b909fd05cc80f10e6e58f0984adeeb7078f2c9a99e4a08e00102d4209900000000000000000000000000000000 '}), AttributeDict ({' epoch ': 1569,' viewNumber ': 22,' blockHash ': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294','blockNumber': 392228,'blockIndex': 7,'validatorIndex': 1,'parentQC': AttributebbDict({'epoch': 1569, '0be69a , 'blockNumber': 392227, 'blockIndex': 6, 'signature': '0x50bcc710ac0182f311065dfcb4b1c8b378788bcfe53217d6f5b29e3178508bec5557eaf7e9d360930c69c43a091e5f1800000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0x7305573c2655de2537e744ef1411ea55e450f8879a19274ede2455f40975f59c1b9df25d3e311464fb5cbd2398fcd48600000000000 000000000000000000000'})]}),'pendingPrepareVote': AttributeDict({'votes': []}),'viewBlocks': AttributeDict({'0': AttributeDict({'hash': '0x201c9308eb4e0af12b98d64694837db7a59780',number 392221,'blockIndex': 0}), '1': AttributeDict({'hash': '0xf6fff5626fc1fd235bb224a4d1b3f68ccd1f9f56ad4d6dff428d33b4b10cff7f','number': 392222,'blockIndex': AttributeDict( 1)),'number': 392222,'hash'{ 1}), '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163ea807898913','number': 392223,'blockIndex': 2}), '3': AttributeDict({'hash': '0x45f49937c4ea70954b08d36fc1223fdefda11f2242222423a2709'b3827225 : AttributeDict({'hash': '0x0341dcfab2ee1ec61b5cd380bd8445ab0a17819cc4f633ee0b3a4ca211c717f9','number': 392225,'blockIndex': 4)), '5': AttributeDict({'hash': '0x0341dcfab2ee1ec61b5cd380bd8445ab0a17819cc4f633ee0b3a4ca211c717f9','number': 392225,'blockIndex': 4}), '5': AttributeDict({'hash': : 5}), '6': AttributeDict({'hash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4f a1ec4082dcb0e90','number': 392227,'blockIndex': 6}), '7': AttributeDict({'hash': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294','number': 392228,'block})','view 7Qcs')' ': AttributeDict({'maxIndex': 7,'qcs': AttributeDict({'0': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x201c9308eb4e0af12b98d64694837db7a58579780ab430aa71ccfdb90741d41f:', '221 , 'blockIndex': 0, 'signature': '0xfbb64c0d0bd6f531f4d9b6303e9b3ede59deb1877c7a31bd95c0403dcfc32dec7b8060574616d500735a2d734d21e90300000000000000000000000000000000', 'validatorSet': '_xxx'}), '1': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0xf6fff5626fc1fd235bb224a4d1b3f68ccd1f9f56ad4d6dff428d33b4b10cff7f ','blockNumber': 392222,'blockIndex': 1,'signature': '0x5dc7f6461e0078b7e474badc1f7b3e0716b52f5f2f2ddcd144bd738e74246d1e5ee3780e0d4f7771b3afd5b109' : 22, 'blockHash': '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163ea807898913', 'blockNumber': 392223, 'blockIndex': 2, 'signature': '0xd3a08cfd6c3a166e1d31dd1e58bb9eb1960bebcfc38ebad0eb0880ab30b2e46112c514980050159ad235615381ac820d00000000000000000000000000000000', 'validatorSet': '_xxx'}), '3': AttributeDict ({ 'epoch ': 1569,' viewNumber ': 22,' blockHash ':' 0x45f49937c4ea54b08d36fc1223fd819da11f709b3827217225c235a0ef9f89a8 ',' blockNumber ': 392224,' blockIndex ': 3,' signature ':' 0xd837920786bce9ea276ff3a6505dc8f48db18e9f7f4272b66dff966c904bbb83adb70f425af9c99a30957a8eae3cf30b00000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' 4 ': AttributeDict ({' epoch ': 1569,' viewNumber ': 22,' blockHash ':' 0x0341dcfab2ee1ec61b5cd380bd8445ab0a17819cc4f633ee0b3a4ca211c717f9 ',' blockNumber ': 392225,' blockIndex ': 4,' signature ':' 0x442a6e1c25857ac5ba84b16ab38918fba3c75681dcf623eacf23d5dbafd293cd0d3d617472e60dccd343c9444c4d428500000000000000000000000000000000 ',' validatorSet ':' x_xx'}), '5': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d', 'blockNumber': 392226, 'blockIndex': 5, 'signature': '0x83a517492b9052de25d3b88413f070becff1e995167094b33805bfaf640b8d1499304844f08478f0525c44e7eb9ec70000000000000000000000000000000000', 'validatorSet' :'_xxx')), '6': AttributeDict(('epoch': 1569,'viewNumber': 22,'blockHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90','blockNumber': 392 6,227,'blockIndex': '0x50bcc710ac0182f311065dfcb4b1c8b378788bcfe53217d6f5b29e3178508bec5557eaf7e9d360930c69c43a091e5f1800000000000000000000000000000000', 'validatorSet': '_xxx'}), '7': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294', 'blockNumber': 392228, 'blockIndex' : 7,'signature': '0x505b4a9e0ed0cb3e26b2b327013bcde06350c7b2926126bbaa02417e386b0ecddc31201ec66e8a29e3c86714d669899000000000000000 000000000000000000','validatorSet':'_xxx'}))))),'viewVotes': AttributeDict({'votes': AttributeDict({'0': AttributeDict({'votes': AttributeDict({'1': AttributeDict ({'epoch': 1569,'viewNumber': 22,'blockHash': '0x201c9308eb4e0af12b98d64694837db7a59780ab430aa71ccfdb90741d41f857','blockNumber': 392221,'blockIndex': 0,'validatorIndex': 1,'parentQC': AttributeDict({'epoch' : 1569, 'viewNumber': 21, 'blockHash': '0x3cc758aa857621036e55cede20943ffd1a3b7e1ebc38dffcc690d4804f706dd0', 'blockNumber': 392220, 'blockIndex': 9, 'signature': '0xb3a89b39e9345d37ea7db2985132a3adb55abb906cd975f137eb608f55be7ea7657b0ecc0ccfa99d3caebb25a088018d00000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature' : '0x599205f55ce4a191405fb5c7ad4a3961bb95a93206525b8218777023f47114a611146a40921f537478339375844cb10b00000000000000000000000000000000')))))), '1': AttributeDict({'votes': AttributeDict({'1', '224xfbbd': AttributeDict({'1', '224xfbbd': AttributeDict('1') 1f9f56ad4d6dff428d33b4b10cff7f','blockNumber': 392222,'blockIndex': 1,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x201c9308eb4e0af12b71ccf'85790741db ': 392221,' blockIndex ': 0,' signature ':' 0xfbb64c0d0bd6f531f4d9b6303e9b3ede59deb1877c7a31bd95c0403dcfc32dec7b8060574616d500735a2d734d21e90300000000000000000000000000000000 ',' validatorSet ':' _xxx '}),' signature ':' 0x422a227b80667cd3f38ff794018ad3f634c0aafdbcc08a9dddff2f46b3d66f829c217dca47add729817a72dae169040000000000000000000000000000000000 '})})}),' 2 ': AttributeDict ({' votes': AttributeDict(('1': AttributeDict(('epoch': 1569,'viewNumber': 22,'blockHash': '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163ea807898913','blockNumber': 392223,'blockHash': 1,'validatorIndex': 1, ,'parentQC': AttributeDict(('epoch': 1569,'viewNumber': 22,'blockHash': '0xf6fff5626fc1fd235bb224a4d1b3f68ccd1f9f56ad4d6dff428d33b4b10cff7f','blockNumber' : 392222, 'blockIndex': 1, 'signature': '0x5dc7f6461e0078b7e474badc1f7b3e0716b52f5f2f2ddcd144bd738e74246d1e5ee3780e0d4f7771b3afd5b10936019500000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0xaedbc8885b066d99905c7813302ce699b895761a1b4ab83efda28dc3b5901b585ada31df67932b68e52edd663f651e9200000000000000000000000000000000'})})}), '3': AttributeDict ({ 'votes ': AttributeDict({'1': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x45f49937c4ea54b08d36fc1223fd819da11f709b3827217225c235a0ef9f89a8','blockNumber': 392224,'blockNumber': 3, 'parentQC': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x812e7f19e6a281928ac77767300560b8ed633d0c45f53a05b0163ea807898913', 'blockNumber': 392223, 'blockIndex': 2, 'signature': '0xd3a08cfd6c3a166e1d31dd1e58bb9eb1960bebcfc38ebad0eb0880ab30b2e46112c514980050159ad235615381ac820d00000000000000000000000000000000', 'validatorSet' :'_xxx'}),'signature': '0xb52fb84d8ea31918023e695fe3619ba14a 1335f6455e6ab42559d7e38c9bcef8105d7c33d433f6fb7eb8d524b1afb58b00000000000000000000000000000000 '})})}),' 4 ': AttributeDict ({' votes': AttributeDict ({ '1': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x0341dcfab2ee1ec61b5cd380bd8445ab0a17819cc4f633ee0b3a4ca211c717f9', 'blockNumber': 392225,'blockIndex': 4,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x45f49937c4ea54b08d36fc1223fd819da11f709b3827217225c235'a0,392224 , 'blockIndex': 3, 'signature': '0xd837920786bce9ea276ff3a6505dc8f48db18e9f7f4272b66dff966c904bbb83adb70f425af9c99a30957a8eae3cf30b00000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0x63dd1a1f8331320b2be8bd04abb2bae53f67fe9c14f70863723a86ccf329d4d95c9446aae360081935df5a7f2ac8681300000000000000000000000000000000'})})}), '5': AttributeDict ({ 'votes': AttributeDict({'1': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b 304465418a129dcc72124414b9674d','blockNumber': 392226,'blockIndex': 5,'validatorIndex': 1,'parentQC': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x0341dcfab2ee1ec61b5cd380bd38445ab0a71711 ': 392225,' blockIndex ': 4,' signature ':' 0x442a6e1c25857ac5ba84b16ab38918fba3c75681dcf623eacf23d5dbafd293cd0d3d617472e60dccd343c9444c4d428500000000000000000000000000000000 ',' validatorSet ':' x_xx '}),' signature ':' 0xf885e975ff4828d1f10be4dd61b7cd4e362d0f4a0b79af9d7a2ac34209341ee79d76281ec7b205f834e5c2c00c0c398800000000000000000000000000000000 '})})}),' 6 ': AttributeDict ({' votes': AttributeDict({'1': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90','blockNumber': 392 6,227,'validatorIndex': 392 6,227, ,'parentQC': AttributeDict(('epoch': 1569,'viewNumber': 22,'blockHash': '0x92e3229453574aa2a7120e3b0dd44d3b1b304465418a129dcc72124414b9674d','blockNumber' : 392226, 'blockIndex': 5, 'signature': '0x83a517492b9052de25d3b88413f070becff1e995167094b33805bfaf640b8d1499304844f08478f0525c44e7eb9ec70000000000000000000000000000000000', 'validatorSet': '_xxx'}), 'signature': '0xbe0d77590be62ec7cb0f5fd2dd5071aa6d97e8b909fd05cc80f10e6e58f0984adeeb7078f2c9a99e4a08e00102d4209900000000000000000000000000000000'})})}), '7': AttributeDict ({ 'votes ': AttributeDict({'1': AttributeDict({'epoch': 1569,'viewNumber': 22,'blockHash': '0x314adeb1ded15f49579ae2bc44d0b060d1d57905e9ddcd8da8ad1628bbee5294','blockNumber': 392228,'blockatorIndex': 7,'validatorIndex': 7, 'parentQC': AttributeDict ({ 'epoch': 1569, 'viewNumber': 22, 'blockHash': '0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90', 'blockNumber': 392227, 'blockIndex': 6, 'signature': '0x50bcc710ac0182f311065dfcb4b1c8b378788bcfe53217d6f5b29e3178508bec5557eaf7e9d360930c69c43a091e5f1800000000000000000000000000000000', 'validatorSet' :'_xxx'}),'signature': '0x7305573c2655de2537e744ef1411ea55e4 50f8879a19274ede2455f40975f59c1b9df25d3e311464fb5cbd2398fcd48600000000000000000000000000000000')))))))))))),'highestQCBlock': AttributeDict({'hash': '0x314adeb1cd8392905e9ae2dbc8d57392905e9060' 0x69ea999a1bdb006bb9b113f20047a3d3ae79433abc1749e4fa1ec4082dcb0e90','number': 392227}),'highestCommitBlock': AttributeDict({'hash': '0x92e3229453574aa2a7120e3b0dd44d3b4}number: 392dcc721304465a4)
220855883097298041197912187592864814478435487109452369765200775161577471

```

##### (8) platon.getStorageAt()

  Return the contents of the specified location of an address

  transfer:

  ```
  platon.getStorageAt(address, position [, defaultBlock])
  ```

  parameter:

- `address`: String-the address to be read.
- `position`: Number-index number in storage.
- `defaultBlock`: Number|String-Optional, use this parameter to override the platon.defaultBlock attribute value.


return value:

An AttributeDict object whose parsed value is the content of the specified location in the storage.

##### (9) platon.getCode

  Returns the code at the specified address.

  transfer:

  ```
  platon.getCode(address [, defaultBlock])
  ```

  parameter:

- `address`: String-the address of the code to be read.
- `defaultBlock`: Number|String-Optional, use this parameter to override the platon.defaultBlock attribute value.


return value:

An AttributeDict object whose parsed value is the code string at the specified address.



##### (10) platon.getBlock()

  Returns the block corresponding to the specified block number or block hash.

  transfer:

  ```
  platon.getBlock(blockHashOrBlockNumber [, returnTransactionObjects])
  ```

  parameter:

- `blockHashOrBlockNumber`: String|Number-Block number or block hash value, or use the following string: "genesis", "latest" or "pending".
- `returnTransactionObjects`: Boolean-Optional, the default value is false. When set to true, all transaction details will be included in the return block, otherwise only the transaction hash will be returned.


return value:

An AttributeDict object whose parsed value is a block object that meets the search conditions and has the following fields:

- number-Number: block number, the block in pending state is null.

- hash 32 Bytes-String: block hash, the block in pending state is null.

- parentHash 32 Bytes-String: parent block hash.

- nonce 8 Bytes-String: The hash of the generated proof-of-work, the pending block is null.

- sha3Uncles 32 Bytes-String: SHA3 value of the uncle data in the block.

- logsBloom 256 Bytes-String: The bloom filter of the log in the block, and the block in the pending state is null.

- transactionsRoot 32 Bytes-String: the root node of the transaction tree in the block.

- stateRoot 32 Bytes-String: The root node of the final state tree in the block.

- miner-String: The address of the miner receiving the reward.

- difficulty-String: difficulty value of the block.

- totalDifficulty-String: The total difficulty value of the whole chain up to the block.

- extraData-String: Block "extra data" field.

- size-Number: block size in bytes.

- gasLimit-Number: The maximum gas value allowed in the block.

- gasUsed-Number: The total amount of gas used by all transactions in the block.

- timestamp-Number: the unix timestamp of the block.

- transactions-Array: transaction object array, or 32-byte transaction hash value, depending on the setting of returnTransactionObjects.

- uncles-Array: Uncle block hash value array.

  ##### (11) platon.getBlockTransactionCount()

  The method returns the number of transactions in the specified block.

  transfer:

  ```
  platon.getBlockTransactionCount(blockHashOrBlockNumber)
  ```

  parameter:

- `blockHashOrBlockNumber`: String|Number-Block number or hash value of the block, or use the following string: "genesis", "latest" or "pending" to specify the block


return value:

An AttributeDict object whose parsed value is the number of transactions in the specified block, of type Number.

  

##### (12) platon.getTransaction()

  Returns the transaction object with the specified hash value.

  transfer:

  ```
platon.getTransaction(transactionHash)
  ```

  parameter:

- `transactionHash`: String-the hash value of the transaction

​    

  return value:

  An AttributeDict object whose parsed value is a transaction object with a given hash value. Refer to platon.waitForTransactionReceipt for the specific content description of this object.

  

##### (13) platon.getRawTransaction()

  Returns the HexBytes value of the transaction object with the specified hash value.

  transfer:

  ```
  platon.getRawTransaction(transactionHash)
  ```

  parameter:

- `transactionHash`: String-the hash value of the transaction

  return value:

  An object of HexBytes values.

  

##### (14) platon.getTransactionFromBlock()

  Returns the transaction object of the specified index number in the specified block.

  transfer:

  ```
 getTransactionFromBlock(hashStringOrNumber, indexNumber)
  ```

  parameter:

- `hashStringOrNumber`: String-the block number or the hash value of the block, or use the following string: "genesis, "latest" or "pending" to specify the block.
- `indexNumber`: Number-transaction index position.

  return value:

  An AttributeDict object whose parsed value is a transaction object. Refer to platon.getTransaction() for the specific content description of the object.

  

##### (15) platon.getTransactionByBlock()

  Returns the transaction object of the specified index number in the specified block.

  transfer:

  ```
  platon.getTransactionByBlock(hashStringOrNumber, indexNumber)
  ```

  parameter:

- `hashStringOrNumber`: Number |String-the block number or the hash value of the block, or use the following string: "genesis, "latest" or "pending" to specify the block.
- `indexNumber`: Number-transaction index position.

  return value:

  An AttributeDict object whose parsed value is a transaction object. Refer to platon.getTransaction() for the specific content description of the object.

#### **5** Send transaction api on chain:

##### (1) sendTransaction(transactionObject)

  Submit a transaction to the platon chain (transaction that has been signed by the node and has not yet been submitted)

  parameter:

- `transactionObject`: Object-The transaction object to be sent, including the following fields:
  - from-String|Number: The account address of the transaction sender. If this field is not set, the platon.defaultAccount attribute value is used. Can be set as an address or the index number in the local wallet platon.accounts.wallet.
  - to-String: Optional, the destination address of the message, this field is null for contract creation transactions.
  - value-Number|String|BN|BigNumber: (optional) The value transferred for the transaction in VON, also the endowment if it’s a contract-creation transaction.
  - gas-Number: optional, default value: to be determined, the total amount of gas used for transactions, unused gas will be refunded.
  - gasPrice-Number|String|BN|BigNumber: Optional, the gas price of the transaction, the unit is VON, the default value is platon.gasPrice attribute value.
  - data-String: Optional, it can be an ABI string containing contract method data, or the initialization code in the contract creation transaction.
  - nonce-Number: Optional, use this field to cover pending transactions that use the same nonce value.

  return value:

  The return value of the `platon.sendTransaction()` method is a 32-byte transaction hash value.

  

##### (2) waitForTransactionReceipt(transaction_hash, timeout)

  Return the receipt object of the specified transaction within the specified time.

  parameter:

- `transaction_hash`: String-the hash value of the transaction.
- `timeout`: Number- Optional waiting time length, in seconds. The default is 120.

  return value:

  An AttributeDict object whose parsed value is the receipt object of the transaction or null. The receipt object has the following fields:

- `blockHash` 32 Bytes-String: The hash value of the exchange in the block.
- `blockNumber`-Number: The number of the exchange in the block.
- `transactionHash` 32 Bytes-String: hash value of the transaction.
- `transactionIndex`-Number: The index position of the transaction in the block.
- `from`-String: The address of the transaction sender.
- `to`-String: The address of the recipient of the transaction. For the transaction that created the contract, the value is null.
- `contractAddress`-String: For the transaction that creates a contract, the value is the address of the created contract, otherwise it is null.
- `cumulativeGasUsed`-Number: The cumulative total gas usage of the block where the transaction is executed.
- `gasUsed`- Number: the total amount of gas for this transaction.
- `logs`-Array: The array of log objects generated by the transaction.

  

Examples of how to use sendTransaction and waitForTransactionReceipt are as follows:

```python
# sendtransaction
to ='lax1qqqjkfwu854vf3ze2dpy5gctmxy3gdgzsngj66'
w3.personal.unlockAccount(address, "password", 999999)
data = {
    "from": address,
    "to": to,
    "value": 0x10909,
    "gas": 1000000,
    "gasPrice": 1000000000,
}
transaction_hex = HexBytes(platon.sendTransaction(data)).hex()
result = platon.waitForTransactionReceipt(transaction_hex)
print(result)

#Output
AttributeDict(('blockHash': HexBytes('0x7bfe17689560c773b1cade579f1bd2cf85aeea9f75177e0e06bcdb4aeebd31a8'),'blockNumber': 385507,'contractAddress': None,'cumulativeGaswUsedjj25 [], 'logsBloom': HexBytes ( '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'), 'status': 1, 'to': 'lax1qqqjkfwu854vf3ze2dpy5gctmxy3gdgzsngj66', 'transactionHash': HexBytes ( '0x377fcd0dfb5e294041fe4274175ed7fce253973fac7abf4e4ff808b5099a454c'), 'transactionIndex': 0})
```

##### (3) platon.getTransactionReceipt()

  Returns the receipt object of the specified transaction.
  If the transaction is pending, it returns null.

  transfer:

  ```
  platon.getTransactionReceipt(hash)
  ```

  parameter:

- `hash`: String-the hash value of the transaction

  return value:

  An AttributeDict object whose parsed value is the receipt object of the transaction or null. Refer to platon.waitForTransactionReceipt for the specific content description of this object.  

##### (4) platon.getTransactionCount()

  Returns the number of transactions issued by the specified address.

  transfer:

  ```
  platon.getTransactionCount(address [, defaultBlock])
  ```

  parameter:

- `address`: String-the address of the account to be queried.
- `defaultBlock`: Number|String-Optional, set this parameter to override the platon.defaultBlock property value.

  return value:

  An AttributeDict object whose resolved value is the number of transactions issued by the specified address.  

##### (6) platon.sendRawTransaction()

  Submit a signed serialized transaction to the platon chain

  ```
platon.sendRawTransaction(signTransaction, private-key)
  ```

  parameter:

- `signTransaction`: Object-The signed transaction object to be sent, including the following fields:

  - from-String|Number: The account address of the transaction sender. If this field is not set, the platon.defaultAccount attribute value is used. Can be set as an address or the index number in the local wallet platon.accounts.wallet    .
  - to-String: Optional, the destination address of the message, this field is null for contract creation transactions   . 
  - value-Number|String|BN|BigNumber: (optional) The value transferred for the transaction in VON, also the endowment if it’s a contract-creation transaction.

  - gas-Number: optional, default value: to be determined, the total amount of gas used for transactions, unused gas will be refunded.

  - gasPrice-Number|String|BN|BigNumber: Optional, the gas price of the transaction, in VON, the default value is platon.gasPrice property value.

  - data-String: Optional, it can be an ABI string containing contract method data, or the initialization code in the contract creation transaction.

  - nonce-Number: Optional, use this field to cover pending transactions that use the same nonce value.

- private-key: the private key used for signing.

  return value:

  The return value is HexBytes containing the 32-byte transaction hash value.

  

##### (7) platon.replaceTransaction()

  Send a new transaction new_transaction to replace the original transaction transaction_hash (pending state).

  transfer:

  ```python
  platon.replaceTransaction`(transaction_hash,new_transaction)
  ```

  parameter:

- transaction_hash-string: The hash value of the transaction in the pending state.

- new_transaction-dict: transaction object, including fields consistent with transactionObject in sendTransaction.

  return value:

   The hash value of new_transaction.

  

##### (8) platon.generateGasPrice()

  Use the selected gas price strategy to calculate a gas price

  transfer:

  ```
  platon.generateGasPrice(gas_price_strategy)
  ```

 Return value:

 The gas price value in wei



##### (9) platon.setGasPriceStrategy()

  Set the selected gas price strategy

  transfer:

  ```
  platon.setGasPriceStrategy(gas_price_strategy)
  ```

  parameter:

- gas_price_strategy: (web3, transaction_params), must be a signature method.

  return:

- The gas price value in wei

##### (10) platon.modifyTransaction()

  Send new parameters to modify the pending transaction

  transfer:

  ```python
  platon.modifyTransaction(transaction_hash, **transaction_params)
  ```

  parameter:

- transaction_hash -string: The hash value of the transaction in the pending state.
- transaction_params: keyword statements corresponding to the parameters of transaction_hash. If value=1000, change the value value in the original transaction to 1000.

  return:

   The hash value of the revised transaction  .

##### (11) platon.sign()

  The method uses the specified account to sign the data, and the account must be unlocked first.

  transfer:

  ```
  platon.sign(dataToSign, address)
  ```

  parameter:

- `dataToSign`: String-The data to be signed. For the string, it will first use the utils.utf8ToHex() method to convert it to hexadecimal.
- `address`: String|Number-The account address used for signing. Or the address or its serial number in the local wallet platon.accounts.wallet

  return value:

   The signature result string.  

##### (12) platon.estimateGas()

  Estimate the gas usage of the transaction by executing a message call.

  transfer:

  ```
  platon.estimateGas(callObject)
  ```

  parameter:

- `callObject`: Object-transaction object, its from attribute is optional

  return value:

  The amount of gas used for simulation calls.



#### 6 Other api

##### (1) platon.filter

  Generate a new filter, according to the different parameters, generate different types of filters

  transfer:

  ```
  platon.filter(params)
  ```

  parameter:

- params
  - 'latest', create a filter in the node to be notified when a new block is generated. To check whether the status has changed.
  - 'pending', create a filter in the node to notify when a pending transaction occurs. To check whether the status has changed.
  - Dictionary data, create a filter to notify when the client receives a matching whisper message.


​    

  ```python
  >>> platon.filter('latest')
  <client_sdk_python.utils.filters.BlockFilter object at 0x0000020640DA1048>
  >>> platon.filter('pending')
  <client_sdk_python.utils.filters.TransactionFilter object at 0x0000020640DA7C08>
  >>> platon.filter({'fromBlock': 1000000,'toBlock': 1000100,'address':'lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja'})
  <client_sdk_python.utils.filters.LogFilter object at 0x0000020640B09D88>
  ```



##### (2) platon.getFilterChanges()

  Polling the specified filter and returning the newly generated log array since the last poll.

  transfer:

  ```
  platon.getFilterChanges(filter_id)
  ```

  parameter:

- filter_id: filter_id of the specified filter.

  

  Example:

  ```python
  >>> filt=platon.filter('latest')
  >>>platon.getFilterChanges(filt.filter_id)
  [HexBytes ( '0x59c4cb22c15ed83279e288ccc94980162e7cc7c1ff9c6b4fb6d9584308727b46'), HexBytes ( '0xb205babee34ba218816d1a32e995a4c8f4ccf95d3315c6a259955f1598ed5e4d'), HexBytes ( '0xb491ef8c0cc55cc1b70e9af766ada828a5a96bbb41f6aa26b87c98dbf09ac762'), HexBytes ( '0x455ae7bee30a02210fc17ade1cec3d783ce9614f81149ea650efc27a39e495a5'), HexBytes ( '0xd8c8f327e6613dc9a638c4ad2e2e37ce511ea80d374fea91d961d3fb55ed0e3a'), HexBytes ( '0x92e85ab7340ae8f6c0da6d5fa5cab5a3ae61f7d69158b612b7caf470de4e0958'), HexBytes ( '0x612d8f92bdec45052576b62a03a5c6d5b219ad5eecf088163cc629efc506a4dc'), HexBytes ( '0xd433b552bf421ee416c31d7c9162d8f08a7580d906538a3e0156f504161c6889'), HexBytes ( '0x5f9d64faf699f6e688989bcb6133c001cd5d24315c22375c5f1935909ee5b31e'), HexBytes ( '0x70b10afd243a5f6c0992bdb80bf75974cf3b8b78c254c054e6824621d2a55a33'), HexBytes ( '0x5f631fc85544b8e04504a71e99a67f8020596221ecfbefefde98fb44f4c3cf65'), HexBytes ( '0x6012ab1a562e818e4ca3e6e1b719e21cd900ac1daaf26c87f983c75e2cddd9ab'), HexBytes ( '0x2cc3d3e257dd9af953968a75a65 3cd9cfb0f6157aca1ba3089ebfe65d32c3543 '), HexBytes (' 0x3696eea818d042c0cae606093262268ec36aa0f23904101cdb25d7c1595bbeae '), HexBytes (' 0x4825078dc7cfb3f065acd1d300a3c111b51445488862bebd44801af6272d36cd '), HexBytes (' 0x3df27825fa9e89b91f83df4769ae6477bf060c5ff078e8f1b05edee136368f4f '), HexBytes (' 0x9e7bdf9ef1ce7e66b3615fa882fa420dfefe3c93efb433ff824d6c7fa73e08ca '), HexBytes (' 0x0d8f9a5a5cd4e95a57f6183c94826861a259e631d0127f4b0f268e104bc3c92e '), HexBytes (' 0xa310dbde88c8721fad793c7f2cb594b2bec108482990d2c56e3dc427f09d21af '), HexBytes (' 0x2e944efb7d3cf8bf9c354a02b34cbc262a044b5e2fd484daad8f7b81663e4cb0 '), HexBytes (' 0x19c72519b3fcf6d74bcef061f0f6b12eabbf9cb13d482db38b94e1fb86dd4b25 '), HexBytes (' 0x435956b99ce8d0777d5d7e19e87d3277eccd757a56da6d100fb7b536ff417ded '), HexBytes (' 0xe291f184a80c1c65a44198ed8c2d35d5ef98f65dada59a0e1fc28aa21bbe69ea '), HexBytes (' 0x21b088023a06a9c85b16fcdd6bab07697905241a139d8a3efc0e0e7a9d7a00a0 '), HexBytes (' 0x6bf9d36ec461b66191ec8f026bec905a0f46a83707960987bb559d13a5186082 '), HexBytes ( '0x85aaf311fe7c2c80340f334b7f52bb0c4282341559e056e852e92135bc04e917'), HexBytes ( '0x033e7b67bd5f509de11e38cc142a70def69048f5d214ac5d15e83102a93a011d'), HexBytes ( '0x02d55d41e12ea9edd986da149e42f302719ad769eb4496872271c063a6ab3bcd'), HexBytes ( '0x3914a9fcf5be7aca72a50d59a1b7cdb18f96540dafba0de84f8297228cb9159a'), HexBytes ( '0xc45b0552883b21012431f9e76ccd5ce9e3c8550ecbbb223a1d69ebc6a354b34d'), HexBytes ( '0xbfa4324da6aafa66907586c535a2207a082063670fff102f8df19ab1a4e665d0')]
  
  ```



##### (3) platon.getFilterLogs()

  Poll the specified filter and return the corresponding log array

  transfer:

  ```
  platon.getFilterLogs(filter_id)
  ```

  parameter:

- filter_id: filter_id of the specified filter



##### (4) platon.uninstallFilter()

  Uninstall the specified filter and return the bool value of success or failure

  transfer:

  ```
  platon.getFilterLogs(filter_id)
  ```

parameter:
- filter_id: filter_id of the specified filter

Example:

```python
>>> platon.uninstallFilter(filt.filter_id)
True
```



##### (5) platon.getLogs()

  Return the history log according to the specified options.

  transfer:

  ```
  platon.getLogs(options)
  ```

  parameter:

- `options`: Object-filter object, including the following fields:    
- fromBlock-Number|String: The number of the earliest block ("latest" may be given to mean the most recent and "pending" currently mining, block). By default "latest".    
- toBlock-Number|String: The number of the latest block ("latest" may be given to mean the most recent and "pending" currently mining, block). By default "latest".    
- address-String|Array: An address or a list of addresses to only get logs from particular account(s).
- topics-Array: An array of values ​​which must each appear in the log entries. The order is important, if you want to leave topics out use null, eg [null, '0x12...']. You can also pass an array for each topic with options for that topic eg [null, ['option1','option2']]

  return value:

  An AttributeDict object whose parsed value is an array of log objects.

  The event object structure in the array is as follows:

- address-String: The source address of the event

- data-String: Contains unindexed log parameters

- topics-Array: an array containing up to 4 32-byte topics, topics 1-3 contain log index parameters

- logIndex-Number: The index position of the event in the block

- transactionIndex-Number: The index position of the transaction containing the event

- transactionHash 32 Bytes-String: The hash value of the transaction containing the event

- blockHash 32 Bytes-String: The hash value of the block containing the event, or null if it is in the pending state

- blockNumber-Number: The block number containing the event, this field is null when it is in the pending state



##### (6) functions()

  Entrance to call contract function

  transfer:

  ```python
  myContract.functions.myMethod([param1[, param2[, ...]]]).transact(options)
  ```

-  parameter:
  - `options`-Object: Options, including the following fields:
  - `from`-String (optional): The address the call “transaction” should be made from.
  - gasPrice-String (optional): The gas price in VON to use for this call “transaction”.
  - gas-Number (optional): The maximum gas provided for this call “transaction” (gas limit).


​      

##### (7) call()

  Call the method of the contract and execute the method directly in the contract without sending any transactions. Therefore, the state of the contract will not be changed.

  transfer:

  ```
  myContract.functions.myMethod([param1[, param2[, ...]]]).call()
  ```

  parameter:

- [param1[, param2[, ...]]]: parameters entered according to the data type defined in myMethod

  return value:

   The parsed value is the return value of the contract method, the Mixed type. If the contract method returns multiple values, the resolved value is an object.

  ```python
  tx_hash1 = payable.functions.setInt64(-9223372036854775808).transact(
      {
          'from':from_address,
          'gas': 1500000,
      }
  )
  print(platon.waitForTransactionReceipt(tx_hash1))
  print('get: {}'.format(
      payable.functions.getInt64().call()
  ))
  
  #Output
  get: -9223372036854775808
  ```

  

##### (8) events

  Subscribe to the specified contract event.

  transfer:

  ```
  myContract.events.MyEvent([options])
  ```

- parameter:
  - options-Object: Optional, options for deployment, including the following fields:    
  - filter-Object: Optional, filter events by index parameter. For example, {filter: {myNumber: [12,13]}} means all events where "myNumber" is 12 or 13
  - fromBlock-Number: Optional, only listen to events that occur in the block with the number specified by this option
  - topics-Array: Optional, used to manually set topics for event filters. If the filter attribute and event signature have been set, then (topic[0]) will not be set automatically

  return value:

  EventEmitter: Event generator, declares the following events:	

- "data" returns Object: Triggered when a new event is received, the parameter is the event object.
- "changed" returns Object: Triggered when the event is removed from the blockchain, the event object will be added with an additional property "removed: true".

- "error" returns Object: Triggered when an error occurs.

  The returned event object structure is as follows:

- event-String: event name.
- signature-String|Null: Event signature, if it is an anonymous event, it will be null.
- address-String: Event source address.
- returnValues-Object: Event return value, for example {myVar: 1, myVar2: '0x234...'}.
- logIndex-Number: The index position of the event in the block.
- transactionIndex-Number: the index position of the event in the transaction.
- transactionHash 32 Bytes-String: The hash value of the transaction where the event is located.
- blockHash 32 Bytes-String: The hash value of the block where the event is located, the value of the pending block is null.
- blockNumber-Number: The number of the block where the event is located, the value of the pending block is null.
- raw.data-String: This field contains unindexed log parameters.
- raw.topics-Array: Up to 4 32-byte long topic string arrays can be stored. Topics 1-3 include index parameters for events.

  Sample code:

  ```python
  greeter = platon.contract(address=tx_receipt.contractAddress, abi=abi)
  
  tx_hash = greeter.functions.setVar(100).transact(
      {
          'from':from_address,
          'gas': 1500000,
      }
  )
  
  tx_receipt = platon.waitForTransactionReceipt(tx_hash)
  print(tx_receipt)
  
  topic_param = greeter.events.MyEvent().processReceipt(tx_receipt)
  print(topic_param)
  
  #Output:
  AttributeDict({'blockHash': HexBytes('0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8b34435174'),'blockNumber': 2014683,'contractAddress': None,'cumulativeqqdUsed': 43148x' from [AttributeDict({'address':'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn','topics': [HexBytes('0x6c2b4666ba8da5a95717621d879a77deactionf3d816709b9cbe9f'b8f3d816709b9cbe9f059'b8f0000000000000'Number00000000'00000064',0000000000000)',00000000000000 HexBytes ( '0xe36b5d2b679d5635ab6dd2b620caa50a476fa84bd93bf7b6c8de807f3a9da483'), 'transactionIndex': 0, 'blockHash': HexBytes ( '0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8b34435174'), 'logIndex': 0, 'removed': False})], 'logsBloom': HexBytes ( '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000020080000000000000000000000000000000000000000000000000000 000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000004000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000008000000000000000 '),' status': 1, 'to': 'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn', 'transactionHash': HexBytes ( '0xe36b5d2b679d5635ab6dd2b620caa50a476fa84bd93bf7b6c8de807f3a9da483'), 'transactionIndex': 0})
  (AttributeDict({'args': AttributeDict({'_var': 100}),'event':'MyEvent','logIndex': 0,'transactionIndex': 0,'transactionHash': HexBytes('0xe36b5d2b679d5635ab6dd2b620caa483807f) 'address':'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn','blockHash': HexBytes('0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8)34435174'), '2014683
  
  ```

 #### 7. Get hrp

```
w3 = Web3(HTTPProvider("http://localhost:6789"))
platon = PlatON(w3)
print(platon.getAddressHrp)
```



### Third part: the contract

#### 1. Contract introduction

The PlatON blockchain supports smart contracts (evm) created using the solidity language, and it also supports WebAssembly (WASM) to execute smart contracts written by users. Among them, WASM is a binary instruction set designed for stack virtual machines. WASM is designed to be available for platform compilation targets like C/C++/Rust and other high-level languages. The original design purpose is to solve the performance problem of JavaScript. WASM is a Web standard that is being promoted by W3C and is supported by browser vendors such as Google, Microsoft, and Mozilla.

For details on the introduction, creation, and compilation of evm and wasm contracts, please refer to [Platon Smart Contract](/docs/en/WASM_Smart_Contract/)



#### **2**、 Contract compilation

The python sdk currently supports bin and abi formed after the evm and wasm contracts are compiled as contract data to interact with the PlatON blockchain.

**(1)** The evm contract (created in solidity language) can be compiled, deployed, and invoked using platon-truffle. For details, please refer to [solidity compiler](https://github.com/PlatONnetwork/solidity) and [platon-truffle](https://platon-truffle.readthedocs.io/en/v0.13.1/getting-started/ installation.html)
**(2)** The wasm contract (created in C/C++/Rust and other languages) can be compiled, deployed, and invoked using PlatON-CDT or platon-truffle. For details, please refer to [PlatON-CDT Compiler](https://github.com/PlatONnetwork/PlatON-CDT/tree/feature/wasm)



#### 3. SDK calls to evm contract

##### (1) Use platon-truffle to compile the evm contract locally

  Obtain bin and abi. Take the Helloworld contract as an example.

 After using platon-truffle to compile Helloworld.sol, get the abi and bytecode (bin) in the generated build/contracts/HelloWorld.json.

##### (2) Deploy Helloworld contract through python SDK

  First connect to the node via Web3

  from_address is the account address on the node

  bytecode and abi are the bin and abi after the evm contract is compiled

  ```python
  from hexbytes import HexBytes
  from client_sdk_python import Web3, HTTPProvider
  from client_sdk_python.eth import PlatON
  from platon_keys.utils import bech32,address
  from client_sdk_python.packages.eth_utils import to_checksum_address
  
  true = True
  false = False
  
  w3 = Web3(HTTPProvider("http://10.1.1.5:6789"))
  platon = PlatON(w3)
  print(w3.isConnected())
  
  from_address = "lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja"
  
  bytecode = '608060405234801561001057600080fd5b50610c28806100206000396000f3fe608060405234801561001057600080fd5b50600436106101375760003560e01c806357609889116100b85780638e418fdb1161007c5780638e418fdb146104b2578063a64be0d5146104d0578063b4feac7c146104ee578063b87df0141461050c578063c0e641fc1461052a578063da193c1f1461054857610137565b80635760988914610352578063687615d71461037057806371ee52021461038e57806378aa6155146104115780637e6b0f571461042f57610137565b806344e24ce0116100ff57806344e24ce01461029c57806347808fc3146102ca5780634b8016b9146102f8578063508242dc1461031657806356230cca1461033457610137565b80631f9c9f3c1461013c578063275ec9761461015a57806335432d3114610178578063383d49e5146101fb5780633f9dbcf914610219575b600080fd5b610144610566565b6040518082815260200191505060405180910390f35b61016261056c565b6040518082815260200191505060405180910390f35b6101806105ca565b6040518080602001828103825283818151815260200191508051906020019080838360005b838110156101c05780820151818401526020810190506101a5565b50505050905090810190601f16 80156101ed5780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b610203610668565b6040518082815260200191505060405180910390f35b61022161066e565b6040518080602001828103825283818151815260200191508051906020019080838360005b83811015610261578082015181840152602081019050610246565b50505050905090810190601f16801561028e5780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b6102c8600480360360208110156102b257600080fd5b810190808035906020019092919050505061070c565b005b6102f6600480360360208110156102e057600080fd5b8101908080359060200190929190505050610811565b005b6103006108a4565b6040518082815260200191505060405180910390f35b61031e6108aa565b6040518082815260200191505060405180910390f35b61033c6108b0565b6040518082815260200191505060405180910390f35b61035a610907565b6040518082815260200191505060405180910390f35b610378610911565b6040518082815260200191505060405180910390f35b610396610917565b6040518080602001828103825283818151815260200191508051906020019080838360005b838110156103 d65780820151818401526020810190506103bb565b50505050905090810190601f1680156104035780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b6104196109b9565b6040518082815260200191505060405180910390f35b6104376109c3565b6040518080602001828103825283818151815260200191508051906020019080838360005b8381101561047757808201518184015260208101905061045c565b50505050905090810190601f1680156104a45780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b6104ba610a65565b6040518082815260200191505060405180910390f35b6104d8610a9b565b6040518082815260200191505060405180910390f35b6104f6610af2565b6040518082815260200191505060405180910390f35b610514610b30565b6040518082815260200191505060405180910390f35b610532610b3a565b6040518082815260200191505060405180910390f35b610550610b44565b6040518082815260200191505060405180910390f35b60025481565b6000806005819055506000600190505b600a8110156105c05760006005828161059157fe5b0614156105a3576005549150506105c7565b806005600082825401925050819055508080 60010191505061057c565b5060055490505b90565b60008054600181600116156101000203166002900480601f0160208091040260200160405190810160405280929190818152602001828054600181600116156101000203166002900480156106605780601f1061063557610100808354040283529160200191610660565b820191906000526020600020905b81548152906001019060200180831161064357829003601f168201915b505050505081565b60035481565b60068054600181600116156101000203166002900480601f0160208091040260200160405190810160405280929190818152602001828054600181600116156101000203166002900480156107045780601f106106d957610100808354040283529160200191610704565b820191906000526020600020905b8154815290600101906020018083116106e757829003601f168201915b505050505081565b6014811015610766576040518060400160405280601381526020017f796f7520617265206120796f756e67206d616e0000000000000000000000000081525060009080519060200190610760929190610b4e565b5061080e565b603c8110156107c0576040518060400160405280601481526020017f796f75206172652061206d6964646c65206d616e00000000000000000000000081525060009080 5190602001906107ba929190610b4e565b5061080d565b6040518060400160405280601181526020017f796f75206172652061206f6c64206d616e0000000000000000000000000000008152506000908051906020019061080b929190610b4e565b505b5b50565b60148113610854576040518060400160405280600c81526020017f6d6f7265207468616e203230000000000000000000000000000000000000000081525061088b565b6040518060400160405280600c81526020017f6c657373207468616e20323000000000000000000000000000000000000000008152505b600690805190602001906108a0929190610b4e565b5050565b60045481565b60015481565b60008060048190555060008090505b600a8110156108fe576000600282816108d457fe5b0614156108e0576108f1565b806004600082825401925050819055505b80806001019150506108bf565b50600454905090565b6000600454905090565b60055481565b606060068054600181600116156101000203166002900480601f0160208091040260200160405190810160405280929190818152602001828054600181600116156101000203166002900480156109af5780601f10610984576101008083540402835291602001916109af565b820191906000526020600020905b8154815290600101906020 0180831161099257829003601f168201915b5050505050905090565b6000600554905090565b606060008054600181600116156101000203166002900480601f016020809104026020016040519081016040528092919081815260200182805460018160011615610100020316600290048015610a5b5780601f10610a3057610100808354040283529160200191610a5b565b820191906000526020600020905b815481529060010190602001808311610a3e57829003601f168201915b5050505050905090565b60008060018190555060008090505b80600160008282540192505081905550806001019050600a8110610a745760015491505090565b6000806003819055506000600190505b600a811015610ae957600060028281610ac057fe5b061415610acc57610ae9565b806003600082825401925050819055508080600101915050610aab565b50600354905090565b60008060028190555060008090505b600a811015610b2757806002600082825401925050819055508080600101915050610b01565b50600254905090565b6000600254905090565b6000600354905090565b6000600154905090565b828054600181600116156101000203166002900490600052602060002090601f016020900481019282601f10610b8f57805160ff1916838001178555610bbd565b8280 0160010185558215610bbd579182015b82811115610bbc578251825591602001919060010190610ba1565b5b509050610bca9190610bce565b5090565b610bf091905b80821115610bec576000816000905550600101610bd4565b5090565b9056fea265627a7a7231582003a28b4281af2c524edc05a0c071a68e9f08b99e0a7e70b37dcc181d06a48e6c64736f6c634300050d0032 '
  
  abi = [{"constant":false,"inputs":[],"name":"doWhileControl","outputs":[{"internalType":"uint256","name":"","type": "uint256"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"doWhileControlResult" ,"outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type": "function"},{"constant":false,"inputs":[],"name":"forBreakControl","outputs":[{"internalType":"uint256","name":"","type ":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":" forBreakControlResult","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type ":"function"},{"constant":false,"inputs":[],"name":"forContinueControl","outputs":[{"internalType":"uint256","name":"", "type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name" :"forContinueControlResu lt","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type ":"function"},{"constant":false,"inputs":[],"name":"forControl","outputs":[{"internalType":"uint256","name":"", "type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name" :"forControlResult","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view", "type":"function"},{"constant":false,"inputs":[],"name":"forReturnControl","outputs":[{"internalType":"uint256","name":" ","type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[]," name":"forReturnControlResult","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view ","type":"function"},{"constant":false,"inputs":[{"internalType":"int256","name":"age","type":"int256"}], "name":"forThreeControlControl","outputs":[],"pa yable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"forThreeControlControlResult","outputs":[{ "internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{" constant":true,"inputs":[],"name":"getForBreakControlResult","outputs":[{"internalType":"uint256","name":"","type":"uint256"}] ,"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"getForContinueControlResult","outputs": [{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}, {"constant":true,"inputs":[],"name":"getForControlResult","outputs":[{"internalType":"uint256","name":"","type":"uint256" }],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"getForReturnControlResult","outputs ":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"fu nction"},{"constant":true,"inputs":[],"name":"getForThreeControlControlResult","outputs":[{"internalType":"string","name":"","type" :"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"getIfControlResult ","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type" :"function"},{"constant":true,"inputs":[],"name":"getdoWhileResult","outputs":[{"internalType":"uint256","name":""," type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":" uint256","name":"age","type":"uint256"}],"name":"ifControl","outputs":[],"payable":false,"stateMutability":"nonpayable", "type":"function"},{"constant":true,"inputs":[],"name":"ifControlResult","outputs":[{"internalType":"string","name":" ","type":"string"}],"payable":false,"stateMutability":"view","type":"function"}]
  
  #Output
  True
  lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja
  ```

  ​ Then use the function contract_deploy(bytecode, fromAddress) to deploy the evm contract on the nodes of the PlatON blockchain by sending transactions, and return the transaction hash transactionHash.

  ​ tx_receipt is the deployment receipt obtained after platon.waitForTransactionReceipt parses the transactionHash (deployment is also a transaction, and the transaction obtains the transaction receipt through platon.waitForTransactionReceipt).

  ```python
  def contract_deploy(bytecode, fromAddress):
      bytecode = bytecode
      transactionHash = platon.sendTransaction(
          {
              "from": fromAddress,
              "gas": 1000000,
              "gasPrice": 1000000000,
              "data": bytecode,
          }
      )
      transactionHash = HexBytes(transactionHash).hex().lower()
      return transactionHash
  
  tx = contract_deploy(bytecode, from_address)
  print(tx)
  tx_receipt = platon.waitForTransactionReceipt(tx)
  print(tx_receipt)
  contractAddress = tx_receipt.contractAddress
  print(contractAddress)
  
  ```

   platon.sendTransaction (parameter)

- parameter:

    - `from` The address of the account that sent the transaction.

    -  `data` The data sent to the chain.

    - `gas` The amount of gas traded.

    - `gasPrice` fuel price.


  Need to write a reasonable value

  

  If the deployment is successful, the output results are as follows

  ```python
  
  #Output
  0x143efc88f581c4356156519cde51064222ec5a42fcb4d83400a8b11893a95074
  AttributeDict ({ 'blockHash': HexBytes ( '0xf73097d8e7b2cc385910a4af3a4dbc7588774bad3f2b6589052503b649af1525'), 'blockNumber': 305798, 'contractAddress':' lax1ws7m2tqr55h8xs7e3jg5svlyu0lk9ktpx03cke ',' cumulativeGasUsed ': 319449,' from ':' lax1yjjzvjph3tw4h2quw6mse25y492xy7fzwdtqja ',' gasUsed ': 319449,' logs ': [],' logsBloom ': HexBytes (' 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 '),' status': 1, 'to': None, 'transactionHash': HexBytes ( '0x143efc88f581c4356156519cde51064222ec5a42fcb4d83400a8b11893a95074'), 'transactionIndex': 0})
  lax1ws7m2tqr55h8xs7e3jg5svlyu0lk9ktpx03cke
  ```

  - among them 

    - The first line of data is the transaction result of platon.sendTransaction in the function contract_deploy.
    - The second line of data is the transaction receipt obtained by platon.waitForTransactionReceipt.
    -  The third line is the contract address where the contract is successfully deployed.

  

##### (3) Call the Helloworld contract (transaction sending)

  Based on the successful deployment of the previous contract, the transaction is sent

  First define a function SendTxn(txn)

  Contains: Sign transaction platon.account.signTransaction (signature with private key)

  ​ Send transaction platon.sendRawTransaction

  ​ Get transaction receipt platon.waitForTransactionReceipt

  ```python
  send_privatekey = "b7a7372e78160f71a1a75e03c4aa72705806a05cf14ef39c87fdee93d108588c"
  def SendTxn(txn):
      signed_txn = platon.account.signTransaction(txn,private_key=send_privatekey)
      res = platon.sendRawTransaction(signed_txn.rawTransaction).hex()
      txn_receipt = platon.waitForTransactionReceipt(res)
      print(res)
      return txn_receipt
    
  ```

  Create a contract instance contract_instance. Because it is an evm contract, the function contract is used. If it is a wasm contract, it corresponds to the function wasmcontract.

  Call method ifControl through functions, input parameter 20, and send transaction information through buildTransaction

  ```python
  contract_instance = platon.contract(address=contractAddress, abi=abi)
  
  txn = contract_instance.functions.ifControl(20).buildTransaction(
      {
          'chainId': 200,
          'nonce':platon.getTransactionCount(from_address),
          'gas': 2000000,
          'value':0,
          'gasPrice':1000000000,
      }
  )
  
  print(SendTxn(txn))
  
  result = contract_instance.functions.getIfControlResult().call()
  print(result)
  ```

parameter:

- `chainId`: chain id.

- `nonce`: serial number.

- `gas` :fuel.
- `value`: value (starting balance of new contract account).
- `gasPrice`: fuel price.

Need to write a reasonable value

 Call the method ifControl, and successfully pass the parameter 20 to the chain. Then get the corresponding information and data on the chain through the corresponding method getIfControlResult.

The output is as follows:

  ```
  #Output:
  0x16c76387cdd06ab82a4beb330b36369a5cfa22b8cf6ddfff58c72aaae4a39df9
  AttributeDict(('blockHash': HexBytes('0xbb1d1c3a7abecac9910509ed3ff2ca97cebdba1e88db0b909ffd646a86d69597'),'blockNumber': 305801,'contractAddress': None,'cumulativeGaswUsedjj25382,'from [], 'logsBloom': HexBytes ( '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'), 'status': 1, 'to': 'lax1ws7m2tqr55h8xs7e3jg5svlyu0lk9ktpx03cke', 'transactionHash': HexBytes ( '0x16c76387cdd06ab82a4beb330b36369a5cfa22b8cf6ddfff58c72aaae4a39df9'), 'transactionIndex': 0})
  ```

- among them 

  - The first line of data is the transaction result of platon.sendRawTransaction in the function SendTxn.

  - The second line of data is the method ifControl sends information to the chain, the result of the transaction.

  - The third line method getIfControlResult gets the information on the chain and the result of the transaction.



##### (4) Event call of evm contract

  The evm contract can monitor and log the detailed information of related transactions through events.

  Take the evmevent contract as an example: it adds the event type MyEvent to the method setVar.

  Greeter is the successfully deployed evm contract.

  First call setVar through functions and pass the parameters to the chain

  Then through greeter.events.MyEvent(), call the event to output the detailed log of the transaction.

  The .events method is the event api dedicated to the contract.

  ```python
  greeter = platon.contract(address=tx_receipt.contractAddress, abi=abi)
  
  tx_hash = greeter.functions.setVar(100).transact(
      {
          'from':from_address,
          'gas': 1500000,
      }
  )
  
  tx_receipt = platon.waitForTransactionReceipt(tx_hash)
  print(tx_receipt)
  
  topic_param = greeter.events.MyEvent().processReceipt(tx_receipt)
print(topic_param)
  ```

  Output after successful operation:

  ```python
  AttributeDict({'blockHash': HexBytes('0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8b34435174'),'blockNumber': 2014683,'contractAddress': None,'cumulativeqqdUsed': 43148x' from [AttributeDict({'address':'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn','topics': [HexBytes('0x6c2b4666ba8da5a95717621d879a77deactionf3d816709b9cbe9f'b8f3d816709b9cbe9f059'b8f0000000000000'Number00000000'00000064',0000000000000)',00000000000000 HexBytes ( '0xe36b5d2b679d5635ab6dd2b620caa50a476fa84bd93bf7b6c8de807f3a9da483'), 'transactionIndex': 0, 'blockHash': HexBytes ( '0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8b34435174'), 'logIndex': 0, 'removed': False})], 'logsBloom': HexBytes ( '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000020080000000000000000000000000000000000000000000000000000 000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000004000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000008000000000000000 '),' status': 1, 'to': 'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn', 'transactionHash': HexBytes ( '0xe36b5d2b679d5635ab6dd2b620caa50a476fa84bd93bf7b6c8de807f3a9da483'), 'transactionIndex': 0})
  (AttributeDict({'args': AttributeDict({'_var': 100}),'event':'MyEvent','logIndex': 0,'transactionIndex': 0,'transactionHash': HexBytes('0xe36b5d2b679d5635ab6dd2b620caa483807f) 'address':'lax1vc6phdxhdkmztpznv5ueduw6cae3swe40whlsn','blockHash': HexBytes('0x78fb61da83dae555c8a8a87fc3296f466afeb7f90e9a3b0ac5689e8)34435174'), '2014683

  ```

  The first line calls the function setVar, the transaction receipt after the transaction is successful.

  The second line calls the event MyEvent() to get the transaction log information.

  Among the values ​​corresponding to'args':

  '_var' is the only parameter value

  In the event of the evm contract, the basic types of data are uint, int, bool, address, bytex.

  

#### 4. SDK calls to wasm contract:



##### (1) Use PlatON-CDT to compile wasm contract locally

  Take wasmcontract.cpp as an example. After installing PlatON-CDT on this machine, enter the code in PlatON-CDT/build/bin.

  ```
  platon-cpp wasmcontract.cpp
  ```

  After successful compilation, there are two files in wasmcontract/build/contracts.

  wasmcontract.abi.json and wasmcontract.wasm, where wasmcontract.abi.json is abi data (json format), wasmcontract.wasm is bin data (binary format).

  ```python
   import binascii
   f = open('D:/wasmcontract.wasm','rb')
   contents=f.read()
   bytecode=binascii.b2a_hex(contents)
  ```

  Because what our chain recognizes is hexadecimal data, we need to use methods similar to binascii.b2a_hex to convert the binary .wasm to hexadecimal bytecode. Facilitate our identification on the chain.

##### (2) Deploy Helloworld contract (wasm type) through python SDK

  After obtaining the bin and abi of the wasm contract, deploy on the chain through Web3.

  In the code below, bytecode is the bin data of the contract, and cabi is the abi data of the contract.

  ```python
  from client_sdk_python import Web3, HTTPProvider
  from client_sdk_python.eth import PlatON
  true = True
  false = False
  
  w3 = Web3(HTTPProvider("http://10.1.1.2:6789"))
  platon = PlatON(w3)
  print(w3.isConnected())
  from_address = "lax1uqug0zq7rcxddndleq4ux2ft3tv6dqljphydrl"
  
  bytecode = '0061736d01000000015c1060027f7f0060017f017f60027f7f017f60017f0060037f7f7f017f60037f7f7f0060047f7f7f7f0060000060047f7f7f7f017f60027f7e0060017f017e60037f7e7e0060037f7e7e017f60017e017f60057f7f7f7f7f006000017f02ce010903656e760c706c61746f6e5f70616e6963000703656e760b706c61746f6e5f73686133000603656e7617706c61746f6e5f6765745f696e7075745f6c656e677468000f03656e7610706c61746f6e5f6765745f696e707574000303656e7617706c61746f6e5f6765745f73746174655f6c656e677468000203656e7610706c61746f6e5f6765745f7374617465000803656e7610706c61746f6e5f7365745f7374617465000603656e760c706c61746f6e5f6576656e74000603656e760d706c61746f6e5f72657475726e000003cd01cb010700020200040000010202020302020500000e000000020001000200010002000001020600020002000200000204000002010102040506000303010400050402050500000104020503010001040c00040b01000004080304000107070704050a0a030103010101010101010d09010000000003000100020100010001010001010a0d09000100000100000201000100010001000001000100010003010001000108030100020100090001030100010100000c0b 0208000300000003030105070501010206000600000000000405017001050505030100020608017f0141908c040b073904066d656d6f72790200115f5f7761736d5f63616c6c5f63746f727300090f5f5f66756e63735f6f6e5f65786974006506696e766f6b650063090a010041010b04202124250af0b902cb01050010c7010baa0301097f230041c0016b22022400200241b908100b2104200241e8006a4102100c2103200241d8006a2004100d200241c8006a2000100d200241406b4100360200200241386a4200370300200241306a420037030020024200370328200241286a20022802582204200228025c100e20022802482205200228024c100e210620032002280228100f2003200241d8006a10102003200241c8006a1010200328020c200341106a28020047044010000b2003280204210720032802002108200241106a1011210020024180016a200110122101200241a8016a4100360200200241a0016a420037030020024198016a4200370300200242003703900120024190016a41001013200241b0016a200110121014410110132109200228029001210a200941046a10152000200a100f20004101101620024190016a200110121017220028020c200041106a28020047044010000b20082007200028020020002802041007200028020c22010440200020013602100b 200641046a1015200504402002200536024c0b200404402002200436025c0b200328020c22000440200320003602100b200241c0016a24000b910101027f20004200370200200041086a410036020020012102024003402002410371044020022d0000450d02200241016a21020c010b0b2002417c6a21020340200241046a22022802002203417f73200341fffdfb776a7141808182847871450d000b0340200341ff0171450d01200241016a2d00002103200241016a21020c000b000b20002001200220016b105020000b1d0020001051200041146a41003602002000420037020c2000200110160b8b0101037f230041306b22032400200341186a1011220220011052100f2002200341086a200110121017220128020c200141106a28020047044010000b200041003602082000420037020020012802042102200128020021042000412010532004200220002802002202200028020420026b1001200128020c22000440200120003602100b200341306a24000b8b0101017f024020012002460440410121030c010b4101210302400240200220016b2202410146044020012c0000417f4c0d010c030b200241374b0d010b200241016a21030c010b2002105420026a41016a21030b027f200041186a28020022010440200041086a280200200041146a280200200110550c010b20000b 2201200128020020036a36020020000b1300200028020820014904402000200110570b0b1600200020012802002200200128020420006b10581a0b190020001051200041146a41003602002000420037020c20000b4d01017f20004200370200200041086a2202410036020020012d0000410171450440200020012902003702002002200141086a28020036020020000f0b200020012802082001280204105020000bc30c02077f027e230041306b22042400200041046a2107027f20014101460440200041086a280200200041146a280200200041186a220228020022031055280200210120022003417f6a3602002007105a4180104f044020072000410c6a280200417c6a105b0b200141384f047f2001105420016a0520010b41016a2102200041186a28020022010440200041086a280200200041146a280200200110550c020b20000c010b02402007105a0d00200041146a28020022014180084f0440200020014180786a360214200041086a2201280200220228020021032001200241046a360200200420033602182007200441186a105c0c010b2000410c6a2802002202200041086a2802006b4102752203200041106a2205280200220620002802046b2201410275490440418020103a2105200220064704400240200028020c220120002802102202470d0020002802082203 200028020422064b04402000200320012003200320066b41027541016a417e6d41027422026a105d220136020c2000200028020820026a3602080c010b200441186a200220066b2201410175410120011b22012001410276200041106a105e2102200028020c210320002802082101034020012003470440200228020820012802003602002002200228020841046a360208200141046a21010c010b0b200029020421092000200229020037020420022009370200200029020c21092000200229020837020c200220093702082002105f200028020c21010b200120053602002000200028020c41046a36020c0c020b02402000280208220120002802042202470d00200028020c2203200028021022064904402000200120032003200620036b41027541016a41026d41027422026a106022013602082000200028020c20026a36020c0c010b200441186a200620026b2201410175410120011b2201200141036a410276200041106a105e2102200028020c210320002802082101034020012003470440200228020820012802003602002002200228020841046a360208200141046a21010c010b0b200029020421092000200229020037020420022009370200200029020c21092000200229020837020c200220093702082002105f200028020821010b2001417c6a200536020020002000 2802082201417c6a22023602082002280200210220002001360208200420023602182007200441186a105c0c010b20042001410175410120011b20032005105e2102418020103a2106024020022802082201200228020c2203470d0020022802042205200228020022084b04402002200520012005200520086b41027541016a417e6d41027422036a105d22013602082002200228020420036a3602040c010b200441186a200320086b2201410175410120011b22012001410276200241106a280200105e21032002280208210520022802042101034020012005470440200328020820012802003602002003200328020841046a360208200141046a21010c010b0b2002290200210920022003290200370200200320093702002002290208210920022003290208370208200320093702082003105f200228020821010b200120063602002002200228020841046a360208200028020c2105034020002802082005460440200028020421012000200228020036020420022001360200200228020421012002200536020420002001360208200029020c21092000200229020837020c200220093702082002105f052005417c6a210502402002280204220120022802002203470d0020022802082206200228020c22084904402002200120062006200820066b41027541016a41026d410274 22036a106022013602042002200228020820036a3602080c010b200441186a200820036b2201410175410120011b2201200141036a4102762002280210105e21062002280208210320022802042101034020012003470440200428022020012802003602002004200428022041046a360220200141046a21010c010b0b20022902002109200220042903183702002002290208210a20022004290320370208200420093703182004200a3703202006105f200228020421010b2001417c6a200528020036020020022002280204417c6a3602040c010b0b0b200441186a20071061200428021c410036020041012102200041186a0b2201200128020020026a360200200441306a240020000ba10101037f41012103024002400240200128020420012d00002202410176200241017122041b220241014d0440200241016b0d032001280208200141016a20041b2c0000417f4c0d010c030b200241374b0d010b200241016a21030c010b2002105420026a41016a21030b027f200041186a28020022010440200041086a280200200041146a280200200110550c010b20000b2201200128020020036a36020020000bea0101047f230041106b22042400200028020422012000280210220241087641fcffff07716a2103027f410020012000280208460d001a2003280200200241ff0771410274 6a0b2101200441086a20001061200428020c210203400240200120024604402000410036021420002802082103200028020421010340200320016b41027522024103490d022000200141046a22013602040c000b000b200141046a220120032802006b418020470d0120032802042101200341046a21030c010b0b2002417f6a220241014d04402000418004418008200241016b1b3602100b20002001105b200441106a24000b8e0201057f2001044020002802042105200041106a2802002202200041146a280200220349044020022001ad2005ad422086843702002000200028021041086a36021020000f0b027f41002002200028020c22046b410375220641016a2202200320046b2203410275220420042002491b41ffffffff01200341037541ffffffff00491b2204450d001a2004410374103a0b2102200220064103746a22032001ad2005ad4220868437020020032000280210200028020c22066b22016b2105200220044103746a2102200341086a2103200141014e044020052006200110361a0b20002002360214200020033602102000200536020c20000f0b200041c00110d201200041004100410110ce0120000b2c01017f20002001280208200141016a20012d0000220041017122021b2001280204200041017620021b10580bc80301087f230041c0016b2203240020 0341ac08100b2105200341e8006a4102100c2104200341d8006a2005100d200341c8006a2000100d200341406b4100360200200341386a4200370300200341306a420037030020034200370328200341286a20032802582205200328025c100e20032802482206200328024c100e210720042003280228100f2004200341d8006a10102004200341c8006a1010200428020c200441106a28020047044010000b2004280204210820042802002109200341106a1011210020034180016a200110122101200320023b018c01200341a8016a4100360200200341a0016a420037030020034198016a4200370300200342003703900120034190016a41001013200341b0016a200110121014220220032f018c0110192002410110132102200328029001210a200241046a10152000200a100f20004102101620034190016a200110121017220020032f018c01101a200028020c200041106a28020047044010000b20092008200028020020002802041007200028020c22010440200020013602100b200741046a1015200604402003200636024c0b200504402003200536025c0b200428020c22000440200420003602100b200341c0016a24000b0c0020002001ad420010561a0b0b0020002001ad420010590bf20301067f230041d0016b22052400200541086a419f08100b2107200541f0006a 4103100c2106200541e0006a2007100d200541d0006a2000100d200541c8006a4100360200200541406b4200370300200541386a420037030020054200370330200541306a200528026022002005280264100e200528025022072005280254100e22082001101920062005280230100f2006200541e0006a10102006200541d0006a101020062001101a200628020c200641106a28020047044010000b200628020421092006280200210a200541186a1011210120054188016a20021012210220052004360298012005200336029401200541b8016a4100360200200541b0016a4200370300200541a8016a4200370300200542003703a001200541a0016a41001013200541c0016a2002101210142203200528029401101920032005280298011019200341011013210320052802a0012104200341046a101520012004100f200141031016200541a0016a2002101210172201200528029401101a2001200528029801101a200128020c200141106a28020047044010000b200a2009200128020020012802041007200128020c22020440200120023602100b200841046a101520070440200520073602540b20000440200520003602640b200628020c22010440200620013602100b200541d0016a24000b2e01017f230041206b22022400200241106a418d08100b2002419908100b100a20 0020013a0010200241206a24000b3301017f230041206b22022400200241106a418008100b2002418708100b20011018200041286a20013b0100200241206a24000b3701017f230041206b22022400200241106a418008100b20012002418708100b20012001101b200041406b2001360200200241206a24000b930201047f20002001470440200128020420012d00002202410176200241017122041b2102200141016a210320012802082105410a21012005200320041b210420002d0000410171220304402000280200417e71417f6a21010b200220014d0440027f2003044020002802080c010b200041016a0b21012002044020012004200210490b200120026a41003a000020002d000041017104402000200236020420000f0b200020024101743a000020000f0b416f2103200141e6ffffff074d0440410b20014101742201200220022001491b220141106a4170712001410b491b21030b2003103a22012004200210c801200020023602042000200341017236020020002001360208200120026a41003a00000b20000b0d00200041b0016a20013a00000b0b00200041b0016a2c00000b230020002001101f1a2000410c6a2001410c6a101f1a200041186a200141186a101f1a0b25002000200110121a2000410c6a2001410c6a10121a200041186a200141186a10121a20000b0d 00200041c8026a20013a00000b0b00200041c8026a2c00000b9e0101047f2000200147044020012802042203200128020022026b41017522042000280208200028020022016b4101754d0440200041046a21052004200028020420016b41017522004b04402002200220004101746a22002001104e1a20002003200510460f0b2005200220032001104e3602000f0b2001044020004100360208200042003702000b200020002004104f104c20022003200041046a10460b0b3e01017f2000420037020020004100360208200128020420012802006b2202044020002002410175104c20012802002001280204200041046a10460b20000b3001027f200141046a21032001280200210203402002200346044020004188046a20011029052002102a21020c010b0b0be00201067f230041206b22042400024020002001460d00200141046a21062001280200210102402000280208450d00200028020021032000200041046a3602002000410036020820002802042102200041003602042002410036020820032802042202200320021b2102034020022203450d0120012006470440200341106a200141106a101f21072003411c6a2001411c6a101f1a024020032802082202450440410021020c010b2003200228020022054604402002410036020020022802042205450d01200510432102 0c010b200241003602042005450d002005104321020b2000200441106a2007104421072000200428021020072003103f2001102a21010c010b0b0340200328020822030d000b200621010b034020012006460d01200441106a2000200141106a104a20002004410c6a2004280210220341106a104421022000200428020c20022003103f2001102a21010c000b000b200441206a24000b3601017f024020002802042201044003402001220028020022010d000c020b000b0340200020002802082200280200470d000b0b20000bd60101067f230041106b22022400200042003702042000200041046a2203360200200141046a21062001280200210103402001200646450440200141106a21050240027f027f0240024020032000280200460440200321040c010b2003103b220441106a2005103c450d010b20032802004504402002200336020c2003210420030c030b2002200436020c200441046a0c010b20002002410c6a2005103d0b22042802000d01200228020c0b2107200220002005104a2000200720042002280200103f0b2001102a21010c010b0b200241106a240020000b2300200041d0016a2001101f1a20004198016a20033a0000200041f8026a20023602000b920101047f2000200147044020012802042203200128020022026b22042000280208200028020022016b 4d0440200041046a21052004200028020420016b22004b04402002200020026a2200200110471a20002003200510460f0b200520022003200110473602000f0b2001044020004100360208200042003702000b2000200020041048104520022003200041046a10460b0b3b01017f2000420037020020004100360208200128020420012802006b2202044020002002104520012802002001280204200041046a10460b20000b2601017f0340200241f800470440200020026a200120026a101f1a2002410c6a21020c010b0b0b2301017f0340200020026a200120026a10121a2002410c6a220241f800470d000b20000b130020002001101f1a2000200128020c36020c0b15002000200110121a2000200128020c36020c20000b3001027f200141046a210320012802002102034020022003460440200041f8066a20011034052002102a21020c010b0b0bd30201067f230041206b22042400024020002001460d00200141046a21062001280200210102402000280208450d00200028020021032000200041046a3602002000410036020820002802042102200041003602042002410036020820032802042202200320021b2102034020022203450d0120012006470440200341106a200141106a101f2107024020032802082202450440410021020c010b20032002280200220546044020 02410036020020022802042205450d012005104321020c010b200241003602042005450d002005104321020b2000200441106a2007104421072000200428021020072003103f2001102a21010c010b0b0340200328020822030d000b200621010b034020012006460d01200441106a2000200141106a103e20002004410c6a2004280210220341106a104421022000200428020c20022003103f2001102a21010c000b000b200441206a24000bd60101067f230041106b22022400200042003702042000200041046a2203360200200141046a21062001280200210103402001200646450440200141106a21050240027f027f0240024020032000280200460440200321040c010b2003103b220441106a2005103c450d010b20032802004504402002200336020c2003210420030c030b2002200436020c200441046a0c010b20002002410c6a2005103d0b22042802000d01200228020c0b2107200220002005103e2000200720042002280200103f0b2001102a21010c010b0b200241106a240020000bfc0801067f03400240200020046a2105200120046a210320022004460d002003410371450d00200520032d00003a0000200441016a21040c010b0b200220046b210602402005410371220745044003402006411049450440200020046a2203200120046a2205290200370200200341 086a200541086a290200370200200441106a2104200641706a21060c010b0b027f2006410871450440200120046a2103200020046a0c010b200020046a2205200120046a2204290200370200200441086a2103200541086a0b21042006410471044020042003280200360200200341046a2103200441046a21040b20064102710440200420032f00003b0000200341026a2103200441026a21040b2006410171450d01200420032d00003a000020000f0b024020064120490d002007417f6a220741024b0d00024002400240024002400240200741016b0e020102000b2005200120046a220328020022073a0000200541016a200341016a2f00003b0000200041036a2108200220046b417d6a2106034020064111490d03200420086a2203200120046a220541046a2802002202410874200741187672360200200341046a200541086a2802002207410874200241187672360200200341086a2005410c6a28020022024108742007411876723602002003410c6a200541106a2802002207410874200241187672360200200441106a2104200641706a21060c000b000b2005200120046a220328020022073a0000200541016a200341016a2d00003a0000200041026a2108200220046b417e6a2106034020064112490d03200420086a2203200120046a220541046a28020022024110742007 41107672360200200341046a200541086a2802002207411074200241107672360200200341086a2005410c6a28020022024110742007411076723602002003410c6a200541106a2802002207411074200241107672360200200441106a2104200641706a21060c000b000b2005200120046a28020022073a0000200041016a21082004417f7320026a2106034020064113490d03200420086a2203200120046a220541046a2802002202411874200741087672360200200341046a200541086a2802002207411874200241087672360200200341086a2005410c6a28020022024118742007410876723602002003410c6a200541106a2802002207411874200241087672360200200441106a2104200641706a21060c000b000b200120046a41036a2103200020046a41036a21050c020b200120046a41026a2103200020046a41026a21050c010b200120046a41016a2103200020046a41016a21050b20064110710440200520032d00003a00002005200328000136000120052003290005370005200520032f000d3b000d200520032d000f3a000f200541106a2105200341106a21030b2006410871044020052003290000370000200541086a2105200341086a21030b2006410471044020052003280000360000200541046a2105200341046a21030b20064102710440200520032f00003b 0000200541026a2105200341026a21030b2006410171450d00200520032d00003a00000b20000b2b01027f200141046a210203402002280200220341046a210220012003470d000b200041a80b6a200110380bbe0201057f024020002001460d00200041046a2102200141046a21030340200228020021020240027f4101200328020022032001460d001a20002002470d0141000b210402402000200246044020040d044114103a22054100360200200541086a200341086a10121a20052104410121060340200120032802042203460d024114103a220241086a200341086a10121a2002200436020020042002360204200641016a2106200221040c000b000b200228020022032000280200220128020436020420012802042003360200034020002002460d0420002000280208417f6a360208200228020421020c000b000b2000280200220220053602042005200236020020002004360200200420003602042000200028020820066a3602080c020b200241086a200341086a101f1a200241046a2102200341046a21030c000b000b0b840101037f200041003602082000200036020420002000360200200141046a2102037f20022802002203200146047f2000054114103a22024100360200200241086a200341086a10121a2002200036020420002802002104200020023602002002 2004360200200420023602042000200028020841016a360208200341046a21020c010b0b0b0b002000410120001b10620b3e01027f024020002802002201044003402001220228020422010d000c020b000b03402000280208220228020020004621012002210020010d000b0b20020bb20101067f02400240200128020420012d00002202410176200241017122031b2205200028020420002d00002202410176200241017122041b2206200520064922071b2202450d002000280208200041016a20041b21002001280208200141016a20031b210103402002450d0120002d0000220320012d00002204460440200141016a2101200041016a21002002417f6a21020c010b0b200320046b22020d010b417f200720062005491b21020b2002411f760b890101027f200041046a2103024020002802042200044002400340024002402002200041106a2204103c0440200028020022040d012001200036020020000f0b20042002103c450d03200041046a210320002802042204450d01200321000b20002103200421000c010b0b2001200036020020030f0b200120003602000c010b200120033602000b20030b2d01017f2000411c103a22033602002000200141046a360204200341106a200210121a200041086a41013a00000b4800200320013602082003420037020020022003360200 20002802002802002201044020002001360200200228020021030b2000280204200310402000200028020841016a3602080bec0101037f200120002001463a000c03400240024020002001460d00200128020822022d000c0d002002200228020822032802002204460440024020032802042204450d0020042d000c0d002004410c6a21010c030b20012002280200470440200210412002280208220228020821030b200241013a000c200341003a000c200310420f0b02402004450d0020042d000c0d002004410c6a21010c020b20012002280200460440200210422002280208220228020821030b200241013a000c200341003a000c200310410b0f0b200241013a000c200320002003463a000c200141013a0000200321010c000b000b5101027f200020002802042201280200220236020420020440200220003602080b200120002802083602082000280208220220022802002000474102746a200136020020002001360208200120003602000b5101027f200020002802002201280204220236020020020440200220003602080b200120002802083602082000280208220220022802002000474102746a200136020020002001360208200120003602040b1d01017f03402000220128020022000d00200128020422000d000b20010b5c01017f0240200028020422030440034002 402002200341106a103c044020032802002200450d040c010b200328020422000d0020012003360200200341046a0f0b200021030c000b000b200041046a21030b2001200336020020030b2001017f20002001103a2202360200200020023602042000200120026a3602080b2800200120006b220141014e044020022802002000200110361a2002200228020020016a3602000b0b1900200120006b2201044020022000200110490b200120026a0b2e01017f2001200028020820002802006b2200410174220220022001491b41ffffffff07200041ffffffff03491b0b8d0301037f024020002001460d00200120006b20026b410020024101746b4d044020002001200210361a0c010b20002001734103712103027f024020002001490440200020030d021a410021030340200120036a2105200020036a2204410371450440200220036b210241002103034020024104490d04200320046a200320056a280200360200200341046a21032002417c6a21020c000b000b20022003460d04200420052d00003a0000200341016a21030c000b000b024020030d002001417f6a21040340200020026a22034103714504402001417c6a21032000417c6a2104034020024104490d03200220046a200220036a2802003602002002417c6a21020c000b000b2002450d042003417f6a200220046a2d 00003a00002002417f6a21020c000b000b2001417f6a210103402002450d03200020026a417f6a200120026a2d00003a00002002417f6a21020c000b000b200320056a2101200320046a0b210303402002450d01200320012d00003a00002002417f6a2102200341016a2103200141016a21010c000b000b0b2c01017f20004128103a22033602002000200141046a360204200341106a2002104b200041086a41013a00000b16002000200110121a2000410c6a2001410c6a10121a0b2301017f20002001104d2202360200200020023602042000200220014101746a3602080b09002000410174103a0b2501017f200120006b220141017521032001044020022000200110490b200220034101746a0b2a002001200028020820002802006b220020002001491b41ffffffff07200041017541ffffffff03491b0b5b01027f02402002410a4d0440200020024101743a0000200041016a21030c010b200241106a4170712204103a21032000200236020420002004410172360200200020033602080b20032001200210c801200220036a41003a00000b160020004100360208200042003702002000410010570b5801027f230041306b22012400200141286a4100360200200141206a4200370300200141186a420037030020014200370310200141106a2001200010121014210020012802 102102200041046a1015200141306a240020020bdd0101047f024020002802042203200028020022026b22042001490440200028020820036b200120046b22024f04400340200341003a00002000200028020441016a22033602042002417f6a22020d000c030b000b200020011048220104402001103a21050b200420056a220421030340200341003a0000200341016a21032002417f6a22020d000b200120056a210520042000280204200028020022046b22016b2102200141014e044020022004200110361a0b2000200536020820002003360204200020023602000f0b200420014d0d002000200120026a3602040b0b1e01017f03402000044020004108762100200141016a21010c010b0b20010b25002000200120026a417f6a220241087641fcffff07716a280200200241ff07714102746a0b900101027f4101210420014280015441002002501b450440034020012002845045044020024238862001420888842101200341016a2103200242088821020c010b0b200341384f047f2003105420036a0520030b41016a21040b027f200041186a28020022030440200041086a280200200041146a280200200310550c010b20000b2203200328020020046a36020020000b2f01017f2000280208200149044020011062200028020020002802041036210220002001360208200020 023602000b0b7001017f4101210302400240024002402002410146044020012c000022024100480d012000200241ff017110d2010c040b200241374b0d01200221030b200020034180017341ff017110d2010c010b2000200210d301200221030b200020012003410010ce010b2000410110d00120000bcf0202037f027e02402001200284500440200041800110d2010c010b20014280015441002002501b4504402001210720022106034020062007845045044020064238862007420888842107200341016a2103200642088821060c010b0b0240200341384f04402003210403402004044020044108762104200541016a21050c010b0b200541c9004f044010000b2000200541b77f6a41ff017110d2012000200028020420056a10d101200028020420002802006a417f6a21052003210403402004450d02200520043a0000200441087621042005417f6a21050c000b000b200020034180017341ff017110d2010b2000200028020420036a10d101200028020420002802006a417f6a210303402001200284500d02200320013c0000200242388620014208888421012003417f6a2103200242088821020c000b000b20002001a741ff017110d2010b2000410110d0010b2801017f200028020820002802046b2201410874417f6a410020011b200028021420002802106a6b0b250101 7f200028020821020340200120024645044020002002417c6a22023602080c010b0b0ba10202057f017e230041206b22052400024020002802082202200028020c2203470d0020002802042204200028020022064b04402000200420022004200420066b41027541016a417e6d41027422036a105d22023602082000200028020420036a3602040c010b200541086a200320066b2202410175410120021b220220024102762000410c6a105e2103200028020821042000280204210203402002200446450440200328020820022802003602002003200328020841046a360208200241046a21020c010b0b2000290200210720002003290200370200200320073702002000290208210720002003290208370208200320073702082003105f200028020821020b200220012802003602002000200028020841046a360208200541206a24000b2501017f200120006b220141027521032001044020022000200110490b200220034102746a0b4f01017f2000410036020c200041106a2003360200200104402001410274103a21040b200020043602002000200420024102746a22023602082000200420014102746a36020c2000200236020420000b2b01027f200028020821012000280204210203402001200247044020002001417c6a22013602080c010b0b0b1b00200120006b2201044020 0220016b22022000200110490b20020b4f01037f20012802042203200128021020012802146a220441087641fcffff07716a21022000027f410020032001280208460d001a2002280200200441ff07714102746a0b360204200020023602000b9b0101047f230041106b220124002001200036020c2000047f41880c200041086a2202411076220041880c2802006a220336020041840c41840c280200220420026a41076a417871220236020002400240200341107420024d044041880c200341016a360200200041016a21000c010b2000450d010b200040000d0010000b20042001410c6a4104103641086a0541000b2100200141106a240020000b090010c701106410650bc52602057f027e230041c0166b2201240010022200106222021003200141d00a6a200141086a20022000106622004100106702400240200141d00a6a10682205500d0041c208106920055104402000106a200141d00a6a106b106c0c020b41c708106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a106e2100200141d00a6a106b22022000101c2002106c0c020b41d008106920055104402000106a200141d00a6a106b22032d00102102200141b8086a101122002002106f100f20002002101a200028020c200041106a28020047044010000b2000280200200028 02041008200028020c22020440200020023602100b2003106c0c020b41d908106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a10702100200141d00a6a106b22022000101d2002106c0c020b41e308106920055104402000106a200141d00a6a106b220341286a2f01002102200141b8086a1011220020021071100f20002002101a200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41ed08106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a10722100200141d00a6a106b22022000101e2002106c0c020b41f708106920055104402000106a200141d00a6a106b220341406b2802002102200141b8086a1011220020021073100f20002002101a200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b418109106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a10682105200141d00a6a106b220041d8006a20053703002000106c0c020b418b09106920055104402000106a200141d00a6a106b220241d8006a2903002105200141b8086a1011220020051074100f2000200510 75200028020c200041106a28020047044010000b200028020020002802041008200028020c22030440200020033602100b2002106c0c020b41950910692005510440200141b8066a107621022000106d410247044010000b200141d00a6a200041011067200141d00a6a20021077200141d00a6a106b220041f8006a200141b8086a20021012101f1a2000106c0c020b419f09106920055104402000106a200141a0046a200141d00a6a106b220241f8006a10122100200141b8086a1011220320001052100f2003200141b8066a200010121017220028020c200041106a28020047044010000b200028020020002802041008200028020c22030440200020033602100b2002106c0c020b41a90910692005510440200141003a00b8082000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b8086a1078200141d00a6a106b22004198016a20012d00b8083a00002000106c0c020b41b109106920055104402000106a200141d00a6a106b22034198016a2d00002102200141b8086a101122002002106f100f20002002101a200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41b909106920055104402000410110790c020b41c1091069200551044020004102107a0c02 0b41c90910692005510440200141b8066a107b2000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b8066a107c200141d00a6a106b220041d0016a200141b8086a200141b8066a1012101f1a2000106c0c020b41d409106920055104402000106a200141b8066a200141d00a6a106b220241d0016a10121a200141b8086a10112200200141b8066a107d100f2000200141b8066a107e200028020c200041106a28020047044010000b200028020020002802041008200028020c22030440200020033602100b2002106c0c020b41df0910692005510440200141b8066a4124107f1a200141b8066a10800121022000106d410247044010000b200141d00a6a200041011067200141d00a6a2002108101200141d00a6a106b22004190026a200141b8086a2002102310222000106c0c020b41ec09106920055104402000106a200141b8086a200141d00a6a106b22034190026a10232102200141b8066a101122002002108201100f20002002108301200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41f909106920055104402000410310790c020b41810a1069200551044020004104107a0c020b41890a106920055104402000106d410247044010000b200141d00a6a 200041011067200141d00a6a1084012100200141d00a6a106b220241e0026a20003b01002002106c0c020b41920a106920055104402000106a200141d00a6a106b220341e0026a2e01002102200141b8086a101122002002108501100f20002002108601200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b419b0a106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a1087012100200141d00a6a106b220241f8026a20003602002002106c0c020b41a40a106920055104402000106a200141d00a6a106b220341f8026a2802002102200141b8086a101122002002108801100f20002002108601200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41ad0a106920055104402000106d410247044010000b200141d00a6a200041011067200141d00a6a1089012105200141d00a6a106b22004190036a20053703002000106c0c020b41b60a106920055104402000106a200141d00a6a106b22024190036a2903002105200141b8086a101122002005108a01100f20002005108b01200028020c200041106a28020047044010000b200028020020002802041008200028 020c22030440200020033602100b2002106c0c020b41bf0a10692005510440200141003602c006200142003703b8062000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b8066a108c01200141d00a6a106b220241e0036a200141b8086a200141b8066a102722001026200028020022030440200020033602040b2002106c20012802b8062200450d02200120003602bc060c020b41c90a106920055104402000106a200141b8066a200141d00a6a106b220341e0036a10272102200141b8086a101122002002108d01100f20002002108e01200028020c200041106a28020047044010000b200028020020002802041008200028020c22040440200020043602100b200228020022000440200220003602040b2003106c0c020b41d30a10692005510440200142003702bc062001200141b8066a4104723602b8062000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b8066a108f01200141d00a6a106b2200200141b8086a200141b8066a102b10282000106c0c020b41da0a106920055104402000106a200141b8066a200141d00a6a106b22034188046a102b2102200141b8086a101122002002109001100f20002002109101200028020c200041106a28020047044010000b200028020020002802041008200028020c2202 0440200020023602100b2003106c0c020b41e10a10692005510440200141b8086a107b200141003a00c808200141003602c408200141c8086a21022000106d410447044010000b200141d00a6a200041011067200141d00a6a200141b8086a107c200141d00a6a2000410210672001200141d00a6a1087013602c408200141d00a6a200041031067200141d00a6a20021078200141d00a6a106b2100200141b8066a200141b8086a10121a2000200141b8066a20012802c40820012d00c808102c2000106c0c020b41f10a10692005510440200141003602a804200142003703a0042000106d410247044010000b200141d00a6a200041011067200141d00a6a200141a0046a109201200141d00a6a106b220341b0046a200141b8086a200141b8066a200141a0046a102e2200102e2202102d200228020022040440200220043602040b200028020022020440200020023602040b2003106c20012802a0042200450d02200120003602a4040c020b41fa0a106920055104402000106a200141b8066a200141d00a6a106b220341b0046a102e2102200141b8086a1011220020022802002002280204109301100f200020021010200028020c200041106a28020047044010000b200028020020002802041008200028020c22040440200020043602100b20022802002200044020022000360204 0b2003106c0c020b41830b10692005510440200141b8066a41f800107f1a200141b8066a10940121022000106d410247044010000b200141d00a6a200041011067200141d00a6a2002109501200141d00a6a106b220041c0056a200141b8086a20021030102f2000106c0c020b418c0b106920055104402000106a200141b8086a200141d00a6a106b220341c0056a10302102200141b8066a101122002002109601100f20002002109701200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41950b10692005510440200141b8066a10980121022000106d410247044010000b200141d00a6a200041011067200141d00a6a2002109901200141d00a6a106b220041d0066a200141b8086a2002103210312000106c0c020b419d0b106920055104402000106a200141b8066a200141d00a6a106b220341d0066a10322102200141b8086a101122002002109a01100f20002002109b01200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41a50b10692005510440200142003702bc062001200141b8066a4104723602b8062000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b806 6a109c01200141d00a6a106b2200200141b8086a200141b8066a103510332000106c0c020b41ac0b106920055104402000106a200141b8066a200141d00a6a106b220341f8066a10352102200141b8086a101122002002109d01100f20002002109e01200028020c200041106a28020047044010000b200028020020002802041008200028020c22020440200020023602100b2003106c0c020b41b30b10692005510440200141b8066a109f0121022000106d410247044010000b200141d00a6a200041011067200141d00a6a200210a001200141d00a6a106b2100200141206a200141b8066a41800210361a200141b8086a200141206a41800210361a20004190096a200141b8086a41800210361a2000106c0c020b41c00b106920055104402000106a200141a0026a200141d00a6a106b22024190096a41800210361a200141a0066a10112200200141a0026a10a101100f200141a0046a200141a0026a41800210361a200141b8066a200141a0046a41800210361a200141206a200141b8066a41800210361a200141b8086a200141206a41800210361a2000200141b8086a4180021058220028020c200041106a28020047044010000b200028020020002802041008200028020c22030440200020033602100b2002106c0c020b41cd0b10692005510440200141003602c00620012001 41b8066a3602bc062001200141b8066a3602b8062000106d410247044010000b200141d00a6a200041011067200141d00a6a200141b8066a10a201200141d00a6a106b2200200141b8086a200141b8066a103922021037200210a3012000106c200141b8066a10a3010c020b41d50b106920055104402000106a200141b8066a200141d00a6a106b220341a80b6a10392102200141b8086a10112200200210a401100f2000200210a501200028020c200041106a28020047044010000b200028020020002802041008200028020c22040440200020043602100b200210a3012003106c0c020b41dd0b10692005510440200141b8086a10a60121022000106d410247044010000b200141d00a6a200041011067200141d00a6a200210a701200141d00a6a106b2100200141b0066a200141c8086a2802002202360200200141a8066a200141c0086a2903002205370300200120012903b80822063703a006200041d80b6a2006370300200041e00b6a2005370300200041e80b6a20023602002000106c0c020b41e80b10692005520d002000106a200141306a2200200141d00a6a106b220241e80b6a280000360200200141286a2203200241e00b6a2900003703002001200241d80b6a290000370320200141b8086a10112204200141206a10a801100f200141b0026a20002802002200360200 200141a8026a200329030022053703002001200129032022063703a002200141a8046a2005370300200141b0046a2000360200200141a8066a2005370300200141b0066a2000360200200120063703a004200120063703a006200141c8066a2000360200200141c0066a2005370300200120063703b8062004200141b8066a41141058220028020c200041106a28020047044010000b200028020020002802041008200028020c22030440200020033602100b2002106c0c010b10000b200141c0166a24000b880101037f41f40b410136020041f80b2802002100034020000440034041fc0b41fc0b2802002201417f6a2202360200200141014845044041f40b4100360200200020024102746a22004184016a280200200041046a28020011030041f40b410136020041f80b28020021000c010b0b41fc0b412036020041f80b200028020022003602000c010b0b0b0d00200020012002411c10a9010bd00202067f017e230041106b220324002001280208220520024b0440200341086a200110cd0120012003280208200328020c10cb0136020c2003200110cd01410021052001027f410020032802002207450d001a410020032802042208200128020c2206490d001a200820062006417f461b210420070b360210200141146a2004360200200141003602080b200141106a2106034020 01280214210402402005200249044020040d01410021040b200020062802002004411410a9011a200341106a24000f0b2003200110cd0141002104027f410020032802002205450d001a410020032802042208200128020c2207490d001a200820076b2104200520076a0b210520012004360214200120053602102003200641002005200410cb0110cc012001200329030022093702102001200128020c2009422088a76a36020c2001200128020841016a22053602080c000b000b880102027f017e230041106b22012400200010aa0102400240200010ab01450d002000280204450d0020002802002d000041c001490d010b10000b200141086a200010ac01200128020c220041094f044010000b200128020821020340200004402000417f6a210020023100002003420886842103200241016a21020c010b0b200141106a240020030b3901027e42a5c688a1c89ca7f94b210103402000300000220250450440200041016a2100200142b383808080207e20028521010c010b0b20010b0e002000106d410147044010000b0bca2901097f230041406a22012400200042ddbe888dc5fcfca4987f370308200041003a0000200141286a1011220320002903081075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d00200141003602 2020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f47044020002001200441016a20062004417f736a1066106e3a0010200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200020002d00003a00100b41002102200041003b0118200041206a220442debe888dc5fcfca4987f370300200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d002001410036022020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f47044020002001200441016a20062004417f736a106610703b0128200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200020002f01183b01280b4100210220004100360230200041386a220442dfbe888dc5fcfca4987f370300200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d002001410036022020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f47044020002001200441016a20062004417f736a106610723602 40200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200020002802303602400b20004200370348200041d0006a220242d9be888dc5fcfca4987f370300200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450440410021040c010b410021042001410036022020014200370318200141186a200510532007200820012802182202200128021c220620026b1005417f47044020002001200241016a20062002417f736a10661068370358200521040b2002450d002001200236021c0b200328020c22020440200320023602100b2004450440200020002903483703580b200041e0006a10762109200041f0006a220242a7cea8ad82a0ff995b370300200041f8006a10762107200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220620026b1005417f4704402001200241016a20062002417f736a106620071077200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402007 2009101f1a0b41002102200041003a00880120004190016a220442f4a2efb3f6e5b8ad03370300200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d002001410036022020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f4704402001200441016a20062004417f736a106620004198016a1078200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200020002d0088013a0098010b200041003a00a001200041a8016a220342badda0b2f6c5fa8e033703002003200041b0016a10ad01450440200020002d00a0013a00b0010b200042003703b801200041c0016a4100360200200041b8016a10762109200041c8016a2202428aedabfdf7a698fe29370300200041d0016a220710761a200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220620026b1005417f4704402001200241016a20062002417f736a10662007107c200521040b2002450d00200120023602 1c0b200328020c22020440200320023602100b200445044020072009101f1a0b41002102200041e0016a4124107f108001210920004188026a220442d4b7b1edebf7e3f66a37030020004190026a1080012107200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450d002001410036022020014200370318200141186a200510532008200620012802182204200128021c220620046b1005417f4704402001200441016a20062004417f736a10662007108101200521020b2004450d002001200436021c0b200328020c22040440200320043602100b20024504402007200910220b200041003a00b802200041c0026a220342ccacbdb4f8a587a93d3703002003200041c8026a10ad01450440200020002d00b8023a00c8020b200041003b01d002200041d8026a220242cfacbdb4f8a587a93d370300200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450440410021040c010b410021042001410036022020014200370318200141186a200510532007200820012802182202200128021c220620026b1005417f47044020002001200241016a20062002417f736a10661084013b01e002200521040b200245 0d002001200236021c0b200328020c22020440200320023602100b2004450440200020002f01d0023b01e0020b41002102200041003602e802200041f0026a220442ceacbdb4f8a587a93d370300200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d002001410036022020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f47044020002001200441016a20062004417f736a10661087013602f802200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200020002802e8023602f8020b200042003703800320004188036a220242c8acbdb4f8a587a93d370300200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450440410021040c010b410021042001410036022020014200370318200141186a200510532007200820012802182202200128021c220620026b1005417f47044020002001200241016a20062002417f736a106610890137039003200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402000200029038003370390030b410021022000 410036029803200041a0036a220442e2e780dbeae4fcabe100370300200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450d002001410036022020014200370318200141186a200510532007200820012802182204200128021c220620046b1005417f47044020002001200441016a20062004417f736a106610723602a803200521020b2004450d002001200436021c0b200328020c22040440200320043602100b200245044020002000280298033602a8030b200042003703b003200041b8036a220242b3debf9dbddf9ca832370300200141286a1011220320022903001075200328020c200341106a28020047044010000b0240200328020022072003280204220810042205450440410021040c010b410021042001410036022020014200370318200141186a200510532007200820012802182202200128021c220620026b1005417f47044020002001200241016a20062002417f736a106610683703c003200521040b2002450d002001200236021c0b200328020c22020440200320023602100b2004450440200020002903b0033703c0030b200042003702c80341002102200041e8036a4100360200200041e0036a22074200370200200041d8036a2204429fe6c89bc186c5be1d37030020 0041d0036a4100360200200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450d002001410036022020014200370318200141186a200510532008200620012802182204200128021c220920046b1005417f4704402001200441016a20092004417f736a10662007108c01200521020b2004450d002001200436021c0b200328020c22040440200320043602100b20024504402007200041c8036a10260b200041f4036a220342003702002000418c046a2202420037020020004180046a2204428cbcadf0bacdb08975370300200020033602f00320004188046a22072002360200200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220920026b1005417f4704402001200241016a20092002417f736a10662007108f01200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402007200041f0036a10290b200042003702980441002102200041b8046a4100360200200041b0046a22074200370200200041a8046a220442bd8c e2969cacfba4b07f370300200041a0046a4100360200200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450d002001410036022020014200370318200141186a200510532008200620012802182204200128021c220920046b1005417f4704402001200441016a20092004417f736a10662007109201200521020b2004450d002001200436021c0b200328020c22040440200320043602100b2002450440200720004198046a102d0b41002102200041c0046a41f800107f1094012109200041b8056a2204429fa88ce0a0829ecea47f370300200041c0056a1094012107200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450d002001410036022020014200370318200141186a200510532008200620012802182204200128021c220620046b1005417f4704402001200441016a20062004417f736a10662007109501200521020b2004450d002001200436021c0b200328020c22040440200320043602100b200245044020072009102f0b200041b8066a1098012109200041c8066a220242bcf8d4f484c7a18d807f370300200041d0066a1098012107200141286a1011220320022903001075200328020c 200341106a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220620026b1005417f4704402001200241016a20062002417f736a10662007109901200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402007200910310b200041e4066a22034200370200200041fc066a22024200370200200041f0066a220442eafdbbb690ac81edca00370300200020033602e006200041f8066a22072002360200200141286a1011220320042903001075200328020c200341106a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220920026b1005417f4704402001200241016a20092002417f736a10662007109c01200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402007200041e0066a10340b20004188076a2209109f011a20004188096a220242d0b4afbcbf988ce0b77f37030020004190096a109f012107200141286a1011220320022903001075200328020c20034110 6a28020047044010000b0240200328020022082003280204220610042205450440410021040c010b410021042001410036022020014200370318200141186a200510532008200620012802182202200128021c220620026b1005417f4704402001200241016a20062002417f736a1066200710a001200521040b2002450d002001200236021c0b200328020c22020440200320023602100b20044504402007200941800210361a0b41002104200041b00b6a4100360200200041ac0b6a200041a80b6a2202360200200041a00b6a220542f4ffce959b97c9c1d500370300200041980b6a4100360200200041940b6a200041900b6a2207360200200020073602900b20022002360200200141286a1011220320052903001075200328020c200341106a28020047044010000b0240200328020022062003280204220910042208450d002001410036022020014200370318200141186a200810532006200920012802182205200128021c220620056b1005417f4704402001200541016a20062005417f736a1066200210a201200821040b2005450d002001200536021c0b200328020c22050440200320053602100b20044504402002200710380b200041b80b6a220710a6011a200041d00b6a220242affad78bbfdecaa6f300370300200041d80b6a10a6012104200141286a10112203200229 03001075200328020c200341106a28020047044010000b0240200328020022062003280204220910042208450440410021050c010b410021052001410036022020014200370318200141186a200810532006200920012802182202200128021c220620026b1005417f4704402001200241016a20062002417f736a1066200410a701200821050b2002450d002001200236021c0b200328020c22020440200320023602100b200545044020042007290300370300200441106a200741106a280200360200200441086a200741086a2903003703000b200141406b240020000b953102087f027e230041c0086b22052400200541c0066a10112201200041d00b6a22022903001074100f200120022903001075200041d80b6a2102200128020c200141106a28020047044010000b2001280204210720012802002106200541c0046a10112104200210a8012108200420054180026a10ae01220310af012004200820032802046a20032802006b100f200541a0026a200241106a280000220836020020054198026a200241086a290000220937030020052002290000220a37039002200541b0026a2009370300200541b8026a2008360200200541086a2009370300200541106a20083602002005200a3703a8022005200a370300200541d0026a2008360200200541c8026a20093703002005200a 3703c00202402004200541c0026a41141058220228020c200241106a280200460440200241046a2104200228020021080c010b200241046a2104100020022802002108200228020c2002280210460d0010000b20062007200820042802001006200328020022040440200320043602040b200228020c22030440200220033602100b200128020c22020440200120023602100b200541c0066a10112202200041a00b6a22012903001074100f200220012903001075200041a80b6a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a10112101200410a40121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f2001200410a5010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200041a80b6a10a301200041900b6a10a301200541a8026a1011220120004188096a22022903001074100f20012002290300107520004190096a2104200128020c200141106a28020047044010000b20012802042108200128 0200210720054190026a10112102200410a1012106200220054180026a10ae01220310af012002200620032802046a20032802006b100f200520044180021036220541c0026a200541800210361a200541c0046a200541c0026a41800210361a200541c0066a200541c0046a41800210361a02402002200541c0066a4180021058220228020c200241106a280200460440200241046a2104200228020021060c010b200241046a2104100020022802002106200228020c2002280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200228020c22030440200220033602100b200128020c22020440200120023602100b200541c0066a10112202200041f0066a22012903001074100f200220012903001075200041f8066a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a101121012004109d0121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f20012004109e010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120 033602100b200228020c22010440200220013602100b200541c0066a10112202200041c8066a22012903001074100f200220012903001075200041d0066a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a101121012004109a0121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f20012004109b010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041b8056a22012903001074100f200220012903001075200041c0056a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a10112101200410960121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f200120041097010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b2007200820062004280200100620 0328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041a8046a22012903001074100f200220012903001075200041b0046a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a1011210120002802b004200041b4046a28020010930121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f2001200410100240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200041b0046a28020022010440200041b4046a20013602000b200028029804220104402000419c046a20013602000b200541c0066a1011220220004180046a22012903001074100f20022001290300107520004188046a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a10112101200410900121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f20 0120041091010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041d8036a22012903001074100f200220012903001075200041e0036a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a101121012004108d0121062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f20012004108e010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200041e0036a28020022010440200041e4036a20013602000b20002802c80322010440200041cc036a20013602000b200541c0046a10112202200041b8036a22012903001074100f200220012903001075200228020c200241106a2802004704401000 0b2002280204210420022802002108200541c0026a10112101200541d8066a4100360200200541d0066a4200370300200541c8066a4200370300200542003703c006200541c0066a20002903c00310b00120052802c0062107200541c0066a41047210152001200541c0066a10ae01220310af012001200720032802046a20032802006b100f200120002903c00310750240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0046a10112202200041a0036a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0026a10112101200541d8066a4100360200200541d0066a4200370300200541c8066a4200370300200542003703c006200541c0066a20002802a803101920052802c0062107200541c0066a41047210152001200541c0066a10ae01220310af012001200720032802046a20032802006b100f200120002802a803101a0240200128020c200141106a280200460440200141046a21072001280200 21060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a1011220220004188036a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a10112101200029039003108a0121072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f2001200029039003108b010240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041f0026a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a1011210120002802f80210880121072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f200120002802f8021086010240 200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041d8026a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a1011210120002f01e00210850121072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f200120002e01e0021086010240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200041c0026a200041c8026a10b101200541c0066a1011220220004188026a22012903001074100f20022001290300107520004190026a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a1011210120041082012106 2001200541c0026a10ae01220310af012001200620032802046a20032802006b100f200120041083010240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041c8016a22012903001074100f200220012903001075200041d0016a2104200228020c200241106a28020047044010000b2002280204210820022802002107200541c0046a101121012004107d21062001200541c0026a10ae01220310af012001200620032802046a20032802006b100f20012004107e0240200128020c200141106a280200460440200141046a2104200128020021060c010b200141046a2104100020012802002106200128020c2001280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200041a8016a200041b0016a10b101200541c0066a1011220220004190016a22012903001074100f200220012903001075200228020c200241106a28020047044010 000b2002280204210420022802002108200541c0046a1011210120002d009801106f21072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f200120002d009801101a0240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112201200041f0006a22022903001074100f200120022903001075200041f8006a2104200128020c200141106a28020047044010000b2001280204210820012802002107200541c0046a101121022004105221062002200541c0026a10ae01220310af012002200620032802046a20032802006b100f024020022005200410121017220228020c200241106a280200460440200241046a2104200228020021060c010b200241046a2104100020022802002106200228020c2002280210460d0010000b20072008200620042802001006200328020022040440200320043602040b200228020c22030440200220033602100b200128020c22020440200120023602100b200541c0066a10112202200041d0006a22012903001074 100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a101121012000290358107421072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f2001200029035810750240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a10112202200041386a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a101121012000280240107321072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f20012000280240101a0240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0 066a10112202200041206a22012903001074100f200220012903001075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a1011210120002f0128107121072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f200120002f0128101a0240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c22010440200220013602100b200541c0066a1011220220002903081074100f200220002903081075200228020c200241106a28020047044010000b2002280204210420022802002108200541c0046a1011210120002d0010106f21072001200541c0026a10ae01220310af012001200720032802046a20032802006b100f200120002d0010101a0240200128020c200141106a280200460440200141046a2107200128020021060c010b200141046a2107100020012802002106200128020c2001280210460d0010000b20082004200620072802001006200328020022040440200320043602040b200128020c22030440200120033602100b200228020c220104 40200220013602100b200541c0086a24000b2601017f02402000280204450d0020002802002d000041c001490d00200010b20121010b20010b800101037f230041106b22012400200010aa0102400240200010ab01450d002000280204450d0020002802002d000041c001490d010b10000b200141086a200010ac01200128020c220041024f044010000b200128020821020340200004402000417f6a210020022d00002103200241016a21020c010b0b200141106a240020030b5801027f230041206b22012400200141186a4100360200200141106a4200370300200141086a42003703002001420037030020012000ad42ff018342001056210020012802002102200041046a1015200141206a240020020b8b0101037f230041106b22012400200010aa0102400240200010ab01450d002000280204450d0020002802002d000041c001490d010b10000b200141086a200010ac01200128020c220041034f044010000b200128020821030340200004402000417f6a210020032d00002002410874722102200341016a21030c010b0b200141106a2400200241ffff03710b5401017f230041206b22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001200041ffff037110192001280200210020014104721015200141206a240020 000b860101037f230041106b22012400200010aa0102400240200010ab01450d002000280204450d0020002802002d000041c001490d010b10000b200141086a200010ac01200128020c220041054f044010000b200128020821030340200004402000417f6a210020032d00002002410874722102200341016a21030c010b0b200141106a240020020b4f01017f230041206b22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001200010192001280200210020014104721015200141206a240020000b5001027f230041206b22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001200010b0012001280200210220014104721015200141206a240020020b0a0020002001420010590b1a0020004200370200200041086a4100360200200010b30120000ba40201057f230041206b22022400024002402000280204044020002802002d000041c001490d010b200241086a10761a0c010b200241186a200010ac01200010b40121030240024002400240200228021822000440200228021c220520034f0d010b41002100200241106a410036020020024200370308410021050c010b200241106a4100360200200242003703082000200520032003417f461b22046a2105200441 0a4b0d010b200220044101743a0008200241086a41017221030c010b200441106a4170712206103a21032002200436020c20022006410172360208200220033602100b03402000200546450440200320002d00003a0000200341016a2103200041016a21000c010b0b200341003a00000b2001200241086a10b501200241206a24000b0e0020012000106e4100473a00000b4401027f230041f00b6b220224002000106d410247044010000b2002200041011067200210b60121002002106b21032002200020011100002003106c200241f00b6a24000b7b01027f230041900c6b220224002000106a200241086a106b2103200241086a20011101002100200241f80b6a10112201200010b701100f20012000108601200128020c200141106a28020047044010000b200128020020012802041008200128020c22000440200120003602100b2003106c200241900c6a24000b180020004200370200200041086a4100360200200010761a0b2801017f230041206b22022400200241086a200041001067200241086a20011077200241206a24000b5001017f230041206b22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001200010b8012001280200210020014104721015200141206a240020000b6e01027f230041406a2202240020 00410110162100200241386a4100360200200241306a4200370300200241286a420037030020024200370320200241206a200241106a200110121014210320002002280220100f200020022001101210171a200341046a1015200241406b24000be10201027f02402001450d00200041003a0000200020016a2203417f6a41003a000020014103490d00200041003a0002200041003a00012003417d6a41003a00002003417e6a41003a000020014107490d00200041003a00032003417c6a41003a000020014109490d002000410020006b41037122026a220341003602002003200120026b417c7122026a2201417c6a410036020020024109490d002003410036020820034100360204200141786a4100360200200141746a410036020020024119490d002003410036021820034100360214200341003602102003410036020c200141706a41003602002001416c6a4100360200200141686a4100360200200141646a41003602002002200341047141187222026b2101200220036a2102034020014120490d0120024200370300200241186a4200370300200241106a4200370300200241086a4200370300200241206a2102200141606a21010c000b000b20000b1900200010761a2000410c6a10761a200041186a10761a20000b5601017f230041206b22022400200241086a20004100 1067200241086a2001107c200241086a200041011067200241086a2001410c6a1077200241086a200041021067200241086a200141186a1077200241206a24000b7a01027f230041406a22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001410010132202200010b8012002200141306a2000410c6a10121014200141206a200041186a1012101441011013210020012802002102200041046a1015200141406b240020020ba30101047f230041e0006b220224002000410310162100200241d8006a4100360200200241d0006a4200370300200241c8006a420037030020024200370340200241406b200110b801200241406b200241306a2001410c6a220310121014200241206a200141186a220410121014210520002002280240100f20002001107e2000200241106a20031012101720022004101210171a200541046a1015200241e0006a24000b3902017f017e230041106b220124002001200010b90120012903002102200141106a2400420020024201837d200242018885a74110744110750b6202027f017e230041206b22012400200141186a4100360200200141106a4200370300200141086a42003703002001420037030020012000ad42308622034230872003423f8710ba01210020012802002102200041046a1015 200141206a240020020b1301017e20002001ac22022002423f8710bb010b3302017f017e230041106b220124002001200010b90120012903002102200141106a2400420020024201837d200242018885a70b5201027f230041206b22012400200141186a4100360200200141106a4200370300200141086a4200370300200142003703002001200010bc01210020012802002102200041046a1015200141206a240020020b4202017f027e230041106b220124002001200010b901200141086a290300210320012903002102200141106a2400420020024201837d2003423f86200242018884850b5701037f230041206b22012400200141186a4100360200200141106a4200370300200141086a420037030020014200370300200120002000423f8710ba01210220012802002103200241046a1015200141206a240020030b0e00200020012001423f8710bb010bb10201047f230041d0006b22022400024002402000280204450d0020002802002d000041c001490d002000106d21032001280208200128020022046b4101752003490440200120022003200128020420046b410175200141086a10bd01220310be01200310bf010b200241286a200010c001200241186a200010c101200141086a21050340200228022c200228021c46044020022802302002280220460d030b2002200241 286a10c2012002107021030240200128020422002001280208490440200020033b01002001200041026a3602040c010b200241386a2001200020012802006b410175220041016a104f2000200510bd0121002002280240220420033b01002002200441026a3602402001200010be01200010bf010b200241286a10c3010c000b000b10000b200241d0006a24000b9c0101037f230041206b22012400200141186a4100360200200141106a4200370300200141086a420037030020014200370300024020002802002000280204460440200110c4010c010b200141001013210220002802042103200028020021000340200020034604402002410110131a05200220002f01001019200041026a21000c010b0b0b2001280200210020014104721015200141206a240020000b4301017f2000200128020420012802006b410175101621022001280204210020012802002101034020002001470440200220012f0100101a200141026a21010c010b0b0be20201057f23004190016b22022400024002402000280204450d0020002802002d000041c001490d00200241c8006a200010c001200241386a200010c101200241146a21040340200228024c200228023c46044020022802502002280240460d030b200241206a200241c8006a10c201200241086a10762100200410762105200241206a 10c501410247044010000b20024180016a10762103200241e8006a200241206a41001067200241e8006a200310772000200310b501200241d8006a10762103200241e8006a200241206a41011067200241e8006a200310772005200310b5012001200241e8006a2000103d22062802004504404128103a22032002290308370210200341186a200241106a280200360200200010b301200341246a200441086a2802003602002003411c6a2004290200370200200510b3012001200228026820062003103f0b200241c8006a10c3010c000b000b10000b20024190016a24000bd00101047f230041e0006b22012400200141206a4100360200200141186a4200370300200141106a42003703002001420037030802402000280208450440200141086a10c4010c010b200041046a2103200141086a410010132102200141346a2104200028020021000340200020034604402002410110131a05200141286a200041106a104b200241001013200141d0006a200141286a10121014200141406b200410121014410110131a2000102a21000c010b0b0b20012802082100200141086a4104721015200141e0006a240020000b6101027f230041206b220224002000200128020810162103200141046a210020012802002101034020002001460440200241206a240005200341021016200241106a 200141106a1012101720022001411c6a101210171a2001102a21010c010b0b0bce0101037f230041206b22022400024002402000280204044020002802002d000041c001490d010b20024100360208200242003703000c010b200241186a200010ac0120022802182103200241106a200010ac0120022802102104200010b40121002002410036020820024200370300200020046a20036b2200450d0020022000104520004101480d002002200228020420032000103620006a3602040b2001280200044020014100360208200142003702000b2001200228020036020020012002290204370204200241206a24000b5301017f230041206b22022400200241186a4100360200200241106a4200370300200241086a420037030020024200370300200220002001100e210020022802002101200041046a1015200241206a240020010b2c01037f200041f8006a21022000210103402001107621032001410c6a21012003410c6a2002470d000b20000b6101037f230041306b22022400200010c501410a47044010000b03402003410a460440200241306a240005200241206a10762104200241086a200020031067200241086a200410772001200410b5012001410c6a2101200341016a21030c010b0b0b800101037f230041306b22012400200141186a4100360200200141106a42003703 00200141086a42003703002001420037030020014100101321030340200241f800464504402003200141206a200020026a101210141a2002410c6a21020c010b0b200341011013210220012802002103200241046a1015200141306a240020030b4401027f230041106b220224002000410a10162103410021000340200041f800460440200241106a24000520032002200020016a101210171a2000410c6a21000c010b0b0b1000200010761a2000410036020c20000b5d01027f230041306b22022400200010c501410247044010000b200241206a10762103200241086a200041001067200241086a200310772001200310b501200241086a2000410110672001200241086a10870136020c200241306a24000b7301027f230041406a22012400200141286a4100360200200141206a4200370300200141186a4200370300200142003703102001200010322100200141106a41001013200141306a200010121014200028020c10bc0141011013210020012802102102200041046a1015200141406b240020020b2a01017f230041106b220224002000410210162002200110121017200128020c108601200241106a24000bd70101047f230041d0006b22022400024002402000280204450d0020002802002d000041c001490d00200241406b200010c001200241306a200010c101200241 106a210403402002280244200228023446044020022802482002280238460d030b200241186a200241406b10c201200241186a200241086a1076220010772001200241cc006a2000103d2205280200450440411c103a22032002290308370210200341186a2004280200360200200010b3012001200228024c20052003103f0b200241406b10c3010c000b000b10000b200241d0006a24000b9e0101037f230041306b22012400200141186a4100360200200141106a4200370300200141086a42003703002001420037030002402000280208450440200110c4010c010b200041046a21032001410010132102200028020021000340200020034604402002410110131a052002200141206a200041106a101210141a2000102a21000c010b0b0b2001280200210020014104721015200141306a240020000b4f01027f230041106b220224002000200128020810162103200141046a210020012802002101034020002001460440200241106a24000520032002200141106a101210171a2001102a21010c010b0b0b2601017f0340200141800246450440200020016a41003a0000200141016a21010c010b0b20000b990101027f23004190046b22022400200010aa0120024188046a200010ac01200228028c042103024002402000280204450d0020034180024b0d0020002802002d000041 c001490d010b10000b20024188026a109f0120034180022003418002491b22006b4180026a200228028804200010361a200241086a20024188026a41800210361a2001200241086a41800210361a20024190046a24000bc00101037f230041a0086b2201240020014198026a410036020020014190026a420037030020014188026a42003703002001420037038002200120004180021036220041a0026a200041800210361a200041a0046a200041a0026a41800210361a200041a0066a200041a0046a41800210361a41012103024003402002418002460d01200041a0066a20026a2101200241016a210220012d0000450d000b41830221030b200020033602800220004180026a4104721015200041a0086a240020030bec0101037f230041d0006b22022400024002402000280204450d0020002802002d000041c001490d00200241406b200010c001200241306a200010c101200241106a210403402002280244200228023446044020022802482002280238460d030b200241186a200241406b10c201200241186a200241086a1076220310774114103a2200410036020020002002290308370208200041106a2004280200360200200310b30120002001360204200128020021032001200036020020002003360200200320003602042001200128020841016a360208200241406b10 c3010c000b000b10000b200241d0006a24000b4d01037f02402000280208450d00200028020422012802002202200028020022032802043602042000410036020820032802042002360200034020002001460d01200128020421010c000b000b0b9d0101037f230041306b22012400200141186a4100360200200141106a4200370300200141086a42003703002001420037030002402000280208450440200110c4010c010b200041046a2102200141001013210303402002280200220220004604402003410110131a052003200141206a200241086a101210141a200241046a21020c010b0b0b2001280200210220014104721015200141306a240020020b4e01027f230041106b22032400200141046a210220002001280208101621000340200228020022022001460440200341106a24000520002003200241086a101210171a200241046a21020c010b0b0b2501017f03402001411446450440200020016a41003a0000200141016a21010c010b0b20000bbe0102027f027e230041406a22022400200010aa01200241386a200010ac01200228023c2103024002402000280204450d00200341144b0d0020002802002d000041c001490d010b10000b200241206a10a6012003411420034114491b22006b41146a2002280238200010361a200241186a200241306a2802002200360200 200241106a200241286a2903002204370300200220022903202205370308200141106a2000360000200141086a200437000020012005370000200241406b24000b850202037f027e23004180016b22012400200141306a4100360200200141286a4200370300200141206a4200370300200141086a200041086a2900002204370300200141106a200041106a280000220236020020014200370318200120002900002205370300200141406b2004370300200141c8006a2002360200200141d8006a2004370300200141e0006a20023602002001200537033820012005370350200141f8006a2002360200200141f0006a200437030020012005370368410121020240034020034114460d01200141e8006a20036a2100200341016a210320002d0000450d000b411521020b20012002360218200141186a410472101520014180016a240020020b750020004200370210200042ffffffff0f370208200020023602042000200136020002402003410871450d00200010c90120024f0d002003410471044010000c010b200042003702000b02402003411071450d00200010c90120024d0d0020034104710440100020000f0b200042003702000b20000b4101017f200028020445044010000b0240200028020022012d0000418101470d00200028020441014d047f100020002802000520010b 2c00014100480d0010000b0b990101037f200028020445044041000f0b200010aa01200028020022022c0000220141004e044020014100470f0b027f4101200141807f460d001a200141ff0171220341b7014d0440200028020441014d047f100020002802000520020b2d00014100470f0b4100200341bf014b0d001a2000280204200141ff017141ca7e6a22014d047f100020002802000520020b20016a2d00004100470b0bd60101047f200110b4012204200128020422024b04401000200128020421020b200128020021052000027f02400240200204404100210120052c00002203417f4a0d01027f200341ff0171220141bf014d04404100200341ff017141b801490d011a200141c97e6a0c010b4100200341ff017141f801490d001a200141897e6a0b41016a21010c010b4101210120050d000c010b41002103200120046a20024b0d0020022001490d00410020022004490d011a200120056a2103200220016b20042004417f461b0c010b41000b360204200020033602000bc20101057f230041406a22022400200241286a1011220320002903001075200328020c200341106a28020047044010000b02402003280200220020032802042205100422064504400c010b2002410036022020024200370318200241186a200610532000200520022802182200200228021c220520 006b1005417f47044020012002200041016a20052000417f736a106610b6013a0000200621040b2000450d002002200036021c0b200328020c22000440200320003602100b200241406b240020040b30002000410036020820004200370200200041011045200028020441fe013a00002000200028020441016a36020420000b6101037f200028020c200041106a28020047044010000b200028020422022001280204200128020022036b22016a220420002802084b047f20002004105720002802040520020b20002802006a2003200110361a2000200028020420016a3602040b0b0020002001420010561a0b8e0201067f230041406a22042400200441286a1011220220002903001074100f200220002903001075200228020c200241106a28020047044010000b2002280204210620022802002107200441106a1011210020012d000010b70121052000200410ae01220310af012000200520032802046a20032802006b100f200020012c00001086010240200028020c200041106a280200460440200041046a2101200028020021050c010b200041046a2101100020002802002105200028020c2000280210460d0010000b20072006200520012802001006200328020022010440200320013602040b200028020c22030440200020033602100b200228020c22000440200220003602 100b200441406b24000b820101047f230041106b2201240002402000280204450d0020002802002d000041c001490d00200141086a200010cd01200128020c210003402000450d0120014100200128020822032003200010cb0122046a20034520002004497222031b3602084100200020046b20031b2100200241016a21020c000b000b200141106a240020020b2201017f03402001410c470440200020016a4100360200200141046a21010c010b0b0b800301037f200028020445044041000f0b200010aa0141012102024020002802002c00002201417f4a0d00200141ff0171220341b7014d0440200341807f6a0f0b02400240200141ff0171220141bf014d0440024020002802042201200341c97e6a22024d047f100020002802040520010b4102490d0020002802002d00010d0010000b200241054f044010000b20002802002d000145044010000b4100210241b7012101034020012003460440200241384f0d030c0405200028020020016a41ca7e6a2d00002002410874722102200141016a21010c010b000b000b200141f7014d0440200341c07e6a0f0b024020002802042201200341897e6a22024d047f100020002802040520010b4102490d0020002802002d00010d0010000b200241054f044010000b20002802002d000145044010000b4100210241f701210103402001 200346044020024138490d0305200028020020016a418a7e6a2d00002002410874722102200141016a21010c010b0b0b200241ff7d490d010b10000b20020b5c00024020002d0000410171450440200041003b01000c010b200028020841003a00002000410036020420002d0000410171450d00200041003602000b20002001290200370200200041086a200141086a280200360200200110b3010b3902017f017e230041106b220124002001200010b90120012903002102200141106a2400420020024201837d200242018885a74118744118750b6202027f017e230041206b22012400200141186a4100360200200141106a4200370300200141086a42003703002001420037030020012000ad42388622034238872003423f8710ba01210020012802002102200041046a1015200141206a240020020b2701017f230041106b220224002000410010132002200110121014410110131a200241106a24000ba10102027f027e230041106b22022400200110aa0102400240200110ab01450d002001280204450d0020012802002d000041c001490d010b10000b200241086a200110ac01200228020c220141114f044010000b20022802082103034020010440200542088620044238888421052001417f6a210120033100002004420886842104200341016a21030c010b0b200020043703 0020002005370308200241106a24000b2301017e20002002423f87220320014201868520024201862001423f888420038510560b2301017e20002002423f87220320014201868520024201862001423f888420038510590b1301017e20002001ac22022002423f8710ba010b4c01017f2000410036020c200041106a2003360200200104402001104d21040b200020043602002000200420024101746a22023602082000200420014101746a36020c2000200236020420000b870101037f200120012802042000280204200028020022046b22036b2202360204200341004a044020022004200310361a200128020421020b200028020021032000200236020020012003360204200028020421022000200128020836020420012002360208200028020821022000200128020c3602082001200236020c200120012802043602000b2b01027f200028020821012000280204210203402001200247044020002001417e6a22013602080c010b0b0b0b0020002001410110c6010b0b0020002001410010c6010b170020002001280204200141086a280200411c10a9011a0bb70102057f017e230041106b22032400200041046a210102402000280200220204402001280200220504402005200041086a2802006a21040b20002004360204200041086a2002360200200341086a20014100200420 0210cb0110cc0120002003290308220637020420004100200028020022012006422088a76b2202200220014b1b3602000c010b200020012802002201047f2001200041086a2802006a0541000b360204200041086a41003602000b200341106a24000b3901017f027f200041186a28020022010440200041086a280200200041146a280200200110550c010b20000b2201200128020041016a3602000b220002402000280204044020002802002d000041bf014b0d010b10000b200010b2010be70101037f230041106b2204240020004200370200200041086a410036020020012802042103024002402002450440200321020c010b410021022003450d002003210220012802002d000041c001490d00200441086a200110cd0120004100200428020c220120042802082202200110cb0122032003417f461b20024520012003497222031b220536020820004100200220031b3602042000200120056b3602000c010b20012802002103200128020421012000410036020020004100200220016b20034520022001497222021b36020820004100200120036a20021b3602040b200441106a24000b3501017f230041106b220041908c0436020c41800c200028020c41076a417871220036020041840c200036020041880c3f003602000b10002002044020002001200210361a0b0b3001017f 200028020445044041000f0b4101210120002802002c0000417f4c047f200010ca01200010b4016a0520010b0b5b00027f027f41002000280204450d001a410020002802002c0000417f4a0d011a20002802002d0000220041bf014d04404100200041b801490d011a200041c97e6a0c010b4100200041f801490d001a200041897e6a0b41016a0b0b2901017f230041206b22022400200241086a20002001411410a90110c9012100200241206a240020000b5b01027f2000027f0240200128020022054504400c010b200220036a200128020422014b0d0020012002490d00410020012003490d011a200220056a2104200120026b20032003417f461b0c010b41000b360204200020043602000b2401017f200110b401220220012802044b044010000b20002001200110ca01200210cc010b2f002000200210cf01200028020020002802046a2001200210361a2000200028020420026a3602042000200310d0010b1b00200028020420016a220120002802084b04402000200110570b0b830201047f02402001450d00034020002802102202200028020c460d01200241786a28020020014904401000200028021021020b200241786a2203200328020020016b220136020020010d012000200336021020004101200028020422032002417c6a28020022016b22021054220441016a2002 4138491b220520036a10d101200120002802006a220320056a2003200210490240200241374d0440200028020020016a200241406a3a00000c010b200441f7016a220341ff014d0440200028020020016a20033a00002000280200200120046a6a210103402002450d02200120023a0000200241087621022001417f6a21010c000b000b10000b410121010c000b000b0b0f00200020011057200020013602040b26002000410110cf01200028020020002802046a20013a00002000200028020441016a3602040b6001027f20011054220241b7016a22034180024e044010000b2000200341ff017110d2012000200028020420026a10d101200028020420002802006a417f6a2100034020010440200020013a0000200141087621012000417f6a21000c010b0b0b0bfa0301004180080bf203746f70696331006461746131006a735f636f6e7472616374006576656e740073657455696e7433324576740073657455696e743136457674007472616e7366657200696e69740073657455696e74380067657455696e74380073657455696e7431360067657455696e7431360073657455696e7433320067657455696e7433320073657455696e7436340067657455696e74363400736574537472696e6700676574537472696e6700736574426f6f6c00676574426f6f6c0073657443686172 0067657443686172007365744d657373616765006765744d657373616765007365744d794d657373616765006765744d794d65737361676500736574496e743800676574496e743800736574496e74313600676574496e74313600736574496e74333200676574496e74333200736574496e74363400676574496e74363400736574566563746f7200676574566563746f72007365744d6170006765744d617000746573744d756c7469506172616d730073657442797465730067657442797465730073657441727261790067657441727261790073657450616972006765745061697200736574536574006765745365740073657446697865644861736800676574466978656448617368007365744c697374006765744c69737400736574416464726573730067657441646472657373'
  cabi = [{"constant":false,"input":[{"name":"input","type":"string[10]"}],"name":"setArray","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getUint32","output":"uint32","type":"Action"},{"constant":false,"input":[{"name":"input","type":"int64"}],"name":"setInt64","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getInt64","output":"int64","type":"Action"},{"constant":false,"input":[{"name":"input","type":"pair<string,int32>"}],"name":"setPair","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getPair","output":"pair<string,int32>","type":"Action"},{"anonymous":false,"input":[{"name":"topic","type":"string"},{"name":"arg1","type":"string"}],"name":"transfer","topic":1,"type":"Event"},{"anonymous":false,"input":[{"name":"topic","type":"string"},{"name":"arg1","type":"string"},{"name":"arg2","type":"uint16"}],"name":"setUint16Evt","topic":1,"type":"Event"},{"constant":false,"input":[{"name":"addr","type":"FixedHash<20>"}],"name":"setAddress","output":"void","type":"Action"},{"anonymous":false,"input":[{"name":"topic1","type":"string"},{"name":"topic2","type":"uint32"},{"name":"arg1","type":"string"},{"name":"arg2","type":"uint32"},{"name":"arg3","type":"uint32"}],"name":"setUint32Evt","topic":2,"type":"Event"},{"constant":false,"input":[],"name":"init","output":"void","type":"Action"},{"constant":false,"input":[{"name":"input","type":"uint8"}],"name":"setUint8","output":"void","type":"Action"},{"baseclass":[],"fields":[{"name":"head","type":"string"}],"name":"message","type":"struct"},{"constant":false,"input":[{"name":"msg","type":"message"}],"name":"setMessage","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getUint8","output":"uint8","type":"Action"},{"constant":false,"input":[{"name":"input","type":"uint16"}],"name":"setUint16","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getUint16","output":"uint16","type":"Action"},{"constant":false,"input":[{"name":"input","type":"uint32"}],"name":"setUint32","output":"void","type":"Action"},{"constant":false,"input":[{"name":"input","type":"uint64"}],"name":"setUint64","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getUint64","output":"uint64","type":"Action"},{"constant":false,"input":[{"name":"input","type":"string"}],"name":"setString","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getString","output":"string","type":"Action"},{"constant":false,"input":[{"name":"input","type":"bool"}],"name":"setBool","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getBool","output":"bool","type":"Action"},{"constant":false,"input":[{"name":"input","type":"int8"}],"name":"setChar","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getChar","output":"int8","type":"Action"},{"constant":true,"input":[],"name":"getMessage","output":"message","type":"Action"},{"baseclass":["message"],"fields":[{"name":"body","type":"string"},{"name":"end","type":"string"}],"name":"my_message","type":"struct"},{"constant":false,"input":[{"name":"msg","type":"my_message"}],"name":"setMyMessage","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getMyMessage","output":"my_message","type":"Action"},{"constant":false,"input":[{"name":"input","type":"int8"}],"name":"setInt8","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getSet","output":"set<string>","type":"Action"},{"constant":true,"input":[],"name":"getInt8","output":"int8","type":"Action"},{"constant":false,"input":[{"name":"input","type":"int16"}],"name":"setInt16","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getInt16","output":"int16","type":"Action"},{"constant":false,"input":[{"name":"input","type":"int32"}],"name":"setInt32","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getInt32","output":"int32","type":"Action"},{"constant":false,"input":[{"name":"vec","type":"uint16[]"}],"name":"setVector","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getVector","output":"uint16[]","type":"Action"},{"constant":false,"input":[{"name":"input","type":"map<string,string>"}],"name":"setMap","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getMap","output":"map<string,string>","type":"Action"},{"constant":false,"input":[{"name":"msg","type":"message"},{"name":"input1","type":"int32"},{"name":"input2","type":"bool"}],"name":"testMultiParams","output":"void","type":"Action"},{"constant":false,"input":[{"name":"input","type":"uint8[]"}],"name":"setBytes","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getBytes","output":"uint8[]","type":"Action"},{"constant":true,"input":[],"name":"getArray","output":"string[10]","type":"Action"},{"constant":false,"input":[{"name":"input","type":"set<string>"}],"name":"setSet","output":"void","type":"Action"},{"constant":false,"input":[{"name":"input","type":"FixedHash<256>"}],"name":"setFixedHash","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getFixedHash","output":"FixedHash<256>","type":"Action"},{"constant":false,"input":[{"name":"input","type":"list<string>"}],"name":"setList","output":"void","type":"Action"},{"constant":true,"input":[],"name":"getList","output":"list<string>","type":"Action"},{"constant":true,"input":[],"name":"getAddress","output":"FixedHash<20>","type":"Action"}]
  
  ```

  The wasm type contract creates a contract instance through platon.wasmcontract

  Call the method .constructor() on the instance to construct the contract, and send the transaction to the chain through transact

```python
  # Instantiate and deploy contract
Payable = platon.wasmcontract(abi=cabi, bytecode=bytecode,vmtype=1)
  
tx_hash = Payable.constructor().transact(
      {
          'from':from_address,
          'gas': 1500000,
      }
  )
  
  # Wait for the transaction to be mined, and get the transaction receipt
  tx_receipt = platon.waitForTransactionReceipt(tx_hash)
  print(tx_receipt)
```

  Among them, tx_receipt is the transaction receipt of the deployment contract

  After the deployment is successful, the output is as follows

```
  #Output
AttributeDict ({ 'blockHash': HexBytes ( '0x7a193be2cf86aedcf844c0478c6f64d226affb55779bad1b2056c7e70e8158d6'), 'blockNumber': 2012981, 'contractAddress':' lax15sh4rpuqr4fvzs4cyj9uea54r5tax7kljqqszk ',' cumulativeGasUsed ': 1233168,' from ':' lax1uqug0zq7rcxddndleq4ux2ft3tv6dqljphydrl ',' gasUsed ': 1233168,' logs ': [],' logsBloom ': HexBytes (' 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 '),' status': 1, 'to': None, 'transactionHash': HexBytes ( '0x717a82ea0ef116e271fb02dbb7d456fe9dd41a2dbd07cac81d079e375b5dade1'), 'transactionIndex': 0} )
  
```

##### (3) Send transaction to Helloworld contract (wasm contract)

  ​ Based on the successful deployment of the previous contract, call the method in the contract.

  ​ payable is an instance after the contract is successfully deployed

  ​ By calling the function setBool, the parameter false is sent to the chain (send transaction)

  ```python
  payable = platon.wasmcontract(address=tx_receipt.contractAddress, abi=cabi,vmtype=1)
  
  tx_hash0 = payable.functions.setBool(false).transact(
      {
          'from':from_address,
          'gas': 1500000,
      }
  )
  print(platon.waitForTransactionReceipt(tx_hash0))
  print('get: {}'.format(
      payable.functions.getBool().call()
  ))
  ```

  payable.functions.getBool().call(), which means that the corresponding information on the chain is obtained through the function getBool (according to the definition of this contract, the parameters uploaded by setBool are obtained).

  After successful operation, the results are as follows:

  ```python
  #Output
  AttributeDict({'blockHash': HexBytes('0x9bcadf4db5d74789901b2176cb7dad3191d2425b61f261966e932f6606d13041'),'blockNumber': 2018575,'contractAddress': None,'cumulativeq [], 'logsBloom': HexBytes ( '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'), 'status': 1, 'to': 'lax1c5h59flven2hzyrylh2tsmn59r5ucms95n5ugc', 'transactionHash': HexBytes ( '0x4c724e7d1833ade363f51f611293682771318e3c86b533f5a78b580c812eb009'), 'transactionIndex': 0})
  get: False
  ```

##### (4) Event call of wasm contract

  Events in the wasm contract are generally written in the function of the contract.

  Take the wasmcontract contract as an example, the method setUint32 contains the event setUint32Evt, and setUint32Evt can be used to monitor and log the transaction results of setUint32.

  Greeter is an example of a successfully deployed wasm type contract

  tx_hash is a transaction instance of function setUint32 passing parameters

  ```python
  greeter = platon.wasmcontract(address=tx_receipt.contractAddress, abi=abi,vmtype=1)
  tx_hash = greeter.functions.setUint32(1000).transact(
      {
          'from': from_address,
          'gas': 1500000,
      }
  )
  
  tx_receipt = platon.waitForTransactionReceipt(tx_hash)
  print(tx_receipt)
  
  topic_param = greeter.events.setUint32Evt().processReceipt(tx_receipt)
print(topic_param)
  ```

  topic_param is the result of the event setUint32Evt call

  The output after successful operation is as follows:

  ```python
(AttributeDict(('args': AttributeDict(('topic1':'topic1','arg1':'data1','arg2': 1000,'arg3': 1000}),'event':'setUint32Evt', ' logIndex ': 0,' transactionIndex ': 0,' transactionHash ': HexBytes (' 0xabac50c6a9d443d9f89065775f0f3d56ddeabd2f2a5e0e1f36d00db703b14d8b '),' address': 'lax1sgsp74pce2vkgwqjd3rzmt55p70psmq7qvnqwn', 'blockHash': HexBytes ( '0x78f15fbacbc745dfd5b35b596d28b61ae2987b6ff9050dc39c716f383e505899'), 'blockNumber': 1477774}),)
  ```

  Among the values ​​​​corresponding to'args':

  'topic1' is the topic value,'arg1','arg2', and'arg3' are the three parameter values ​​defined in the event.



### Four part: Ppos

#### 1.staking

```python
from client_sdk_python import Web3, HTTPProvider
from client_sdk_python.ppos import Ppos
w3 = Web3(HTTPProvider("http://localhost:6789"))
ppos = Ppos(w3)
```

##### Initiate a pledge

Calling method

```
ppos.createStaking(benifit_address, node_id, external_id, node_name, website, details, amount,program_version,program_version_sign, bls_pubkey, bls_proof, pri_key, reward_per, typ=2, transaction_cfg=None)
```

Parameter Description

- **typ**: Indicates whether the account free amount or the account's lock amount is used for staking, 0: free amount; 1: lock amount;2: Give priority to lock amount, use free amount provided that staking amount over lock amount.

- **benifit_address**: Income account for accepting block rewards and staking rewards.

- **node_id**: The idled node Id (also called the candidate's node Id).

- **external_id**: External Id (with length limit, Id for the third party to pull the node description).

- **node_name**: The name of the staking node (with a length limit indicating the name of the node).

- **website**: The third-party home page of the node (with a length limit indicating the home page of the node).

- **details**: Description of the node (with a length limit indicating the description of the node).

- **amount**: staking von (unit:von, 1LAT = 10\*\*18 von).

- **program_version**: The real version of the program, admin_getProgramVersion.

- **program_version_sign**: The real version of the program is signed, admin_getProgramVersion.

- **bls_pubkey**: Bls public key.

- **bls_proof**: Proof of bls, obtained by pulling the proof interface, admin_getSchnorrNIZKProve.

- **pri_key**: Private key for transaction.

- **reward_per**: Proportion of the reward share obtained from the commission, using BasePoint 1BP = 0.01%.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Modify pledge information

Calling method

```
ppos.editCandidate(benifit_address, node_id, external_id, node_name, website, details, pri_key, reward_per, transaction_cfg=None)
```

Parameter Description

- **benifit_address**: Income account for accepting block rewards and staking rewards.

- **node_id**: The idled node Id (also called the candidate's node Id).

- **external_id**: External Id (with length limit, Id for the third party to pull the node description).

- **node_name**: The name of the staking node (with a length limit indicating the name of the node).

- **website**: The third-party home page of the node (with a length limit indicating the home page of the node).

- **details**: Description of the node (with a length limit indicating the description of the node).

- **pri_key**: Private key for transaction.

- **reward_per**: Proportion of the reward share obtained from the commission, using BasePoint 1BP = 0.01%.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Increase stake

Calling method

```
ppos.increaseStaking(node_id, amount, pri_key, typ=2, transaction_cfg=None)
```

Parameter Description

- **typ**: Indicates whether the account free amount or the account's lock amount is used for staking, 0: free amount; 1: lock amount;2: Give priority to lock amount, use free amount provided that staking amount over lock amount.

- **node_id**: The idled node Id (also called the candidate's node Id).

- **amount**: staking von (unit:von, 1LAT = 10\*\*18 von).

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration..
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Cancel the pledge (initiate all cancellation at one time, and arrive at the account multiple times)

Calling method

```
ppos.withdrewStaking(node_id, pri_key, transaction_cfg=None)
```

Parameter Description

- **node_id**: The idled node Id (also called the candidate's node Id).

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

#### 2 .delegate

##### Initiate a delegation

Calling method

```
ppos.delegate(typ, node_id, amount, pri_key, transaction_cfg=None)
```

Parameter Description

- **typ**: Indicates whether the account free amount or the account's lock amount is used for delegate, 0: free amount; 1: lock amount.

- **node_id**: The idled node Id (also called the candidate's node Id).

- **amount**: Amount of delegate (unit:von, 1LAT = 10\*\*18 von).

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Shareholding reduction/revocation commission

Calling method

```
ppos.withdrewDelegate(staking_blocknum, node_id, amount, pri_key, transaction_cfg=None)
```

Parameter Description

- **staking_blocknum**: A unique indication of a pledge of a node.

- **node_id**: The idled node Id (also called the candidate's node Id).

- **amount**: The amount of the entrusted reduction (unit:von, 1LAT = 10\*\*18 von).

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Withdrawing delegated rewards

Calling method

```
ppos.withdrawDelegateReward(pri_key, transaction_cfg=None)
```

Parameter Description

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

#### 3.query

##### Query the validator queue of the current settlement cycle

Calling method

```
ppos.getVerifierList(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the list of validators in the current consensus cycle

Calling method

```
ppos.getValidatorList(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the list of all real-time candidates

Calling method

```
ppos.getCandidateList(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the NodeID and pledge Id of the node entrusted by the current account address

Calling method

```
ppos.getRelatedListByDelAddr(del_addr, from_address=None)
```

Parameter Description

- **del_addr**: Client's account address.
- **from_address**: Used to call the rpc call method.

##### Query the current delegation information of a single node

Calling method

```
ppos.getDelegateInfo(staking_blocknum, del_address, node_id, from_address=None)
```

Parameter Description

- **staking_blocknum**: Block height at the time of staking.
- **del_address**: Client's account address.
- **node_id**: Verifier's node ID.
- **from_address**: Used to call the rpc call method.

##### Query the pledge information of the current node

Calling method

```
ppos.getCandidateInfo(node_id, from_address=None)
```

Parameter Description

- **node_id**: Verifier's node ID.
- **from_address**: Used to call the rpc call method.

##### Query the block reward of the current settlement cycle

Calling method

```
ppos.getPackageReward(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the pledge reward of the current settlement cycle

Calling method

```
ppos.getStakingReward(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the average time of packing blocks

Calling method

```
ppos.getAvgPackTime(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### The query account has not withdrawn commission rewards at each node.

Calling method

```
ppos.getDelegateReward(address, node_ids=[])
```

Parameter Description

- **address**:account address to be queried.
- **node_ids**:the string array of the node id to be queried, if it is empty, query all nodes delegated by the account.

#### 4. Double sign

##### Report double sign

Calling method

```
ppos.reportDuplicateSign(typ, data, pri_key, transaction_cfg=None)
```

Parameter Description

- **typ**: Represents duplicate sign type, 1:prepareBlock, 2: prepareVote, 3:viewChange.

- **data**: Json value of single evidence, format reference RPC interface Evidences.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Query whether the node has been reported too much signature

Calling method

```
ppos.checkDuplicateSign(typ, node_id, block_number, from_address=None)
```

Parameter Description

- **typ**: Represents double sign type, 1:prepareBlock, 2: prepareVote, 3:viewChange.
- **check_address**: Reported node address.
- **block_number**: Duplicate-signed block height.
- **from_address**: Used to call the rpc call method.

#### 5. Lock position

##### Create a hedging plan

Calling method

```
ppos.createRestrictingPlan(account, plan, pri_key, transaction_cfg=None)
```

Parameter Description

- **account**: Locked account release account.

- **plan**:
  An is a list of `RestrictingPlan` types (array), and `RestrictingPlan` is defined as follows:
  
  ~~~
  type RestrictingPlan struct {
  Epoch uint64
  Amount *big.Int
  }
  ~~~
  
  - where Epoch: represents a multiple of the billing period.
    The product of the number of blocks per billing cycle indicates that the locked fund
    s are released at the target block height. Epoch * The number of blocks per cycle is
    at least greater than the maximum irreversible block height.
  - Amount: indicates the amount to be released on the target block.
  
- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Get lock information

Calling method

```
ppos.getRestrictingInfo(account, from_address=None)
```

Parameter Description

- **account**: Locked account release account.
- **from_address**: Used to call the rpc call method.

#### 6. Governance

```python
from client_sdk_python import Web3, HTTPProvider
from client_sdk_python.pip import Pip
w3 = Web3(HTTPProvider("http://localhost:6789"))
pip = Pip(w3)
```

##### Text proposal

Calling method

```
pip.submitText(verifier, pip_id, pri_key, transaction_cfg=None)
```

Parameter Description

- **verifier**: The certified submitting the proposal.

- **pip_id**: PIPID.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Upgrade proposal

Calling method

```
pip.submitVersion(verifier, pip_id, new_version, end_voting_rounds, pri_key, transaction_cfg=None)
```

Parameter Description

- **verifier**: The certified submitting the proposal.

- **pip_id**: PIPID.

- **new_version**: upgraded version.

- **end_voting_rounds**: The number of voting consensus rounds.
  Explanation: Assume that the transaction submitted by the proposal is rounded when the consensus round
  number of the package is packed into the block, then the proposal voting block is high,
  which is the 230th block height of the round of the round1 + endVotingRounds
  (assuming a consensus round out of block 250, ppos The list is 20 blocks high in advance,
  250, 20 are configurable), where 0 <endVotingRounds <= 4840 (about 2 weeks, the actual discussion
  can be calculated according to the configuration), and is an integer).
  
- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Parameter proposal

Calling method

```
pip.submitParam(verifier, pip_id, module, name, new_value, pri_key, transaction_cfg=None)
```

Parameter Description

- **verifier**: The certified submitting the proposal.

- **pip_id**: PIPID.

- **module**: parameter module.

- **name**: parameter name.

- **new_value**: New parameter value.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Delete proposal

Calling method

```
pip.submitCancel(verifier, pip_id, end_voting_rounds, tobe_canceled_proposal_id, pri_key, transaction_cfg=None)
```

Parameter Description

- **verifier**: The certified submitting the proposal.

- **pip_id**: PIPID.

- **end_voting_rounds**:
  The number of voting consensus rounds. Refer to the instructions for submitting the upgrade proposal.
  At the same time, the value of this parameter in this interface.
  cannot be greater than the value in the corresponding upgrade proposal.
  
- **tobe_canceled_proposal_id**: Upgrade proposal ID to be cancelled.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

Calling method

```
pip.vote(verifier, proposal_id, option, program_version, version_sign, pri_key, transaction_cfg=None)
```

Parameter Description

- **verifier**: The certified submitting the proposal.

- **proposal_id**: Proposal ID.

- **option**: Voting option.

- **program_version**: Node code version, obtained by rpc getProgramVersion interface.

- **version_sign**: Code version signature, obtained by rpc getProgramVersion interface.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Version Statement

Calling method

```
pip.declareVersion(active_node, program_version, version_sign, pri_key, transaction_cfg=None)
```

Parameter Description

- **active_node**: The declared node can only be a verifier/candidate.

- **program_version**: The declared version, obtained by rpc's getProgramVersion interface.

- **version_sign**: The signed version signature, obtained by rpc's getProgramVersion interface.

- **pri_key**: Private key for transaction.

- **transaction_cfg**: Transaction basic configuration.
  
  ~~~
  type: 
  	dict
  example:
      cfg = {
           "gas":100000000,
           "gasPrice":2000000000000,
           "nonce":1,
       }
  ~~~
  
  

##### Query proposal

Calling method

```
pip.getProposal(proposal_id, from_address=None)
```

Parameter Description

- **proposal_id**: proposal id.
- **from_address**: Used to call the rpc call method.

##### Query proposal results

Calling method

```
pip.getTallyResult(proposal_id, from_address=None)
```

Parameter Description

- **proposal_id**: proposal id.
- **from_address**: Used to call the rpc call method.

##### Query the cumulative number of votes available for a proposal

Calling method

```
pip.getAccuVerifiersCount(proposal_id, block_hash, from_address=None)
```

Parameter Description

- **proposal_id**: proposal id.
- **block_hash**: block hash.
- **from_address**: Used to call the rpc call method.

##### Query the list of proposals

Calling method

```python
pip.listProposal(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the effective version of the node's chain

Calling method

```python
pip.getActiveVersion(from_address=None)
```

Parameter Description

- **from_address**: Used to call the rpc call method.

##### Query the governance parameter value of the current block height

Calling method

```python
pip.getGovernParamValue(module, name, from_address=None)
```

Parameter Description

- **module**: Parameter module.
- **name**: parameter name.
- **from_address**:Used to call the rpc call method.

##### Query the governance parameter list

Calling method

```python
pip.listGovernParam(self, module=None, from_address=None)
```

Parameter Description

- module:Parameter module.
- from_address: Used to call the rpc call method.