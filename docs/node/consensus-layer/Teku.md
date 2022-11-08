---
---

# Teku



## Overview

Teku is a consensus client built to meet institutional needs and security requirements. Built by PegaSys an arm of ConsenSys, dedicated to building enterprise-ready clients and tools for interacting with the core Ethereum platform. More information on [Teku]([https://consensys.net/knowledge-base/ethereum-2/teku/](https://consensys.net/knowledge-base/ethereum-2/teku/))

This project builds a customized version of the Teku client with Gnosischain modifications.
[Repository](https://github.com/gnosischain/teku-client) 


## Option 1: with docker

**Beacon & Validator Node**


```
docker pull gnosischain/teku:{upstream_version}-{testnet}
docker pull gnosischain/teku:latest-chiado
```


Example Docker-compose.yml 

[https://github.com/gnosischain/teku-client/blob/main/docker-compose.yml](https://github.com/gnosischain/teku-client/blob/main/docker-compose.yml) 

This should be added to `docker-compose.yml` in `/gnosis/`

```
  consensus:
    stop_grace_period: 30s
    container_name: consensus-client
    restart: unless-stopped
    image: consensys/teku:latest
    depends_on:
      config_consensus:
        condition: service_completed_successfully
    networks:
    - sedge
    volumes:
    - ./consensus-data:/var/lib/teku
    - <path-to>/jwtsecret:/tmp/jwt/jwtsecret
    - ./config.yml:/network_config/config.yml
    - ./genesis.ssz:/network_config/genesis.ssz
    ports:
    - 9000:9000/tcp
    - 9000:9000/udp
    - 5054:5054/tcp
    expose:
    - 4000
    command:
    - --log-destination=CONSOLE
    - --logging=info
    - --initial-state=/network_config/genesis.ssz
    - --network=/network_config/config.yml
    - --p2p-enabled=true
    - --p2p-port=9000
    - --p2p-peer-upper-bound=50
    - --rest-api-enabled=true
    - --rest-api-host-allowlist=*
    - --rest-api-interface=0.0.0.0
    - --rest-api-port=4000
    - --rest-api-cors-origins=*
    - --rest-api-docs-enabled=false
    - --data-beacon-path=/var/lib/teku/beacon
    - --data-path=/var/lib/teku
    - --data-storage-archive-frequency=2048
    - --data-storage-mode=PRUNE
    - --data-storage-non-canonical-blocks-enabled=false
    - --ee-endpoint=http://execution:8551
    - --validators-proposer-default-fee-recipient=<FEE-RECIPIENT-ADDRESS>
    - --ee-jwt-secret-file=/tmp/jwt/jwtsecret
    - --metrics-enabled=true
    - --metrics-host-allowlist=*
    - --metrics-interface=0.0.0.0
    - --metrics-port=5054
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "10"
  validator-import:
    container_name: validator-import-client
    build:
      context: github.com/NethermindEth/teku-init-validator
    depends_on:
      config_consensus:
        condition: service_completed_successfully
    volumes:
    - ./validator-data:/data
    - ./keystore:/keystore
  validator:
    container_name: validator-client
    image: consensys/teku:latest
    depends_on:
      validator-import:
        condition: service_completed_successfully
      config_consensus:
        condition: service_completed_successfully
    networks:
    - sedge
    ports:
    - 5056:5056
    volumes:
    - ./validator-data:/data
    - ./config.yml:/network_config/config.yml
    - ./genesis.ssz:/network_config/genesis.ssz
    command:
    - validator-client
    - --network=/network_config/config.yml
    - --beacon-node-api-endpoint=http://execution:8545
    - --data-path=/data
    - --log-destination=CONSOLE
    - --validators-graffiti=<CUSTOM-GRAFFITI-HERE>
    - --validator-keys=/data/keys:/data/passwords
    - --metrics-enabled=true
    - --metrics-host-allowlist=*
    - --metrics-interface=0.0.0.0
    - --metrics-port=5056
    - --validators-proposer-default-fee-recipient=<FEE-RECIPIENT-ADDRESS>
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "10"
networks:
  sedge:
    name: sedge_network
```

**Import validator keys**

Add your keystores in `./validator-data/keys` and the `password.txt` in `./validator-data/secrets`

**Run Beacon Chain node with the attached Validator Process**


```
docker-compose up -d consensus-client
docker-compose up -d validator-client
```


**Make Deposit**

:::caution
**At this stage you should have your EL and CL fully Synced and validators must be imported to your CL **
:::

See Section Deposit