# 西瓜SDK 支付通知接口

<div id="doc"></div>

## 1. 快速接入
西瓜SDK通知游戏服务器支付成功：

	通知URL：由游戏方提供（如：http://172.63.55.62:18888/moon/pay）
	POST Body为JSON字符串：
	{
	    "channelId": "mi",
	    "customInfo": "2323423413412351251245",
	    "gameTradeNo": "99887766",
	    "paidAmount": "9800",
	    "paidTime": "20150723150128",
	    "payStatus": "1",
	    "productDesc": "productDesc1",
	    "productId": "productId1",
	    "productName": "productName1",
	    "productQuantity": "1",
	    "roleId": "224455",
	    "serverId": "1",
	    "sign": "afb3496f05333fbfa184f8e8af39eb7f198e37a7",
	    "totalAmount": "9800",
	    "tradeNo": "a150221000012131",
	    "ts": "20150723150028",
	    "type": "notify-game",
	    "uid": "mi__30854",
	    "xgAppId": "2008"
	}

游戏服务器发放道具成功返回如下JSON字符串，否则西瓜SDK会隔一段时间重新发送：

	{
    	"code": "0",
    	"msg": "success"
	}

游戏服务器二次验证订单状态：

	http://p2.xgsdk.com/pay/verify-order/{xgAppId}

	示例：
	http://p2.xgsdk.com/pay/verify-order/2008?tradeNo=a150221000012131&sign=86e396a999e9673731be6609c4dc7bca8945ada6&ts=20150723150028&type=verify-order

	签名算法：
	HmacSHA1("tradeNo=a150221000012131&ts=20150723150028&type=verify-order", appSecretKey)

返回结果：

	{
	    "channelId": "mi",
	    "customInfo": "2323423413412351251245",
	    "gameTradeNo": "99887766",
	    "paidAmount": "9800",
	    "paidTime": "20150723150128",
	    "payStatus": "1",
	    "productDesc": "productDesc1",
	    "productId": "productId1",
	    "productName": "productName1",
	    "productQuantity": "1",
	    "roleId": "224455",
	    "serverId": "1",
	    "sign": "afb3496f05333fbfa184f8e8af39eb7f198e37a7", // HmacSHA1(按字母顺序key1=value1&key2=value2&...，appSecretKey)
	    "totalAmount": "9800",
	    "tradeNo": "a150221000012131",
	    "ts": "20150723150028",
	    "type": "notify-game",
	    "uid": "mi__30854",
	    "xgAppId": "2008"
	}

## 2. 接入详细说明
### 1. 文档概述

此文档是西瓜SDK支付通知接口接入文档。包括如下2个接口：  
 1. 支付通知接口
 2. 二次查询验证订单接口

西瓜订单服务器在订单状态改变时,会主动调用支付通知接口通知游戏服务器订单信息。
游戏服务器在接受到通知请求时,应同时调用充值验证接口向西瓜订单服务器查询订单信息是否正确。
如果只接入支付通知接口,将不能防止订单信息伪造。  
**注意：** 如游戏无支付要求，则无需接入。

<div id="category" style="display:none"></div>

#### 1.1 文档结构

<ol>
	<li>
		<a href="#doc">文档概述</a>
			<ul type="disc">
				<li><a href="#docStructure">文档结构</a></li>
			</ul>
	</li>
	<li>
		<a href="#configure">支付通知接口</a>
			<ul type="disc">
				<li><a href="#conditions">功能</a></li>
				<li><a href="#steps">输入</a></li>
				<li><a href="#import">输出</a>
				<li><a href="#import_android">请求样例</a>
				<li><a href="#import_1">返回值样例</a>
        		<li><a href="#import_2">错误码</a>
        		<li><a href="#import_4">签名和验签</a>
        		<li><a href="#import_3">游戏逻辑处理建议</a>
			</ul>
	</li>
	<li>
		<a href="#adjust">二次查询验证订单</a>
			<ul type="disc">
				<li><a href="#copyJar">功能</a></li>
				<li><a href="#copyInterface">输入</a></li>
				<li><a href="#adjustActivity">输出</a>
				<li><a href="#androidMk">请求样例</a>
      	        <li><a href="#androidMk1">返回值样例</a>
                <li><a href="#androidMk2">错误码</a>
			</ul>
	</li>
	<li>
		<a href="#version">文档版本</a>
	</li>
</ol>


<div id="configure"></div>

#### 2 支付通知接口（通知游戏支付结果）

<div id="conditions"></div>

##### 2.1 功能

<table>
<tr>
<td>发起方</td><td>XGSDK服务端</td>
</tr>
<tr>
<td>接收方</td><td>游戏服务器</td>
</tr>
<tr>
<td>接口类型</td><td>HTTP POST  
 Content-Type：application/json； charset=UTF-8</td>
</tr>
<tr>
<td nowrap>字符集编码</td><td>UTF-8</td>
</tr>
<tr>
<td>安全机制</td><td>签名</td>
</tr>
<tr>
<td>请求地址</td><td>游戏方提供URL，XGSDK服务器主动调用</td>
</tr>
<tr>
<td>功能描述</td><td>当接收到渠道的支付结果回调信息后，XGSDK服务端会对订单支付信息进行确认，完成后，XGSDK服务端会将订单支付结果信息推送到游戏服务器提供的支付订单回调地址。</td>
</table>

<div id="steps"></div>

##### 2.2 输入

**参数说明：** 参数为一个json的字符串

<table>
<tr>
<td>参数</td><td >必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为notify-game</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>XGSDK分配的游戏编号</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>运营渠道编号</td>
</tr>
<tr>
<td>uid</td><td>是</td><td>String</td><td>渠道的用户编号</td>
</tr>
<tr>
<td>zoneId</td><td>否</td><td>String</td><td>游戏区编号</td>
</tr>
<tr>
<td>serverId</td><td>否</td><td>String</td><td>游戏服编号</td>
</tr>
<tr>
<td>roleId</td><td>是</td><td>String</td><td>角色编号</td>
</tr>
<tr>
<td>roleName</td><td>否</td><td>String</td><td>角色名称</td>
</tr>
<tr>
<td>roleLevel</td><td>否</td><td>String</td><td>角色等级</td>
</tr>
<tr>
<td>roleVipLevel</td><td>否</td><td>String</td><td>角色VIP等级</td>
</tr>
<tr>
<td>currencyName</td><td>否</td><td>String</td><td>支付货币名称</td>
</tr>
<tr>
<td>productId</td><td>是</td><td>String</td><td>商品编号</td>
</tr>
<tr>
<td>productName</td><td>否</td><td>String</td><td>商品名称</td>
</tr>
<tr>
<td>productDesc</td><td>否</td><td>String</td><td>商品描述</td>
</tr>
<tr>
<td>productQuantity</td><td>否</td><td>String</td><td>商品数量</td>
</tr>
<tr>
<td>productUnitPrice</td><td>否</td><td>String</td><td>商品单价(单位分)</td>
</tr>
<tr>
<td>totalAmount</td><td>是</td><td>String</td><td>总面额(单位分)</td>
</tr>
<tr>
<td>paidAmount</td><td>是</td><td>String</td><td>总支付金额(单位分)</td>
</tr>
<tr>
<td>customInfo</td><td>否</td><td>String</td><td>游戏方自定义字段，支付成功后回调的时候，透传原样返回</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028  
对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>gameTradeNo</td><td>否</td><td>String</td><td>游戏侧订单号</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥</td>
</tr>
<tr>
<td>tradeNo</td><td>是</td><td>String</td><td>Xgsdk分配的订单号</td>
</tr>
<tr>
<td>paidTime</td><td>是</td><td>String</td><td>支付时间 yyyyMMddHHmmss</td>
</tr>
<tr>
<td>payStatus</td><td>是</td><td>String</td><td>订单支付状态  
1 支付成功  
2 支付失败</td>
</tr>
</table>

<div id="import"></div>

##### 2.3 输出
**返回结果为JSON格式的字符串，分别有如下几个字段：**

<table>
<tr>
<td>参数</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>返回码，参见<a href="#import_2">错误码</a>章节</td>
</tr>
<tr>
<td>msg</td><td>接口调用信息提示</td>
</tr>
</table>

<div id="import_android"></div>

##### 2.4 请求样例
**请求参数:**  
type=notify-game  
xgAppId=1024appid  
channelId=mi  
uid=30854  
serverId=1  
roleId=224455  
productId=productId1  
productName=productName1  
productDesc=productDesc1  
productQuantity=1  
totalAmount=9800  
paidAmount=9800  
customInfo=2323423413412351251245  
gameTradeNo=99887766  
tradeNo=2984456  
paidTime=20150723145928  
payStatus=1  

**当前时间戳ts:** 20150723150028  

**游戏服务端密钥:** 654321

**则请求签名源串为：**
channelId=mi&customInfo=2323423413412351251245&gameTradeNo=99887766&paidAmount=9800&paidTime=20150723150128&payStatus=1&productDesc=productDesc1&productId=productId1&productName=productName1&productQuantity=1&roleId=224455&serverId=1&totalAmount=9800&tradeNo=2984456&ts=20150723150028&type=notify-game&uid=30854&xgAppId=1024appid

**请求签名为：**  
afb3496f05333fbfa184f8e8af39eb7f198e37a7  

**请求样例：**  
http://172.63.55.62:18888/moon/pay  
postBody:  
{"channelId":"mi","customInfo":"2323423413412351251245","gameTradeNo":"99887766","paidAmount":"9800","paidTime":"20150723150128","payStatus":"1","productDesc":"productDesc1","productId":"productId1","productName":"productName1","productQuantity":"1","roleId":"224455","serverId":"1","totalAmount":"9800","tradeNo":"2984456","ts":"20150723150028","type":"notify-game","uid":"30854","xgAppId":"1024appid","sign":"afb3496f05333fbfa184f8e8af39eb7f198e37a7"}

<div id="import_1"></div>

##### 2.5 返回值样例

	{
	    "code": "0",
	    "msg": "success"
	}


<div id="import_2"></div>

##### 2.6 错误码
<table>
<tr>
<td nowrap>错误码</td> <td>备注</td>
</tr>
<tr>
<td>0</td> <td>成功</td>
</tr>
<tr>
<td>-1</td> <td>签名失败</td>
</tr>
<tr>
<td>1</td> <td>请求重发，表示游戏服前置机收到xg服务器通知，但是由于游戏服务器正在升级，不能处理响应，请求延后重新发送</td>
</tr>
<tr>
<td>2</td> <td>重复订单，表示游戏服务器之前已收到了同样订单的通知，为避免因网络等原因导致道具或者游戏代币重复到账，建议游戏做订单排重
</td>
</tr>
<tr>
<td>-2</td> <td>xgAppId不存在</td>
</tr>
<tr>
<td>-3</td> <td>channelId不存在</td>
</tr>
<tr>
<td>-4</td> <td>区服不存在</td>
</tr>
<tr>
<td>-5</td> <td>账号不存在</td>
</tr>
<tr>
<td>-6</td> <td>订单号不存在</td>
</tr>
<tr>
<td>-7</td> <td>渠道号和XG编号与订单中创建时的参数不一致
</td>
</tr>
<tr>
<td>-8</td> <td>发布计划编号不存在</td>
</tr>
<tr>
<td>-98</td> <td>请求参数疑似被篡改</td>
</tr>
<tr>
<td>-99</td> <td>XG系统内部服务器错误</td>
</tr>
<tr>
<td>-100</td> <td>获取登录验证参数失败</td>
</tr>
<tr>
<td>-101</td> <td>获取渠道参数失败</td>
</tr>
<tr>
<td>-102</td> <td>连接渠道登陆验证接口失败</td>
</tr>
<tr>
<td>-103</td> <td>渠道登陆验证结果失败</td>
</tr>
<tr>
<td>-200</td> <td>商品不存在
</td>
</tr>
<tr>
<td>-201</td> <td>商品不一致</td>
</tr>
<tr>
<td>-202</td> <td>金额不一致</td>
</tr>
<tr>
<td>-203</td> <td>渠道验证订单失败</td>
</tr>
<tr>
<td>-204</td> <td>支付通知中的渠道分配游戏编号与订单创建时不一致</td>
</tr>
<tr>
<td>-205</td> <td>支付通知中的渠道分配用户编号与订单创建时不一致</td>
</tr>
<tr>
<td>-206</td> <td>支付通知中的订单支付时间与订单创建时相差超过一天</td>
</tr>
<tr>
<td>-207</td> <td>支付通知中的渠道分配商品编号与订单创建时不一致</td>
</tr>
<tr>
<td>-208</td> <td>支付通知中的渠道分配商品名称与订单创建时不一致</td>
</tr>
<tr>
<td>-209</td> <td>支付通知中的渠道分配商品数量与订单创建时不一致</td>
</tr>
<tr>
<td>-210</td> <td>支付通知中的渠道分配支付金额与订单创建时不一致</td>
</tr>
<tr>
<td>-212</td> <td>解析支付通知中的渠道参数失败</td>
</tr>
<tr>
<td>-301</td> <td>查询渠道订单超时</td>
</tr>
<tr>
<td>-302</td> <td>查询渠道订单失败</td>
</tr>
<tr>
<td>-401</td> <td>创建渠道订单失败</td>
</tr>
</table>


<div id="import_4"></div>

##### 2.7 签名和验签

**签名算法采用HmacSHA1**

1. 传入参数key按字母排序；
2. 按key1=value1&key2=value2&...来拼接签名源串，将值为空的参数和sign签名字段去掉，不加入签名源串，key和value不进行任何编码（如不进行URLEncoder）；
3. 然后对最后生成的字符串进行HmacSHA1计算，得到签名串。

<!--
**签名示例：**
- **传入参数：**
productQuantity=1&productDesc=paymentDes017&productId=payment019&productName=10&appId=xyfm&channelId=xiaomi&customInfo=9091e0cc37364fd54307cb076a64a5206b52a83bc8086ce27583df220191d245d27a1b93e68389fb65c0e6bd5a075f566663&tradeNo=100265&paidAmount=1&payStatus=1&payTime=2014-12-4 20:40:15&uid=25613430&sign=a7dc0c91632d473a7bdba944b4d43222d97293ac&totalAmount=1

- **签名密钥appSecret：** 2498b561-2338-4af5-a561-c2f88aed5753

- **签名源串：**
productQuantity=1&productDesc=paymentDes017&productId=payment019&productName=10&appId=xyfm&channelId=xiaomi&customInfo=9091e0cc37364fd54307cb076a64a5206b52a83bc8086ce27583df220191d245d27a1b93e68389fb65c0e6bd5a075f566663&tradeNo=100265&paidAmount=1&payStatus=1&payTime=2014-12-4 20:40:15&uid=25613430&totalAmount=12498b561-2338-4af5-a561-c2f88aed5753

- **最后的签名串为：**
 a7dc0c91632d473a7bdba944b4d43222d97293ac

-->
<div id="import_3"></div>

##### 2.8 游戏逻辑处理建议
1. 如果有游戏升级不能响应，则需返回String ERR_RESEND = "1"；
2. 先要进行验证签名，不通过则需返回String ERR_SIGN = "-1"；
3. 如果游戏保留了游戏订单但找不到对应订单时，则需返回String ERR_ORDERID_NOTEXIST = "-6"；
4. 做订单排重，如果重复则需返回String ERR_REPEAT = "2"；
5. 如果游戏保留了游戏订单，最好能够验证XG订单消息和游戏订单消息的一致性，如productId、productQuantity、paidAmount、uid、roleId，保证人、财、物是否一致，避免有人在网络截获包，同时密钥泄露时，冒充发送通知，这时需返回String ERR_EXCEPTION = "-98"；
6. 发送验证请求到xg的sdkserver服务器，验证订单，如果订单不存在或者需再次验证订单一致性时，需返回String ERR_EXCEPTION = "-98"；
7. 如果系统发生内部异常，则需返回String ERR_SYSTEM = "-99"。  

若以上7步都正常，则返回String SUCCESS = "0"。

<div id="adjust"></div>

#### 3. 二次查询验证订单

<div id="copyJar"></div>

##### 3.1 功能
**发起方：** 游戏服务器  
**接收方：** XGSDK服务端  
**接口类型：** HTTP POST  
**字符集编码：** UTF-8  
**安全机制：** 签名  
**请求地址：**  
http://p2.xgsdk.com/pay/verify-order/{xgAppId}  
其中xgAppId是XGSDK分配的游戏编号，如西游伏魔是1024appid。  
**功能描述：** 用于游戏服务器验证收到的订单通知是否有效。


<div id="copyInterface"></div>

##### 3.2 输入
**参数说明：**
<table>
<tr>
<td>参数名称</td><td>必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify-order</td>
</tr>
<tr>
<td>tradeNo</td><td>是</td><td>String</td><td>订单编号</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥</td>
</tr>
</table>

<div id="adjustActivity"></div>

##### 3.3 输出
**返回结果为JSON格式的字符串，分别有如下几个字段：**
<table>
<tr>
<td>字段</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>返回码，参见<a href="#androidMk2">错误码</a>章节</td>
</tr>
<tr>
<td>msg</td><td>接口调用信息提示</td>
</tr>
<tr>
<td>data</td><td>接口返回数据</td>
</tr>
</table>

**data字段具体说明：**  
参数说明：
<table>
<tr>
<td>参数名称</td><td>必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify-order</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>XGSDK分配的游戏编号</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>运营渠道编号</td>
</tr>
<tr>
<td>uid</td><td>是</td><td>String</td><td>渠道的用户编号</td>
</tr>
<tr>
<td>zoneId</td><td>否</td><td>String</td><td>游戏区编号</td>
</tr>
<tr>
<td>serverId</td><td>否</td><td>String</td><td>游戏服编号</td>
</tr>
<tr>
<td>roleId</td><td>是</td><td>String</td><td>角色编号</td>
</tr>
<tr>
<td>roleName</td><td>否</td><td>String</td><td>角色名称</td>
</tr>
<tr>
<td>roleLevel</td><td>否</td><td>String</td><td>角色等级</td>
</tr>
<tr>
<td>roleVipLevel</td><td>否</td><td>String</td><td>角色VIP等级</td>
</tr>
<tr>
<td>currencyName</td><td>否</td><td>String</td><td>支付货币名称</td>
</tr>
<tr>
<td>productId</td><td>是</td><td>String</td><td>商品编号</td>
</tr>
<tr>
<td>productName</td><td>否</td><td>String</td><td>商品名称</td>
</tr>
<tr>
<td>productDesc</td><td>否</td><td>String</td><td>商品描述</td>
</tr>
<tr>
<td>productQuantity</td><td>否</td><td>String</td><td>商品数量</td>
</tr>
<tr>
<td>productUnitPrice</td><td>否</td><td>String</td><td>商品单价(单位分)</td>
</tr>
<tr>
<td>totalAmount</td><td>是</td><td>String</td><td>总面额(单位分)</td>
</tr>
<tr>
<td>paidAmount</td><td>是</td><td>String</td><td>总支付金额(单位分)</td>
</tr>
<tr>
<td>customInfo</td><td>否</td><td>String</td><td>游戏方自定义字段，支付成功后回调的时候，透传原样返回</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028  
对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>gameTradeNo</td><td>否</td><td>String</td><td>游戏侧订单号</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥</td>
</tr>
<tr>
<td>tradeNo</td><td>是</td><td>String</td><td>Xgsdk分配的订单号</td>
</tr>
<tr>
<td>paidTime</td><td>是</td><td>String</td><td>支付时间 yyyyMMddHHmmss</td>
</tr>
<tr>
<td>payStatus</td><td>是</td><td>String</td><td>订单支付状态  
1 支付成功  
2 支付失败</td>
</tr>
</table>


<div id="androidMk"></div>

##### 3.4 请求样例
**请求参数:**  
**tradeNo:** 2984456  
**当前时间戳ts:** 20150723150028  
**游戏服务端密钥:** 123456  
**则请求签名源串为：**  tradeNo=2984456&ts=20150723150028&type=verify-order  
**请求签名为：**
86e396a999e9673731be6609c4dc7bca8945ada6  
**请求样例：**  
http://p2.xgsdk.com/pay/verify-order/1024appid?tradeNo=2984456&sign=86e396a999e9673731be6609c4dc7bca8945ada6&ts=20150723150028&type=verify-order

<div id="androidMk1"></div>

##### 3.5 返回值样例
**响应签名源串为：**
channelId=mi&customInfo=2323423413412351251245&gameTradeNo=99887766&paidAmount=9800&paidTime=20150723150128&payStatus=1&productDesc=productDesc1&productId=productId1&productName=productName1&productQuantity=1&roleId=224455&serverId=1&totalAmount=9800&tradeNo=2984456&ts=20150723150028&type=verify-order&uid=30854&xgAppId=1024appid

**响应验签密钥为游戏服务端验签密钥：** 654321

**响应签名为：**
b990455f7f184c632f7fe1a8369d620392f5cdc8

**最终返回为：**

	{
	    "code": "0",
		"msg": "success",
	    "data": {
	        "channelId": "mi",
	        "customInfo": "2323423413412351251245",
	        "gameTradeNo": "99887766",
	        "paidAmount": "9800",
	        "paidTime": "20150723150128",
	        "payStatus": "1",
	        "productDesc": "productDesc1",
	        "productId": "productId1",
	        "productName": "productName1",
	        "productQuantity": "1",
	        "roleId": "224455",
	        "serverId": "1",
	        "sign": "b990455f7f184c632f7fe1a8369d620392f5cdc8",
	        "totalAmount": "9800",
	        "tradeNo": "2984456",
	        "ts": "20150723150028",
	        "type": "verify-order",
	        "uid": "30854",
	        "xgAppId": "1024appid"
	    }
	}

<div id="androidMk2"></div>

##### 3.6 错误码
<table>
<tr>
<td nowrap>错误码</td> <td>备注</td>
</tr>
<tr>
<td>0</td> <td>成功</td>
</tr>
<tr>
<td>-1</td> <td>签名失败</td>
</tr>
<tr>
<td>1</td> <td>请求重发，表示游戏服前置机收到xg服务器通知，但是由于游戏服务器正在升级，不能处理响应，请求延后重新发送</td>
</tr>
<tr>
<td>2</td> <td>重复订单，表示游戏服务器之前已收到了同样订单的通知，为避免因网络等原因导致道具或者游戏代币重复到账，建议游戏做订单排重
</td>
</tr>
<tr>
<td>-2</td> <td>xgAppId不存在</td>
</tr>
<tr>
<td>-3</td> <td>channelId不存在</td>
</tr>
<tr>
<td>-4</td> <td>区服不存在</td>
</tr>
<tr>
<td>-5</td> <td>账号不存在</td>
</tr>
<tr>
<td>-6</td> <td>订单号不存在</td>
</tr>
<tr>
<td>-7</td> <td>渠道号和XG编号与订单中创建时的参数不一致
</td>
</tr>
<tr>
<td>-8</td> <td>发布计划编号不存在</td>
</tr>
<tr>
<td>-98</td> <td>请求参数疑似被篡改</td>
</tr>
<tr>
<td>-99</td> <td>XG系统内部服务器错误</td>
</tr>
<tr>
<td>-100</td> <td>获取登录验证参数失败</td>
</tr>
<tr>
<td>-101</td> <td>获取渠道参数失败</td>
</tr>
<tr>
<td>-102</td> <td>连接渠道登陆验证接口失败</td>
</tr>
<tr>
<td>-103</td> <td>渠道登陆验证结果失败</td>
</tr>
<tr>
<td>-200</td> <td>商品不存在
</td>
</tr>
<tr>
<td>-201</td> <td>商品不一致</td>
</tr>
<tr>
<td>-202</td> <td>金额不一致</td>
</tr>
<tr>
<td>-203</td> <td>渠道验证订单失败</td>
</tr>
<tr>
<td>-204</td> <td>支付通知中的渠道分配游戏编号与订单创建时不一致</td>
</tr>
<tr>
<td>-205</td> <td>支付通知中的渠道分配用户编号与订单创建时不一致</td>
</tr>
<tr>
<td>-206</td> <td>支付通知中的订单支付时间与订单创建时相差超过一天</td>
</tr>
<tr>
<td>-207</td> <td>支付通知中的渠道分配商品编号与订单创建时不一致</td>
</tr>
<tr>
<td>-208</td> <td>支付通知中的渠道分配商品名称与订单创建时不一致</td>
</tr>
<tr>
<td>-209</td> <td>支付通知中的渠道分配商品数量与订单创建时不一致</td>
</tr>
<tr>
<td>-210</td> <td>支付通知中的渠道分配支付金额与订单创建时不一致</td>
</tr>
<tr>
<td>-212</td> <td>解析支付通知中的渠道参数失败</td>
</tr>
<tr>
<td>-301</td> <td>查询渠道订单超时</td>
</tr>
<tr>
<td>-302</td> <td>查询渠道订单失败</td>
</tr>
<tr>
<td>-401</td> <td>创建渠道订单失败</td>
</tr>

</table>

---

<div id="version"></div>
#### 4. 文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>
</tr>
<tr>
	<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.7.31</td>
</tr>
<tr>
	<td>2.0 </td><td>1.1</td> <td>初版</td> <td>初版</td> <td>2015.10.16</td>
</tr>
</table>
