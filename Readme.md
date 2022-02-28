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
## 0. clone project POA
```
# git clone https://github.com/thanachaiTP/workshop-poa.git -b validator validator
# cd validator
```

## 1. set password to password.txt file
```
# head -1 /dev/urandom | base64 | md5sum
```

## 2. create new account
copy Public address of the key ใส่ใน docker-compose.yaml ในบรรทัด unlock
```
# docker run --rm -it -v $PWD:/poa -w /poa ethereum/client-go:v1.10.16 --datadir /poa/node --password password.txt  account new
```

## 3. init-genesis
```
# docker run --rm -it -v $PWD:/poa -w /poa ethereum/client-go:v1.10.16 --datadir /poa/node --nousb --password password.txt init genesis.json
```


## 4. deploy
```
# docker-compose up -d
# docker-compose logs -f
```

## 5. JavaScript Console

> https://geth.ethereum.org/docs/rpc/server

```
# docker exec -it validator sh
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

command-line สำหรับ get enode
```
# docker exec -it validator geth --datadir /poa/node attach --exec 'admin.nodeInfo'
```
command-line สำหรับ add validator
```
# docker exec -it validator geth --datadir /poa/node attach --exec 'clique.propose("0x048F519b032bAfa19Cf28D0cbf717a5fd119fA7A", true)'
```
command-line สำหรับ check validator
```
# docker exec -it validator geth --datadir /poa/node attach --exec 'clique.getSigners()'
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

