# Reading events off of the eth blockchain

**Pre-reqs:**
To access the Ethereum Blockchain, we'll be using **geth**, the official Go Ethereum CLI client. It is the entry point into the Ethereum network (main-, test- or private net), capable of running as a full node (default) archive node (retaining all historical state) or a light node (retrieving data live).

Starting a ethereum/client-go container:
```
docker pull ethereum/client-go:v1.8.3

docker run -d --name ethereum-node \
-p 8545:8545 -p 30303:30303 \
ethereum/client-go:v1.8.3 \
--syncmode light \
--rpc --rpcaddr 0.0.0.0
```

---

## Not all HelloWorld are the same

In a traditional object oriented language, running a "HelloWorld.exension" should result in the same output whether Alice runs the file or Bob runs the file.

On the ethereum blockchain, when a program is written, it must be "deployed" to the blockchain by a user's wallet. This means that although 2 ".sol" programs may have 100% identical code, they will each by pushed to the blockchain and have a unique "Contract Address", along with some metadata such as the user who deployed the contract.

Here we will use remix.ethereum.org, a free Solidity Web Compiler, to showcase a simple Hello World smart contract deployed by Alice vs Hello World Deployed by Bobbie

- HelloWorld.sol deployed by alice's wallet `0xca35b7d915458ef540ade6068dfe2f44e8fa733c`:
  - <img src="https://user-images.githubusercontent.com/9003246/36337223-91e5f504-1346-11e8-9832-3e6852eef631.png" width="350">


- HelloWorld.sol deployed by Bob's wallet `0x14723a09acff6d2a60dcdf7aa4aff308fddc160c`:
 - <img src="https://user-images.githubusercontent.com/9003246/36337235-b64e358c-1346-11e8-90c2-29a27b0c15f5.png" width="350">


Even more interesting, once a "smart contract" is deployed to a "contract address", EACH FUNCTION in the smart contract has a unique "MethodID" and "Topic" which can be used to see when that given function belonging to the smart contract was invoked

---

## Notebook Demos

### Option 1 - Run the notebooks with Virtual Environments
```
# 1. Install conda/miniconda 
wget -P /tmp/ https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Silent Install to prefix=/opt/miniconda3/ 
bash /tmp/Miniconda3-latest-Linux-x86_64.sh -b -f -p /opt/miniconda3/

# 2. Create virtual env with blockchain demos packages
conda create -n eth_demo_env python=3.5
source activate eth_demo_env
pip install -r <git source>/series01-eth_blockchain/demo01/requirements.txt


# 3. Start a notebook
jupyter notebook --NotebookApp.ip=0.0.0.0 \
--NotebookApp.token='there_is_no_spoon' \
--NotebookApp.port=8895
```
Connect to the notebook via http://<yourhost>:8895 , using the token you set - `there_is_no_spoon`


### Option 2 - Run the notebook and virtual envs in a docker container
```
# 1. Build the docker image
cd <git source>/series01-eth_blockchain/demo01/
docker build -t ethdemo01 .

# 2. Start the docker container
docker run -it --name ethdemo01notebook \
-p 8895:8895 ethdemo01
```
Connect to the notebook via http://<yourhost>:8895 , using the token - `there_is_no_spoon`


### Part 1 - "Reading smart contract execution logs off of the blockchain"
Given range of blocks, query all ed_trades


### Part 2 - "Creating a kafka topic for every new block recorded to the ethereum blockchain"
Poll latest eth block, produce kafka topic event on eth-chain-blocks topic 


### Part 3 - "Filtering through the noise from the blockchain - Creating topic filters in kafka"

Option 1:
   Python Listen to eth-chain-blocks topic, run etl and produces ed_trade topic
Option 2:
   Nifi listens to eth-chain-blocks topic, runs custom processor and produces ed_trade topic
