# ETH API

## 请求模板

### curl

curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}' -H 'content-type: application/json;' [http://127.0.0.1:8545/](http://127.0.0.1:8332/)

### post请求

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_accounts", 
	"params": [] 
}

### Geth控制台

eth.accounts

## API

| method                    | description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| personal_newAccount       | 返回新账号的以太坊地址                                       |
| eth_accounts              | 返回客户端持有的地址列表                                     |
| personal_listAccounts     | 返回密钥库中所有密钥对应的以太坊账户地址                     |
| net_peerCount             | 返回当前客户端所连接的对端节点数量                           |
| eth_syncing               | 对于已经同步的客户端，该调用返回一个描述同步状态的对象；对于未同步客户端，返回false |
| eth_blockNumber           | 返回最新块的编号。同步完成前，返回0                          |
| eth_getBlockByNumber      | 返回指定编号的块                                             |
| eth_sendTransaction       | 创建一个新的消息调用交易，如果数据字段中包含代码，则创建一个合约 |
| personal_sendTransaction  | 创建一个新的消息调用交易，如果数据字段中包含代码，则创建一个合约 |
| eth_getTransactionByHash  | 返回指定哈希对应的交易                                       |
| eth_getTransactionReceipt | 返回指定交易的收据，使用哈希指定交易，挂起的交易其收据无效。可以获取到合约地址 |
| eth_getBalance            | 返回指定地址账户的余额。                                     |
| eth_call                  | 立刻执行一个新的消息调用，无需在区块链上创建交易。调用合约方法 |

### personal_newAccount

生成一个新的私钥并直接存入密钥库目录。密钥文件使用指定的密钥 加密。

返回新账号的以太坊地址。

#### 参数

密码字符串

#### 返回值

返回新账号的以太坊地址

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "personal_newAccount", 
	"params": [""] 
}

#### 响应

{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x3d80b31a78c30fc628f20b2c89d7ddbf6e53cedc"
}

### eth_accounts

返回客户端持有的地址列表

#### 参数

无

#### 返回值

客户端持有的地址字符串列表

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_accounts", 
	"params": [] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [  
        "0xa4aa2bd2c5c957d4e1fca45b1b638ea308d80688"
    ]
}

### personal_listAccounts

返回密钥库中所有密钥对应的以太坊账户地址。

#### 参数

无

#### 返回值

密钥库中所有密钥对应的以太坊账户地址

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "personal_listAccounts", 
	"params": [] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [  
        "0xa4aa2bd2c5c957d4e1fca45b1b638ea308d80688"
    ]
}

### net_peerCount

返回当前客户端所连接的对端节点数量

#### 参数

无

#### 返回值

QUANTITY - 整数，所连接对端节点旳数量

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "net_peerCount", 
	"params": [] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x32"
}

### eth_syncing

对于已经同步的客户端，该调用返回一个描述同步状态的对象；对于未同步客户端，返回false。

#### 参数

DATA, 32 字节 - 交易哈希

params: [
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
]

#### 返回值

Object|Boolean, 同步状态对象或false。同步对象的结构如下：

- startingBlock：开始同步的起始区块编号； 
- currentBlock：当前正在导入的区块编号； 
- highestBlock：通过所链接的节点获得的当前最高的区块高度； 
- pulledStates：当前已经拉取的状态条目数； 
- knownStates：当前已知的待拉取的总状态条目数；

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_syncing", 
	"params": [] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "currentBlock": "0x7ac2e8",
        "highestBlock": "0x7ac34a",
        "knownStates": "0x13b14b0c",
        "pulledStates": "0x13b134bb",
        "startingBlock": "0x7a1274"
    }
}

### **eth_blockNumber**

返回最新块的编号。

#### **参数**

无

#### **返回值**

节点当前块编号

#### **示例代码**

{

​	"jsonrpc": "2.0", 

​	"id":1, 

​	"method": "eth_blockNumber", 

​	"params": [] 

}

#### **响应**

{

​    "jsonrpc": "2.0",

​    "id": 1,

​    "result": "0x5b9ac5"

}

### eth_getBlockByNumber

返回指定编号的块。

#### 参数

QUANTITY|TAG - 整数块编号，或字符串"earliest"、"latest" 或"pending"
Boolean - 为true时返回完整的交易对象，否则仅返回交易哈希
params: [
   '0x1b4', // 436
   true
]

#### 返回值

Object - 匹配的块对象，如果未找到块则返回null，结构如下：

- number: QUANTITY - 块编号，挂起块为null
- hash: DATA, 32 Bytes - 块哈希，挂起块为null
- parentHash: DATA, 32 Bytes - 父块的哈希
- nonce: DATA, 8 Bytes - 生成的pow哈希，挂起块为null
- sha3Uncles: DATA, 32 Bytes - 块中叔伯数据的SHA3哈希
- logsBloom: DATA, 256 Bytes - 快日志的bloom过滤器，挂起块为null
- transactionsRoot: DATA, 32 Bytes - 块中的交易树根节点
- stateRoot: DATA, 32 Bytes - 块最终状态树的根节点
- receiptsRoot: DATA, 32 Bytes - 块交易收据树的根节点
- miner: DATA, 20 Bytes - 挖矿奖励的接收账户
- difficulty: QUANTITY - 块难度，整数
- totalDifficulty: QUANTITY - 截止到本块的链上总难度
- extraData: DATA - 块额外数据
- size: QUANTITY - 本块字节数
- gasLimit: QUANTITY - 本块允许的最大gas用量
- gasUsed: QUANTITY - 本块中所有交易使用的总gas用量
- timestamp: QUANTITY - 块时间戳
- transactions: Array - 交易对象数组，或32字节长的交易哈希数组
- uncles: Array - 叔伯哈希数组

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_getBlockByNumber", 
	"params": ["0x7a9069",true] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "difficulty": "0x7f513fbddfc21",
        "extraData": "0x505059452d65746865726d696e652d61736961312d36",
        "gasLimit": "0x7a1200",
        "gasUsed": "0x0",
        "hash": "0x53d1c1988d37709bbfdd072b77ad365e86b148720dd8ece75d86ceae8db5b127",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0xea674fdde714fd979de3edf0f56aa9716b898ec8",
        "mixHash": "0x9fda3bf71ca5798ab0d3fa5ec05f055b25766f296446551bb4bfeb6678c4662f",
        "nonce": "0xa550ffc3ecd7d909",
        "number": "0x7a9070",
        "parentHash": "0xa151e01ed632fce674535e28f3445caf54f2cdf4d6d4264b80fcdff0e0c268a0",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x21c",
        "stateRoot": "0x73dff36e3482666241a12b425998eff78de58b3377e2990c90f56d8c5fb6e349",
        "timestamp": "0x5d132269",
        "totalDifficulty": "0x2472687a2308b04fd7c",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}

### eth_sendTransaction

创建一个新的消息调用交易，如果数据字段中包含代码，则创建一个合约。

#### 参数

- from: DATA, 20字节 - 发送交易的源地址
- to: DATA, 20字节 - 交易的目标地址，当创建新合约时可选
- gas: QUANTITY - 交易执行可用gas量，可选整数，默认值90000，未用gas将返还。
- gasPrice: QUANTITY - gas价格，可选，默认值：待定(To-Be-Determined)
- value: QUANTITY - 交易发送的金额，可选整数
- data: DATA - 合约的编译带啊或被调用方法的签名及编码参数
- nonce: QUANTITY - nonce，可选。可以使用同一个nonce来实现挂起的交易的重写

#### 返回值

DATA, 32字节 - 交易哈希，如果交易还未生效则返回0值哈希

当创建合约时，在交易生效后，使用eth_getTransactionReceipt调用获取合约地址。

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_sendTransaction", 
	"params": [{
		"nonce":"0x15",
		"from": "0xb6cd75af6594f46374378cf3a7d9cbfc06485994",
		"to": "0x60be0313411e34e8e2ec7094b27a291d827d9b9c",
		"gas": "0xea60", 
		"gasPrice": "0x3b9aca00", 
		"value": "0x0", 
		"data": "0xa9059cbb000000000000000000000000696d69b81c6bdf6d46ddb66ee2175df7f9de7c4600000000000000000000000000000000000000000000000ad78ebc5ac6200000"
	}] 
}

#### 响应

{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}

### personal_sendTransaction

sendTransaction方法验证指定的密码并提交交易，该方法的交易参数 与eth_sendTransaction一样，同时包含from账户地址。

如果密码可以成功解密交易中from地址对应的私钥，那么该方法将验证交易、 签名并广播到以太坊网络中。

由于在sendTransaction方法调用时，from账户并未在节点中全局解锁 （仅在该调用内解锁），因此from账户不能用于其他RPC调用。

#### 参数

- from: DATA, 20字节 - 发送交易的源地址
- to: DATA, 20字节 - 交易的目标地址，当创建新合约时可选
- gas: QUANTITY - 交易执行可用gas量，可选整数，默认值90000，未用gas将返还。
- gasPrice: QUANTITY - gas价格，可选，默认值：待定(To-Be-Determined)
- value: QUANTITY - 交易发送的金额，可选整数
- data: DATA - 合约的编译带啊或被调用方法的签名及编码参数
- nonce: QUANTITY - nonce，可选。可以使用同一个nonce来实现挂起的交易的重写
- string: 密码

#### 返回值

DATA, 32字节 - 交易哈希，如果交易还未生效则返回0值哈希

当创建合约时，在交易生效后，使用eth_getTransactionReceipt调用获取合约地址。

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "personal_sendTransaction", 
	"params": [{
		"nonce":"0x15",
		"from": "0x76dd0e29b7c86fc246ea36919f077740f287391e",
		"to": "0x76dd0e29b7c86fc246ea36919f077740f287391e",
		"gas": "0x1", 
		"gasPrice": "0x1", 
		"value": "0x0", 
		"data": ""
	},"123456"] 
}

**发送代币**

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "personal_sendTransaction", 
	"params": [{
		"from": "0xba2b6ed7815cac17748843a18ead8bff8119a367",
		"to": "0xc6fb0a9bf91f9f49b78d7c9135cd362500863f5f",
		"value": "0x0", 
		"data": "0xa9059cbb000000000000000000000000f67da09bc9671249734f967f55b0d4851a53188a0000000000000000000000000000000000000000000000000de0b6b3a7640000"
	},"1"] 
}

#### 响应

{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x8474441674cdd47b35b875fd1a530b800b51a5264b9975fb21129eeb8c18582f"
}

### eth_getTransactionByHash

返回指定哈希对应的交易。

#### 参数

DATA, 32 字节 - 交易哈希

params: [
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
]

#### 返回值

Object - 交易对象，如果没有找到匹配的交易则返回null。结构如下：

- hash: DATA, 32字节 - 交易哈希
- nonce: QUANTITY - 本次交易之前发送方已经生成的交易数量
- blockHash: DATA, 32字节 - 交易所在块的哈希，对于挂起块，该值为null
- blockNumber: QUANTITY - 交易所在块的编号，对于挂起块，该值为null
- transactionIndex: QUANTITY - 交易在块中的索引位置，挂起块该值为null
- from: DATA, 20字节 - 交易发送方地址
- to: DATA, 20字节 - 交易接收方地址，对于合约创建交易，该值为null
- value: QUANTITY - 发送的以太数量，单位：wei
- gasPrice: QUANTITY - 发送方提供的gas价格，单位：wei
- gas: QUANTITY - 发送方提供的gas可用量
- input: DATA - 随交易发送的数据

#### 示例代码

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_getTransactionByHash", 
	"params": ["0xbda4981702d5d5f059bdf95f5904bce3ab8f0781564d9f3797b7758988d7af06"] 
}

#### 响应

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockHash": "0x5e2a785940396ff889ae353b2de65c985dd089cae5559e5772a634ebf48b1f0b",
        "blockNumber": "0x7a90ac",
        "from": "0x00fd20d34dc028b64369a27cd251e97e91e5884b",
        "gas": "0xfde8",
        "gasPrice": "0x46c7cfe00",
        "hash": "0xbda4981702d5d5f059bdf95f5904bce3ab8f0781564d9f3797b7758988d7af06",
        "input": "0xa9059cbb000000000000000000000000d3007b63763c02b88fb6bd83146680dccbf5a97600000000000000000000000000000000000000000000000083d6c7aab6360000",
        "nonce": "0x0",
        "to": "0x8e870d67f660d95d5be530380d0ec0bd388289e1",
        "transactionIndex": "0xc7",
        "value": "0x0",
        "v": "0x26",
        "r": "0x6d2983dcb848f19e91d9148b962cd0298283a42d0d54bedbba83b7dac373e087",
        "s": "0x352d5f4ac2687976cee3aa5f48c46b3a007e4db522e29d57ac0cd9c4c2a4c502"
    }
}

### eth_getTransactionReceipt

返回指定交易的收据，使用哈希指定交易。

需要指出的是，挂起的交易其收据无效。

#### 参数

DATA, 32字节 - 交易哈希

params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]

#### 返回值

Object - 交易收据对象，如果收据不存在则为null。交易对象的结构如下：

- transactionHash: DATA, 32字节 - 交易哈希
- transactionIndex: QUANTITY - 交易在块内的索引序号
- blockHash: DATA, 32字节 - 交易所在块的哈希
- blockNumber: QUANTITY - 交易所在块的编号
- from: DATA, 20字节 - 交易发送方地址
- to: DATA, 20字节 - 交易接收方地址，对于合约创建交易该值为null
- cumulativeGasUsed: QUANTITY - 交易所在块消耗的gas总量
- gasUsed: QUANTITY - 该次交易消耗的gas用量
- contractAddress: DATA, 20字节 - 对于合约创建交易，该值为新创建的合约地址，否则为null
- logs: Array - 本次交易生成的日志对象数组
- logsBloom: DATA, 256字节 - bloom过滤器，轻客户端用来快速提取相关日志

返回的结果对象中还包括下面二者之一 :

- root : DATA 32字节，后交易状态根(pre Byzantium)
- status: QUANTITY ，1 (成功) 或 0 (失败)

### **eth_getBalance**

返回指定地址账户的余额。

#### **参数**

- `DATA` - 20字节，要检查余额的地址
- `QUANTITY|TAG` - 整数块编号，或者字符串"latest", "earliest" 或 "pending"

#### **返回值**

当前余额，单位：wei

#### **示例代码**

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_getBalance", 
	"params": ["0xBA2B6ed7815CaC17748843a18EAd8BfF8119A367","latest"] 
}

#### **响应**

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x16345785d8a0000"
}

### **eth_call**

立刻执行一个新的消息调用，无需在区块链上创建交易。

#### **参数**

`Object` - 交易调用对象

- from: DATA, 20 Bytes - 发送交易的原地址，可选
- to: DATA, 20 Bytes - 交易目标地址
- gas: QUANTITY - 交易可用gas量，可选。eth_call不消耗gas，但是某些执行环节需要这个参数
- gasPrice: QUANTITY - gas价格，可选
- value: QUANTITY - 交易发送的以太数量，可选
- data: DATA - 方法签名和编码参数的哈希，可选
- QUANTITY|TAG - 整数块编号，或字符串"latest"、"earliest"或"pending"

#### **返回值**

所执行合约的返回值

#### **示例代码**

##### 查询代币余额

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_call", 
	"params": [{
	  "from": "0xba2b6ed7815cac17748843a18ead8bff8119a367",
	  "to": "0xc6fb0a9bf91f9f49b78d7c9135cd362500863f5f",
	  "data": "0x70a08231000000000000000000000000ba2b6ed7815cac17748843a18ead8bff8119a367"
	},"latest"]
}

##### **查询代币余额响应**

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x0000000000000000000000000000000000000000000000008ac7230489e80000"
}

##### 查询代币精度

{
	"jsonrpc": "2.0", 
	"id":1, 
	"method": "eth_call", 
	"params": [{
		"to": "0xc6fb0a9bf91f9f49b78d7c9135cd362500863f5f",
		"data": "0x313ce567"
		},"latest"] 
}

##### **查询代币精度响应**

{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x0000000000000000000000000000000000000000000000000000000000000012"
}

