# MapRDB Data Access Gateway Demo
This demo pulls data from the Ethereum Blockchain through a local geth client (running in a docker container), perform light transformations to obtain valid JSON transaction records, and push the data into an existing Mapr-DB cluster (using the data-access-gateway RESTful iterface). We then use the REST api to send a query to MapR-DB, and retrieve selected attributes of "interesting" transactions (for example, those whose creators significantly overpaid to prioritize) for further analysis.

### Before you begin
This demo is easiest to run from the edge node of a secured mapr cluster. An "edge node" here means a linux host (i'm using centos7.4) capable of running docker containers, and no special MapR packages or configurations required. A functional mapr cluster is also assumed, with Data Access Gateways accessible from the edge node. 


#### Start the Geth container
To access the Ethereum Blockchain, we'll be using **geth**, the official Go Ethereum CLI client. It is the entry point into the Ethereum network (main-, test- or private net), capable of running as a full node (default) archive node (retaining all historical state) or a light node (retrieving data live).

Starting a ethereum/client-go container:
```
docker run -d --name ethereum-node \
-p 8545:8545 -p 30303:30303 \
ethereum/client-go:v1.8.3 \
--syncmode light \
--rpc --rpcaddr 0.0.0.0
```

#### Optional: setup persistent storage on mfs
This notebook can be optionally securely persisted to MapR-FS, by starting this docker container with a volume mount on top of a mapr-loopbacknfs client (on the underlying host) using testuser's mapr ticket, but this is not required for the demo.
```
// add instructions 
```
Alternatively, persist the notebook on a local directory on the edge node. 

#### Run notebook and virtual envs in a docker container
```
# Clone this project into testuser's home directory
git clone https://github.com/kapetre/geth_maprdb_demo

# 1. Build the docker image
cd geth_mapr_demo/
docker build -t jupyterlab .

# 2. Start the docker container
docker run -it \
--name jupy \
-v /mfs/democluster/user/testuser/geth_maprdb_demo:/Notebooks \
-p 8895:8895 jupyterlab 
```
Connect to the notebook via http://<yourhost>:8895 , using the token `thereisnospoon` configured in start-nb.sh


