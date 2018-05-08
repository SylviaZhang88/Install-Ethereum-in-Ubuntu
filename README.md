
# Ethereum Bootstrap
通过本文所述方法和项目中的脚本，我们可以快速的搭建好自己的私链进行开发测试。

仓库中包含的工具有：
* 一个测试账户导入脚本，在首次部署时将五个测试账户私钥导入以太坊节点。
  一个genesis.json配置文件，为对应的五个测试账户提供初始资金（以太币），方便开发测试。
    一个快速启动私有链节点并进入交互模式的脚本。
    一个合约样例：contracts/Token.sol。这是一个使用合约语言Solidity编写的智能合约。Token合约的功能是发行一种token（可以理解为货币，积分等等），只有合约的创建者有发行权，token的拥有者有使用权，并且可以自由转账。
*

## 环境说明
> 系统版本： Ubuntu 14.04    
项目地址： https://github.com/janx/ethereum-bootstrap  
搭建参考： https://github.com/dragonetail/ethereum-bootstrap/tree/master/1.SingleNode  


## 配置阿里源
> 参考： https://www.cnblogs.com/lyon2014/p/4715379.html`

1.lsb_release -a  
```shell
No LSB modules are available.   
Distributor ID:	Ubuntu   
Description:	Ubuntu 14.04.5 LTS   
Release:	14.04   
Codename:	trusty   
```

2.cd /etc/apt
3.mv sources.list sources.list_bak

4.vi sources.list
```shell 
deb http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe    
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe  
```

5.vi /etc/resolv.conf 
> nameserver 223.5.5.5

6.apt-get update
  
## 安装官方的ethereum安装包
1.apt-get install software-properties-common
2.add-apt-repository -y ppa:ethereum/ethereum
3.apt-get update
4.apt-get install ethereum solc
5.apt-get install git
6.apt-get install expect

## clone项目
cd /root
```shell
#ethereum 源项目地址：https://github.com/cryptape/ethereum-bootstrap.git
git clone https://github.com/dragonetail/ethereum-bootstrap.git
```

## 创建bootnode
1.cd ethereum-bootstrap/3.LocalNetworkCluster

2.mkdir bootnode
cd bootnode

3.生成bootnode的节点Key(bootnode的节点Key)
bootnode --genkey=boot.key
ll
```shell
-rw------- 1 root root   64 May  8 06:06 boot.key   
```

4.使用genesis.json初始化bootnode的chaincode信息
geth --datadir data --networkid 20180412  init ../genesis.json 
> INFO [05-08|06:14:20] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:14:20] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/bootnode/data/geth/chaindata cache=16 handles=16  
INFO [05-08|06:14:20] Writing custom genesis block   
INFO [05-08|06:14:20] Persisted trie from memory database      nodes=7 size=1.31kB time=73.153µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:14:20] Successfully wrote genesis state         database=chaindata                                                                   hash=194258…f0efa3  
INFO [05-08|06:14:20] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/bootnode/data/geth/lightchaindata cache=16 handles=16  
INFO [05-08|06:14:20] Writing custom genesis block   
INFO [05-08|06:14:20] Persisted trie from memory database      nodes=7 size=1.31kB time=470.262µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:14:20] Successfully wrote genesis state         database=lightchaindata    


生成了data目录，存储了chaincode的数据
ls -ltr
```shell
total 12  
-rw------- 1 root root   64 May  8 06:06 boot.key  
drwx------ 4 root root 4096 May  8 06:14 data  
```

5.启动bootnode，获取bootnode的接入地址串
nohup bootnode --nodekey=boot.key &
> INFO [05-08|06:16:30] UDP listener up                          self=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@[::]:30301  
> 
> 如果重复测试，需要删除boot.key和data/  




## 本机启动第一个节点：
1.mkdir node1
cd node1/
root@ethereum-1:~/ethereum-bootstrap/3.LocalNetworkCluster/node1# ls -ltr
> total 0

2.下面使用genesis.json初始化node1的chaincode信息  
geth --datadir data --networkid 20180412  init ../genesis.json   
> INFO [05-08|06:22:19] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:22:19] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/chaindata cache=16 handles=16  
INFO [05-08|06:22:19] Writing custom genesis block   
INFO [05-08|06:22:19] Persisted trie from memory database      nodes=7 size=1.31kB time=63.088µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:22:19] Successfully wrote genesis state         database=chaindata                                                                  hash=194258…f0efa3  
INFO [05-08|06:22:19] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/lightchaindata cache=16 handles=16  
INFO [05-08|06:22:19] Writing custom genesis block   
INFO [05-08|06:22:19] Persisted trie from memory database      nodes=7   size=1.31kB time=408.738µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:22:19] Successfully wrote genesis state           database=lightchaindata  

3.启动节点，参数主要为网络ID与bootnode的一致，bootnodes的内容指向bootnode节点的地址串，指导端口  
geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console  
```shell
INFO [05-08|06:23:08] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:23:08] Starting peer-to-peer node               instance=Geth/v1.8.7-stable-66432f38/linux-amd64/go1.10  
INFO [05-08|06:23:08] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/chaindata cache=768 handles=512  
WARN [05-08|06:23:08] Upgrading database to use lookup entries   
INFO [05-08|06:23:08] Initialised chain configuration            config="{ChainID: 15 Homestead: 0 DAO: <nil> DAOSupport: false EIP150: <nil>   EIP155: 0 EIP158: 0 Byzantium: <nil> Constantinople: <nil> Engine: unknown}"
INFO [05-08|06:23:08] Database deduplication successful        deduped=0  
INFO [05-08|06:23:08] Disk storage enabled for ethash caches   dir=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/ethash count=3  
INFO [05-08|06:23:08] Disk storage enabled for ethash DAGs     dir=/root/.ethash                                                           count=2
INFO [05-08|06:23:08] Initialising Ethereum protocol           versions="[63 62]" network=20180412  
INFO [05-08|06:23:08] Loaded most recent local header          number=0 hash=194258…f0efa3 td=131072  
INFO [05-08|06:23:08] Loaded most recent local full block      number=0 hash=194258…f0efa3 td=131072  
INFO [05-08|06:23:08] Loaded most recent local fast block      number=0 hash=194258…f0efa3 td=131072  
INFO [05-08|06:23:08] Regenerated local transaction journal    transactions=0 accounts=0  
INFO [05-08|06:23:08] Starting P2P networking   
INFO [05-08|06:23:10] UDP listener up                          self=enode://c9f3df1fe5ecb1418be32f5a4b047d02a0fa30de776a99127be94c44d470935d964e275a99e56b7d29c00c8a80673028dec8ba7fd90e4f48fceac18de061f125@[::]:30303  
INFO [05-08|06:23:10] RLPx listener up                         self=enode://c9f3df1fe5ecb1418be32f5a4b047d02a0fa30de776a99127be94c44d470935d964e275a99e56b7d29c00c8a80673028dec8ba7fd90e4f48fceac18de061f125@[::]:30303  
INFO [05-08|06:23:10] IPC endpoint opened                      url=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth.ipc  
Welcome to the Geth JavaScript console!  
instance: Geth/v1.8.7-stable-66432f38/linux-amd64/go1.10  
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0   txpool:1.0 web3:1.0  

> admin.nodeInfo.enode    
"enode://c9f3df1fe5ecb1418be32f5a4b047d02a0fa30de776a99127be94c44d470935d964e275a99e56b7d29c00c8a80673028dec8ba7fd90e4f48fceac18de061f125@[::]:30303"  
 
> admin.nodeInfo  
{  
  enode: "enode://c9f3df1fe5ecb1418be32f5a4b047d02a0fa30de776a99127be94c44d470935d964e275a99e56b7d29c00c8a80673028dec8ba7fd90e4f48fceac18de061f125@[::]:30303",  
  id: "c9f3df1fe5ecb1418be32f5a4b047d02a0fa30de776a99127be94c44d470935d964e275a99e56b7d29c00c8a80673028dec8ba7fd90e4f48fceac18de061f125",  
  ip: "::",  
  listenAddr: "[::]:30303",  
  name: "Geth/v1.8.7-stable-66432f38/linux-amd64/go1.10",  
  ports: {  
    discovery: 30303,  
    listener: 30303  
  },  
  protocols: {  
    eth: {  
      config: {  
        chainId: 15,  
        eip150Hash:   "0x0000000000000000000000000000000000000000000000000000000000000000",  
        eip155Block: 0,  
        eip158Block: 0,   
        homesteadBlock: 0  
      },  
      difficulty: 131072,  
      genesis:   "0x19425866b7d3298a15ad79accf302ba9d21859174e7ae99ce552e05f13f0efa3",  
      head:   "0x19425866b7d3298a15ad79accf302ba9d21859174e7ae99ce552e05f13f0efa3",  
      network: 20180412  
    }    
  }  
}  
```


## 启动第二个节点
1.
root@ethereum-2:~/ethereum-bootstrap/3.LocalNetworkCluster# mkdir node2
cd node2/

2.
geth --datadir data --networkid 20180412 init ../genesis.json 
```shell
INFO [05-08|06:29:16] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:29:16] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node2/data/geth/chaindata cache=16 handles=16  
INFO [05-08|06:29:16] Writing custom genesis block   
INFO [05-08|06:29:16] Persisted trie from memory database      nodes=7 size=1.31kB time=61.556µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:29:16] Successfully wrote genesis state         database=chaindata                                                                hash=194258…f0efa3  
INFO [05-08|06:29:16] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node2/data/geth/lightchaindata cache=16 handles=16  
INFO [05-08|06:29:16] Writing custom genesis block   
INFO [05-08|06:29:16] Persisted trie from memory database      nodes=7 size=1.31kB time=417.029µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:29:16] Successfully wrote genesis state         database=lightchaindata     
```

3.
geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console

4.验证节点是否正常接入  
这个时候在第一个节点或第二个节点上执行net.peerCount和admin.peers，可以看到相关节点的ID信息已经互相接入了  
```shell
> net.peerCount  
1  
> admin.peers  
[{  
    caps: ["eth/63"],  
    id: "9407b373f1297f6674cd467fbdff1ac624cccc08013c52b56f93d6f754f82883682edae879291190f06e2b30304419e073cc4e400e3f16203820a6dd8754629e",    
    name: "Geth/v1.8.7-stable-66432f38/linux-amd64/go1.10",   
    network: {  
      inbound: false,  
      localAddress: "192.168.0.150:45259",  
      remoteAddress: "192.168.0.151:30303",  
      static: false,  
      trusted: false  
    },  
    protocols: {  
      eth: {  
        difficulty: 131072,  
        head:   "0x19425866b7d3298a15ad79accf302ba9d21859174e7ae99ce552e05f13f0efa3",  
        version: 63  
      }  
    }  
}]  
```


### 开发环境搭建 
apt-get install nodejs   
apt-get install npm  
apt-get install mongodb  
git clone http://zhanghy@172.17.20.127/wangzg/ethereum-api.git  
cd ethereum-api & npm install  
运行nmp install提示需要 npm>=3,node>=6.5.0  
安装符合版本
node -v
    v8.11.1
npm -v
    5.6.0

重新运行npm install

修改配置：config/config.js db和node项 ##
npm start
初始化本地数据 curl http://locathost:3000/blocks/sync
http://locathost:3000
ethereum的搭建参见https://github.com/dragonetail/ethereum-bootstrap


apt-get remove nodejs
apt-get remove npm
apt-get remove mongodb
