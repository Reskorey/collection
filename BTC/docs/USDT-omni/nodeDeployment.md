# USDT 节点部署

## Ubuntu系统

### 节点下载

https://bintray.com/artifact/download/omni/OmniBinaries/omnicore-0.5.0-x86_64-linux-gnu.tar.gz

程序解压参考BTC节点部署，节点配置，节点启动参照本文节点配置

https://github.com/BhpDevGroup/docs/blob/master/BTC/nodeDeployment.md

### 节点配置

USDT与BTC部署到统一台机器上时，注意通过配置文件修改端口及数据文件路径以避免冲突

```
# 数据存储目录

datadir=/root/usdt/data

# 告知 Bitcoin-Qt 和 bitcoind 接受JSON-RPC命令（是否启用命令和接受RPC服务）

server=1

# 设置 gen=1 以尝试比特币挖矿

gen=0

# 后台执行（是否后台执行）

daemon=0

# 节点连接端口

port=8334

# 监听 RPC 链接,正式默认端口8333 测试默认18333（最好设置好，免得不清楚）

rpcport=8335

#RPC服务账号和密码，不设置的话是有默认密码的，本文没去深究默认，直接用自己设置的

rpcuser=123456

rpcpassword=abcdef

#允许那些IP访问RPC接口，以下写法为默认所有ip都可访问

rpcallowip=0.0.0.0/0

rpcconnect=127.0.0.1

#交易索引

txindex=1 

#手续费
paytxfee=0.0001

minrelaytxfee=0.0001
datacarriersize=80
logtimestamps=1
omnidebug=tally  
omnidebug=packets
omnidebug=pending
```

### 启动USDT节点程序

本文没有启动后台运行程序，所以建议在服务器开个tmux 启动节点程序。命令为：

```
tmux new -s 1    
```

说明：1为session 名字。

在此目录下执行启动节点程序命令：

```
正式网节点：./omnicored  -conf=/root/usdt/bitcoin.conf 
测试网节点：./omnicored  -conf=/root/usdt/bitcoin.conf  -testnet  
```

说明:-conf=/root/usdt/bitcoin.conf,此局就是说明按照此配置文件启动节点，文件路径为完完整的文件路径，上面已经说明，此路径可自定义设置，启动节点是需要写明完整路径即可。

**启动成功后就会自动更新节点数据了。注意：启动usdt节点，包括omnicore-cli 命令，rpc服务，区块数据同步。**

### USDT停止程序

./omnicore-cli  -rpcconnect=127.0.0.1 -rpcuser=用户名 -rpcpassword=密码 -rpcport=8335 stop