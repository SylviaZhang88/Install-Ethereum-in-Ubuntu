
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
> No LSB modules are available.   
Distributor ID:	Ubuntu   
Description:	Ubuntu 14.04.5 LTS   
Release:	14.04   
Codename:	trusty   

2.cd /etc/apt
3.mv sources.list sources.list_bak

 4.vi sources.list
 >deb http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe  
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe  
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe  

5.vi /etc/resolv.conf 
> nameserver 223.5.5.5

6.apt-get update
  
## 安装官方的ethereum安装包
1.apt-get install software-properties-common
2.add-apt-repository -y ppa:ethereum/ethereum
3.apt-get update
4.apt-get install ethereum solc
apt-get install git
apt-get install expect


cd /root
# git clone https://github.com/cryptape/ethereum-bootstrap.git
git clone https://github.com/dragonetail/ethereum-bootstrap.git
git clone http://zhanghy@172.17.20.127/wangzg/ethereum-api.git

cd ethereum-bootstrap/3.LocalNetworkCluster