# USDT API

## 请求模板

### omnicore-cli

在bin目录执行

./omnicore-cli -rpcconnect=127.0.0.1 -rpcuser=bhp -rpcpassword=123456 -rpcport=8335  getblockcount

**注意：-rpcconnect=127.0.0.1 -rpcuser=123456 -rpcpassword=abcdef 这段为配置文件中的内容**

### post请求

{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "getblockcount", 
	"params": []
}

### postman工具请求

![1561715552877](https://github.com/BhpDevGroup/docs/raw/master/BTC/img/post-header.png)

第一个框为服务器地址及端口，第二个框为用户名和密码，此为配置文件bticoin.conf中设置的rpcuser和rpcpassword,然后写消息内容如图：

![1561715580139](https://github.com/BhpDevGroup/docs/raw/master/BTC/img/post-body.png)

**支持原有的BTC接口，可参考：https://github.com/BhpDevGroup/docs/blob/master/BTC/api.md**

**第三方文档：http://cw.hubwiz.com/card/c/omni-rpc-api/1/1/1/**

## API

| method                | description                                    |
| --------------------- | ---------------------------------------------- |
| omni_sendsto          | 调用创建并广播一个发送给属主的交易             |
| omni_funded_send      | 调用创建并发送一个简单充值交易                 |
| omni_sendall          | 调用将指定生态系统中的所有可用代币发送给接收方 |
| omni_getbalance       | 返回指定地址和资产的代币余额                   |
| omni_gettransaction   | 获取指定Omni交易的详细信息                     |
| omni_listtransactions | 返回钱包交易清单，可以使用地址或区块进行过滤   |

### omni_sendsto

调用创建并广播一个发送给属主的交易。

#### 参数

- fromaddress：发送地址，字符串，必需
- propertyid：代币ID，数值，必需
- amount：分发数量，字符串，必需
- redeemaddress：赎回地址，字符串，可选，默认值：发送地址
- distributionproperty：资产持有人ID，数值，可选

#### 返回值

调用返回16进制编码的交易哈希。

#### 示例代码

```
$ omnicore-cli "omni_sendsto" 
    "32Z3tJccZuqQZ4PhJR2hxHC3tjgjA8cbqz" "37FaKponF7zqoMLUjEiko25pDiuVH5YLEa" 3 "5000"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_sendsto", 
	"params": ["32Z3tJccZuqQZ4PhJR2hxHC3tjgjA8cbqz", "37FaKponF7zqoMLUjEiko25pDiuVH5YLEa", 3, "5000"]
}
```

#### 响应

{
    "result": "62577d10a02249db698509cf3d4b060268771b085e3d3fc1e27aeec1cc460d31",
    "error": null,
    "id": "1"
}

### omni_funded_send

调用创建并发送一个简单充值交易。

发送方发出的所有比特币都将被消耗掉，如果比特币来自指定的手续费 来源，那么找零将返回该来源地址。

#### 参数

- fromaddress：发送地址，字符串，必需
- toaddress：接收地址，字符串，必需
- propertyid：代币ID，数值，必需
- amount：代币数量，字符串，必需
- feeaddress：支付手续费的地址，字符串，必需

#### 返回值

调用返回16进制编码的交易哈希。

#### 示例代码

```
$ omnicore-cli "omni_funded_send" "1DFa5bT6KMEr6ta29QJouainsjaNBsJQhH" 
    "15cWrfuvMxyxGst2FisrQcvcpF48x6sXoH" 1 "100.0" 
    "15Jhzz4omEXEyFKbdcccJwuVPea5LqsKM1"
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_funded_send", 
	"params": ["1DFa5bT6KMEr6ta29QJouainsjaNBsJQhH", "15cWrfuvMxyxGst2FisrQcvcpF48x6sXoH", 1, "100.0", "15Jhzz4omEXEyFKbdcccJwuVPea5LqsKM1"]
}
```

#### 响应

{
    "result": "62577d10a02249db698509cf3d4b060268771b085e3d3fc1e27aeec1cc460d31",
    "error": null,
    "id": "1"
}

### omni_sendall

调用将指定生态系统中的所有可用代币发送给接收方。

#### 参数

- fromaddress：发送方地址，字符串，必需
- toaddress：接收地址，字符串，必需
- ecosystem：代币生态系统，数值，1 - 主生态，2 - 测试生态，必需
- redeemaddress：赎回地址，字符串，可选
- referenceamount：要发送给接收方的比特币数量，字符串，可选

#### 返回值

调用返回16进制编码的交易哈希。

#### 示例代码

```
$ omnicore-cli "omni_sendall" "3M9qvHKtgARhqcMtM5cRT9VaiDJ5PSfQGY" "37FaKponF7zqoMLUjEiko25pDiuVH5YLEa" 2
```

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_sendall", 
	"params": ["3M9qvHKtgARhqcMtM5cRT9VaiDJ5PSfQGY", "37FaKponF7zqoMLUjEiko25pDiuVH5YLEa",2]
}
```

#### 响应

{
    "result": "62577d10a02249db698509cf3d4b060268771b085e3d3fc1e27aeec1cc460d31",
    "error": null,
    "id": "1"
}

### omni_getbalance

返回指定地址和资产的代币余额。

#### 参数

- address：地址，字符串，必需
- propertyid：资产ID，数值，必需

#### 返回值

- balance：地址的可用余额
- reserved：保留金额（the amount reserved by sell offers and accepts）
- frozen：发行人冻结的金额（仅适用于托管属性）

#### 示例代码

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_getbalance", 
	"params": ["mqpb7ucc41Z9iap7VJsdcKzLji5U2Gd6Fd",2]
}
```

#### 响应

{
    "result": {
        "balance": "10.00000000",
        "reserved": "0.00000000",
        "frozen": "0.00000000"
    },
    "error": null,
    "id": "1"
}

### omni_gettransaction

获取指定Omni交易的详细信息

#### 参数

- txid：交易哈希，字符串，必需

#### 返回值

- txid：16进制编码的交易哈希
- sendingaddress：发送方比特币地址
- referenceaddress：接收方的比特币地址
- ismine：交易是否与钱包内某个地址相关
- confirmations：交易的确认数
- fee：交易手续费
- blocktime： 包含交易的区块的时间戳
- valid：交易是否有效
- positioninblock：交易在区块内的序号
- version：交易版本
- type_int：交易类型代码
- type：交易类型字符串
-  [...] ：其他交易类型相关的属性

#### 示例代码

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_gettransaction", 
	"params": ["62577d10a02249db698509cf3d4b060268771b085e3d3fc1e27aeec1cc460d31"]
}
```

#### 响应

{
    "result": {
        "txid": "62577d10a02249db698509cf3d4b060268771b085e3d3fc1e27aeec1cc460d31",
        "fee": "0.00135538",
        "sendingaddress": "1zgmvYi5x1wy3hUh7AjKgpcVgpA8Lj9FA",
        "referenceaddress": "1NpqmMzYgk5NJ9mBNt24jKAgHGduqdfauG",
        "ismine": false,
        "version": 0,
        "type_int": 0,
        "type": "Simple Send",
        "propertyid": 31,
        "divisible": true,
        "amount": "707.21300000",
        "valid": true,
        "blockhash": "0000000000000000002062b26a7da85f426041a2d9ed91fed29948e60bc927e4",
        "blocktime": 1561963774,
        "positioninblock": 15,
        "block": 583276,
        "confirmations": 1
    },
    "error": null,
    "id": "1"
}

### omni_listtransactions

返回钱包交易清单，可以使用地址或区块进行过滤

#### 参数

- txid：地址过滤器，字符串，可选
- count：返回结果数量，数值，可选，默认值：10
- skip：跳过结果数量，数值，可选，默认值：0
- startblock：检索的起始区块，数值，可选，默认值：0
- endblock ：检索的最后区块，数值，可选，默认值：999999999

#### 返回值

- txid：交易的十六进制编码哈希
- sendingaddress：发送方的比特币地址
- referenceaddress：接收方的比特币地址（如有）
- ismine：交易是否与钱包内某个地址相关
- propertyid：代币编号
- divisible：是否可见
- confirmations：交易的确认数
- fee：交易手续费
-  amount：转账金额
- valid：交易是否有效
- blockhash：区块hash
- blocktime： 包含交易的区块的时间戳
- positioninblock：交易在区块内的序号
- version：交易版本
- type_int：交易类型代码
- type：交易类型字符串
-  [...] ：其他交易类型相关的属性

#### 示例代码

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_listtransactions", 
	"params": []
}
```

#### 响应

{
    "result": [
        {
            "txid": "6fd82d82e6f3d8f50ed8462c0b1788a01c3bc22de5e5999f3ae15afef7e229dc",
            "fee": "0.00093427",
            "sendingaddress": "mpd8ojfCCahVGrptjGPkWpDDb6FEQDGUyy",
            "referenceaddress": "mqpb7ucc41Z9iap7VJsdcKzLji5U2Gd6Fd",
            "ismine": true,
            "version": 0,
            "type_int": 0,
            "type": "Simple Send",
            "propertyid": 1,
            "divisible": true,
            "amount": "5.00000000",
            "valid": true,
            "blockhash": "000000000000001537ba0f69e90715ca72f6c64538ec2262f63b5b53f9e23717",
            "blocktime": 1561967808,
            "positioninblock": 22,
            "block": 1566906,
            "confirmations": 481
        },

​		...

​    ],
​    "error": null,
​    "id": "1"
}

### omni_funded_send

omni_funded_send调用创建并发送一个简单充值交易。  发送方发出的所有比特币都将被消耗掉，如果比特币来自指定的手续费 来源，那么找零将返回该来源地址。

#### 参数

- fromaddress：发送地址，字符串，必需
-  toaddress：接收地址，字符串，必需
-  propertyid：代币ID，数值，必需 amount：代币数量，字符串，
- 必需 feeaddress：支付手续费的地址，字符串，必需

#### 返回值

- `omni_funded_send`调用返回16进制编码的交易哈希

#### 示例代码

```
{
	"jsonrpc": "2.0", 
	"id":"1", 
	"method": "omni_getbalance", 
	"params": ["1DFa5bT6KMEr6ta29QJouainsjaNBsJQhH"， 
    "15cWrfuvMxyxGst2FisrQcvcpF48x6sXoH"， 31 ，"100.0" 
    "15Jhzz4omEXEyFKbdcccJwuVPea5LqsKM1"]
}
```

#### 响应

{
    "result": "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d",
    "error": null,
    "id": "1"
}



