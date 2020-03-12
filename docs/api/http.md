# HTTP API 接口

## API INDEX ###

| transaction                   |  account                           |   shielded transactions                      |
|-------------------------------|------------------------------------|----------------------------------------------|
| createtransaction             |  updateaccount                     | getexpandedspendingkey                       |
| gettransactionsign            |  createaccount                     | getakfromask                                 |
| gettransactionbyid            |  createaddress                     | getnkfromnsk                                 |
| gettransactioninfobyid        |  getaccountnet                     | getspendingkey                               |
| gettransactioncountbyblocknum |  getaccount                        | getdiversifier                               |
| getdeferredtransactionbyid    |  generateaddress                   | getincomingviewingkey                        |
| canceldeferredtransactionbyid |  validateaddress                   | getzenpaymentaddress                         |
| getdeferredtransactioninfobyid|  getaccountresource                | scannotebyivk                                |
| getsignweight                 |  setaccountid                      | scanandmarknotebyivk                         |
| addtransactionsign            |  getaccountbyid                    | scannotebyovk                                |
|                               |  accountpermissionupdate           | getrcm                                       |
|                               |  getdelegatedresource              | getmerkletreevoucherinfo                     |
|                               |  getdelegatedresourceaccountindex  | isspend                                      |
|    **block**                  |  freezebalance                     | createspendauthsig                           |
| getnowblock                   |  unfreezebalance                   | createshieldnullifier                        |
| getblockbynum                 |  unfreezeasset                     | getshieldtransactionhash                     |
| getblockbyid                  |  withdrawbalance                   | createshieldedtransaction                    |
| getblockbylimitnext           |  votewitnessaccount                | createshieldedtransaction<br>withoutspendauthsig |
| getblockbylatestnum           |  updatewitness                     | getnewshieldedaddress                        |
|                               |  createwitness                     |                                              |
|                               |  getbrokerage                      |                                              |
|                               |  getreward                         |                                              |
|                               |  updateBrokerage                   |                                              |


|   asset                        |  exchange               | transfer                        |
|--------------------------------|-------------------------|---------------------------------|
|  createassetissue              | exchangecreate          | easytransferbyprivate           |
|  participateassetissue         | exchangeinject          | easytransferassetbyprivate      |
|  getassetissuebyaccount        | exchangewithdraw        | transferasset                   |
|  getassetissuebyname           | exchangetransaction     | easytransfer                    |
|  getassetissuelistbyname       | getexchangebyid         | easytransferasset               |
|  getassetissuelist             | getpaginatedexchangelist|                                 |
|  getpaginatedassetissuelist    | getpaginatedexchangelist|                                 |
|  getassetissuebyid             | listexchanges           |                                 |
|  updateasset                   | getpaginatedexchangelist|                                 |



|   proposal                       | smart contract          | others                |
|----------------------------------|-------------------------|-----------------------|
|  getpaginatedproposallist        | deploycontract          | broadcasttransaction  |
|  proposalcreate                  | getcontract             | broadcasthex          |
|  getproposalbyid                 | triggerconstantcontract | listnodes             |
|  listproposals                   | triggersmartcontract    | listwitnesses         |
|  proposalapprove                 | clearabi                | getnextmaintenancetime|
|  proposaldelete                  | updateenergylimit       | getnodeinfo           |
|  getapprovedlist                 | updatesetting           | getchainparameters    |


## hexString和base58check转码demo
java:
[https://github.com/tronprotocol/wallet-cli/blob/master/src/main/java/org/tron/demo/TransactionSignDemo.java#L92](https://github.com/tronprotocol/wallet-cli/blob/master/src/main/java/org/tron/demo/TransactionSignDemo.java#L92)

php:
[https://github.com/tronprotocol/Documentation/blob/master/TRX_CN/index.php](https://github.com/tronprotocol/Documentation/blob/master/TRX_CN/index.php)

**3.6版本之后，增加参数visible，当visible设置为true时，相应的地址和字符串不再需要转码。该参数针对所有的接口有效，包括solidityNode接口和FullNode接口**

当参数visible为true时，输入的地址必须为base58格式，字符串为可见字符串，输出的格式也是如此；如果设置为false
或不设置时，接口行为同原来保持一致。如果参数格式与visible设置不匹配，将会报错。

设置参数方式：

- 不需要参数的查询接口，通过在url中增加参数。如
```text
http://127.0.0.1:8090/wallet/listexchanges?visible=true
```
- POST方式请求接口时，通过在json体最外层中增加参数。如
```json
curl -X POST http://127.0.0.1:8090/wallet/createtransaction -d
'{
    "owner_address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
    "to_address": "TJCnKsPa7y5okkXvQAidZBzqx3QyQ6sxMW",
    "amount": 1000000,
    "visible": true
}'
```
- GET方式请求接口时，通过在url中增加参数。同方式1。


## SolidityNode接口

solidityNode默认的http端口是8091，启动solidityNode的时候会同时启动http服务。
```text
/walletsolidity/getaccount
作用：查询一个账号的信息
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getaccount -d '{"address": "41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}'
参数说明：address 默认为hexString
返回值：Account对象

/walletsolidity/listwitnesses
作用：查询超级代表列表
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/listwitnesses
参数说明：
返回值：所有超级代表列表

/walletsolidity/getassetissuelist
作用：查询所有Token列表
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getassetissuelist
参数说明：
返回值：所有Token列表

/walletsolidity/getpaginatedassetissuelist
作用：分页查询Token列表
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getpaginatedassetissuelist -d '{"offset": 0, "limit":10}'
参数说明：offset是起始Token的index，limit是期望返回的Token数量
返回值：Token列表

/walletsolidity/getassetissuebyname(Odyssey-v3.2开始支持)
作用：根据名称查询token。
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getassetissuebyname -d '{"value": "44756354616E"}'
参数说明：通证名称，默认为hexString。
返回值：token。
注意：Odyssey-v3.2开始，推荐使用getassetissuebyid或者getassetissuelistbyname替换此接口，因为从3.2开始将允许通证名称相同。如果存在相同的通证名称，此接口将会报错。

/walletsolidity/getassetissuelistbyname(Odyssey-v3.2开始支持)
作用：根据名称查询token list。
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getassetissuelistbyname -d '{"value": "44756354616E"}'
参数说明：通证名称，默认为hexString。
返回值：token列表。

/walletsolidity/getassetissuebyid(Odyssey-v3.2开始支持)
作用：根据id查询token。
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getassetissuebyid -d '{"value": "1000001"}'
参数说明：通证id
返回值：token。

/walletsolidity/getnowblock
作用：查询最新block
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getnowblock
参数说明：
返回值：solidityNode上的最新block

/walletsolidity/getblockbynum
作用：按照高度查询block
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getblockbynum -d '{"num" : 100}'
参数说明：num是块的高度
返回值：指定高度的block

/walletsolidity/gettransactionbyid
作用：根据id查询交易
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/gettransactionbyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：指定ID的Transaction

/walletsolidity/gettransactioncountbyblocknum(Odyssey-v3.2开始支持)
作用：查询特定block上transaction的个数
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/gettransactioncountbyblocknum -d '{"num" : 100}'
参数说明：num是块的高度.
返回值e：transaction的个数.

/walletsolidity/gettransactioninfobyid
作用：根据id查询交易的fee，所在的block
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/gettransactioninfobyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：Transaction的交易fee，所在block的高度，创建时间

/walletsolidity/getdelegatedresource(Odyssey-v3.2开始支持)
作用：查看一个账户代理给另外一个账户的资源情况
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getdelegatedresource -d '
{
"fromAddress": "419844f7600e018fd0d710e2145351d607b3316ce9",
"toAddress": "41c6600433381c731f22fc2b9f864b14fe518b322f"
}'
参数说明：
fromAddress：是要查询的账户地址，默认为hexString格式
toAddress：代理对象的账户地址，默认为hexString格式
返回值：账户的资源代理的列表，列表的元素为DelegatedResource

/walletsolidity/getdelegatedresourceaccountindex(Odyssey-v3.2开始支持)
作用：查看一个账户的资源代理情况
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getdelegatedresourceaccountindex -d '
{
"value": "419844f7600e018fd0d710e2145351d607b3316ce9",
}'
参数说明：
value：是要查询的账户地址，默认为hexString格式
返回值：账户的DelegatedResourceAccountIndex

/walletsolidity/getexchangebyid(Odyssey-v3.2开始支持)
作用：根据id查询交易对
demo：curl -X POST  http://127.0.0.1:8091/walletsolidity/getexchangebyid -d {"id":1}
参数说明：
id：交易对id
返回值：交易对

/walletsolidity/listexchanges(Odyssey-v3.2开始支持)
作用：查询所有交易对
demo：curl -X POST  http://127.0.0.1:8091/walletsolidity/listexchanges
参数说明：
返回值：所有交易对

walletsolidity/getaccountbyid
作用：通过accountId查询一个账号的信息
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getaccountbyid -d '{"account_id":"6161616162626262"}'
参数说明：account_id 默认为hexString格式
返回值：Account对象

walletsolidity/getblockbyid
作用：通过ID查询块
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getblockbyid-d '{"value":
"0000000000038809c59ee8409a3b6c051e369ef1096603c7ee723c16e2376c73"}'
参数说明：块ID。
返回值：块。

walletsolidity/getblockbylimitnext
作用：按照范围查询块
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getblockbylimitnext -d '{"startNum": 1, "endNum": 2}'
参数说明：
   startNum：起始块高度，包含此块
   endNum：截止块高度，不包含此此块
返回值：块的列表。

walletsolidity/getblockbylatestnum
作用：查询最新的几个块
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getblockbylatestnum -d '{"num": 5}'
参数说明：块的数量。
返回值：块的列表。

/walletextension/gettransactionsfromthis（新版本将不再支持）
作用：查询某个账号的出账交易记录
demo: curl -X POST  http://127.0.0.1:8091/walletextension/gettransactionsfromthis -d '{"account"
: {"address" : "41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}, "offset": 0, "limit": 10,"startTime": 1546099200000, "endTime": 1552028828000}'
参数说明：address是账号地址，需要是hexString格式；offset是起始交易的index，不能大于10000，否则报错；limit是期望返回的交易数量，最大为50
，这个值可能会调整。当limit>50，或offset+limit>10000时，调整后满足limit<=50且offset+limit<=10000，startTime
:起始时间，endTime:结束时间，获取[startTime,endTime]时间段的交易。如果不传递，默认获取获取最近7天内的交易
返回值：Transaction列表，按交易创建时间的降序排列，total在[startTime,endTime]时间段内允许分页的最大交易数，rangeTotal在[startTime,
endTime]时间段内的所有交易数。
备注：该接口在新版本节点中将不再提供，如需要该功能，可以使用中心节点提供的接口，47.90.247.237:8091/walletextension/gettransactionsfromthis,
   使用参考getTransactionsFromThis。

/walletextension/gettransactionstothis（新版本将不再支持）
作用：查询某个账号的入账交易记录
demo: curl -X POST  http://127.0.0.1:8091/walletextension/gettransactionstothis -d '{"account" :
{"address" : "41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}, "offset": 0, "limit": 10,"startTime": 1546099200000, "endTime": 1552028828000}'
参数说明：address是账号地址，需要是hexString格式；offset是起始交易的index；limit是期望返回的交易数量，最大为50，这个值可能会调整。当limit>50
，或offset+limit>10000时，调整后满足limit<=50且offset+limit<=10000，startTime:起始时间，endTime
:结束时间，获取[startTime,endTime]时间段的交易。如果不传递，默认获取获取最近7天内的交易
返回值：Transaction列表，按交易创建时间的降序排列，total在[startTime,endTime]时间段内允许分页的最大交易数，rangeTotal在[startTime,
endTime]时间段内的所有交易数。
备注：该接口在新版本节点中将不再提供，如需要该功能，可以使用中心节点提供的接口，47.90.247.237:8091/walletextension/gettransactionstothis,
   使用参考getTransactionsToThis。

/wallet/getnodeinfo(Odyssey-v3.2开始支持)
作用：获取当前node的信息
demo: curl -X GET http://127.0.0.1:8091/wallet/getnodeinfo
参数说明：无
返回值：当前节点的信息NodeInfo

/walletsolidity/getdeferredtransactionbyid
作用：通过交易id查询延迟交易
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getdeferredtransactionbyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：指定ID的Transaction

/walletsolidity/getdeferredtransactioninfobyid
作用：根据id查询交易的fee，所在的block
demo: curl -X POST  http://127.0.0.1:8091/walletsolidity/getdeferredtransactioninfobyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：Transaction的交易fee，所在block的高度，创建时间

/walletsolidity/getmerkletreevoucherinfo
作用：获得一个note的默克尔树信息
demo: curl -X POST  http://127.0.0.1:8090/walletsolidity/getmerkletreevoucherinfo -d
'{
    "out_points":[{
        "hash":"185b3e085723f5862b3a3c3cf54d52f5c1eaf2541e3a1e0ecd08bc12cd958d74",
        "index":0
    }]
}'
参数说明：
out_points：Note信息
返回值：一个note的默克尔树信息

/walletsolidity/scannotebyivk
作用：查询与ivk相关的所有的notes
demo: curl -X POST  http://127.0.0.1:8090/walletsolidity/scannotebyivk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ivk": "80a481c3c739e54b4e0608090b3a1a6e9f8dce42346e95bf5a2d8a487bf45c05"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ivk：Incoming viewing key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）

- /walletsolidity/scanandmarknotebyivk
作用：查询与ivk相关的所有的notes, 包含是否花费状态
demo: curl -X POST  http://127.0.0.1:8090/walletsolidity/scanandmarknotebyivk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ivk": "80a481c3c739e54b4e0608090b3a1a6e9f8dce42346e95bf5a2d8a487bf45c05",
    "ak": "1d4f9b5551f4aa9443ceb263f0e208eb7e26080264571c5ef06de97a646fe418",
    "nk": "748522c7571a9da787e43940c9a474aa0c5c39b46c338905deb6726fa3678bdb"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ivk：Incoming viewing key
ak：Ak key
nk：Nk key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）

/walletsolidity/scannotebyovk
作用：查询与ovk相关的所有的notes
demo: curl -X POST  http://127.0.0.1:8090/walletsolidity/scannotebyovk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ovk": "705145aa18cbe6c11d5d0011419a98f3d5b1d341eb4727f1315597f4bdaf8539"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ovk：Outgoing viewing key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）



/walletsolidity/isspend
作用：查询一个note是否已经被花费
demo: curl -X POST  http://127.0.0.1:8090/walletsolidity/isspend -d
'{
    "ak": "a3e65d509b675aaa2aeda977ceff11eebd76218079b6f543d78a615e396ca129",
    "nk": "62cfda9bea09a53cf2a21022057913734a8458969e11e0bb9c59ead48fbce83e",
    "note": {
        "payment_address": "ztron1aqgauawtkelxfu2w6s48cwh0mchjt6kwpj44l4wym3pullx0294j4r4v7kpm75wnclzycsw73mq",
        "rcm": "74a16c1b27ec7fbf06881d9d35ddaab1554838b1bddcd54f6bd8a9fb4ba0b80a",
        "value": 500000000
    },
    "txid": "7d09e471bb047d3ac044d5d6691b3721a2dddbb683ac02c207fbe78af6302463",
    "index": 1
}'
参数说明：
ak：Ak
nk：Nk
note：Note信息
txid：交易id
index：Note索引
返回值：一个note是否已经被花费状态

```

## FullNode接口
FullNode默认的http端口是8090，启动FullNode的时候会同时启动http服务。

```text
wallet/createtransaction
作用： 创建一个转账的Transaction，如果转账的to地址不存在，则在区块链上创建该账号
demo: curl -X POST  http://127.0.0.1:8090/wallet/createtransaction -d '{"to_address": "41e9d79cc47518930bc322d9bf7cddd260a0260a8d", "owner_address": "41D1E7A6BC354106CB410E65FF8B181C600FF14292", "amount": 1000 }'
参数说明：
to_address是转账转入地址，默认为hexString
owner_address是转账转出地址，默认为hexString
amount是转账数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：转账合约

/wallet/gettransactionsign
作用：对交易签名，该api有泄漏private key的风险，请确保在安全的环境中调用该api
demo: curl -X POST  http://127.0.0.1:8090/wallet/gettransactionsign -d '{
"transaction" : {"txID":"454f156bf1256587ff6ccdbc56e64ad0c51e4f8efea5490dcbc720ee606bc7b8","raw_data":{"contract":[{"parameter":{"value":{"amount":1000,"owner_address":"41e552f6487585c2b58bc2c9bb4492bc1f17132cd0","to_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292"},"type_url":"type.googleapis.com/protocol.TransferContract"},"type":"TransferContract"}],"ref_block_bytes":"267e","ref_block_hash":"9a447d222e8de9f2","expiration":1530893064000,"timestamp":1530893006233}}, "privateKey": "your private key"
}'
参数说明：transaction是通过http api创建的合约，privateKey是用户private key
返回值：签名之后的transaction

wallet/broadcasttransaction
作用：对签名后的transaction进行广播
demo：curl -X POST  http://127.0.0.1:8090/wallet/broadcasttransaction -d '{"signature":["97c825b41c77de2a8bd65b3df55cd4c0df59c307c0187e42321dcc1cc455ddba583dd9502e17cfec5945b34cad0511985a6165999092a6dec84c2bdd97e649fc01"],"txID":"454f156bf1256587ff6ccdbc56e64ad0c51e4f8efea5490dcbc720ee606bc7b8","raw_data":{"contract":[{"parameter":{"value":{"amount":1000,"owner_address":"41e552f6487585c2b58bc2c9bb4492bc1f17132cd0","to_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292"},"type_url":"type.googleapis.com/protocol.TransferContract"},"type":"TransferContract"}],"ref_block_bytes":"267e","ref_block_hash":"9a447d222e8de9f2","expiration":1530893064000,"timestamp":1530893006233}}'
参数说明：签名之后的Transaction
返回值：广播是否成功

- wallet/broadcasthex
作用：对签名后的transaction hex进行广播
demo：curl -X POST  http://127.0.0.1:8090/wallet/broadcasttransaction -d '{"transaction":"0A8A010A0202DB2208C89D4811359A28004098A4E0A6B52D5A730802126F0A32747970652E676F6F676C65617069732E636F6D2F70726F746F636F6C2E5472616E736665724173736574436F6E747261637412390A07313030303030311215415A523B449890854C8FC460AB602DF9F31FE4293F1A15416B0580DA195542DDABE288FEC436C7D5AF769D24206412418BF3F2E492ED443607910EA9EF0A7EF79728DAAAAC0EE2BA6CB87DA38366DF9AC4ADE54B2912C1DEB0EE6666B86A07A6C7DF68F1F9DA171EEE6A370B3CA9CBBB00"}'
参数说明：签名之后的Transaction hex
返回值：广播是否成功

wallet/updateaccount
作用：修改账号名称
demo：curl -X POST  http://127.0.0.1:8090/wallet/updateaccount -d '{"account_name": "0x7570646174654e616d6531353330383933343635353139" ,"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292"}'
参数说明：
account_name是账号名称，默认为hexString格式
owner_address是要修改名称的账号地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：修改名称的Transaction

wallet/votewitnessaccount
作用：对超级代表进行投票
demo：curl -X POST  http://127.0.0.1:8090/wallet/votewitnessaccount -d '{
"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292",
"votes": [{"vote_address": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0", "vote_count": 5}]
}'
参数说明：
owner_address是投票人地址，默认为hexString格式
votes.vote_address是被投票的超级代表的地址，默认为hexString格式
vote_count是投票数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：投票的Transaction

wallet/getBrokerage
作用：查询witness当前Brokerage比例
demo：curl -X GET  http://127.0.0.1:8090/wallet/getBrokerage -d '{
"address":"41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}'
参数说明：
address是被投票的超级代表的地址，默认为hexString格式
返回值：witness当前Brokerage比例

wallet/getReward
作用：查询投票人未领取的奖励
demo：curl -X GET
http://127.0.0.1:8090/wallet/getReward -d '{
"address":"41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}'
参数说明：
address是投票人地址，默认为hexString格式
返回值：投票人未领取的奖励

wallet/updateBrokerage
作用：更新witness当前Brokerage比例
demo：curl -X POST  http://47.252.81.126:8090/wallet/updateBrokerage  -d '{
"owner_address":"41E552F6487585C2B58BC2C9BB4492BC1F17132CD0",
"brokerage":30}'
参数说明：
owner_address是被投票的超级代表的地址，默认为hexString格式
brokerage是witness想要更新为的Brokerage比例
返回值：更新Brokerage的Transaction

wallet/createassetissue
作用：发行Token
demo：curl -X POST  http://127.0.0.1:8090/wallet/createassetissue -d '{
"owner_address":"41e552f6487585c2b58bc2c9bb4492bc1f17132cd0",
"name":"0x6173736574497373756531353330383934333132313538",
"abbr": "0x6162627231353330383934333132313538",
"total_supply" :4321,
"trx_num":1,
"num":1,
"start_time" : 1530894315158,
"end_time":1533894312158,
"description":"007570646174654e616d6531353330363038383733343633",
"url":"007570646174654e616d6531353330363038383733343633",
"free_asset_net_limit":10000,
"public_free_asset_net_limit":10000,
"frozen_supply":{"frozen_amount":1, "frozen_days":2}
}'
参数说明：
owner_address发行人地址，默认为hexString格式
name是token名称，默认为hexString格式
abbr是token简称，默认为hexString格式
total_supply是发行总量
trx_num和num是token和trx的最小单位兑换比
start_time和end_time是token发行起止时间
description是token说明，默认为hexString格式
url 是token发行方的官网，默认为hexString格式
free_asset_net_limit是Token的总的免费带宽
public_free_asset_net_limit 是每个token拥护者能使用本token的免费带宽
frozen_supply是token发行者可以在发行的时候指定冻结的token
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：
发行Token的Transaction

wallet/updatewitness
作用：修改witness的url
demo：curl -X POST  http://127.0.0.1:8090/wallet/updatewitness -d '{
"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292",
"update_url": "007570646174654e616d6531353330363038383733343633"
}'
参数说明：
owner_address是创建人地址，默认为hexString格式
update_url是更新的官网的url，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：
更新witness的Transaction

wallet/createaccount
作用：创建账号，一个已经激活的账号创建一个新账号，需要花费0.1trx
demo：curl -X POST  http://127.0.0.1:8090/wallet/createaccount -d '{"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292", "account_address": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0"}'
参数说明：
owner_address是已经激活的账号，默认为hexString格式
account_address是新账号的地址，默认为hexString格式，这个地址需要事先创建好
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：创建账号的Transaction

wallet/createwitness
作用：申请成为超级代表
demo：curl -X POST  http://127.0.0.1:8090/wallet/createwitness -d '{"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292", "url": "007570646174654e616d6531353330363038383733343633"}'
参数说明：
owner_address是申请成为超级代表的账号地址，默认为hexString格式
url是官网地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：申请超级代表的Transaction

wallet/transferasset
作用：转账Token
demo：curl -X POST  http://127.0.0.1:8090/wallet/transferasset -d '{"owner_address":"41d1e7a6bc354106cb410e65ff8b181c600ff14292", "to_address": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0", "asset_name": "0x6173736574497373756531353330383934333132313538", "amount": 100}'
参数说明：
owner_address是token转出地址，默认为hexString格式
to_address是token转入地址，默认为hexString格式
asset_name是token名称，默认为hexString格式
amount是token转账数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：token转账的Transaction
【注意】
- 当前的asset_name为token名称。当委员会通过AllowSameTokenName提议后asset_name改为token ID的String类型。

wallet/easytransfer
作用：快捷转账，该api存在泄漏密码的风险，请确保在安全的环境中调用该api。调用该api前请先调用createAddress生成地址。
demo：curl -X POST http://127.0.0.1:8090/wallet/easytransfer -d '{
"passPhrase": "your password",
"toAddress": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0",
"amount":100
}'
参数说明：
passPhrase是用户密码，默认为hexString格式
toAddress是转入地址，默认为hexString格式
amount是转账trx数量
返回值：
对应的Transaction和广播是否成功的状态

wallet/easytransferasset
作用：快捷转账，该api存在泄漏密码的风险，请确保在安全的环境中调用该api。调用该api前请先调用createAddress生成地址。
demo：curl -X POST http://127.0.0.1:8090/wallet/easytransferasset -d '{
"passPhrase": "your password",
"toAddress": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0",
"assetId": "1000001",
"amount":100
}'
参数说明：
passPhrase是用户密码，默认为hexString格式
toAddress是转入地址，默认为hexString格式
assetId是通证的ID
amount是转账通证数量,单位是通证的最小单位
返回值：
对应的Transaction和广播是否成功的状态

wallet/createaddress
作用：通过密码创建地址，该api存在泄漏密码的风险，请确保在安全的环境中调用该api。
demo：curl -X POST http://127.0.0.1:8090/wallet/createaddress -d '{"value": "3230313271756265696a696e67"}'
参数说明：value是用户密码，默认为hexString格式
返回值：一个地址

wallet/participateassetissue
作用：参与token发行
demo：curl -X POST http://127.0.0.1:8090/wallet/participateassetissue -d '{
"to_address": "41e552f6487585c2b58bc2c9bb4492bc1f17132cd0",
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
"amount":100,
"asset_name":"3230313271756265696a696e67"
}'
参数说明：
to_address是Token发行人的地址，默认为hexString格式
owner_address是参与token人的地址，默认为hexString格式
amount是参与token的数量
asset_name是token的名称，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：参与token发行的transaction
【注意】
- 当前的asset_name为token名称。当委员会通过AllowSameTokenName提议后asset_name改为token ID的String类型。

wallet/freezebalance
作用：冻结trx，获取带宽，获取投票权
demo：curl -X POST http://127.0.0.1:8090/wallet/freezebalance -d '{
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
"frozen_balance": 10000,
"frozen_duration": 3,
"resource" : "BANDWIDTH",
"receiver_address":"414332f387585c2b58bc2c9bb4492bc1f17342cd1"
}'
参数说明：
owner_address是冻结trx账号的地址，默认为hexString格式
frozen_balance是冻结trx的数量
frozen_duration是冻结天数，最少是3天
resource: 冻结trx获取资源的类型(可以是BANDWIDTH或者ENERGY，BANDWIDTH为带宽，ENERGY为虚拟机消耗资源)
receiverAddress表示受委托账户的地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：冻结trx的transaction
【注意】资源委托功能需要委员会开启

wallet/unfreezebalance
作用：解冻已经结束冻结期的trx，会同时失去这部分trx带来的带宽和投票权
demo：curl -X POST http://127.0.0.1:8090/wallet/unfreezebalance -d '{
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
"resource": "BANDWIDTH",
"receiver_address":"414332f387585c2b58bc2c9bb4492bc1f17342cd1"
}'
参数说明：
owner_address是解冻trx账号的地址，默认为hexString格式
resource可以是BANDWIDTH或者ENERGY
receiverAddress表示受委托账户的地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：解冻trx的transaction
【注意】资源委托功能需要委员会开启

wallet/unfreezeasset
作用：解冻已经结束冻结期的Token
demo：curl -X POST http://127.0.0.1:8090/wallet/unfreezeasset -d '{
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
}'
参数说明：
owner_address是解冻token账号的地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：解冻token的transaction

wallet/withdrawbalance
作用：超级代表提现奖励到balance，每24个小时可以提现一次
demo：curl -X POST http://127.0.0.1:8090/wallet/withdrawbalance -d '{
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
}'
参数说明：
owner_address是提现账号的地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：提现Trx的transaction

wallet/updateasset
作用：修改token信息
demo：curl -X POST http://127.0.0.1:8090/wallet/updateasset -d '{
"owner_address":"41e472f387585c2b58bc2c9bb4492bc1f17342cd1",
"description": ""，
"url": "",
"new_limit" : 1000000,
"new_public_limit" : 100
}'
参数说明：
owner_address是token发行人的地址，默认为hexString格式
description是token的描述，默认为hexString格式
url是token发行人的官网地址，默认为hexString格式
new_limit是token每个持有人能够使用的免费带宽
new_public_limit是该token全部的免费带宽
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：
修改Token信息的transaction

wallet/listnodes
作用：查询api所在机器连接的节点。
demo: curl -X POST  http://127.0.0.1:8090/wallet/listnodes
参数说明：无
返回值：节点列表。

wallet/getassetissuebyaccount
作用：查询账户发行的token。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getassetissuebyaccount -d '{"address": "41F9395ED64A6E1D4ED37CD17C75A1D247223CAF2D"}'
参数说明：发行者账户地址，默认为hexString格式
返回值：用户发行的token（一个用户只能发行一个token）。

wallet/getaccountnet
作用：查询带宽信息。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getaccountnet -d '{"address": "4112E621D5577311998708F4D7B9F71F86DAE138B5"}'
参数说明：账户地址，默认为hexString格式
返回值：带宽信息。

wallet/getassetissuebyname
作用：根据名称查询token。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getassetissuebyname -d '{"value": "44756354616E"}'
参数说明：通证名称，默认为hexString格式
返回值：token。
注意：Odyssey-v3.2开始，推荐使用getassetissuebyid或者getassetissuelistbyname替换此接口，因为从3.2开始将允许通证名称相同。如果存在相同的通证名称，此接口将会报错。

wallet/getassetissuelistbyname(Odyssey-v3.2开始支持)
作用：根据名称查询token list。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getassetissuelistbyname -d '{"value": "44756354616E"}'
参数说明：通证名称，默认为hexString格式
返回值：token列表。

wallet/getassetissuebyid(Odyssey-v3.2开始支持)
作用：根据id查询token。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getassetissuebyid -d '{"value": "1000001"}'
参数说明：通证id
返回值：token。

wallet/getnowblock
作用：查询最新块。
demo: curl -X POST  http://127.0.0.1:8090/wallet/getnowblock
参数说明：无
返回值：当前块。

wallet/getblockbynum
作用：通过高度查询块
demo: curl -X POST  http://127.0.0.1:8090/wallet/getblockbynum -d '{"num": 1}'
参数说明：块高度。
返回值：块。

wallet/getblockbyid
作用：通过ID查询块
demo: curl -X POST  http://127.0.0.1:8090/wallet/getblockbyid -d '{"value": "0000000000038809c59ee8409a3b6c051e369ef1096603c7ee723c16e2376c73"}'
参数说明：块ID。
返回值：块。

wallet/getblockbylimitnext
作用：按照范围查询块
demo: curl -X POST  http://127.0.0.1:8090/wallet/getblockbylimitnext -d '{"startNum": 1, "endNum": 2}'
参数说明：
startNum：起始块高度，包含此块
endNum：截止块高度，不包含此此块
返回值：块的列表。

wallet/getblockbylatestnum
作用：查询最新的几个块
demo: curl -X POST  http://127.0.0.1:8090/wallet/getblockbylatestnum -d '{"num": 5}'
参数说明：块的数量。
返回值：块的列表。

wallet/gettransactionbyid
作用：通过ID查询交易
demo: curl -X POST  http://127.0.0.1:8090/wallet/gettransactionbyid -d '{"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"}'
参数说明：交易ID。
返回值：交易信息。

wallet/gettransactioninfobyid(Odyssey-v3.2开始支持)
作用：根据id查询交易的fee，所在的block
demo: curl -X POST  http://127.0.0.1:8090/wallet/gettransactioninfobyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：Transaction的交易fee，所在block的高度，创建时间

/wallet/gettransactioncountbyblocknum(Odyssey-v3.2开始支持)
作用：查询特定block上transaction的个数
demo: curl -X POST  http://127.0.0.1:8090/wallet/gettransactioncountbyblocknum -d '{"num" : 100}'
参数说明：num是块的高度.
返回值e：transaction的个数.

wallet/getaccount
作用：查询一个账号的信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getaccount -d '{"address": "41E552F6487585C2B58BC2C9BB4492BC1F17132CD0"}'
参数说明：address 默认为hexString格式
返回值：Account对象

wallet/listwitnesses
作用：查询所有witness列表
demo: curl -X POST  http://127.0.0.1:8090/wallet/listwitnesses
参数说明：无
返回值：witness列表。

wallet/getassetissuelist
作用：查询所有token列表
demo: curl -X POST  http://127.0.0.1:8090/wallet/getassetissuelist
参数说明：无
返回值：token列表。

wallet/getpaginatedassetissuelist
作用：分页查询token列表
demo: curl -X POST  http://127.0.0.1:8090/wallet/getpaginatedassetissuelist -d '{"offset": 0, "limit": 10}'
参数说明：offset是起始Token的index，limit是期望返回的Token数量
返回值：token列表。

wallet/getpaginatedproposallist(Odyssey-v3.5开始支持)
作用：分页查询proposal列表
demo: curl -X POST  http://127.0.0.1:8090/wallet/getpaginatedproposallist -d '{"offset": 0, "limit": 10}'
参数说明：offset是起始Token的index，limit是期望返回的Token数量
返回值：token列表。

wallet/getpaginatedexchangelist(Odyssey-v3.2开始支持)
作用：分页查询交易对列表
demo: curl -X POST  http://127.0.0.1:8090/wallet/getpaginatedexchangelist -d '{"offset": 0, "limit":10}'
参数说明：offset是起始交易对的index，limit是期望返回的交易对数量
返回值：提案列表

wallet/getnextmaintenancetime
作用：获取下次统计投票的时间
demo: curl -X POST  http://127.0.0.1:8090/wallet/getnextmaintenancetime
参数说明：无
返回值：下次统计投票时间的毫秒数。

wallet/easytransferbyprivate
作用：快捷转账
demo: curl -X POST  http://127.0.0.1:8090/wallet/easytransferbyprivate -d '{"privateKey": "D95611A9AF2A2A45359106222ED1AFED48853D9A44DEFF8DC7913F5CBA727366", "toAddress":"4112E621D5577311998708F4D7B9F71F86DAE138B5","amount":10000}'
参数说明：
   privateKey：私钥，默认为hexString格式
   toAddress：转入账户地址，默认为hexString格式
   amount：转账的drop数量。
返回值：交易，含执行结果。
警告：该api有泄漏private key的风险，请确保在安全的环境中调用该api。

wallet/easytransferassetbyprivate
作用：快捷转账
demo: curl -X POST  http://127.0.0.1:8090/wallet/easytransferassetbyprivate -d '{"privateKey": "D95611A9AF2A2A45359106222ED1AFED48853D9A44DEFF8DC7913F5CBA727366", "toAddress":"4112E621D5577311998708F4D7B9F71F86DAE138B5",
"assetId": "1000001",
"amount":10000}'
参数说明：
   privateKey：私钥，默认为hexString格式
   toAddress：转入账户地址，默认为hexString格式
   assetId：通证ID。
   amount：转账的通证数量，单位是通证的最小单位。
返回值：交易，含执行结果。
警告：该api有泄漏private key的风险，请确保在安全的环境中调用该api。

wallet/generateaddress
作用：生成私钥和地址
demo: curl -X POST  http://127.0.0.1:8090/wallet/generateaddress
参数说明：无
返回值：地址和私钥
警告：该api有泄漏private key的风险，请确保在安全的环境中调用该api。

wallet/validateaddress
作用：检查地址是否正确
demo: curl -X POST  http://127.0.0.1:8090/wallet/validateaddress -d '{"address": "4189139CB1387AF85E3D24E212A008AC974967E561"}'
参数说明：地址，可以是base58checksum、hexString、base64格式
返回值：地址正确或者错误

wallet/deploycontract
作用：部署合约
demo: curl -X POST  http://127.0.0.1:8090/wallet/deploycontract -d '{"abi":"[{\"constant\":false,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"},{\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"}],\"name\":\"get\",\"outputs\":[{\"name\":\"value\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]","bytecode":"608060405234801561001057600080fd5b5060de8061001f6000396000f30060806040526004361060485763ffffffff7c01000000000000000000000000000000000000000000000000000000006000350416631ab06ee58114604d5780639507d39a146067575b600080fd5b348015605857600080fd5b506065600435602435608e565b005b348015607257600080fd5b50607c60043560a0565b60408051918252519081900360200190f35b60009182526020829052604090912055565b600090815260208190526040902054905600a165627a7a72305820fdfe832221d60dd582b4526afa20518b98c2e1cb0054653053a844cf265b25040029","parameter":"","call_value":100,"name":"SomeContract","consume_user_resource_percent":30,"fee_limit":10,"origin_energy_limit": 10,"owner_address":"41D1E7A6BC354106CB410E65FF8B181C600FF14292"}'
参数说明：
abi：abi
bytecode：bytecode，需要是hexString格式
parameter：构造函数的参数列表，需要按照ABI encoder编码后转话为hexString格式。如果构造函数没有参数，该参数可以不用设置。
consume_user_resource_percent：指定的使用该合约用户的资源占比，是[0, 100]之间的整数。如果是0，则表示用户不会消耗资源。如果开发者资源消耗完了，才会完全使用用户的资源。
fee_limit：最大消耗的SUN（1TRX = 1,000,000SUN）
call_value：本次调用往合约转账的SUN（1TRX = 1,000,000SUN）
owner_address：发起deploycontract的账户地址，默认为hexString格式
name：合约名
origin_energy_limit: 创建者设置的，在一次合约执行或创建过程中创建者自己消耗的最大的energy，是大于0的整数
call_token_value:本次调用往合约中转账10币的数量，如果不设置token_id，这项设置为0或者不设置
token_id:本次调用往合约中转账10币的id，如果没有，不需要设置
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：TransactionExtention, TransactionExtention中包含未签名的交易Transaction

wallet/triggersmartcontract
作用：调用合约
demo: curl -X POST  http://127.0.0.1:8090/wallet/triggersmartcontract -d '{"contract_address":"4189139CB1387AF85E3D24E212A008AC974967E561","function_selector":"set(uint256,uint256)","parameter":"00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000002","fee_limit":10,"call_value":100,"owner_address":"41D1E7A6BC354106CB410E65FF8B181C600FF14292"}'
参数说明：
contract_address，默认为hexString格式
function_selector，函数签名，不能有空格
parameter：调用参数[1,2]的虚拟机格式，使用remix提供的js工具，将合约调用者调用的参数数组[1,2]转化为虚拟机所需要的参数格式
fee_limit：最大消耗的SUN（1TRX = 1,000,000SUN）
call_value：本次调用往合约转账的SUN（1TRX = 1,000,000SUN）
owner_address：发起triggercontract的账户地址，默认为hexString格式
call_token_value:本次调用往合约中转账10币的数量，如果不设置token_id，这项设置为0或者不设置
token_id:本次调用往合约中转账10币的id，如果没有，不需要设置
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：TransactionExtention, TransactionExtention中包含未签名的交易Transaction

wallet/getcontract
作用：获取合约
demo: curl -X POST  http://127.0.0.1:8090/wallet/getcontract -d '{"value":"4189139CB1387AF85E3D24E212A008AC974967E561"}'
参数说明：
value：合约地址，默认为hexString格式
返回值：SmartContract，智能合约的内容

wallet/proposalcreate
作用：创建提案
demo: curl -X POST  http://127.0.0.1:8090/wallet/proposalcreate -d {"owner_address" : "419844F7600E018FD0D710E2145351D607B3316CE9","parameters":[{"key": 0,"value": 100000},{"key": 1,"value": 2}] }
参数说明：
owner_address：创建人地址
parameters：提案参数
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：创建提案的交易

wallet/getproposalbyid
作用：根据id查询提案
demo: curl -X POST  http://127.0.0.1:8090/wallet/getproposalbyid -d {"id":1}
参数说明：
id：提案id
返回值：提案详细信息

wallet/listproposals
作用：查询所有提案
demo: curl -X POST  http://127.0.0.1:8090/wallet/listproposals
参数说明：无
返回值：提案列表信息

wallet/proposalapprove
作用：提案批准
demo: curl -X POST  http://127.0.0.1:8090/wallet/proposalapprove -d {"owner_address" : "419844F7600E018FD0D710E2145351D607B3316CE9", "proposal_id":1, "is_add_approval":true}
参数说明：
owner_address：批准人地址，默认为hexString格式
proposal_id：提案id
is_add_approval：是否批准
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：批准提案的交易

wallet/proposaldelete
作用：删除提案
demo: curl -X POST  http://127.0.0.1:8090/wallet/proposaldelete -d {"owner_address" : "419844F7600E018FD0D710E2145351D607B3316CE9", "proposal_id":1}
参数说明：
owner_address：删除人的地址，只有提案所有人允许删除提案，默认为hexString格式
proposal_id：提案id
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：删除提案的交易

wallet/getaccountresource
作用：查询账户的资源信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getaccountresource -d {"address" : "419844f7600e018fd0d710e2145351d607b3316ce9"}
参数说明：
address：查询账户的地址，默认为hexString格式
返回值：账户的资源信息

wallet/exchangecreate
作用：创建交易对
demo：curl -X POST  http://127.0.0.1:8090/wallet/exchangecreate -d {"owner_address":"419844f7600e018fd0d710e2145351d607b3316ce9", 、
"first_token_id":token_a, "first_token_balance":100, "second_token_id":token_b,"second_token_balance":200}
参数说明：
first_token_id  ：第1种token的id，默认为hexString格式
first_token_balance：第1种token的balance
second_token_id ： 第2种token的id，默认为hexString格式
second_token_balance：第2种token的balance
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：创建交易对的transaction。

wallet/exchangeinject
作用：给交易对注资，注资后可以防止交易对价格波动太大
demo：curl -X POST  http://127.0.0.1:8090/wallet/exchangeinject -d {"owner_address":"419844f7600e018fd0d710e2145351d607b3316ce9", "exchange_id":1, "token_id":"74726f6e6e616d65", "quant":100}
参数说明：
owner_address：交易对创建者的地址，默认为hexString格式
exchange_id：交易对id
token_id： token的id，一般情况是token的name，默认为hexString格式
quant：注资token的数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：注资的transaction。

wallet/exchangewithdraw
作用：对交易对撤资，撤资后容易引起交易对价格波动太大。
demo：curl -X POST  http://127.0.0.1:8090/wallet/exchangewithdraw -d {"owner_address":"419844f7600e018fd0d710e2145351d607b3316ce9", "exchange_id":1, "token_id":"74726f6e6e616d65", "quant":100}
参数说明：
owner_address：是交易对创建者的地址，默认为hexString格式
exchange_id：交易对id
token_id： token的id，一般情况是token的name，需要是hexString格式
quant：撤资token的数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：撤资的transaction

wallet/exchangetransaction
作用：参与交易对交易。
demo：curl -X POST  http://127.0.0.1:8090/wallet/exchangetransaction -d {"owner_address":"419844f7600e018fd0d710e2145351d607b3316ce9", "exchange_id":1, "token_id":"74726f6e6e616d65", "quant":100,"expected":10}
参数说明：
owner_address：是交易对创建者的地址，默认为hexString格式
exchange_id：交易对id
token_id： 卖出的token的id，一般情况是token的name，默认为hexString格式
quant：卖出token的数量
expected：期望买入token的数量
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：token交易的transaction

wallet/getexchangebyid
作用：根据id查询交易对
demo：curl -X POST  http://127.0.0.1:8090/wallet/getexchangebyid -d {"id":1}
参数说明：
id：交易对id
返回值：交易对

wallet/listexchanges
作用：查询所有交易对
demo：curl -X POST  http://127.0.0.1:8090/wallet/listexchanges
参数说明：
返回值：所有交易对

wallet/getchainparameters
作用：查询所有交易对
demo：curl -X POST  http://127.0.0.1:8090/wallet/getchainparameters
参数说明：
返回值：区块链委员会可以设置的所有参数

wallet/updatesetting
作用：更新合约的consume_user_resource_percent
demo: curl -X POST  http://127.0.0.1:8090/wallet/updatesetting -d '{"owner_address": "419844f7600e018fd0d710e2145351d607b3316ce9", "contract_address": "41c6600433381c731f22fc2b9f864b14fe518b322f", "consume_user_resource_percent": 7}'
参数说明：
owner_address：是交易对创建者的地址，默认为hexString格式
contract_address：要修改的合约的地址，默认为hexString格式
consume_user_resource_percent：指定的使用该合约用户的资源占比
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：TransactionExtention, TransactionExtention中包含未签名的交易Transaction

wallet/updateenergylimit
作用：更新合约的origin_energy_limit
demo: curl -X POST  http://127.0.0.1:8090/wallet/updatesetting -d '{"owner_address": "419844f7600e018fd0d710e2145351d607b3316ce9", "contract_address": "41c6600433381c731f22fc2b9f864b14fe518b322f", "origin_energy_limit": 7}'
参数说明：
owner_address：是交易对创建者的地址，默认为hexString格式
contract_address：要修改的合约的地址，默认为hexString格式
origin_energy_limit：创建者设置的，在一次合约执行或创建过程中创建者自己消耗的最大的energy
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：TransactionExtention, TransactionExtention中包含未签名的交易Transaction

wallet/getdelegatedresource(Odyssey-v3.2开始支持)
作用：查看一个账户代理给另外一个账户的资源情况
demo: curl -X POST  http://127.0.0.1:8090/wallet/getdelegatedresource -d '
{
"fromAddress": "419844f7600e018fd0d710e2145351d607b3316ce9",
"toAddress": "41c6600433381c731f22fc2b9f864b14fe518b322f"
}'
参数说明：
fromAddress：是要查询的账户地址，默认为hexString格式
toAddress：代理对象的账户地址，默认为hexString格式
返回值：账户的资源代理的列表，列表的元素为DelegatedResource

wallet/getdelegatedresourceaccountindex(Odyssey-v3.2开始支持)
作用：查看一个账户给哪些账户代理了资源
demo: curl -X POST  http://127.0.0.1:8090/wallet/getdelegatedresourceaccountindex -d '
{
"value": "419844f7600e018fd0d710e2145351d607b3316ce9",
}'
参数说明：
value：是要查询的账户地址，默认为hexString格式
返回值：账户的资源代理概况，结构为DelegatedResourceAccountIndex

wallet/getnodeinfo(Odyssey-v3.2.2开始支持)
作用：查看节点的信息
demo: curl  http://127.0.0.1:8090/wallet/getnodeinfo
返回值：节点当前状态的相关信息(省略)

wallet/setaccountid
作用：设置一个账户的accountID
demo: curl -X POST  http://127.0.0.1:8090/wallet/setaccountid -d '{
"owner_address":"41a7d8a35b260395c14aa456297662092ba3b76fc0","account_id":"6161616162626262"}'
参数说明：
owner_address：是交易对创建者的地址，默认为hexString格式
account_id accountid,默认为hexString格式
返回值:设置AccountID的transaction

wallet/getaccountbyid
作用：通过accountId查询一个账号的信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getaccountbyid -d
'{"account_id":"6161616162626262"}'
参数说明：account_id 默认为hexString格式
返回值：Account对象

wallet/getdeferredtransactionbyid
作用：通过交易id查询延迟交易
demo: curl -X POST  http://127.0.0.1:8090/wallet/getdeferredtransactionbyid -d '{"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"}'
参数说明：交易ID。
返回值：交易信息。

wallet/canceldeferredtransactionbyid
作用：创建取消延迟交易对象
demo: curl -X POST  http://127.0.0.1:8090/wallet/canceldeferredtransactionbyid -d '{
"transactionId":"34e6b6497b71100756790a7f20cd729376768dd2bebb6a4a9c5e87b920d5de10",
"ownerAddress":"41a7d8a35b260395c14aa456297662092ba3b76fc0"}'
参数说明：
owner_address：取消交易账户的地址，同时也是发起交易的账户地址，默认为hexString格式
transactionId:交易ID
返回值：交易对象。

wallet/getdeferredtransactioninfobyid
作用：根据id查询延迟交易的fee，所在的block
demo: curl -X POST  http://127.0.0.1:8090/wallet/getdeferredtransactioninfobyid -d '{"value" : "309b6fa3d01353e46f57dd8a8f27611f98e392b50d035cef213f2c55225a8bd2"}'
参数说明：value是交易id
返回值：Transaction的交易fee，所在block的高度，创建时间

wallet/triggerconstantcontract
作用：调用常量合约，产生的交易不上链
demo: curl -X POST  http://127.0.0.1:8090/wallet/triggerconstantcontract -d '{"contract_address":"4189139CB1387AF85E3D24E212A008AC974967E561","function_selector":"set(uint256,uint256)","parameter":"00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000002","fee_limit":10,"call_value":100,"owner_address":"41D1E7A6BC354106CB410E65FF8B181C600FF14292"}'
参数说明：
contract_address，默认为hexString格式
function_selector，函数签名，不能有空格
parameter：调用参数[1,2]的虚拟机格式，使用remix提供的js工具，将合约调用者调用的参数数组[1,2]转化为虚拟机所需要的参数格式
fee_limit：最大消耗的SUN（1TRX = 1,000,000SUN）
owner_address：发起triggercontract的账户地址，默认为hexString格式
可选参数Permission_id，多重签名时使用，设置交易多重签名时使用的permissionId
返回值：TransactionExtention, TransactionExtention中包含未签名的交易Transaction

wallet/clearabi
作用：创建清除智能合约ABI的交易对象
demo: curl -X POST  http://127.0.0.1:8090/wallet/clearabi -d '{
"owner_address":"41a7d8a35b260395c14aa456297662092ba3b76fc0",
"contract_address":"417bcb781f4743afaacf9f9528f3ea903b3782339f"}'
参数说明：
owner_address：创建合约的账户地址，默认为hexString格式
contract_address：合约地址,默认为hexString
返回值:交易对象

wallet/addtransactionsign
作用：给交易签名，支持多重签名
demo: curl -X POST  http://127.0.0.1:8090/wallet/addtransactionsign -d '{
    "transaction": {
        "visible": true,
        "txID": "752cece5a68e40e30eaeeb4c5844b3f4b004d23485ccef42e0609a9a90eeb675",
        "raw_data": {
            "contract": [{
                "parameter": {
                    "value": {
                        "data": "a9059cbb0000000000000000000000415a523b449890854c8fc460ab602df9f31fe4293f00000000000000000000000000000000000000000000000000000000000001f4",
                        "owner_address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
                        "contract_address": "TVf6sdWWu8zvmtcWfZerMDefoptiVhbhXC"
                    },
                    "type_url": "type.googleapis.com/protocol.TriggerSmartContract"
                },
                "type": "TriggerSmartContract"
            }],
            "ref_block_bytes": "0883",
            "ref_block_hash": "84c32fcee77f6be7",
            "expiration": 1556449785000,
            "fee_limit": 10000,
            "timestamp": 1556449725625
        },
        "raw_data_hex": "0a020883220884c32fcee77f6be740a8e98b9da62d5aae01081f12a9010a31747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e54726967676572536d617274436f6e747261637412740a1541a7d8a35b260395c14aa456297662092ba3b76fc0121541d7f5e9b3b997006444c1646ecfae6549b5737e622244a9059cbb0000000000000000000000415a523b449890854c8fc460ab602df9f31fe4293f00000000000000000000000000000000000000000000000000000000000001f470b999889da62d9001904e"
    },
    "privateKey": "950139607044677436d29ff1ea2900c9402f783a91547cdc47cf706f1129c76a"
    }'
参数说明：
transaction：交易对象
privateKey： owner_address对应的私钥，hexString格式，存在泄漏私钥的风险。
返回值:签名后的交易对象

wallet/getsignweight
作用：查询多重签名的交易的相关信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getsignweight -d '{
    "signature": [
        "e0bd4a60f1b3c89d4da3894d400e7e32385f6dd690aee17fdac4e016cdb294c5128b66f62f3947a7182c015547496eba95510c113bda2a361d811b829343c36501",
        "596ead6439d0f381e67f30b1ed6b3687f2bd53ce5140cdb126cfe4183235804741eeaf79b4e91f251fd7042380a9485d4d29d67f112d5387bc7457b355cd3c4200"
    ],
    "txID": "0ae84a8439f5aa8fd2c458879a4031a7452aebed8e6e99ffbccd26842d4323c4",
    "raw_data": {
        "contract": [{
            "parameter": {
                "value": {
                    "amount": 1000000,
                    "owner_address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
                    "to_address": "TJCnKsPa7y5okkXvQAidZBzqx3QyQ6sxMW"
                },
                "type_url": "type.googleapis.com/protocol.TransferContract"
            },
            "type": "TransferContract"
        }],
        "ref_block_bytes": "163d",
        "ref_block_hash": "77ef4ace148b05ba",
        "expiration": 1555664823000,
        "timestamp": 1555664763418
    },
    "raw_data_hex": "0a02163d220877ef4ace148b05ba40d8c5e5a6a32d5a69080112630a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412320a1541a7d8a35b260395c14aa456297662092ba3b76fc01215415a523b449890854c8fc460ab602df9f31fe4293f18c0843d2802709af4e1a6a32d",
    "visible": true}'
参数说明：
参数整体是一个完整的交易
返回值:已签名权重是否达到阈值（即是否满足验签标准），签名地址列表，permission的详细信息，已签名的权重及交易信息。

wallet/getapprovedlist
作用：查询多重签名的交易的相关信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getapprovedlist -d '{
    "signature": [
        "e0bd4a60f1b3c89d4da3894d400e7e32385f6dd690aee17fdac4e016cdb294c5128b66f62f3947a7182c015547496eba95510c113bda2a361d811b829343c36501",
        "596ead6439d0f381e67f30b1ed6b3687f2bd53ce5140cdb126cfe4183235804741eeaf79b4e91f251fd7042380a9485d4d29d67f112d5387bc7457b355cd3c4200"
    ],
    "txID": "0ae84a8439f5aa8fd2c458879a4031a7452aebed8e6e99ffbccd26842d4323c4",
    "raw_data": {
        "contract": [{
            "parameter": {
                "value": {
                    "amount": 1000000,
                    "owner_address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
                    "to_address": "TJCnKsPa7y5okkXvQAidZBzqx3QyQ6sxMW"
                },
                "type_url": "type.googleapis.com/protocol.TransferContract"
            },
            "type": "TransferContract"
        }],
        "ref_block_bytes": "163d",
        "ref_block_hash": "77ef4ace148b05ba",
        "expiration": 1555664823000,
        "timestamp": 1555664763418
    },
    "raw_data_hex": "0a02163d220877ef4ace148b05ba40d8c5e5a6a32d5a69080112630a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412320a1541a7d8a35b260395c14aa456297662092ba3b76fc01215415a523b449890854c8fc460ab602df9f31fe4293f18c0843d2802709af4e1a6a32d",
    "visible": true}'
参数整体是一个完整的交易
返回值:已签名权重是否达到阈值（即是否满足验签标准），签名地址列表，交易信息。

wallet/accountpermissionupdate
作用：为账户创建多重签名
demo: curl -X POST  http://127.0.0.1:8090/wallet/accountpermissionupdate -d
'{
    "owner_address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
    "owner": {
        "type": 0,
        "permission_name": "owner",
        "threshold": 1,
        "keys": [{
            "address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
            "weight": 1
        }]
    },
    "witness": {
        "type": 1,
        "permission_name": "witness",
        "threshold": 1,
        "keys": [{
            "address": "TRGhNNfnmgLegT4zHNjEqDSADjgmnHvubJ",
            "weight": 1
        }]
    },
    "actives": [{
        "type": 2,
        "permission_name": "active12323",
        "threshold": 2,
        "operations": "7fff1fc0033e0000000000000000000000000000000000000000000000000000",
        "keys": [{
            "address": "TNhXo1GbRNCuorvYu5JFWN3m2NYr9QQpVR",
            "weight": 1
        }, {
            "address": "TKwhcDup8L2PH5r6hxp5CQvQzZqJLmKvZP",
            "weight": 1
        }]
    }],
    "visible": true}'
参数说明：
owner_address：创建合约的账户地址，默认为hexString格式
owner：账户owner权限的分配信息
witness：出块权限的分配信息，如果不是witness，不需要设置
actives：其他功能权限的分配信息
返回值:账户创建多重签名的transaction

wallet/getexpandedspendingkey
作用：获取expanded spending keys
demo: curl -X POST  http://127.0.0.1:8090/wallet/getexpandedspendingkey -d
'{
    "value": "06b02aaa00f230b0887ff57a6609d76691369972ac3ba568fe7a8a0897fce7c4"
}'
参数说明：
value：Spending key
返回值： Expanded spending keys. 由三个key组成，分别是 ask、 nsk和ovk.

wallet/getakfromask
作用：从ask获得ak
demo: curl -X POST  http://127.0.0.1:8090/wallet/getakfromask -d
'{
    "value": "653b3a3fdd40b60d2f53ba121df8840f6590384993f8fa9a0ecb0dfb23496604"
}'
参数说明：
value：Ask
返回值：Ak

wallet/getnkfromnsk
作用：从nsk获得nk
demo: curl -X POST  http://127.0.0.1:8090/wallet/getnkfromnsk -d
'{
    "value": "428ff3c9e101dc1fca08f7b0e3387b23b68016746ae565aefc19d112b696db01"
}'
参数说明：
value：Nsk
返回值：Nk

wallet/getspendingkey
作用：获得spending key
demo: curl -X GET  http://127.0.0.1:8090/wallet/getspendingkey
参数说明：无
返回值：Spending key

wallet/getdiversifier
作用：To get diversifier
demo: curl -X GET  http://127.0.0.1:8090/wallet/getdiversifier
参数说明：无
返回值: Diversifier

wallet/getincomingviewingkey
作用：获得incoming viewing key
demo: curl -X POST  http://127.0.0.1:8090/wallet/getincomingviewingkey -d
'{
    "ak":"b443f1a303ef5837ba95750b48b6fef15f9c77f63a8c28c161bcd6613f423b5c",
    "nk":"632137e69179df3d10e252fcce85d13464c3163fe7a619edf8d43ebefa8162d9"
 }'
参数说明：
ak：Ak
nk：Nk
返回值：Incoming viewing key

wallet/getzenpaymentaddress
作用：获得支付地址
demo: curl -X POST  http://127.0.0.1:8090/wallet/getzenpaymentaddress -d
'{
    "ivk":"8c7852e10862d8eec058635974f70f24c1f8d73819131bb5b54028d0a9408a03",
    "d":"736ba8692ed88a5473e009"
 }'
参数说明：
ivk：Ivk
d：D
返回值：支付地址

wallet/createshieldedtransactionwithoutspendauthsig
作用：To create shielded transaction without using ask
demo: curl -X POST  http://127.0.0.1:8090/wallet/createshieldedtransactionwithoutspendauthsig -d
'{
    "ivk":"8c7852e10862d8eec058635974f70f24c1f8d73819131bb5b54028d0a9408a03",
    "d":"736ba8692ed88a5473e009"
 }'
参数说明：
transparent_from_address：透明发送者的地址
from_amount：从透明地址转出的数额
ask：Ask
nsk：Nsk
ovk：Ovk
shielded_receives：匿名接收者信息
shieldedSpends：匿名发送者信息
transparent_to_address：透明接收者地址
to_amount：转入透明地址的数额
返回值：交易对象

wallet/createshieldedtransactionwithoutspendauthsig
作用：创建匿名交易（不需要提供ask）
demo: curl -X POST  http://127.0.0.1:8090/wallet/createshieldedtransactionwithoutspendauthsig -d
'{
    "ak": "bf051629fd8122cd9dd8591d72947b026c214cf7cdac1f68eff97179727d38e9",
    "nsk": "42963d26af8122204273fa3489d9efd6babf1f7179ff193c955a1f3d9c2df10c",
    "ovk": "bc9848a83966709655b12efadc9e978785858316045e0115a0e72567a9a2a823",
    "shielded_spends": [
        {
            "note": {
                "value": 500000000,
                "payment_address": "ztron1jld8fmvujrz2vgkc867zuwklmewy4ypw0wtwgweqs2paee0uhc8f3azy90el770arksa2kunl02",
                "rcm": "723053bcbfecdf5da66c18ab0376476ef308c61b7abe891b2c01e903bcb87c0e"
            },
            "alpha": "2608999c3a97d005a879ecdaa16fd29ae434fb67b177c5e875b0c829e6a1db04",
            "voucher": {
                "tree": {
                    "left": {
                        "content": "a3d5c9b2db9699f32afec5febbd5586ce9ff33a0bef6fee5691028313b8e1f6a"
                    },
                    "parents": [
                        {
                            "content": "d9c38484296b3aa8f5e8b59d418a3775e2bb414e75498ad352e4614f05aae548"
                        },
                        {
                            "content": "d0420777afdc4151c3f14fbe4c714d82dc15873edb1ca65ebb3887334a4bae15"
                        }
                    ]
                },
                "rt": "fb1115d5ddd16c5427c3a608d6b5add5967e70f51c890307c6142083a2c28565"
            },
            "path": "2020b2eed031d4d6a4f02a097f80b54cc1541d4163c6b6f5971f88b6e41d35c538142012935f14b676509b81eb49ef25f39269ed72309238b4c145803544b646dca62d20e1f34b034d4a3cd28557e2907ebf990c918f64ecb50a94f01d6fda5ca5c7ef722028e7b841dcbc47cceb69d7cb8d94245fb7cb2ba3a7a6bc18f13f945f7dbd6e2a20a5122c08ff9c161d9ca6fc462073396c7d7d38e8ee48cdb3bea7e2230134ed6a20d2e1642c9a462229289e5b0e3b7f9008e0301cbb93385ee0e21da2545073cb582016d6252968971a83da8521d65382e61f0176646d771c91528e3276ee45383e4a20fee0e52802cb0c46b1eb4d376c62697f4759f6c8917fa352571202fd778fd712204c6937d78f42685f84b43ad3b7b00f81285662f85c6a68ef11d62ad1a3ee0850200769557bc682b1bf308646fd0b22e648e8b9e98f57e29f5af40f6edb833e2c492008eeab0c13abd6069e6310197bf80f9c1ea6de78fd19cbae24d4a520e6cf3023208d5fa43e5a10d11605ac7430ba1f5d81fb1b68d29a640405767749e841527673206aca8448d8263e547d5ff2950e2ed3839e998d31cbc6ac9fd57bc6002b15921620cd1c8dbf6e3acc7a80439bc4962cf25b9dce7c896f3a5bd70803fc5a0e33cf00206edb16d01907b759977d7650dad7e3ec049af1a3d875380b697c862c9ec5d51c201ea6675f9551eeb9dfaaa9247bc9858270d3d3a4c5afa7177a984d5ed1be245120d6acdedf95f608e09fa53fb43dcd0990475726c5131210c9e5caeab97f0e642f20bd74b25aacb92378a871bf27d225cfc26baca344a1ea35fdd94510f3d157082c201b77dac4d24fb7258c3c528704c59430b630718bec486421837021cf75dab65120ec677114c27206f5debc1c1ed66f95e2b1885da5b7be3d736b1de98579473048204777c8776a3b1e69b73a62fa701fa4f7a6282d9aee2c7a6b82e7937d7081c23c20ba49b659fbd0b7334211ea6a9d9df185c757e70aa81da562fb912b84f49bce722043ff5457f13b926b61df552d4e402ee6dc1463f99a535f9a713439264d5b616b207b99abdc3730991cc9274727d7d82d28cb794edbc7034b4f0053ff7c4b68044420d6c639ac24b46bd19341c91b13fdcab31581ddaf7f1411336a271f3d0aa52813208ac9cf9c391e3fd42891d27238a81a8a5c1d3a72b1bcbea8cf44a58ce738961320912d82b2c2bca231f71efcf61737fbf0a08befa0416215aeef53e8bb6d23390a20e110de65c907b9dea4ae0bd83a4b0a51bea175646a64c12b4c9f931b2cb31b4920d8283386ef2ef07ebdbb4383c12a739a953a4d6e0d6fb1139a4036d693bfbb6c20d0420777afdc4151c3f14fbe4c714d82dc15873edb1ca65ebb3887334a4bae1520d9c38484296b3aa8f5e8b59d418a3775e2bb414e75498ad352e4614f05aae5482001000000000000000000000000000000000000000000000000000000000000000600000000000000"
        }
    ],
    "shielded_receives": [
        {
            "note": {
                "value": 40000000,
                "payment_address": "ztron1wd46s6fwmz99gulqpxul6zffqtevzfpl93ng3s5834fhwf6e7w5l6zmjhmpvtwsc4wxa7dusmvr",
                "rcm": "ccced07d36641fc93cba33cddda7064cb82f6962a0bdf15a4240a4a742770e03"
            }
        }
    ]
}'
参数说明：
transparent_from_address：透明发送者的地址
from_amount：从透明地址转出的数额
ak：Ak
nsk：Nsk
ovk：Ovk
shielded_receives：匿名接收者信息
shieldedSpends：匿名发送者信息
transparent_to_address：透明接收者地址
to_amount：转入透明地址的数额
返回值：交易对象

wallet/scannotebyivk
作用：查询与ivk相关的所有的notes
demo: curl -X POST  http://127.0.0.1:8090/wallet/scannotebyivk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ivk": "80a481c3c739e54b4e0608090b3a1a6e9f8dce42346e95bf5a2d8a487bf45c05"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ivk：Incoming viewing key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）

- /wallet/scanandmarknotebyivk
作用：查询与ivk相关的所有的notes, 包含是否花费状态
demo: curl -X POST  http://127.0.0.1:8090/wallet/scanandmarknotebyivk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ivk": "80a481c3c739e54b4e0608090b3a1a6e9f8dce42346e95bf5a2d8a487bf45c05",
    "ak": "1d4f9b5551f4aa9443ceb263f0e208eb7e26080264571c5ef06de97a646fe418",
    "nk": "748522c7571a9da787e43940c9a474aa0c5c39b46c338905deb6726fa3678bdb"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ivk：Incoming viewing key
ak：Ak key
nk：Nk key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）

wallet/scannotebyovk
作用：查询与ovk相关的所有的notes
demo: curl -X POST  http://127.0.0.1:8090/wallet/scannotebyovk -d
'{
    "start_block_index": 0,
    "end_block_index": 100,
    "ovk": "705145aa18cbe6c11d5d0011419a98f3d5b1d341eb4727f1315597f4bdaf8539"
}'
参数说明：
start_block_index：开始区块高度，包含自身
end_block_index：结束区块高度，不包含自身
ovk：Outgoing viewing key
返回值：Notes列表
注意：区间限制（end_block_index - start_block_index <= 1000）

wallet/getrcm
作用：获得一个rcm
demo: curl -X GET  http://127.0.0.1:8090/wallet/getrcm
参数说明：无
返回值：rcm

wallet/getmerkletreevoucherinfo
作用：获得一个note的默克尔树信息
demo: curl -X POST  http://127.0.0.1:8090/wallet/getmerkletreevoucherinfo -d
'{
    "out_points":[{
        "hash":"185b3e085723f5862b3a3c3cf54d52f5c1eaf2541e3a1e0ecd08bc12cd958d74",
        "index":0
    }]
}'
参数说明：
out_points：Note信息
返回值：一个note的默克尔树信息

wallet/isspend
作用：查询一个note是否已经被花费
demo: curl -X POST  http://127.0.0.1:8090/wallet/isspend -d
'{
    "ak": "a3e65d509b675aaa2aeda977ceff11eebd76218079b6f543d78a615e396ca129",
    "nk": "62cfda9bea09a53cf2a21022057913734a8458969e11e0bb9c59ead48fbce83e",
    "note": {
        "payment_address": "ztron1aqgauawtkelxfu2w6s48cwh0mchjt6kwpj44l4wym3pullx0294j4r4v7kpm75wnclzycsw73mq",
        "rcm": "74a16c1b27ec7fbf06881d9d35ddaab1554838b1bddcd54f6bd8a9fb4ba0b80a",
        "value": 500000000
    },
    "txid": "7d09e471bb047d3ac044d5d6691b3721a2dddbb683ac02c207fbe78af6302463",
    "index": 1
}'
参数说明：
ak：Ak
nk：Nk
note：Note信息
txid：交易id
index：Note索引
返回值：一个note是否已经被花费状态

wallet/createspendauthsig
作用：为一个交易创建一个签名
demo: curl -X POST  http://127.0.0.1:8090/wallet/createspendauthsig -d
'{
    "ask": "e3ebcba1531f6d9158d9c162660c5d7c04dadf77d85d7436a9c98b291ff69a09",
    "tx_hash": "3b78fee6e956f915ffe082284c5f18640edca9c57a5f227e5f7d7eb65ad61502",
    "alpha": "2608999c3a97d005a879ecdaa16fd29ae434fb67b177c5e875b0c829e6a1db04"
}'
参数说明：
ask：Ask
tx_hash：交易哈希
alpha：Alpha
返回值：签名

wallet/createshieldnullifier
作用：To create a shielded nullifier
demo: curl -X POST  http://127.0.0.1:8090/wallet/createshieldnullifier -d
'{
    "note": {
        "payment_address": "ztron1aqgauawtkelxfu2w6s48cwh0mchjt6kwpj44l4wym3pullx0294j4r4v7kpm75wnclzycsw73mq",
        "rcm": "74a16c1b27ec7fbf06881d9d35ddaab1554838b1bddcd54f6bd8a9fb4ba0b80a",
        "value": 500000000
    },
    "voucher": {
        "tree": {
            "left": {
                "content": "a4d763fae3fee78964ccdf7567ec3062c95a5b97825d731202d3dfa6cb01c143"
            }
        },
        "rt": "7dc3652c2a16e8518a8be0e3e038f9d28c3eb96f13e8da8acc2a9b650702f33e"
    },
    "ak": "a3e65d509b675aaa2aeda977ceff11eebd76218079b6f543d78a615e396ca129",
    "nk": "62cfda9bea09a53cf2a21022057913734a8458969e11e0bb9c59ead48fbce83e"
}'
参数说明：
note：Note信息
voucher：Voucher信息
ak：Ak
nk：Nk
返回值：匿名的nullifier

wallet/getshieldtransactionhash
作用：获得一笔匿名交易的hash
demo: curl -X POST  http://127.0.0.1:8090/wallet/getshieldtransactionhash -d
'{
    "txID": "de639a64497d86bb27e34a2953093a0cc488ec4c7bc9624ac5857d3799748595",
    "raw_data": {
        "contract": [
            {
                "parameter": {
                    "value": {
                        "binding_signature": "2b8ae5e11ecad3e6946f54b7ad513bd8692a3edae72d29e266b28e47c9b37ccdb38e3b6433575694b6681136b1734f85afcfe672061d2ee7368755ad0b96a80b",
                        "spend_description": [
                            {
                                "value_commitment": "cbe1063adbe7e10919421fa6133f03150253913f5aff02d165e2c019cea4a869",
                                "anchor": "fb1115d5ddd16c5427c3a608d6b5add5967e70f51c890307c6142083a2c28565",
                                "nullifier": "93e329d464e1dbddc8bb4d2dcc939a796dfe11e985d4e9033a15edf0e3df4f35",
                                "rk": "10c702d6dff1509502ee5acc0b01d4b4531b2ff53b0dd54488aea6031b5e6d16",
                                "zkproof": "abf64b3beacfd873b1db764c3da9f739993518f3f740e761cb8af60682b7171892895c3ccfb550c3cf757e906dbf5313a3676b8226b0b84960f76a185c8d3fdfc3fa9c08479a704852d7b3dfeb913cf13e01c25657561e00a06c61e7c65b50b812902ddc4f17bfe2bcb2f247c2dc6132d0f0e0abcecc0332fdd99077af10d07bbdb88c4fd257948428e233c57f84eee8b2eeab2162c1aeccf2e1dfaa306d5803a8b2d281a549440fbd5a3657a830c1ca07a384cea446aa077b195b29b23023b1"
                            }
                        ],
                        "receive_description": [
                            {
                                "value_commitment": "f6d45db8ec5a1c8dbbde040b4ea138efbe8db2d0597ed2306ff3fdd0620b3c5a",
                                "note_commitment": "ec3f5472ac8114a9a07987d1c2a0e1254504e352d9574971e77084293900312e",
                                "epk": "719eeb5ebaeeccc55c9f0d73767aadf0c0513603400ccb50bd789637d984b8e6",
                                "c_enc": "3a6c4fe0e79f5b23fed34a419c4728d0b26bca23180a22871743b0a9444c27663cf07c55a0ea6db504d70421768bf17384e180b2ad8b8be88ff5cf662c53a4ba086effc3a4b1df39265f71dfac884bff5a69e1dcdcae8aecf6ae443168ffab692a5c1e4908b415dd830dcf6432fae1c32461132080da74d6b83d3d00887eb2ce9965a749f8d8410ea4182969371ac2fd5e0e74d27d883492a08e6209cd9959d74bb67c2a9fe7faac5a4777f1bff19cf0b6398a2faa9b194bbb93d60f132f382f7d693a722e8cbca1da084ee7e0c371397419a7259d1fa0943078cfe5ea352e4b53907bb6c04ca8ad409fb0ae0b110a6b312200e21ab79d543ae7aeb16802cf87afdac1e8954038caa42818f4ca2847fd642360c098accfeeade4abd1cc9ca3315a4336be224ba3516973c7dae3f41875457236675993df38d3a544470c4f9335d77b005e6a9aec40fd881b34852ec9bbbcc3d24ee92930eae770a5462ce04c4e37b0524ef07e00e8d58c810d6aefb19fa7bc2c3a2fdfab6dd4fe73dbecc0795a280f9b7ca35cc8bc1062aed8e26bd81ba33c6f4c318974636f6d796723e77772ced3dbc1f42afec6fc9bb61f8beac704affea9baf2e2de226250c1d427c7d78b1eb1d239e1f3eb6af0f017b80541333f4fce17340048d826b9b0be8477c996ad8bfc3440dc686fdff6d0d63986db4d95962d7977289cbfd14c745de7c79d4dc0bcd220e5b4ced5b409e79142e0f336e44ca29a9a87f6f43707d8c4936e895236dd2b393a478a8bc27b1f682496ba84a0ddc549da06cb7855c4d8680dc66ac40240733b7f",
                                "c_out": "50be6e77854d4c427b2af4f16e5275f0b0c206b3ea2d2a24ffb287ea356f323523354cd83d15e7c48e6f1fa103dfca3d49ca2263dbb0cd8bfb35d72cdcad1351de6fba7a30aea27184a68bcda19cc6da",
                                "zkproof": "a4e6c50d5753092d005689922c2bdeafc98775bce59db840974163ace23c13fec18112e32aae1c39842c645ed172ad8fa277e63c1e3d6d7fb12eb15d56b573237b776f562a81d0e6be362d147d8604fdfec421482270ca82950de1883fda06e719f5d256d7a039769bffc570a1778d70c17295d1c0336a6ae0903d2460dc139a9563c2d40f37bffefa73003a55af1ff0861b6f79ef40099b6a0cb25ab3f40727210e4629647d0711abff125712a5f0d64fcb6e6a6b0b34478d7da0552b493a80"
                            }
                        ]
                    },
                    "type_url": "type.googleapis.com/protocol.ShieldedTransferContract"
                },
                "type": "ShieldedTransferContract"
            }
        ],
        "ref_block_bytes": "0d59",
        "ref_block_hash": "7356ce5c35d8265e",
        "expiration": 1559237283000,
        "timestamp": 1559201285590
    },
    "raw_data_hex": "0a020d5922087356ce5c35d8265e40b899a3ceb02d5a940b0833128f0b0a35747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e536869656c6465645472616e73666572436f6e747261637412d50a1acb020a20cbe1063adbe7e10919421fa6133f03150253913f5aff02d165e2c019cea4a8691220fb1115d5ddd16c5427c3a608d6b5add5967e70f51c890307c6142083a2c285651a2093e329d464e1dbddc8bb4d2dcc939a796dfe11e985d4e9033a15edf0e3df4f35222010c702d6dff1509502ee5acc0b01d4b4531b2ff53b0dd54488aea6031b5e6d162ac001abf64b3beacfd873b1db764c3da9f739993518f3f740e761cb8af60682b7171892895c3ccfb550c3cf757e906dbf5313a3676b8226b0b84960f76a185c8d3fdfc3fa9c08479a704852d7b3dfeb913cf13e01c25657561e00a06c61e7c65b50b812902ddc4f17bfe2bcb2f247c2dc6132d0f0e0abcecc0332fdd99077af10d07bbdb88c4fd257948428e233c57f84eee8b2eeab2162c1aeccf2e1dfaa306d5803a8b2d281a549440fbd5a3657a830c1ca07a384cea446aa077b195b29b23023b122c2070a20f6d45db8ec5a1c8dbbde040b4ea138efbe8db2d0597ed2306ff3fdd0620b3c5a1220ec3f5472ac8114a9a07987d1c2a0e1254504e352d9574971e77084293900312e1a20719eeb5ebaeeccc55c9f0d73767aadf0c0513603400ccb50bd789637d984b8e622c4043a6c4fe0e79f5b23fed34a419c4728d0b26bca23180a22871743b0a9444c27663cf07c55a0ea6db504d70421768bf17384e180b2ad8b8be88ff5cf662c53a4ba086effc3a4b1df39265f71dfac884bff5a69e1dcdcae8aecf6ae443168ffab692a5c1e4908b415dd830dcf6432fae1c32461132080da74d6b83d3d00887eb2ce9965a749f8d8410ea4182969371ac2fd5e0e74d27d883492a08e6209cd9959d74bb67c2a9fe7faac5a4777f1bff19cf0b6398a2faa9b194bbb93d60f132f382f7d693a722e8cbca1da084ee7e0c371397419a7259d1fa0943078cfe5ea352e4b53907bb6c04ca8ad409fb0ae0b110a6b312200e21ab79d543ae7aeb16802cf87afdac1e8954038caa42818f4ca2847fd642360c098accfeeade4abd1cc9ca3315a4336be224ba3516973c7dae3f41875457236675993df38d3a544470c4f9335d77b005e6a9aec40fd881b34852ec9bbbcc3d24ee92930eae770a5462ce04c4e37b0524ef07e00e8d58c810d6aefb19fa7bc2c3a2fdfab6dd4fe73dbecc0795a280f9b7ca35cc8bc1062aed8e26bd81ba33c6f4c318974636f6d796723e77772ced3dbc1f42afec6fc9bb61f8beac704affea9baf2e2de226250c1d427c7d78b1eb1d239e1f3eb6af0f017b80541333f4fce17340048d826b9b0be8477c996ad8bfc3440dc686fdff6d0d63986db4d95962d7977289cbfd14c745de7c79d4dc0bcd220e5b4ced5b409e79142e0f336e44ca29a9a87f6f43707d8c4936e895236dd2b393a478a8bc27b1f682496ba84a0ddc549da06cb7855c4d8680dc66ac40240733b7f2a5050be6e77854d4c427b2af4f16e5275f0b0c206b3ea2d2a24ffb287ea356f323523354cd83d15e7c48e6f1fa103dfca3d49ca2263dbb0cd8bfb35d72cdcad1351de6fba7a30aea27184a68bcda19cc6da32c001a4e6c50d5753092d005689922c2bdeafc98775bce59db840974163ace23c13fec18112e32aae1c39842c645ed172ad8fa277e63c1e3d6d7fb12eb15d56b573237b776f562a81d0e6be362d147d8604fdfec421482270ca82950de1883fda06e719f5d256d7a039769bffc570a1778d70c17295d1c0336a6ae0903d2460dc139a9563c2d40f37bffefa73003a55af1ff0861b6f79ef40099b6a0cb25ab3f40727210e4629647d0711abff125712a5f0d64fcb6e6a6b0b34478d7da0552b493a802a402b8ae5e11ecad3e6946f54b7ad513bd8692a3edae72d29e266b28e47c9b37ccdb38e3b6433575694b6681136b1734f85afcfe672061d2ee7368755ad0b96a80b70d68b8ebdb02d"
}'
参数说明：
transaction：交易对象
返回值：一笔匿名交易的hash

wallet/createshieldedtransaction
作用：创建匿名交易
请参照：[示例](Tron-shielded-transaction.md)
参数说明：
transparent_from_address：透明发送者的地址
from_amount：从透明地址转出的数额
ask：Ask
nsk：Nsk
ovk：Ovk
shielded_receives：匿名接收者信息
shieldedSpends：匿名发送者信息
transparent_to_address：透明接收者地址
to_amount：转入透明地址的数额
返回值：交易对象

wallet/getnewshieldedaddress

作用: 获得shieldedAddress
demo: curl -X GET  http://127.0.0.1:8090/wallet/getnewshieldedaddress
参数说明: 无
返回值: Spending key
返回值: Ask key
返回值: Nsk key
返回值: Outgoing viewing key
返回值: Ak Key
返回值: Nk key
返回值: incoming viewing key
返回值: Diversifier
返回值: pkD
返回值: payment address
```
