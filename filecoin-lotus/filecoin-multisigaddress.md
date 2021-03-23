**filecoin 多签地址**

一、生成多签地址及转账

1. 生成单地址的多签地址

要求钱包里面已经有地址并且地址里面有钱，有钱的地址为 address1

首先在钱包里生成一个地址 address2 作为多签地址的原地址

生成address2 地址的多签地址命令为：

```
lotus msig create --from address1 address2
```

说明:address1 仅仅作为手续费地址

*返回值：*

```
Created new multisig:  f01026  f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7mi
```

返回值说明： f01026  等同于 f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7mi

返回结果即为address2 的多签地址，此多签地址与address1 无关。

然后就可以向多签地址*f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i里面转账了。


单地址的多签地址转账：

```
lotus msig propose --from address2 multisigaddress toaddress value
```

说明：address2 为此转账签名和提供手续费，要求 address2 需要有足以提供手续费的余额。

toaddress为转账接受地址, value 转账金额（单位FIL）

返回：

```
send proposal in message: bafy2bzacedjfmyxwf77bsvzpp7eepb2fjglnk776l5s5gwz3cnrzgc6gpbcim

Transaction ID: 0

Transaction was executed during propose

Exit Code: 0

Return Value: 
```

返回值说明:
bafy2bzacedjfmyxwf77bsvzpp7eepb2fjglnk776l5s5gwz3cnrzgc6gpbcim 为交易id

Exit Code: 0 表示转账成功

Return Value:  为空

2. 生成多地址的多签地址

要求钱包里有地址并且地址里有足以支付手续的余额，设此地址为 address1

然后再创建两个地址作为多签地址的原地址，address2, address3

(若address2或者address3 中也有足以支付手续费的余额，也可作为创建多签地址的手续费地址)

创建address2和address3多签地址命令为：

```
 lotus msig create --from address1 address2 address3
```

说明：address1 仅作为手续费地址。

*返回值：*

```
Created new multisig:  f01028   f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq
```

返回值说明： f01028  等同于  f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq

返回结果即为address2，address3 的多签地址，此多签地址与address1 无关。

然后就可以向多签地址*f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i里面转账了。


多地址的多签地址转账：

第一步：

```
lotus msig propose --from address2 multisigaddress  toaddress  value
```

说明：address2 作为此交易多签地址的签名地址和手续费地址，要求余额足以支付手续费

multisigaddress : 多签地址

toaddress： 转账接受地址

value: 转账金额（单位FIL）

返回：

```
send proposal in message:  bafy2bzaceao7redelerd2jdvxyg4id2ynvs4rltvyqdrfplyafkgnltduw3mo
Transaction ID: 0
```

返回值说明：
bafy2bzaceao7redelerd2jdvxyg4id2ynvs4rltvyqdrfplyafkgnltduw3mo ：交易hash,此交易已发送，转账未确认
Transaction ID: 0  : 交易id

第二步：

第二个多签地址的原地址签名授权交易有效

```
lotus msig approve --from  address3  multisigaddress  0
```

说明：address3 为多签地址的第二个原地址，作为签名地址和此交易的手续费地址，需要足以支付手续费的余额。

multisigaddress  多签地址

0  : 此值为第一步返回的Transaction ID

返回：

```
sent approval in message:  bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem
```

说明：bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem 为交易id

通过JSON-RPC 查询多签地址交易。

请求：

```json
{
	"jsonrpc": "2.0",
	"method": "Filecoin.ChainGetMessage",
	"params": [{
		"/": "bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem"
	}],
	"id": 3
}
```


返回:

```json
{
	"jsonrpc": "2.0",
	"result": {
		"Version": 0,
		"To": "f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq",
		"From": "f1rravrqjmyjeoteselh4wo7sfjzsafv5djarw3hi",
		"Nonce": 1,
		"Value": "0",
		"GasLimit": 3634576,
		"GasFeeCap": "101651",
		"GasPremium": "100597",
		"Method": 3,
		"Params": "ggBA",
		"CID": {
			"/": "bafy2bzaceakvso6sdufd2wcnne42dbdw7rovdlt6abv5rujoigm2pd5ye2cqa"
		}
	},
	"id": 3
}
```


返回不准确：返回的from 地址为手续费地址，to地址为多签地址。没有命令中的金额接受地址。

可以 通过 Filecoin.StateReplay  接口查询链上具体的消息执行结果。