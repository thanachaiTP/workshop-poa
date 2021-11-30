# Deploy Blockchain POA (Validator)
> networkid 39

# รายละเอียด System Requirements
|VM Spec  | Descriptions     |
|---------|------------------|
|OS       | Ubuntu 20.04 LTS |
|CPU      |vCPU 8 Cores      |
|RAM      |16 GB             |
|DISK SSD | 200 GB           |

# รายละเอียด Software Requirements
| Software      | Version           |
|---------------|-------------------|
|docker         | 20.10.6 or higher |
|docker-compose | 1.29.1 or higher  |
|geth  | ethereum/client-go:v1.10.12 |

# Install docker and docker-compose
```
# curl https://get.docker.com | sh

# curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# chmod +x /usr/local/bin/docker-compose
```
## 1. clone project POA
```
# git clone https://github.com/thanachaiTP/workshop-poa.git -b rpc rpc
# cd rpc
```

## 2. init-genesis
```
# docker run --rm -it -v $PWD:/poa -w /poa ethereum/client-go:v1.10.12 --datadir /poa/node --nousb --password password.txt init genesis.json
```


## 3. deploy
```
# docker-compose up -d
# docker-compose logs -f
```

## 4. JavaScript Console

> https://geth.ethereum.org/docs/rpc/server

```
# docker exec -it rpc sh
# geth --datadir /poa/node attach
```

|                   |                   Command                                             |
|-------------------|-----------------------------------------------------------------------|
|Get Signer         | > clique.getSigners()                                                 |
|Check Peers Count  | > net.peerCount                                                       |
|Check IP Peers     | > admin.peers                                                         |
|Add Peers Static   | > admin.addPeer("enode://a979fb575495b8d6db44f75@52.16.188.185:30303")|
|Check NodeInfo     | > admin.nodeInfo                                                      |
|Add Singer         | > clique.propose("0xd881234E73223d1623E0d56789942eA1c0B67890", true)  |
|Remoce Signer      | > clique.propose("0xd881234E73223d1623E0d56789942eA1c0B67890", false) |
|Check Vote         | > clique.proposals                                                    |

1 command-line
```
# docker exec -it rpc geth --datadir /poa/node attach --exec 'admin.nodeInfo'
```
# หากต้องการ reset data เพื่อทำการ join ใหม่
## 7. removedb
```
# docker run --rm -it -v $PWD:/poa -w /poa ethereum/client-go:v1.10.12 --datadir /poa/node --nousb --password password.txt removedb
```
และให้ทำการ init genesis ใหม่อีกครั้งก่อนการ deploy
```
# docker run --rm -it -v $PWD:/poa -w /poa ethereum/client-go:v1.10.12 --datadir /poa/node --nousb --password password.txt init genesis.json
```

