## Overview
### Network ID
The network id is only set in these files.
- Mainnet: 12300 (set in ./poohnet/config/el/genesis.json)
- Mainnet for merge: 12300 (set in ./poohnet/config/el/genesis_merge.json)
- Testnet: 12301 (set in ./poohnet/config/el/genesis_testnet.json)
- Devnet: 12302 (set in ./poohnet/config/el/genesis_devnet.json)

## Run Testnet

### stop & remote docker
```
docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED       STATUS          PORTS                                                                                                                                                                                          NAMES
2a9587c59d4c   linked0/poohnet-pow:latest   "geth --datadir=/dat…"   7 weeks ago   Up 32 minutes   0.0.0.0:6060->6060/tcp, :::6060->6060/tcp, 0.0.0.0:8545->8545/tcp, :::8545->8545/tcp, 0.0.0.0:30303->30303/tcp, :::30303->30303/tcp, 8546/tcp, 0.0.0.0:30303->30303/udp, :::30303->30303/udp   pow-node```
```

CONTAINER ID를 확인하고 중단 시킴
```
docker stop 2a9587c59d4c
```

CONTAINER ID를 확인하고 제거하기 
```
docker rm 2a9587c59d4c
```

### go to poohnet folder
```
cd poohnet
```

### init node
- 노드가 하나인 테스트넷을 실행시킬때
- Applied cancun upgrade to `genesis_testnet.json`
    ```
    ./init testnet 1
    ```
- If you want run `geth` with old london fork, use this command
    ```
    ./init testnet-london 1
    ```
    - This uses `genesis_testnet_london.json`.
 
`Password:` 프롬프트에 대해서는 로컬시스템 사용자 계정 암호를 입력

### run node
```
./enode testnet node1
```

## Run with local binary
다음은 로컬에서 빌드한 바이너리를 이용해서 노드를 띄우는 경우

### compile
```
./compile
```

### init node
`poohnet` 폴더로 이용
```
cd poohnet
```
노드가 하나인 테스트넷을 실행시킬때
```
./init testnet 1
```

### run node
init은 위의 [Run Testnet](#run-testnet) 참고
```
./enode-cmd
```

## PoohnetFund constract 적용 (for Testnet)
### network stop
```
docker ps
CONTAINER ID   IMAGE                     COMMAND                  CREATED       STATUS         PORTS                                                                                                          NAMES
0996629dab6f   linked0/poohgeth:latest   "geth --datadir=/dat…"   2 hours ago   Up 7 minutes   0.0.0.0:6060->6060/tcp, 0.0.0.0:8545->8545/tcp, 0.0.0.0:30303->30303/tcp, 8546/tcp, 0.0.0.0:30303->30303/udp   poohgeth-1

docker stop 0996629dab6f
```

### genesis_func_testnet.json 변경
genesis_func_testnet.json에서 config 항목 수정

### update 실행
```
./update fund

```

### poohnet 재실행
```
./enode testnet node1
```
"Successfully wrote genesis state" 확인
```
Config: fund
Configure folder: config
Genesis file: genesis_fund_testnet.json
2024/05/15 04:02:23 maxprocs: Leaving GOMAXPROCS=16: CPU quota undefined
INFO [05-15|04:02:23.559] Maximum peer count                       ETH=50 LES=0 total=50
INFO [05-15|04:02:23.560] Smartcard socket not found, disabling    err="stat /run/pcscd/pcscd.comm: no such file or directory"
INFO [05-15|04:02:23.566] Set global gas cap                       cap=50,000,000
INFO [05-15|04:02:23.566] Initializing the KZG library             backend=gokzg
INFO [05-15|04:02:23.602] Using pebble as the backing database 
INFO [05-15|04:02:23.602] Allocated cache and file handles         database=/data/geth/chaindata cache=16.00MiB handles=16
INFO [05-15|04:02:23.642] Opened ancient database                  database=/data/geth/chaindata/ancient/chain readonly=false
INFO [05-15|04:02:23.677] Successfully wrote genesis state         database=chaindata                          hash=1a43f4..37cd42
INFO [05-15|04:02:23.678] Using pebble as the backing database 
INFO [05-15|04:02:23.678] Allocated cache and file handles         database=/data/geth/lightchaindata          cache=16.00MiB handles=16
INFO [05-15|04:02:23.710] Opened ancient database                  database=/data/geth/lightchaindata/ancient/chain readonly=false
INFO [05-15|04:02:23.718] Successfully wrote genesis state         database=lightchaindata                          hash=1a43f4..37cd42
```

## Setup Blockscout
[Setup Blockscout](./docs/setup-blockscout.md) 참고