
## 环境说明
`系统版本： Ubuntu 14.04`

## 配置阿里源
`参考： https://www.cnblogs.com/lyon2014/p/4715379.html`

lsb_release -a  
> No LSB modules are available.   
Distributor ID:	Ubuntu   
Description:	Ubuntu 14.04.5 LTS   
Release:	14.04   
Codename:	trusty   

cd /etc/apt
mv sources.list sources.list_bak

 vi sources.list
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

apt-get update

## 