# BTC API

## 请求模板

### bitcoin-cli

在bin目录执行

bitcoin-cli  -rpcconnect=127.0.0.1 -rpcuser=123456 -rpcpassword=abcdef -rpcport=8332 getnetworkinfo

**注意：-rpcconnect=127.0.0.1 -rpcuser=123456 -rpcpassword=abcdef 这段为配置文件中的内容**

### CURL post请求

 curl --user myusername : password --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnetworkinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8332/

### post请求

{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getnetworkinfo", 
	"params": []
}

### postman工具请求

![1561715552877](https://github.com/BhpDevGroup/docs/raw/master/BTC/img/post-header.png)

第一个框为服务器地址及端口，第二个框为用户名和密码，此为配置文件bticoin.conf中设置的rpcuser和rpcpassword,然后写消息内容如图：

![1561715580139](https://github.com/BhpDevGroup/docs/raw/master/BTC/img/post-body.png)

**更多请求可参考bitcoin的文档或者第三方翻译的文档。**

**Bitcoin文档：https://bitcoin.org/en/developer-reference#remote-procedure-calls-rpcs**

**第三方文档：https://blog.csdn.net/ffzhihua/article/details/80706122**

**汇智网中文版：http://cw.hubwiz.com/card/c/bitcoin-json-rpc-api/1/4/7/**

**ChainQuery：http://chainquery.com/bitcoin-api/encryptwallet**

## API

| method                 | description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| getnewaddress          | 返回一个用于接收支付的新的比特币地址，如果调用时指定了 账户，那么该地址接收到的支付将计入该账户。调用需要节点启用钱包支持。 |
| backupwallet           | 调用可以将钱包文件wallet.data安全地拷贝到指定文件或目录。 该调用需要节点启用钱包支持。 |
| dumpprivkey            | 调用导出指定地址对应的私钥，格式为WIF。调用需要节点启用钱包支持，而且钱包解锁或未加密。 |
| dumpwallet             | 调用将钱包里的所有密钥导出到指定的文件。调用需要节点启用钱包支持，并且钱包解锁或未加密。 |
| encryptwallet          | 调用将返回一个提醒信息，提示钱包已加密、节点重启。           |
| importwallet           | 调用可以导入钱包转储文件。调用需要节点启用钱包支持。         |
| walletpassphrase       | 将钱包解密密钥存储在内存中，存储时间为指定的秒数调用需要节点启用钱包支持。 |
| walletpassphrasechange | 将钱包密码从“旧密码”更改为“新密码”。调用需要节点启用钱包支持。 |
| settxfee               | 调用设置钱包交易支付时采用的每千字节手续费率。调用需要节点启用钱包支持。 |
| sendfrom               | 从本地帐户向比特币地址转账，调用需要节点启用钱包支持。       |
| sendtoaddress          | 向指定的地址发送指定数量的比特币，调用需要节点启用钱包支持。 |
| getbalance             | 返回钱包中所有账户（或指定账户）的比特币数量，调用需要节点启用钱包支持。 |
| gettransaction         | 获取指定钱包内交易的详细信息，调用需要节点启用钱包支持。     |
| listsinceblock         | 返回指定区块之后发生的与钱包相关的所有交易，调用需要节点启用钱包支持。 |
| getnetworkinfo         | 查看网络状态                                                 |
| getpeerinfo            | 查看网络节点                                                 |
| getblockchaininfo      | 调用返回区块链的当前状态                                     |

### getnewaddress

调用返回一个用于接收支付的新的比特币地址，如果调用时指定了 账户，那么该地址接收到的支付将计入该账户。调用需要节点启用钱包支持。

#### 参数

- Account：新地址所属账户，可选，默认值：""
- AddressType：地址类型，可以是`legacy`、`p2sh-segwit`和`bech32`，可以 使用`-addresstype`设置默认地址类型

#### 返回值

返回一个用于接收支付的新的比特币地址

#### 示例代码

```
~$ bitcoin-cli -testnet getnewaddress "doc test"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getnewaddress", 
	"params": []
}
```

#### 响应

{
    "result": "344WCRrRKojgmeFacFmiCNZ4cb8QFYshNm",
    "error": null,
    "id": "1"
}

### backupwallet 

可以将钱包文件wallet.data安全地拷贝到指定文件或目录。 该调用需要节点启用钱包支持。

#### 参数

- destination：备份目录或文件名，如果是文件名，则该文件被覆盖；如果是目录， 那么该目录下将新建或覆盖wallet.data文件。

#### 返回值

成功时backupwallet调用返回null，否则返回一个错误对象。

#### 示例代码

下面的命令将节点钱包文件备份到/tmp/backup.dat文件：

```
~$ bitcoin-cli -testnet backupwallet /tmp/backup.dat
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "backupwallet", 
	"params": ["/tmp/backup.dat"]
}
```

#### 响应

{

​    "result": null,

​    "error": null,

​    "id": "1"

}

### dumpprivkey

调用导出指定地址对应的私钥，格式为WIF。该调用需要 节点启用钱包支持，而且钱包解锁或未加密。

#### 参数

- Address：一个钱包内的P2PKH地址

#### 返回值

返回指定地址对应的WIF格式的私钥。

#### 示例代码

```
~$ bitcoin-cli -testnet dumpprivkey moQR7i8XM4rSGoNwEsw3h4YEuduuP6mxw7
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "dumpprivkey", 
	"params": ["moQR7i8XM4rSGoNwEsw3h4YEuduuP6mxw7"]
}
```

#### 响应

{

​    "result": “cTVNtBK7mBi2yc9syEnwbiUpnpGJKohDWzXMeF4tGKAQ7wvomr95”,

​    "error": null,

​    "id": "1"

}

### dumpwallet

调用将钱包里的所有密钥导出到指定的文件。该调用 需要节点启用钱包，并且钱包解锁或未加密。

#### 参数

- Filename：导出文件名

#### 返回值

成功时，调用返回`null`，否则返回一个错误对象。

#### 示例代码

```
~$ bitcoin-cli -testnet dumpwallet /tmp/dump.txt
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "dumpwallet", 
	"params": ["/tmp/dump.txt"]
}
```

导出文件是文本格式的，因此可以直接查看，例如使用more查看：

```
~$ more /tmp/dump.txt
```

文件内容类似如下：

```
# Wallet dump created by Bitcoin v0.9.1.0-g026a939-beta (Tue, 8 Apr 2014 12:04:06 +0200)
# * Created on 2014-04-29T20:46:09Z
# * Best block at time of backup was 227221 (0000000026ede4c10594af8087748507fb06dcd30b8f4f48b9cc463cabc9d767),
#   mined on 2014-04-29T21:15:07Z

cTtefiUaLfXuyBXJBBywSdg8soTEkBNh9yTi1KgoHxUYxt1xZ2aA 2014-02-05T15:44:03Z label=test1 # addr=mnUbTmdAFD5EAg3348Ejmonub7JcWtrMck
cQNY9v93Gyt8KmwygFR59bDhVs3aRDkuT8pKaCBpop82TZ8ND1tH 2014-02-05T16:58:41Z reserve=1 # addr=mp4MmhTp3au21HPRz5waf6YohGumuNnsqT
cNTEPzZH9mjquFFADXe5S3BweNiHLUKD6PvEKEsHApqjX4ZddeU6 2014-02-05T16:58:41Z reserve=1 # addr=n3pdvsxveMBkktjsGJixfSbxacRUwJ9jQW
cTVNtBK7mBi2yc9syEnwbiUpnpGJKohDWzXMeF4tGKAQ7wvomr95 2014-02-05T16:58:41Z change=1 # addr=moQR7i8XM4rSGoNwEsw3h4YEuduuP6mxw7
cNCD679B4xi17jb4XeLpbRbZCbYUugptD7dCtUTfSU4KPuK2DyKT 2014-02-05T16:58:41Z reserve=1 # addr=mq8fzjxxVbAKxUGPwaSSo3C4WaUxdzfw3C
```

### encryptwallet

调用使用指定的密文加密钱包。该操作只需调用一次，一旦启用加密， 每次需要使用钱包中的密钥时，就需要输入密文。

如果在命令行使用这个调用，需要注意你使用的shell可能会保存输入的命令（包括输入 的密文）。另外，一旦钱包启用加密，目前没有其他的RPC接口可以禁用其加密。如果 需要一个不加密的钱包，你只能再创建一个新的钱包，然后使用`dumpwallet`调用的 输出来恢复加密钱包中的密钥。

#### 参数

- Passphrase：用于加密钱包的密文，最短1个字符

#### 返回值

返回一个提醒信息，提示钱包已加密、节点重启

#### 示例代码

```
~$ bitcoin-cli -testnet encryptwallet "my pass phrase"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "encryptwallet", 
	"params": ["my pass phrase"]
}
```

#### 响应

wallet encrypted; Bitcoin server stopping, restart to run with encrypted
wallet. The keypool has been flushed, you need to make a new backup.

#### walletpassphrase 

`importwallet`调用可以导入钱包转储文件（通过`dumpwallet`调用获得）。 该文件中的私钥将添加到节点钱包中。由于加入了新的私钥，该调用可能 需要重新扫描区块链。

`importwallet`调用需要节点启用钱包功能。

#### 参数

- Filename：要导入的钱包转储文件名

#### 返回值

成功时`importwallet`调用返回null。

#### 示例代码

```
~$ bitcoin-cli -testnet importwallet /tmp/dump.txt
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "importwallet", 
	"params": ["/tmp/dump.txt"]
}
```

#### 响应

{

​    "result": null,

​    "error": null,

​    "id": "1"

}

### walletpassphrase 

将钱包解密密钥存储在内存中，存储时间为指定的秒数。在钱包已解锁时发出walletpassphrase命令将设置一个新的解锁时间，该时间将覆盖旧的解锁时间。

#### 参数

- passphrase：打开钱包的密码。
- seconds ：解密密钥将自动从内存中删除的秒数，600=10分钟。

#### 返回值

null

#### 示例代码

{

​	"jsonrpc": "2.0", 

​	"id":"1", 

​	"method": "walletpassphrase", 

​	"params": ["my pass phrase", 60]

}

#### 响应

{

​    "result": null,

​    "error": null,

​    "id": "1"

}

#### walletpassphrasechange

将钱包密码从“旧密码”更改为“新密码”。调用需要节点启用钱包支持。

#### 参数

- Old Passphrase：当前密码
- New Passphrase：新密码

#### 返回值

null

#### 示例代码

{

​	"jsonrpc": "2.0", 

​	"id":"1", 

​	"method": "walletpassphrasechange", 

​	"params": ["old","new"]

}

#### settxfee

调用设置钱包交易支付时采用的每千字节手续费率。该调用需要 节点启用钱包功能。

#### 参数

- FeePerKB：每千字节的手续费

#### 返回值

成功时返回true。

#### 示例代码

下面的命令将手续费率设置为0.001btc/kb：

```
~$ bitcoin-cli -testnet settxfee 0.00100000
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "settxfee", 
	"params": [0.00100000]
}
```

#### 响应

{
    "result": true,
    "error": null,
    "id": "1"
}

#### sendfrom 

从本地帐户向比特币地址转账。该调用需要 节点启用钱包功能。

#### 参数

- from account：指定花费的账户名称。为空表示使用默认帐户
- to address：应发送到的P2PKH或P2SH地址
- amount to spend：花费金额。将确保账户有足够的比特币支付该金额（但支付的交易费用不包括在计算中，因此账户可以花费其余额加上交易费用的总和）。
- minimum confirmations：输入交易必须具有的将其输出贷记到此帐户余额的最小确认数。传出交易总是计算在内，和使用MOVE RPC进行的move交易一样。如果帐户的余额不足以支付此交易，则付款将被拒绝。使用0支出未确认的传入付款。默认值为1。
- comment：分配给此交易的本地存储（非广播）注释。默认为无注释。
- to comment：分配给此交易的本地存储（非广播）注释。用于描述付款对象。默认为无注释。

#### 返回值

返回已发送的交易ID。

#### 示例代码

```
~$ bitcoin-cli -testnet sendmany "tabby" "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.01 6 "donation" "seans outpost"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "sendmany", 
	"params": ["tabby", "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd", 0.01, 6, "donation", "seans outpost"]
}
```

#### 响应

{
    "result": "a2a2eb18cb051b5fe896a32b1cb20b179d981554b6bd7c5a956e56a0eecb04f0",
    "error": null,
    "id": "1"
}

### sendtoaddress

向指定的地址发送指定数量的比特币。该调用需要节点启用钱包功能

#### 参数

- ToAddress：接收地址
- Amount：发送的比特币数量
- Comment：备注文本
- CommentTo：备注接收人
- AutoFeeSubtract：是否自动扣除手续费，默认值：false

#### 返回值

调用返回交易ID

#### 示例代码

```
~$ bitcoin-cli -testnet sendtoaddress mmXgiR6KAhZCyQ8ndr2BCfEq1wNG2UnyG6 
  0.1 "sendtoaddress example" "Nemo From Example.com"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "sendtoaddress", 
	"params": ["mmXgiR6KAhZCyQ8ndr2BCfEq1wNG2UnyG6", 0.1, "sendtoaddress example", "Nemo From Example.com"]

}
```

#### 响应

{
    "result": "a2a2eb18cb051b5fe896a32b1cb20b179d981554b6bd7c5a956e56a0eecb04f0",
    "error": null,
    "id": "1"
}

### getbalance

返回钱包中所有账户（或指定账户）的比特币数量，该调用 需要节点启用钱包功能。

#### 参数

- Account：要查看余额的钱包账户，可选，默认值为*，表示全部账户
- Confirmations: 可计入余额的UTXO所需要的最小确认数，可选，默认值：6
- WatchOnlyIncl: 是否包含那些仅用于跟踪的地址，可选，默认值：true

#### 返回值

返回以bitcoin为单位的余额。

#### 示例代码

```
~$ bitcoin-cli -testnet getbalance "your account name" 1 true
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getbalance", 
	"params": ["*", 1, true]
}
```

####  响应

{
    "result": 0.00000001,
    "error": null,
    "id": "1"
}

### gettransaction

获取指定钱包内交易的详细信息。该调用需要节点 启用钱包功能。

#### 参数

- TXID：要查看详情的交易ID
- WatchOnlyIncl：是否包含watch-only地址

#### 返回值

返回指定ID的交易的详细信息

- amount：交易金额，正数表示该交易增加钱包余额，负数表示该交易减少钱包余额

- fee：交易手续费，仅针对转出交易

- confirmations：交易确认数，0表示未确认，-1表示存在冲突

- generated：币基交易则该值为true

- blockhash：交易所在区块的哈希

- blockindex：交易所在区块的编号

- blocktime：交易所在区块的unix时间

- txid：交易ID

- walletconflicts：冲突交易数组，成员为冲突交易的ID

- timereceived：节点收到交易的unix时间

- bip125-replacable：是否可替换交易

- comment： 保存在钱包中的交易备注，

- to：保存在钱包中的交易目标备注

- details：输入输出详情数组，成员结构如下：

  - involvesWatchonly：是否包含watch-only地址

    - account：交易影响的账户名称
    - address：对端地址
    - category：交易类别，可以是
      - send：发送交易
        - receive：接收交易
        - generate：成熟币基交易
        - immature：未成熟币基交易
        - orphan：孤儿块中的币基交易

    - amount
    - vout
    - fee
    - abandoned

- hex：串行序列化字符串

#### 示例代码

```
~$ bitcoin-cli -testnet gettransaction
  1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "gettransaction", 
	"params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"]
}
```

#### 响应

{

​    "amount" : 0.00000000,

​    "fee" : 0.00000000,

​    "confirmations" : 106670,

​    "blockhash" : "000000008b630b3aae99b6fe215548168bed92167c47a2f7ad4df41e571bcb51",

​    "blockindex" : 1,

​    "blocktime" : 1396321351,

​    "txid" : "5a7d24cd665108c66b2d56146f244932edae4e2376b561b3d396d5ae017b9589",

​    "walletconflicts" : [

​    ],

​    "time" : 1396321351,

​    "timereceived" : 1418924711,

​    "bip125-replaceable" : "no",

​    "details" : [

​        {

​            "account" : "",

​            "address" : "mjSk1Ny9spzU2fouzYgLqGUD8U41iR35QN",

​            "category" : "send",

​            "amount" : -0.10000000,

​            "vout" : 0,

​            "fee" : 0.00000000

​        },

​        {

​            "account" : "doc test",

​            "address" : "mjSk1Ny9spzU2fouzYgLqGUD8U41iR35QN",

​            "category" : "receive",

​            "amount" : 0.10000000,

​            "vout" : 0

​        }

​    ],

​    "hex" : "0100000001cde58f2e37d000eabbb60d9cf0b79ddf67cede6dba58732539983fa341dd5e6c010000006a47304402201feaf12908260f666ab369bb8753cdc12f78d0c8bdfdef997da17acff502d321022049ba0b80945a7192e631c03bafd5c6dc3c7cb35ac5c1c0ffb9e22fec86dd311c01210321eeeb46fd878ce8e62d5e0f408a0eab41d7c3a7872dc836ce360439536e423dffffffff0180969800000000001976a9142b14950b8d31620c6cc923c5408a701b1ec0a02088ac00000000"}

### listsinceblock

返回指定区块之后发生的与钱包相关的所有交易。 该调用需要节点启用钱包功能。

#### 参数

- HeaderHash：区块哈希
- TargetConfirmations：目标确认数，默认值：1
- IncludeWatchOnly：是否包含watch-only地址，默认值：false

#### 返回值

- listsinceblock调用返回一个描述对象，其结构如下：
- transactions：交易数组，成员对象的结构如下：
- involvesWatchOnly：是否包含watch-only地址
- account：账户名称
- address：地址
- category：交易类别：
- send：发送交易
- receive：接收交易
- generate：成熟币基交易
- immature：未成熟币基交易
- orphan：孤儿块币基交易
- amount：数量，负数表示支付，整数表示接收支付
- vout：交易输出序号
- fee：手续费
- confirmations：确认数
- generated：是否币基交易
- blockhash：区块哈希
- blockindex：区块序号
- blocktime：区块时间戳

- txid：交易id

- walletconflicts：冲突交易数组，成员为交易ID

- time：交易打包时间戳

- timereceived：节点接收到交易时刻的时间戳

- bip125-replaceable：是否可调换交易

- comment：备注信息

- to：目标备注信息

- lastblock： 前一个区块的哈希

#### 示例代码

```
~$ bitcoin-cli -testnet listsinceblock 
              00000000688633a503f69818a70eac281302e9189b1bb57a76a05c329fcda718 
              6 true
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "listsinceblock", 
	"params": ["0000000099c744455f58e6c6e98b671e1bf7f37346bfd4cf5d0274ad8ee660cb",6, true]
}
```

#### 响应

{

​    "transactions" : [

​        {

​            "account" : "doc test",

​            "address" : "mmXgiR6KAhZCyQ8ndr2BCfEq1wNG2UnyG6",

​            "category" : "receive",

​            "amount" : 0.10000000,

​            "vout" : 0,

​            "confirmations" : 76478,

​            "blockhash" : "000000000017c84015f254498c62a7c884a51ccd75d4dd6dbdcb6434aa3bd44d",

​            "blockindex" : 1,

​            "blocktime" : 1399294967,

​            "txid" : "85a98fdf1529f7d5156483ad020a51b7f3340e47448cf932f470b72ff01a6821",

​            "walletconflicts" : [

​            ],

​            "time" : 1399294967,

​            "timereceived" : 1418924714,

​            "bip125-replaceable": "no"        

​        },

​        {

​            "involvesWatchonly" : true,

​            "account" : "someone else's address2",

​            "address" : "n3GNqMveyvaPvUbH469vDRadqpJMPc84JA",

​            "category" : "receive",

​            "amount" : 0.00050000,

​            "vout" : 0,

​            "confirmations" : 34714,

​            "blockhash" : "00000000bd0ed80435fc9fe3269da69bb0730ebb454d0a29128a870ea1a37929",

​            "blockindex" : 11,

​            "blocktime" : 1411051649,

​            "txid" : "99845fd840ad2cc4d6f93fafb8b072d188821f55d9298772415175c456f3077d",

​            "walletconflicts" : [

​            ],

​            "time" : 1418695703,

​            "timereceived" : 1418925580,

​            "bip125-replaceable": "no"

​        }

​    ],

​    "lastblock" : "0000000000984add1a686d513e66d25686572c7276ec3e358a7e3e9f7eb88619"

}

### getnetworkinfo

查看网络状态

#### 参数

无

#### 返回值

- version：服务器版本
- subversion：服务器subversion字符串
- protocolversion：协议版本
- localservices：提供给网络的服务
- localrelay：如果从对等方请求事务中继，则为true
- timeoffset：时间偏移量
- networkactive：是否启用了p2p网络
- connections：连接数
- networks：
  - name：网络
  - limited：是使用-onlynet限制的网络
  - reachable：网络是否可达,
  - proxy：用于此网络的代理，如果没有，则为空
  - proxy_randomize_credentials：是否使用随机凭证

- relayfee：BTC / kB交易的最低中继费

- incrementalfee：mempool限制的最小费用增量或BTC / kB中的BIP 125替换
- localaddresses：
  - address：网络地址
  - port：网络端口
  - score：分数

- warnings：任何网络和区块链警告

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getnetworkinfo", 
	"params": []
}

#### 响应

{
    "result": {
        "version": 170100,
        "subversion": "/Satoshi:0.17.1/",
        "protocolversion": 70015,
        "localservices": "000000000000040d",
        "localrelay": true,
        "timeoffset": 0,
        "networkactive": true,
        "connections": 46,
        "networks": [
            {
                "name": "ipv4",
                "limited": false,
                "reachable": true,
                "proxy": "",
                "proxy_randomize_credentials": false
            },
            {
                "name": "ipv6",
                "limited": false,
                "reachable": true,
                "proxy": "",
                "proxy_randomize_credentials": false
            },
            {
                "name": "onion",
                "limited": true,
                "reachable": false,
                "proxy": "",
                "proxy_randomize_credentials": false
            }
        ],
        "relayfee": 0.00001,
        "incrementalfee": 0.00001,
        "localaddresses": [],
        "warnings": ""
    },
    "error": null,
    "id": "1"
}

### getpeerinfo

查看网络节点

#### 参数

无

#### 返回值

- id：节点索引

- addr：IP地址和端口

- addrlocal：本地地址

- addrbind：绑定的连接的地址

- services：提供的服务

- relaytxes：是否接受转发的Tx

- lastsend：自上次发送的纪元（1970年1月1日GMT）以来的秒数

- lastrecv"：自上次收到纪元（1970年1月1日GMT）以来的秒数

- bytessent：发送的总字节数

- bytesrecv：接收的总字节数

- conntime：自纪元（1970年1月1日GMT）以来的连接时间（以秒为单位）

- timeoffset：以秒为单位的时间偏移量

- pingtime:ping时间（如果可用）

- minping:：最小观察到的ping时间（如果有的话）

- version：节点版本，例如7001

- subver：字符串版本

- inbound：入站（true）或出站（false）

- addnode：连接是由于addnode / -connect还是由于是自动/入站连接

- startingheight：节点起始高度（块）

- banscore：禁令分值

- synced_headers：与节点项共有的最后一个标头

- synced_blocks：与节点共有的最后一个块         

- inflight：

  - n：目前从节点方询问的块的高度

- whitelisted：节点是否列入白名单            

- bytessent_per_msg：

  - addr：按消息类型聚合发送的总字节数

     ...

- bytesrecv_per_msg:

  - addr：按消息类型聚合接收的总字节数

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getpeerinfo", 
	"params": []
}

#### 响应

{
    "result": [
        {
            "id": 8,
            "addr": "80.84.54.26:8333",
            "addrlocal": "47.103.41.106:37576",
            "addrbind": "172.19.166.116:37576",
            "services": "000000000000000d",
            "relaytxes": true,
            "lastsend": 1562141248,
            "lastrecv": 1562141254,
            "bytessent": 149048963,
            "bytesrecv": 545472453,
            "conntime": 1560913810,
            "timeoffset": 0,
            "pingtime": 0.370666,
            "minping": 0.288554,
            "version": 70015,
            "subver": "/Satoshi:0.14.1/",
            "inbound": false,
            "addnode": false,
            "startingheight": 581362,
            "banscore": 0,
            "synced_headers": 583595,
            "synced_blocks": 583595,
            "inflight": [],
            "whitelisted": false,
            "bytessent_per_msg": {
                "addr": 472495,
                "cmpctblock": 35902,
                "feefilter": 32,
                "getaddr": 24,
                "getblocktxn": 1528,
                "getdata": 30965564,
                "getheaders": 25272,
                "headers": 134963,
                "inv": 115561672,
                "notfound": 362364,
                "ping": 326208,
                "pong": 326816,
                "reject": 208067,
                "sendcmpct": 858,
                "sendheaders": 24,
                "tx": 627024,
                "verack": 24,
                "version": 126
            },
            "bytesrecv_per_msg": {
                "addr": 274572,
                "blocktxn": 613605,
                "cmpctblock": 18917479,
                "feefilter": 32,
                "getdata": 450926,
                "getheaders": 1053,
                "headers": 95474,
                "inv": 96961078,
                "notfound": 229587,
                "ping": 326816,
                "pong": 326208,
                "reject": 2576,
                "sendcmpct": 66,
                "sendheaders": 24,
                "tx": 427272807,
                "verack": 24,
                "version": 126
            }
        },
		...
    ],
    "error": null,
    "id": "1"
}

### getblockchaininfo

调用返回区块链的当前状态

#### 参数

无

#### 返回值

- chain：区块链名称，可以是：main、test或regtest
- blocks：本地最优链中的已验证区块数量
- headers：本地最有区块头链表中的已验证区块头数量
- bestblockhash：本地最优链中最高区块的哈希
- difficulty：区块难度
- mediantime：最近区块之前的11个区块的中位时间
- verificationprogress：区块验证进度，0.0~1.0
- chainwork：
- pruned：
- pruneheight：
- softforks：软分叉描述数组，成员结构如下：
  - id：
  - version：
  - enforce：
  - status：
  - found：
  - required：
  - window：
  - reject：
  - status：
- bip9_softforks
  - name: 一个BIP39软分叉描述对象，结构如下
  - status：当前的状态，可以是以下值之一：
    - defined
    - started
    - locked_in
    - active
    - failed
  - bit：在软件版本字段中的位号
  - startTime：软分叉开始时间戳
  - timeout：超时时间戳
  - since：

#### 示例代码

```
~$ bitcoin-cli getblockchaininfo
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getblockchaininfo", 
	"params": []
}
```

#### 响应

{
    "result": {
        "chain": "main",
        "blocks": 583599,
        "headers": 583599,
        "bestblockhash": "0000000000000000000c6b5aef198549bc47712feacf45163b1426179b3212ee",
        "difficulty": 7934713219630.606,
        "mediantime": 1562140728,
        "verificationprogress": 0.9999953005655758,
        "initialblockdownload": false,
        "chainwork": "000000000000000000000000000000000000000006fb784c562c44ee28697dd0",
        "size_on_disk": 259124921649,
        "pruned": false,
        "softforks": [
            {
                "id": "bip34",
                "version": 2,
                "reject": {
                    "status": true
                }
            },
            {
                "id": "bip66",
                "version": 3,
                "reject": {
                    "status": true
                }
            },
            {
                "id": "bip65",
                "version": 4,
                "reject": {
                    "status": true
                }
            }
        ],
        "bip9_softforks": {
            "csv": {
                "status": "active",
                "startTime": 1462060800,
                "timeout": 1493596800,
                "since": 419328
            },
            "segwit": {
                "status": "active",
                "startTime": 1479168000,
                "timeout": 1510704000,
                "since": 481824
            }
        },
        "warnings": ""
    },
    "error": null,
    "id": "1"
}