

**Lotus 多签地址使用及第三方API 整理**

一、生成多签地址及转账

1. 生成单地址的多签地址

要求钱包里面已经有地址并且地址里面有钱，有钱的地址为 address1

首先在钱包里生成一个地址 address2 作为多签地址的原地址

生成address2 地址的多签地址命令为：

*lotus msig create --from* *address1 address2*

说明:address1 仅仅作为手续费地址

 

*返回值：Created new multisig:  f01026  f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i

返回值说明： *f01026  等同于 f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i

返回结果即为address2 的多签地址，此多签地址与address1 无关。

然后就可以向多签地址*f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i里面转账了。

 

单地址的多签地址转账：

lotus msig propose --from address2 multisigaddress toaddress value

说明：address2 为此转账签名和提供手续费，要求 address2 需要有足以提供手续费的余额。

toaddress为转账接受地址, value 转账金额（单位FIL）

返回：

send proposal in message: bafy2bzacedjfmyxwf77bsvzpp7eepb2fjglnk776l5s5gwz3cnrzgc6gpbcim

Transaction ID: 0

Transaction was executed during propose

Exit Code: 0

Return Value: 

返回值说明:bafy2bzacedjfmyxwf77bsvzpp7eepb2fjglnk776l5s5gwz3cnrzgc6gpbcim 为交易id

Exit Code: 0 表示转账成功

Return Value:  为空

 

 

2. 生成多地址的多签地址

要求钱包里有地址并且地址里有足以支付手续的余额，设此地址为 address1

然后再创建两个地址作为多签地址的原地址，address2, address3

(若address2或者address3 中也有足以支付手续费的余额，也可作为创建多签地址的手续费地址)

创建address2和address3多签地址命令为：

 lotus msig create --from address1 address2 address3

说明：address1 仅作为手续费地址。

 

*返回值：*Created new multisig:  f01028   f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq

返回值说明： *f01028*  *等同于* *f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq*

返回结果即为address2，address3 的多签地址，此多签地址与address1 无关。

然后就可以向多签地址*f2qn537c7meoe3yf3jwu4iwflkgaydzfl5w35x7m*i里面转账了。

 

 

多地址的多签地址转账：

第一步：

lotus msig propose --from address2 multisigaddress  toaddress  value

说明：address2 作为此交易多签地址的签名地址和手续费地址，要求余额足以支付手续费

multisigaddress : 多签地址

toaddress： 转账接受地址

value: 转账金额（单位FIL）

 

返回：send proposal in message:  bafy2bzaceao7redelerd2jdvxyg4id2ynvs4rltvyqdrfplyafkgnltduw3mo

Transaction ID: 0

返回值说明：

bafy2bzaceao7redelerd2jdvxyg4id2ynvs4rltvyqdrfplyafkgnltduw3mo ：交易hash,此交易已发送，转账未确认

Transaction ID: 0  : 交易id

 

第二步：

第二个多签地址的原地址签名授权交易有效

lotus msig approve --from  address3  multisigaddress  0

说明：address3 为多签地址的第二个原地址，作为签名地址和此交易的手续费地址，需要足以支付手续费的余额。

multisigaddress  多签地址

0  : 此值为第一步返回的Transaction ID

 

返回： sent approval in message:  bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem

说明：bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem 为交易id

 

通过JSON-RPC 查询多签地址交易。

 

请求：

{ "jsonrpc": "2.0", "method": "Filecoin.ChainGetMessage", "params": [ {

​               "/": "bafy2bzaceanygulo2su5shav3oy73gab7eh3fkkoryi5hncs7eitqyllz6hem"

​            }], "id": 3}

 

返回:

{

​    "jsonrpc": "2.0",

​    "result": {

​        "Version": 0,

​        "To": "f2ag3auuldyefn2r4j5ljmnwsbwymitinodw5xvjq",

​        "From": "f1rravrqjmyjeoteselh4wo7sfjzsafv5djarw3hi",

​        "Nonce": 1,

​        "Value": "0",

​        "GasLimit": 3634576,

​        "GasFeeCap": "101651",

​        "GasPremium": "100597",

​        "Method": 3,

​        "Params": "ggBA",

​        "CID": {

​            "/": "bafy2bzaceakvso6sdufd2wcnne42dbdw7rovdlt6abv5rujoigm2pd5ye2cqa"

​        }

​    },

​    "id": 3

}

 

返回不准确：返回的from 地址为手续费地址，to地址为多签地址。没有命令中的金额接受地址。

 

 

二、第三方api

1.获取当前算力排名

https://filfox.info/api/v1/miner/top-miners/power?count=2

 

说明：count =2 表示当前算力前两名。

返回：{

​	"height": 208500,

​	"timestamp": 1604561400,

​	"miners": [{

​		"address": "f02770",    //挖矿地址

​		"tag": {

​			"name": "时空云&灵动",  //旷工名称

​			"signed": false

​		},

​		"rawBytePower": "54802717545070592", //原始算力 //此值除于1024的5次方后，单位为（PiB）

​		"qualityAdjPower": "54802717545070592", //有效算力//此值除于1024的5次方后，单位为（PiB）

​		"blocksMined": 770,    //出块数量

​		"weightedBlocksMined": 918,  //出块分数 //Filecoin挖矿模型中，一个高度（tipset）下可能有多个区块							   //（block），每个区块可能获得多份奖励（win count）。累计出块份数=每次出块获							 //得奖励份数的总和

​		"totalRewards": "11044715193371636732689",

​		"rewardPerByte": 70.14458342823498,

​		"rawBytePowerDelta": "137267154780160",

​		"qualityAdjPowerDelta": "137267154780160"

​	}, {

​		"address": "f01248",

​		"tag": {

​			"name": "ZH",

​			"signed": true

​		},

​		"location": {

​			"ip": "183.62.113.83",

​			"continentCode": "AS",

​			"continentName": "Asia",

​			"countryName": "China",

​			"regionName": "Guangdong",

​			"city": "Guangzhou",

​			"flag": "https://assets.ipstack.com/flags/cn.svg",

​			"flagEmoji": "������"

​		},

​		"rawBytePower": "50532008224358400",

​		"qualityAdjPower": "50532008224358400",

​		"blocksMined": 687,

​		"weightedBlocksMined": 798,

​		"totalRewards": "9598551116294736962475",

​		"rewardPerByte": 64.66585739425021,

​		"rawBytePowerDelta": "-1049174611066880",

​		"qualityAdjPowerDelta": "-1049174611066880"

​	}],

​	"totalRawBytePower": "871958213119967232",

​	"totalQualityAdjPower": "871353138127306752"

}

 

3.获取矿工时间段内挖矿状态

请求URL: https://filfox.info/api/v1/address/f01248/mining-stats?duration=24h 

说明：f01248矿工地址， 

duration=24h : 24 小时内挖矿状态，可选：duration=24h  一天，duration=7d 七天，

duration=30d 一个月， duration=1y 一年

 

返回：

{

​	"rawBytePowerGrowth": "8363160318771200",

​	"qualityAdjPowerGrowth": "8363160318771200",

​	"rawBytePowerDelta": "7748224081199104",

​	"qualityAdjPowerDelta": "7748224081199104",

​	"blocksMined": 5334,

​	"weightedBlocksMined": 6225,

​	"totalRewards": "73454194072750329562945",

​	"networkTotalRewards": "1105698077596388208514674",

​	"equivalentMiners": 7968.452380952381,

​	"rewardPerByte": 498.5289859503904,

​	"luckyValue": 0.9996470378295539,

​	"durationPercentage": 1

}

 

4.获取当前链信息

请求URL： https://api.filscan.io:8700/rpc/v1 

请求json：{"id":1,"jsonrpc":"2.0","params":[],"method":"filscan.StatChainInfo"}

返回:

{

​    "jsonrpc": "2.0",

​    "result": {

​        "data": {

​            "latest_height": 208514,

​            "latest_block_reward": "12.0656966093057",

​            "total_blocks": 816509,

​            "total_rewards": "9918112.03138008",

​            "power_ratio": "668122249254229.3333",

​            "fil_per_tera": "0.208456946879088",

​            "total_quality_power": "871539161750831104",

​            "active_miners": 722

​        }

​    }

}

返回说明：

latest_height : 当前区块高度

latest_block_reward：每赢票奖励 (单位：FIL)

total_blocks ：全网出块数量

total_rewards：全网出块奖励 (单位：FIL)

power_ratio：存储速度（此值除于1024的4次方后，单位为： Tib/h）

fil_per_tera： 算力收益（单位：FIL/T）

total_quality_power:全网有效算力 （此值除于1024的5次方后，单位为：Pib）

active_miners: 活跃旷工数

 

 

5.获取全网质押等信息

请求URL :    https://api.filscan.io:8700/rpc/v1 

请求json:   {"id":1,"jsonrpc":"2.0","params":[],"method":"filscan.StatInfo"}

返回：

{

​    "jsonrpc": "2.0",

​    "result": {

​        "avg_blocks": "4.55",

​        "avg_msgs": "359.37",

​        "avg_gas_price": "0",

​        "avg_gas_price_per_height": "0",

​        "pledge_collateral": "15069931.15",

​        "floating": "28025778.36",

​        "outstanding": "43109522.34"

​    }

}

说明：

avg_blocks：平均每高度区块数量

avg_msgs：平均区块打包消息数量

avg_gas_price：

avg_gas_price_per_height：

pledge_collateral：全网质押 （单位：FIL）

floating：流通中的FIL（单位：FIL）

outstanding：全网可用FIL（单位：FIL）
