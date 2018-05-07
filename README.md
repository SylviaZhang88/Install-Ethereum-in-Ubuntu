
## 环境说明
`系统版本： Ubuntu 14.04    
项目地址： https://github.com/janx/ethereum-bootstrap
`

## 配置阿里源
`参考： https://www.cnblogs.com/lyon2014/p/4715379.html`

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

## 安装geth
1.apt-get install software-properties-common
2.add-apt-repository -y ppa:ethereum/ethereum
3.apt-get update
4.apt-get install ethereum solc



