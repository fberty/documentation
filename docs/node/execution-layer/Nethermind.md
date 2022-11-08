---
---

# Nethermind {#nethermind}

## Overview 

Execution layer client developed by nethermind team [https://nethermind.io/nethermind-client/](https://nethermind.io/nethermind-client/) 

There are 2 main options for running Nethermind:
* Option 1: As a system process
* Option 2: Using Docker

Nethermind can be configured to run different types of nodes: 
* Full Node
* Archival Node


## Option 1: Running as System Process

##### Installing Nethermind {#installing-nethermind}

[https://downloads.nethermind.io/](https://downloads.nethermind.io/) 


##### Running Nethermind {#running-nethermind}

Nethermind has ‘Nethermind launcher’ an easy GUI where you can configure your node from release. 

[https://docs.nethermind.io/nethermind/first-steps-with-nethermind/running-nethermind-post-merge#running-release](https://docs.nethermind.io/nethermind/first-steps-with-nethermind/running-nethermind-post-merge#running-release) 

Windows 
```
# Gnosis Mainnet
./Nethermind.Runner --config xDai --JsonRpc.JwtSecretFile=<PATH to jwt.hex>

# Chiado Testnet
./Nethermind.Runner --config chiado --JsonRpc.JwtSecretFile=<PATH to jwt.hex>
```

Linux and MAC
```
# Gnosis Mainnet
nethermind --config xDai --JsonRpc.JwtSecretFile=<PATH to jwt.hex>

# Chiado Testnet
nethermind --config chiado --JsonRpc.JwtSecretFile=<PATH to jwt.hex>
```

## Option 2: Using Docker


##### Pulling Nethermind Docker Images {#pulling-nethermind-docker-images}

[https://docs.nethermind.io/nethermind/installing-nethermind/docker](https://docs.nethermind.io/nethermind/installing-nethermind/docker) 


##### Using Docker to run Nethermind {#using-docker-to-run-nethermind}

**Ensure the prerequisite steps have been completed in ‘Advanced> Initial Set up’ section.**

**Create Folder for Nethermind**

```
cd
mkdir /home/<USER>/gnosis
mkdir /home/<user>/gnosis/execution-client
```


**make the JWT Secret **

This is the token that allows the EL client to communicate with the CL client, we use `rand` to create a random string, and store the `jwt.hex `to `/var/lib/jwtsecret/ `this directory can be changed, if so you need to reflect the correct directory.

```
sudo mkdir -p /var/lib/jwtsecret
openssl rand -hex 32 | sudo tee /var/lib/jwtsecret/jwt.hex > /dev/null
```


**Create Docker-compose file**

```
cd gnosis
nano /home/<USER>/gnosis/docker-compose.yml
```

**Paste the following**


```
version: "3.9"
services:
  execution:
    stop_grace_period: 30s
    container_name: execution-client
    restart: always
    image: nethermind/nethermind:latest
    networks:
    - gnosischain
    volumes:
    - /home/<user>/gnosis/execution-data:/execution-data/data
    - /home/<user>/gnosis/jwtsecret:/<path-to>/jwtsecret
    - /etc/timezone:/etc/timezone:ro
    - /etc/localtime:/etc/localtime:ro
    ports:
    - 30303:30303/tcp
    - 30303:30303/udp
    expose:
    - 8545
    - 8551
    command:
    - --config=xdai
    - --datadir=/execution-data/data
    - --log=INFO
    - --Sync.SnapSync=false
    - --JsonRpc.Enabled=true
    - --JsonRpc.Host=0.0.0.0
    - --JsonRpc.Port=8545
    - --JsonRpc.EnabledModules=[Web3,Eth,Subscribe,Net,]
    - --JsonRpc.JwtSecretFile=/jwtsecret
    - --JsonRpc.EngineHost=0.0.0.0
    - --JsonRpc.EnginePort=8551
    - --Network.DiscoveryPort=30303
    - --HealthChecks.Enabled=false
    - --Pruning.CacheMb=2048
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "10"
networks:
  gnosischain:
    name: gnosischain_network
```

**Start the EL container and check logs**

```
cd gnosis
sudo docker-compose up -d execution-client
sudo docker-compose logs -f execution-client 
```

## Nethermind Archival Node

An archival node executes heavy historical sync verifying all the transactions and keeping all the historical state. In Nethermind, the default configuration activates the pruning functionality.

:::caution
Make sure there's enough disk space to accommodate the archive data, the minimum amount of disk required to run the archive node is as of today +2 TB.
:::

Select Network: `xdai_archive`

```
--config=xdai_archive
```

Set the Following variable 
```
NETHERMIND_PRUNINGCONFIG_MODE: "None"
```

In docker-compose 
```
	environment:
    - NETHERMIND_PRUNINGCONFIG_MODE: "None"
```