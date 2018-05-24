
# Ethereum Bootstrap
> 通过本文所述方法和项目中的脚本，我们可以快速的搭建好自己的私链进行开发测试。  
>仓库中包含的工具有：  
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
```shell
lsb_release -a  
No LSB modules are available.   
Distributor ID:	Ubuntu   
Description:	Ubuntu 14.04.5 LTS   
Release:	14.04   
Codename:	trusty   


cd /etc/apt
mv sources.list sources.list_bak


vim sources.list 
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


vi /etc/resolv.conf 
> nameserver 223.5.5.5

apt-get update
```

## 安装官方的ethereum安装包
```shell
apt-get install software-properties-common
add-apt-repository -y ppa:ethereum/ethereum
apt-get update
apt-get install ethereum solc
apt-get install git
apt-get install expect
```

## clone项目
```shell
cd /root
#ethereum 源项目地址：https://github.com/cryptape/ethereum-bootstrap.git
git clone https://github.com/dragonetail/ethereum-bootstrap.git
```

## 创建bootnode
```shell 
# 1.mkdir bootnode
cd ethereum-bootstrap/3.LocalNetworkCluster
cd bootnode

# 2.生成bootnode的节点Key(bootnode的节点Key)
bootnode --genkey=boot.key
ll
-rw------- 1 root root   64 May  8 06:06 boot.key   


# 4.使用genesis.json初始化bootnode的chaincode信息
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


# 5.生成了data目录，存储了chaincode的数据
ls -ltr
total 12  
-rw------- 1 root root   64 May  8 06:06 boot.key  
drwx------ 4 root root 4096 May  8 06:14 data  


# 6.启动bootnode，获取bootnode的接入地址串

nohup bootnode --nodekey=boot.key &
> INFO [05-08|06:16:30] UDP listener up                          self=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@[::]:30301  
> 
> 如果重复测试，需要删除boot.key和data/  
```



## 本机启动第一个节点：
```shell 
# 1. 创建节点路径
mkdir node1
cd node1/
root@ethereum-1:~/ethereum-bootstrap/3.LocalNetworkCluster/node1# ls -ltr
> total 0

# 2.下面使用genesis.json初始化node1的chaincode信息  
geth --datadir data --networkid 20180412  init ../genesis.json   
INFO [05-08|06:22:19] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:22:19] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/chaindata cache=16 handles=16  
INFO [05-08|06:22:19] Writing custom genesis block   
INFO [05-08|06:22:19] Persisted trie from memory database      nodes=7 size=1.31kB time=63.088µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:22:19] Successfully wrote genesis state         database=chaindata                                                                  hash=194258…f0efa3  
INFO [05-08|06:22:19] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node1/data/geth/lightchaindata cache=16 handles=16  
INFO [05-08|06:22:19] Writing custom genesis block   
INFO [05-08|06:22:19] Persisted trie from memory database      nodes=7   size=1.31kB time=408.738µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:22:19] Successfully wrote genesis state           database=lightchaindata  


# 3.启动节点，参数主要为网络ID与bootnode的一致，bootnodes的内容指向bootnode节点的地址串，指导端口  
geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console  
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
```shell
root@ethereum-2:~/ethereum-bootstrap/3.LocalNetworkCluster# mkdir node2
cd node2/

geth --datadir data --networkid 20180412 init ../genesis.json 
INFO [05-08|06:29:16] Maximum peer count                       ETH=25 LES=0 total=25  
INFO [05-08|06:29:16] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node2/data/geth/chaindata cache=16 handles=16  
INFO [05-08|06:29:16] Writing custom genesis block   
INFO [05-08|06:29:16] Persisted trie from memory database      nodes=7 size=1.31kB time=61.556µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:29:16] Successfully wrote genesis state         database=chaindata                                                                hash=194258…f0efa3  
INFO [05-08|06:29:16] Allocated cache and file handles         database=/root/ethereum-bootstrap/3.LocalNetworkCluster/node2/data/geth/lightchaindata cache=16 handles=16  
INFO [05-08|06:29:16] Writing custom genesis block   
INFO [05-08|06:29:16] Persisted trie from memory database      nodes=7 size=1.31kB time=417.029µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B  
INFO [05-08|06:29:16] Successfully wrote genesis state         database=lightchaindata     

geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console


# 验证节点是否正常接入  
# 这个时候在第一个节点或第二个节点上执行net.peerCount和admin.peers，可以看到相关节点的ID信息已经互相接入了  

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

重新启动环境
```shell
# 启动bootnode
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/bootnode
nohup bootnode --nodekey=boot.key &

# 启动节点一
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/node1

geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console 

# 启动节点二
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/node2

geth --datadir ./data --networkid 20180412 --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console

# 查看是否正常启动
> admin.peers

```


### 开发环境搭建 
1.安装nodejs、npm、mongodb
```shell
# 直接通过install安装的nodejs、mpm版本过低，通过步骤2中方法安装
apt-get install nodejs   
apt-get install npm  
apt-get install mongodb  

运行nmp install会提示需要 npm>=3,node>=6.5.0  
安装符合版本
node -v
    v8.11.1
npm -v
    5.6.0

运行npm start会提示需要不低于mongo 2（2.6）
apt-get remove nodejs
apt-get remove npm
apt-get remove mongodb
```

2.安装nodejs、npm、mongodb
```shell
# 安装nodejs和npm
wget https://npm.taobao.org/mirrors/node/v8.11.1/node-v8.11.1-linux-x64.tar.xz
tar -xvf node-v8.11.1-linux-x64.tar.xz
mv node-v8.11.1-linux-x64 /opt/
sudo ln -s /opt/node-v8.5.0-linux-x64/bin/node /usr/local/bin/node
sudo ln -s /opt/node-v8.11.1-linux-x64/bin/npm /usr/local/bin/npm
node -v
    v8.11.1
npm -v
    5.6.0


# 使用ali源安装安装mongodb 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 
echo "deb [ arch=amd64 ] http://mirrors.aliyun.com/mongodb/apt/ubuntu trusty/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list  

# 安装最新版本
apt-get install -y mongodb-org

# 安装指定版本
sudo apt-get install -y mongodb-org=3.6.4 mongodb-org-server=3.6.4 mongodb-org-shell=3.6.4 mongodb-org-mongos=3.6.4 mongodb-org-tools=3.6.4  
# 运行mongodb
# 默认端口27017 

npm install -g bower

```

3.配置服务
```shell
# 克隆项目  
git clone http://zhanghy@172.17.20.127/wangzg/ethereum-api.git    
cd ethereum-api & npm install
bower install --allow-root


# 修改配置config.js中的db和node项
vi config/config.js

  db: {
    url: 'mongodb://127.0.0.1:27017/ethtree'
  },
  node: {
    ws: "ws://192.168.0.150:8546"
  }

```

4.之前启动节点时没有启用WS-RPC服务器，重新启动节点
```shell
# 节点一
geth --datadir ./data --networkid 20180412 --ws --wsaddr 0.0.0.0  --wsport 8546 --wsorigins=* --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console 
# 节点二
geth --datadir ./data --networkid 20180412 --ws --wsaddr 0.0.0.0  --wsport 8546 --wsorigins=* --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console


root@ethereum-1:~/ethereum-bootstrap/3.LocalNetworkCluster/node1# geth attach ./data/geth.ipc 
Welcome to the Geth JavaScript console!

```

5.启动  
```shell
npm start  

express-session deprecated undefined resave option; provide resave option app.js:37:9
express-session deprecated undefined saveUninitialized option; provide saveUninitialized option app.js:37:9
mongodb opened
# Error: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by /root/ethereum-api/node_modules/scrypt/build/Release/scrypt.node)
# 解决方式参见：GLIBCXX_3.4.21 not found
```

6.初始化本地数据 
```shell
curl http://localhost:3000/blocks/sync
# 访问
http://192.168.0.150:3000/（http://locathost:3000）
```


## 操作
1.账号基本操作
```shell
# 查看算力
> eth.hashrate
0

# 查看节点Block序号
> eth.blockNumber
0

# 创建账号
> personal.newAccount("123456")
"0x585b0cc9f603b824c1344542821c162ae80d351a"

# 以太坊的一个保护机制，每隔一段时间账户就会自动锁定，这个时候任何以太币在账户之间的转换都会被拒绝，需要解锁账户
> personal.unlockAccount("0x585b0cc9f603b824c1344542821c162ae80d351a")
Unlock account 0x585b0cc9f603b824c1344542821c162ae80d351a
Passphrase: 123456
true

> eth.accounts
["0x585b0cc9f603b824c1344542821c162ae80d351a"]

# 查询账号余额
> web3.eth.getBalance(web3.eth.accounts[0])
0
> web3.eth.getBalance("0x585b0cc9f603b824c1344542821c162ae80d351a")
0
> web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
0

# balance： 地址拥有Wei的数量。1Ether=10^18Wei
```

2.挖矿
```shell
> miner.start(1)
INFO [05-09|06:13:18] Updated mining threads                   threads=1
INFO [05-09|06:13:18] Transaction pool price threshold updated price=18000000000
INFO [05-09|06:13:18] Etherbase automatically configured       address=0x585b0cC9F603b824c1344542821c162AE80d351A
INFO [05-09|06:13:18] Starting mining operation 
null
> INFO [05-09|06:13:18] Commit new mining work                   number=1 txs=0 uncles=0 elapsed=455.73µs
INFO [05-09|06:13:22] Generating DAG in progress               epoch=0 percentage=0 elapsed=1.964s
INFO [05-09|06:13:24] Generating DAG in progress               epoch=0 percentage=1 elapsed=4.020s
INFO [05-09|06:13:26] Generating DAG in progress               epoch=0 percentage=2 elapsed=5.985s
INFO [05-09|06:13:28] Generating DAG in progress               epoch=0 percentage=3 elapsed=7.953s
...
INFO [05-09|06:16:41] Generating DAG in progress               epoch=0 percentage=99 elapsed=3m21.759s
INFO [05-09|06:16:41] Generated ethash verification cache      epoch=0 elapsed=3m21.764s
INFO [05-09|06:16:44] Successfully sealed new block            number=1 hash=2b1e2f…4d60f7
INFO [05-09|06:16:44] 🔨 mined potential block                  number=1 hash=2b1e2f…4d60f7
INFO [05-09|06:16:44] Commit new mining work                   number=2 txs=0 uncles=0 elapsed=301.198µs
INFO [05-09|06:16:44] Successfully sealed new block            number=2 hash=1667f9…dbf770
INFO [05-09|06:16:44] 🔨 mined potential block                  number=2 hash=1667f9…dbf770
INFO [05-09|06:16:44] Commit new mining work                   number=3 txs=0 uncles=0 elapsed=215.381µs
...
INFO [05-09|06:18:37] Commit new mining work                   number=26 txs=0 uncles=0 elapsed=163.553µs
> miner.stop()
true

```

在其他节点上能够看到Block的同步信息
```shell
> INFO [05-09|06:16:40] Block synchronisation started 
INFO [05-09|06:16:40] Imported new state entries               count=2 elapsed=68.63µs processed=2 pending=0 retry=0 duplicate=0 unexpected=0
WARN [05-09|06:16:41] Discarded bad propagated block           number=1 hash=2b1e2f…4d60f7
INFO [05-09|06:16:41] Imported new block headers               count=2 elapsed=1.076s  number=2 hash=1667f9…dbf770 ignored=0
INFO [05-09|06:16:41] Imported new chain segment               blocks=2 txs=0 mgas=0.000 elapsed=775.448µs mgasps=0.000 number=2 hash=1667f9…dbf770 cache=768.00B
INFO [05-09|06:16:41] Mining too far in the future             wait=4s
INFO [05-09|06:16:41] Fast sync complete, auto disabling 
INFO [05-09|06:16:46] Imported new chain segment               blocks=1 txs=0 mgas=0.000 elapsed=7.662ms   mgasps=0.000 number=3 hash=924e22…8a20c7 cache=1.13kB
INFO [05-09|06:16:48] Imported new chain segment               blocks=1 txs=0 mgas=0.000 elapsed=4.929ms   mgasps=0.000 number=4 hash=b9c945…330684 cache=1.49kB
INFO [05-09|06:16:48] Mining too far in the future             wait=4s
INFO [05-09|06:16:54] Imported new chain segment               blocks=1 txs=0 mgas=0.000 elapsed=5.165ms   mgasps=0.000 number=5 hash=0332cb…864c21 cache=1.84kB
INFO [05-09|06:16:56] Imported new chain segment               blocks=1 txs=0 mgas=0.000 elapsed=11.614ms  mgasps=0.000 number=6 hash=08ef2d…183923 cache=2.20kB
```

查看eth.blockNumber
```shell
> eth.blockNumber
25

> web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
> # blockNumber * 5
125

```

3.转账
```shell
# 注：在任意节点上执行转账操作，然后在任意节点启动挖矿，可以完成转账交易的最终记账
> personal.listAccounts
["0x585b0cc9f603b824c1344542821c162ae80d351a"]

> eth.sendTransaction({from:"0x585b0cc9f603b824c1344542821c162ae80d351a",to:"0XADE59D945A323E56664D530613D3725A7267EBA4",value:web3.toWei(11,"ether")})
"0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951"
> txpool.content
{
  pending: {
    0x585b0cC9F603b824c1344542821c162AE80d351A: {
      0: {
        blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
        blockNumber: null,
        from: "0x585b0cc9f603b824c1344542821c162ae80d351a",
        gas: "0x15f90",
        gasPrice: "0x430e23400",
        hash: "0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951",
        input: "0x",
        nonce: "0x0",
        r: "0x25f2ea28a72a96f84748f3968a1328cf0a5baf78726c967d6f8ba1ffb7c76639",
        s: "0x4e1d85a59686cded94b2c63e9d35f468b6178b3d6b843ff3e7d1bae28413176e",
        to: "0xade59d945a323e56664d530613d3725a7267eba4",
        transactionIndex: "0x0",
        v: "0x42",
        value: "0x8ac7230489e80000"
      }
    }
  },
  queued: {}
}
> txpool.inspect
{
  pending: {
    0x585b0cC9F603b824c1344542821c162AE80d351A: {
      0: "0xAdE59D945A323e56664D530613D3725a7267EBA4: 10000000000000000000 wei + 90000 gas × 18000000000 wei"
    }
  },
  queued: {}
}

> eth.getTransaction("0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951")
{
  blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
  blockNumber: null,
  from: "0x585b0cc9f603b824c1344542821c162ae80d351a",
  gas: 90000,
  gasPrice: 18000000000,
  hash: "0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951",
  input: "0x",
  nonce: 0,
  r: "0x25f2ea28a72a96f84748f3968a1328cf0a5baf78726c967d6f8ba1ffb7c76639",
  s: "0x4e1d85a59686cded94b2c63e9d35f468b6178b3d6b843ff3e7d1bae28413176e",
  to: "0xade59d945a323e56664d530613d3725a7267eba4",
  transactionIndex: 0,
  v: "0x42",
  value: 10000000000000000000
}

> eth.getTransactionReceipt("0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951")
null
> eth.getRawTransaction("0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951")
# "0xf86d80850430e2340083015f9094ade59d945a323e56664d530613d3725a7267eba4888ac7230489e800008042a025f2ea28a72a96f84748f3968a1328cf0a5baf78726c967d6f8ba1ffb7c76639a04e1d85a59686cded94b2c63e9d35f468b6178b3d6b843ff3e7d1bae28413176e

> txpool.status
{
  pending: 1,
  queued: 0
}

# 开启挖矿，交易成功
> miner.start(1)
null
> miner.stop()
true
> txpool.status
{
  pending: 0,
  queued: 0
}

> eth.getTransaction("0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951")
{
  blockHash: "0x292db8d6cf436171c8a2478cf12482ffcd570de1e28fa7ecce6b15d2c5e12dc1",
  blockNumber: 26,
  from: "0x585b0cc9f603b824c1344542821c162ae80d351a",
  gas: 90000,
  gasPrice: 18000000000,
  hash: "0xa5ba027c636b422710bc7dabc61adb2983b82aae379568c59d730023bd8f5951",
  input: "0x",
  nonce: 0,
  r: "0x25f2ea28a72a96f84748f3968a1328cf0a5baf78726c967d6f8ba1ffb7c76639",
  s: "0x4e1d85a59686cded94b2c63e9d35f468b6178b3d6b843ff3e7d1bae28413176e",
  to: "0xade59d945a323e56664d530613d3725a7267eba4",
  transactionIndex: 0,
  v: "0x42",
  value: 10000000000000000000
}

# 再查看eth.blockNumber （原来是25）
> eth.blockNumber
29

# 查看账号的金额（原来是125）
> web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
135
```


节点启动，进入console
```shell 
# bootnode
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/bootnode
nohup bootnode --nodekey=boot.key &

# 节点一
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/node1/

geth --datadir ./data --networkid 20180412 --ws --wsaddr 0.0.0.0  --wsport 8546 --wsorigins=* --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console 

# 节点二
cd /root/ethereum-bootstrap/3.LocalNetworkCluster/node2/

geth --datadir ./data --networkid 20180412 --ws --wsaddr 0.0.0.0  --wsport 8546 --wsorigins=* --bootnodes=enode://912a150237986ed3df6998fe516b1ae7e82f729546c695ace3aa172f5276ca6dffffaf9125eaa7c8b1adea60f002bd7df2d15d021d1678657663af46606c06d6@192.168.0.150:30301 --port 30303 console
```


## 智能合约开发
```shell
# 智能合约开发语言solidity
# 编译solidity程序需要对应的编译器，安装solc
cd ethereum-api
npm install -g solc
/opt/node-v8.11.1-linux-x64/bin/solcjs -> /opt/node-v8.11.1-linux-x64/lib/node_modules/solc/solcjs
+ solc@0.4.24
added 66 packages in 22.068s

# 使用solc编译合约Coin.sol(页面上的编译既调用了solc)
cd /root/ethereum-api/public/solidity
solc --abi --bin testContract.sol 
```

## 一些参考链接

```shell
## 以太坊
# 以太坊的工作原理
https://blog.csdn.net/itchosen/article/details/78655903

# 以太坊的账户和基本单位
https://blog.csdn.net/ddffr/article/details/77007353

# 以太坊源码P2P网络及节点发现机制
https://blog.csdn.net/DDFFR/article/details/78919530

# 以太坊数据存储分析
https://blog.csdn.net/ddffr/article/details/77500513

# web3 api
https://web3js.readthedocs.io/en/1.0/web3-eth-subscribe.html

## 以太坊的一些原理（智能合约）
https://www.94eth.com/tutorial/shen-ru-qian-chu-yi-tai-fang-01-qu-kuai-lian

# 智能合约remix IDE 
https://remix.ethereum.org/#version=soljson-v0.4.15+commit.bbb8e64f.js

## 开发
# angularjs chart C3
https://amsterdam.luminis.eu/2015/01/01/c3js-directives-for-angularjs/


```
Address Growth Chart
Block Difficulty Growth Chart
Total Daily GasUsed Chart
Average GasLimit Chart