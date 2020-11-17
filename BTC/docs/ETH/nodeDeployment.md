# ETH 节点部署

## Ubuntu系统

### 下载节点方式

节点地址：https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.8.27-4bcc0a37.tar.gz

### 下载源码方式

#### 安装环境

1. 更新系统：apt-get update

2. 安装git：apt-get install git

3. 安装curl：apt-get install curl

4. 安装Go：

   - 下载：curl -O https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz
   - 解压：tar -C /root/eth -xzf go1.12.6.linux-amd64.tar.gz（PS：路径 “/root/eth” 可以修改为自己的路径）
   - 配置环境变量：
     - mkdir -p ~/go; echo "export GOPATH=$HOME/go" >> ~/.bashrc（PS：进入目录）
     - echo "export PATH=$PATH:/root/eth/go/bin" >> ~/.bashrc（PS：路径 “:/root/eth/go/bin” 为 ”:/自己的安装目录/go/bin“）
     - source ~/.bashrc（PS：立即生效）

   - 查看环境变量：新开一个连接查看env
   - 查看Go版本：新开一个连接查看go version
   - 安装go和c编译器：sudo apt-get install -y build-essential

#### 下载ETH项目

git clone https://github.com/ethereum/go-ethereum

#### 编译ETH源码

- 进入源码目录： cd /root/eth/go-ethereum
- 编译源码：make geth

#### 其他操作

- 修改环境变量：vi ~/.bashrc
- 卸载go语言：apt-get purge golang-go

