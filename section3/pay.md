# 西瓜SDK 支付通知接口

<div id="doc"></div>

## 1. 文档概述

此文档是西瓜SDK支付通知接口接入文档。包括如下2个接口：  
 1. 支付通知接口
 2. 二次查询验证订单接口

西瓜订单服务器在订单状态改变时,会主动调用支付通知接口通知游戏服务器订单信息。
游戏服务器在接受到通知请求时,应同时调用充值验证接口向西瓜订单服务器查询订单信息是否正确。
如果只接入支付通知接口,将不能防止订单信息伪造。  
**注意：** 如游戏无支付要求，则无需接入。

<div id="category" style="display:none"></div>

### 1.1 文档结构

<ol >
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
			</ul>
	</li>
	<li>
		<a href="#version">文档版本</a>
	</li>
</ol>


<div id="configure"></div>

## 2. 支付通知接口（通知游戏支付结果）

<div id="conditions"></div>

### 2.1 功能

<table>
<tr>
<td >发起方</td><td>XGSDK服务端</td>
</tr>
<tr>
<td>接收方</td><td>游戏服务器</td>
</tr>
<tr>
<td>接口类型</td><td>HTTP POST</td>
</tr>
<tr>
<td>字符集编码</td><td>UTF-8</td>
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

### 2.2 输入

**参数说明：**

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

### 2.3 输出
**返回结果为JSON格式的字符串，分别有如下几个字段：**

<table>
<tr>
<td>参数</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>返回码，参见错误码章节</td>
</tr>
<tr>
<td>msg</td><td>接口调用信息提示</td>
</tr>
</table>

<div id="import_android"></div>

### 2.4 请求样例
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
http://172.63.55.62:18888/moon/pay?channelId=mi&customInfo=2323423413412351251245&gameTradeNo=99887766&paidAmount=9800&paidTime=20150723150128&payStatus=1&productDesc=productDesc1&productId=productId1&productName=productName1&productQuantity=1&roleId=224455&serverId=1&totalAmount=9800&tradeNo=2984456&ts=20150723150028&type=notify-game&uid=30854&xgAppId=1024appid&sign=afb3496f05333fbfa184f8e8af39eb7f198e37a7


<div id="import_1"></div>

### 2.5 返回值样例

```
{
    "code": "0",
    "msg": "success"
}
```


<div id="adjust"></div>

## 3. 二次查询验证订单

<div id="copyJar"></div>

### 3.1 功能
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

### 3.2 输入
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

### 3.3 输出
**返回结果为JSON格式的字符串，分别有如下几个字段：**
<table>
<tr>
<td>字段</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>返回码，参见错误码章节</td>
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

### 3.4 请求样例
**请求参数:**  
**tradeNo:** 2984456  
**当前时间戳ts:** 20150723150028  
**游戏服务端密钥:** 123456  
**则请求签名源串为：**  tradeNo=2984456&ts=20150723150028&type=verify-order  
**请求签名为：**
86e396a999e9673731be6609c4dc7bca8945ada6
**请求样例：**  
http://p2.xgsdk.com/pay/verify-order/1024appid?tradeNo=2984456&sign=86e396a999e9673731be6609c4dc7bca8945ada6&ts=20150723150028&type=verify-order

### 3.5 返回值样例
**响应签名源串为：**
channelId=mi&customInfo=2323423413412351251245&gameTradeNo=99887766&paidAmount=9800&paidTime=20150723150128&payStatus=1&productDesc=productDesc1&productId=productId1&productName=productName1&productQuantity=1&roleId=224455&serverId=1&totalAmount=9800&tradeNo=2984456&ts=20150723150028&type=verify-order&uid=30854&xgAppId=1024appid

**响应验签密钥为游戏服务端验签密钥：** 654321

**响应签名为：**
b990455f7f184c632f7fe1a8369d620392f5cdc8

**最终返回为：**
```
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
    "totalAmount": "9800",
    "tradeNo": "2984456",
    "ts": "20150723150028",
    "type": "verify-order",
    "uid": "30854",
    "xgAppId": "1024appid",
    "sign": "b990455f7f184c632f7fe1a8369d620392f5cdc8"
}
}
```
---

<div id="version"></div>

### 文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.7.31</td>
</tr>
</table>
