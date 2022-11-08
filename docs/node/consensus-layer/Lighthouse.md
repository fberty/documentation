---
---

# Lighthouse


## Overview 

Lighthouse is an Ethereum and Gnosischain consensus client written in Rust by [Sigma Prime](https://lighthouse.sigmaprime.io/)

This project builds a customized version of the Lighthouse client with Gnosischain modifications.
[Repository](https://github.com/gnosischain/lighthouse-client) 



## Option 1: with docker

**Beacon Node**


```
docker pull gnosischain/lighthouse-beacon:latest-<gnosis or chiado> 
```


**Validator Node**


```
docker pull gnosischain/lighthouse-validator:latest-<gnosis or chiado> 
```


**Example Docker Compose file**

https://github.com/gnosischain/lighthouse-client/blob/main/docker-compose.yml

This should be added to `docker-compose.yml` in `/gnosis/`

```
  consensus:
    stop_grace_period: 30s
    container_name: consensus-client
    restart: always
    image: gnosischain/lighthouse-beacon:latest-<gnosis or chiado> 
    networks:
    - sedge
    volumes:
    - ./consensus-data/data:/consensus-data/data
    - ./gnosis/jwtsecret:/jwtsecret:/jwtsecret
    ports:
    - 9000:9000/tcp
    - 9000:9000/udp
    - 5054:5054/tcp
    expose:
    - 4000
    command:
    - lighthouse
    - bn
    - --disable-upnp
    - --datadir=/consensus-data/data
    - --port=9000
    - --http
    - --http-address=0.0.0.0
    - --http-port=4000
    - --network=gnosis
    - --target-peers=50
    - --execution-endpoints=http://execution:8551
    - --execution-jwt=<path-to>/jwtsecret
    - --eth1-endpoints=http://execution:8545
    - --debug-level=info
    - --validator-monitor-auto
    - --subscribe-all-subnets
    - --import-all-attestations
    - --metrics
    - --metrics-port=5054
    - --metrics-address=0.0.0.0
    - --checkpoint-sync-url=https://rpc-gbc.gnosischain.com
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "10"
  validator-import:
    container_name: validator-import-client
    build:
      context: github.com/NethermindEth/lighthouse-init-validator
      args:
        LH_VERSION: sigp/lighthouse:v3.1.2
        NETWORK: gnosis
    networks:
    - sedge
    volumes:
    - ./keystore:/keystore
    - ./consensus-data:/consensus-data/data
  validator:
    container_name: validator-client
    image: gnosischain/lighthouse-validator:latest-<gnosis or chiado>
    depends_on:
      validator-import:
        condition: service_completed_successfully
    networks:
    - sedge
    ports:
    - 5056:5056
    volumes:
    - ./validator-data:/data
    command:
    - lighthouse
    - vc
    - --network=gnosis
    - --beacon-nodes=http://execution:8545
    - --graffiti=<your custom graffiti here>
    - --debug-level=info
    - --validators-dir=/data/validators
    - --metrics
    - --metrics-port=5056
    - --metrics-address=0.0.0.0
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "10"
networks:
  gnosischain:
    name: gnosischain_network
    ```

**Import validator keys**

your keystores in `./keystore` and the `password.txt` in a file `./keystore/password.txt`

```
cd gnosis
docker-compose up validator-import-client; docker-compose down-client
```

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