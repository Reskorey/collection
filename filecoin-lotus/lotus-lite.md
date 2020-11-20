**lotus 轻节点搭建**

 

1.要求：两台服务器，一台作为全节点服务器（不需要钱包），另一台作为轻节点服务器（部署钱包）。

 

2.在全节点服务器上搭建一个全节点，全节点上不部署钱包，仅仅作为一个同步区块的节点。

启动lotus 程序（建议使用v1.1.2版本程序，v1.1.1增加lite-mode - market storage and retrieval clients）

配置为：全节点内网ip为：172.19.166.130， 外网ip：47.103.51.105



  ListenAddress = "/ip4/172.19.166.130/tcp/5005/http"
  RemoteListenAddress = ""
  Timeout = "30s"



全节点可将访问白名单设置为所有内部的轻节点及内部服务，不需要对第三方公开。

 

3.在另一台服务器搭建一个轻节点，节点搭建参照全节点搭建，只是启动的时候通过轻节点的方式启动即可。轻节点部署钱包，不对外公开，充币程序访问轻节点钱包即可。

启动命令为：

FULLNODE_API_INFO=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJyZWFkIiwid3JpdGUiLCJzaWduIiwiYWRtaW4iXX0.fzpHtg9VFX1K8s5vbyrHpGoWYEcJESybHziADoLw5Wc:/ip4/47.103.51.105/tcp/5005/http lotus daemon --lite

若要实现多个轻节点，只需要按照同样的方式连接到全节点即可。

 

 

4.注意：在轻节点访问本地轻节点钱包是不需要加入全节点参数FULLNODE_API_INFO，否则即为通过轻节点访问了全节点的RPC、钱包等。

例如：查询轻节点钱包使用命令 lotus wallet list 

如果要通过轻节点访问全节点钱包可在命令以前加入FULLNODE_API_INFO参数，例如：FULLNODE_API_INFO=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJyZWFkIiwid3JpdGUiLCJzaWduIiwiYWRtaW4iXX0.fzpHtg9VFX1K8s5vbyrHpGoWYEcJESybHziADoLw5Wc:/ip4/47.103.51.105/tcp/5005/http lotus wallet list

 

