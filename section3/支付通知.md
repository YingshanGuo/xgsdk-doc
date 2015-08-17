#西瓜SDK 支付通知接口
****
<div id="doc"></div>

##1 文档概述

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font face="微软雅黑">此文档是西瓜SDK支付通知接口接入文档。包括如下2个接口：<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1. 支付通知接口<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2. 充值验证接口<br/>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;西瓜订单服务器在订单状态改变时,会主动调用支付通知接口通知游戏服务器订单信息。
游戏服务器在接受到通知请求时,应同时调用充值验证接口向西瓜订单服务器查询订单信息是否正确。
如果只接入支付通知接口,将不能防止订单信息伪造。</font>
<font face="微软雅黑" color="FF0000">注意</font>：<font face="微软雅黑">如游戏无支付要求，则无需接入。</font>

<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">
<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>

<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
 $(document).ready(function(){
      $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        $(item).attr("id","wow"+i);
        $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
        $(".newh2").css("margin-left",0);
        $(".newh3").css("margin-left",20);
        $(".newh4").css("margin-left",40);
        $(".newh5").css("margin-left",60);
        $(".newh6").css("margin-left",80);
      });
 });
</script>
<div id="category" style="display:none"></div>

###1.1 文档结构

<div>
<ol type='1'>
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
		<a href="#adjust">充值验证接口</a>
			<ul type="disc">
				<li><a href="#copyJar">功能</a></li>
				<li><a href="#copyInterface">输入</a></li>
				<li><a href="#adjustActivity">输出</a>
				<li><a href="#androidMk">请求样例</a>
      	<li><a href="#androidMk1">返回值样例</a>
			</ul>
	</li>
	<li>
		<a href="#ready">错误码</a>
	</li>
	<li>
		<a href="#version">文档版本</a>
	</li>
</ol>
</div>

<div id="SDKDownload"></div>

##2 支付通知接口（通知游戏支付结果）

<div id="conditions"></div>

###2.1 功能

<table>
<tr>
<td width="100">发起方</td><td>XGSDK服务端</td>
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
<td>功能描述</td><td>当接收到渠道的支付结果通知后，XGSDK服务端会对订单支付信息进行确认。确认通过后，将订单支付结果信息推送到游戏服务器提供的支付订单通知URL。</td>
</table>

<div id="copyInterface"></div>

###2.2 请求

**参数说明：**

<table>
<tr>
<td>参数</td><td width="50">必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为notify-game</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150811085930对应2015/8/11 08:59:30</td>
</tr>
<tr>
<td>gameTradeNo</td><td>是</td><td>String</td><td>游戏端订单号</td>
</tr>
<tr>
<td>orderId</td><td>是</td><td>String</td><td>订单号</td>
</tr>
<tr>
<td>sdkUid</td><td>是</td><td>String</td><td>玩家唯一标识，游戏服务器请用此字段作为对玩家的唯一标识</td>
</tr>
<tr>
<td>payStatus</td><td>是</td><td>String</td><td>订单支付状态：<br/>
1 代表支付成功<br/>
2 代表支付失败</td>
</tr>
<tr>
<td>payTime</td><td>是</td><td>String</td><td>支付时间 yyyyMMddHHmmss</td>
</tr>
<tr>
<td>failedDesc</td><td>否</td><td>String</td><td>如果成功支付，则为空串</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>XGSDK分配的游戏编号</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道在XGSDK的唯一标识</td>
</tr>
<tr>
<td>appGoodsId</td><td>否</td><td>String</td><td>游戏内的商品ID</td>
</tr>
<tr>
<td>appGoodsName</td><td>否</td><td>String</td><td>游戏内的商品名称</td>
</tr>
<tr>
<td>appGoodsDesc</td><td>否</td><td>String</td><td>商品描述</td>
</tr>
<tr>
<td>appGoodsAmount</td><td>否</td><td>String</td><td>购买的商品的数量</td>
</tr>
<tr>
<td>originalPrice</td><td>否</td><td>String</td><td>购买的商品的原价，单位：分，1元=100分</td>
</tr>
<tr>
<td>totalPrice</td><td>是</td><td>String</td><td>支付金额，单位：分，1元=100分</td>
</tr>
<tr>
<td>zoneId</td><td>否</td><td>String</td><td>游戏区编号</td>
</tr>
<tr>
<td>serverId</td><td>否</td><td>String</td><td>游戏服编号</td>
</tr>
<tr>
<td>roleId</td><td>否</td><td>String</td><td>角色编号</td>
</tr>
<tr>
<td>roleName</td><td>否</td><td>String</td><td>角色昵称</td>
</tr>
<tr>
<td>currencyName</td><td>否</td><td>String</td><td>货币单位，如人民币为CNY</td>
</tr>
<tr>
<td>custom</td><td>否</td><td>String</td><td>游戏方自定义字段，在请求新订单的时候传到服务器，有的话就原样返回</td>
</tr>
<tr>
<td>channelTradeNo</td><td>否</td><td>String</td><td>渠道订单号</td>
</tr>
<tr>
<td>giftValue</td><td>否</td><td>String</td><td>使用礼品卡的金额</td>
</tr>
<tr>
<td>deviceId</td><td>否</td><td>String</td><td>设备id</td>
</tr>
<tr>
<td>deviceBrand</td><td>否</td><td>String</td><td>设备提供商： 小米、华为、三星、苹果</td>
</tr>
<tr>
<td>deviceModel</td><td>否</td><td>String</td><td>设备型号：小米note、华为meta7 iphone 6 plus等</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥</td>
</tr>
</table>

<div id="import"></div>

####2.2.1 请求样例

**请求参数：**

<table>
<tr>
<td>appGoodsAmount</td><td>1</td>
</tr>
<tr>
<td>appGoodsId</td><td>product1</td>
</tr>
<tr>
<td>appGoodsName</td><td>60元宝</td>
</tr>
<tr>
<td>appGoodsDesc</td><td>元宝</td>
</tr>
<tr>
<td>channelId</td><td>mi</td>
</tr>
<tr>
<td>channelTradeNo</td><td>21143936345994521949</td>
</tr>
<tr>
<td>currencyName</td><td>CNY</td>
</tr>
<tr>
<td>custom</td><td>222323417123491234</td>
</tr>
<tr>
<td>deviceBrand</td><td>Xiaomi</td>
</tr>
<tr>
<td>deviceId</td><td>imei_869630010064289</td>
</tr>
<tr>
<td>deviceModel</td><td>MI2</td>
</tr>
<tr>
<td>failedDesc</td><td></td>
</tr>
<tr>
<td>gameTradeNo</td><td>99887766</td>
</tr>
<tr>
<td>giftValue</td><td>300</td>
</tr>
<tr>
<td>orderId</td><td>2984456</td>
</tr>
<tr>
<td>originalPrice</td><td>2000</td>
</tr>
<tr>
<td>payStatus</td><td>1（代表支付成功）</td>
</tr>
<tr>
<td>payTime</td><td>20150811085928</td>
</tr>
<tr>
<td>roleId</td><td>224455</td>
</tr>
<tr>
<td>roleName</td><td>魔域苍龙</td>
</tr>
<tr>
<td>sdkUid</td><td>30854</td>
</tr>
<tr>
<td>serverId</td><td>1</td>
</tr>
<tr>
<td>totalPrice</td><td>1800</td>
</tr>
<tr>
<td>ts</td><td>20150811085930</td>
</tr>
<tr>
<td>type</td><td>notify-game</td>
</tr>
<tr>
<td>xgAppId</td><td>2001</td>
</tr>
<tr>
<td>zoneId</td><td>101</td>
</tr>
</table>

**当前时间戳ts:** 20150811085930

**游戏服务端密钥:** aefc5134be1543dea3217144eb71e8f8

**则请求签名源串为（按Key值升序排列）:**

	appGoodsAmount=1&appGoodsId=product1&appGoodsName=60元宝&appGoodsDesc=元宝&channelId=mi&channelTradeNo=21143936345994521949&currencyName=CNY&custom=222323417123491234&deviceBrand=Xiaomi&deviceId=imei_869630010064289&deviceModel=MI2&failedDesc=&gameTradeNo=99887766&giftValue=300&orderId=2984456&originalPrice=2000&payStatus=1&payTime=20150811085928&roleId=224455&roleName=魔域苍龙&sdkUid=30854&serverId=1&totalPrice=1800&ts=20150811085930&type=notify-game&xgAppId=2001&zoneId=101

**对上面的请求签名源串进行HmacSHA1签名的结果为：** 
    
    a139d226c48b79c1c9f9a41d8c3e48a88ef6420b

**请求样例：**

	http://172.63.55.62:18888/moon/pay-notify
    POST body: {"appGoodsAmount":"1","appGoodsDesc":"元宝","appGoodsId":"product1","appGoodsName":"60元宝","channelId":"mi","channelTradeNo":"21143936345994521949","currencyName":"CNY","custom":"222323417123491234","deviceBrand":"Xiaomi","deviceId":"imei_869630010064289","deviceModel":"MI2","failedDesc":"","gameTradeNo":"99887766","giftValue":"300","orderId":"2984456","originalPrice":"2000","payStatus":"1","payTime":"20150811085928","roleId":"224455","roleName":"魔域苍龙","sdkUid":"30854","serverId":"1","sign":"a139d226c48b79c1c9f9a41d8c3e48a88ef6420b","totalPrice":"1800","ts":"20150811085930","type":"notify-game","xgAppId":"2001","zoneId":"101"}

<div id="import_1"></div>

###2.3 返回

游戏服务器收到XGSDK的订单通知后，需要返回一个确认信息给XGSDK。否则XGSDK会没隔一段时间通知游戏服务器一次，知道收到游戏服务器的确认为止。

确认结果为JSON格式的字符串，分别有如下几个字段：

<table>
<tr>
<td>参数</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>返回码：<br/>
0 - 成功<br/>
1 - 通知失败<br/>
2 - 重复通知<br/>
其它 - 无效订单
</td>
</tr>
<tr>
<td>msg</td><td>接口调用信息提示</td>
</tr>
</table>

<div id="import_android"></div>



####2.3.1 返回值样例

	{
    "code": "0",
    "msg": "success"
	}

<div id="adjust"></div>

##3 充值验证接口（二次查询验证订单）

<div id="copyJar"></div>

###3.1 功能

<table>
<tr>
<td>发起方</td><td>游戏服务器</td>
</tr>
<tr>
<td>接收方</td><td>XGSDK服务端</td>
</tr>
<tr>
<td>接口类型</td><td>HTTP GET</td>
</tr>
<tr>
<td>字符集编码</td><td>UTF-8</td>
</tr>
<tr>
<td>安全机制</td><td>签名</td>
</tr>
<tr>
<td>请求地址</td><td>http://pay.xgsdk.com:8180/pay/verify-order/{xgAppId}</td>
</tr>
</table>

**其中xgAppid是游戏在XGSDK的唯一标示，如西游伏魔是1024appid。**

**功能描述:**
用于游戏服务器验证收到的订单通知是否有效。

<div id="copyInterface"></div>

###3.2 请求

**参数说明：**

<table>
<tr>
<td width="70">参数名称</td><td width="40">必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify-order</td>
</tr>
<tr>
<td>orderId</td><td>是</td><td>String</td><td>订单编号</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150811085930对应2015/8/11 08:59:30</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥</td>
</tr>
</table>

<div id="adjustActivity"></div>

####3.2.1 请求样例

**请求参数orderId：** 2984456

**当前时间戳ts：** 20150811085930

**游戏服务端密钥：** aefc5134be1543dea3217144eb71e8f8

**则请求签名源串为（按Key值升序排列）:**<br/>
orderId=2984456&ts=20150811085930&type=verify-order

**对上面的请求签名源串进行HmacSHA1签名的结果为：** <br/>
03d44abdfec6c58e7261644b09b5791a045f4d25

**最终请求样例：**

	http://pay.xgsdk.com:8180/pay/verify-order/1024appid?orderId=2984456&sign=03d44abdfec6c58e7261644b09b5791a045f4d25&ts=20150723150028&type=verify_order

<div id="androidMk1"></div>

###3.3 返回

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

<table>
<tr>
<td>返回字段</td><td width="40">必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>gameTradeNo</td><td>否</td><td>String</td><td>游戏侧订单号</td>
</tr>
<tr>
<td>orderId</td><td>是</td><td>String</td><td>订单号</td>
</tr>
<tr>
<td>sdkUid</td><td>是</td><td>String</td><td>用户唯一标识，游戏服务器请用此字段作为对玩家的唯一标识</td>
</tr>
<tr>
<td>payStatus</td><td>是</td><td>String</td><td>订单支付状态<br/>
1 支付成功<br/>
2 支付失败</td>
</tr>
<tr>
<td>payTime</td><td>是</td><td>String</td><td>支付时间 yyyyMMddHHmmss</td>
</tr>
<tr>
<td>failedDesc</td><td>否</td><td>String</td><td>如果成功支付，则为空串</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>XGSDK分配的游戏编号</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道在XGSDK的唯一标识</td>
</tr>
<tr>
<td>appGoodsId</td><td>否</td><td>String</td><td>游戏内的商品ID</td>
</tr>
<tr>
<td>appGoodsName</td><td>否</td><td>String</td><td>游戏内的商品名称</td>
</tr>
<tr>
<td>appGoodsDesc</td><td>否</td><td>String</td><td>商品描述</td>
</tr>
<tr>
<td>appGoodsAmount</td><td>否</td><td>String</td><td>购买的商品的数量</td>
</tr>
<tr>
<td>totalPrice</td><td>是</td><td>String</td><td>支付金额：单位：分，1元=100分</td>
</tr>
<tr>
<td>originalPrice</td><td>否</td><td>String</td><td>购买的商品的原价，单位：分，1元=100分</td>
</tr>
<tr>
<td>zoneId</td><td>否</td><td>String</td><td>游戏区编号</td>
</tr>
<tr>
<td>serverId</td><td>否</td><td>String</td><td>游戏服编号</td>
</tr>
<tr>
<td>roleId</td><td>否</td><td>String</td><td>角色编号</td>
</tr>
<tr>
<td>roleName</td><td>否</td><td>String</td><td>角色昵称</td>
</tr>
<tr>
<td>currencyName</td><td>否</td><td>String</td><td>货币单位，如人民币为CNY</td>
</tr>
<tr>
<td>custom</td><td>否</td><td>String</td><td>游戏方自定义字段，在请求新订单的时候传到服务器，有的话就原样返回</td>
</tr>
<tr>
<td>deviceId</td><td>否</td><td>String</td><td>设备id</td>
</tr>
<tr>
<td>deviceBrand</td><td>否</td><td>String</td><td>设备提供商： 小米、华为、三星、苹果</td>
</tr>
<tr>
<td>deviceModel</td><td>否</td><td>String</td><td>设备型号：小米note、华为meta7 iphone 6 plus等</td>
</tr>
</table>

<div id="androidMk"></div>

####3.3.1 返回值样例

	{
	    "code": "0",
	    "msg": "success",
	    "data": {
	        "appGoodsAmount": "1",
			"appGoodsDesc": "元宝",
	        "appGoodsId": "product1",
	        "appGoodsName": "60元宝",
	        "channelId": "mi",
	        "currencyName": "CNY",
	        "custom": "222323417123491234",
            "deviceBrand": "Xiaomi",
			"deviceId": "imei_869630010064289",
			"deviceModel": "MI2",
	        "gameTradeNo": "99887766",
	        "orderId": "2984456",
	        "originalPrice": "2000",
			"payStatus": "1",//（代表支付成功）
	        "payTime": "20150811085928",
	        "roleId": "224455",
	        "roleName": "魔域苍龙",
	        "sdkUid": "30854",
	        "serverId": "1",
	        "totalPrice": "1800",
			"xgAppid": "1024appid",
			"zoneId": "101"
	    }

	}


<div id="ready"></div>

##3 错误码

<table>
<tr>
<td>错误码</td><td>备注</td>
</tr>
<tr>
<td>0</td><td>成功</td>
</tr>
<tr>
<td>-1</td><td>签名失败</td>
</tr>
<tr>
<td>1</td><td>请求重发</td>
</tr>
<tr>
<td>2</td><td>重复订单</td>
</tr>
<tr>
<td>-2</td><td>xgAppid不存在</td>
</tr>
<tr>
<td>-3</td><td>channelId不存在</td>
</tr>
<tr>
<td>-4</td><td>区服不存在</td>
</tr>
<tr>
<td>-5</td><td>账号不存在</td>
</tr>
<tr>
<td>-6</td><td>订单号不存在</td>
</tr>
<tr>
<td>-99</td><td>系统错误</td>
</tr>
<tr>
<td>-100</td><td>获取登录验证参数失败</td>
</tr>
<tr>
<td>-101</td><td>获取渠道参数失败</td>
</tr>
<tr>
<td>-102</td><td>连接渠道登陆验证接口失败</td>
</tr>
<tr>
<td>-103</td><td>渠道登陆验证结果失败</td>
</tr>
<tr>
<td>-200</td><td>商品不存在</td>
</tr>
<tr>
<td>-201</td><td>商品不一致</td>
</tr>
<tr>
<td>-202</td><td>金额不一致</td>
</tr>
<tr>
<td>-203</td><td>渠道验证订单失败</td>
</tr>
</table>

---

<div id="version"></div>

###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>DK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.7.31</td>
</tr>
</table>
