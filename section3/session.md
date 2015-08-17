#西瓜SDK session验证文档
---


##1、文档概述

此文档是西瓜SDK服务端接入登录验证文档。介绍游戏服务器如何验证用户登录信息，游戏客户端在接收到西瓜登录成功的回调后，
将对应的信息发送到游戏服务器，游戏服务器使用登录认证接口向西瓜登录服务器验证并获取用户登录信息。
** 注意：** 登录认证接口为登录流程必接接口。



###1.1 文档结构


<ol>
	<li>
		<a href="#doc">文档概述</a>
			<ul>
				<li><a href="#docStructure">文档结构</a></li>
			</ul>
	</li>
	<li>
		<a href="#configure">登录认证接口</a>
			<ul>
				<li><a href="#conditions">功能</a></li>
				<li><a href="#steps">输入</a></li>
				<li><a href="#import">输出</a>
				<li><a href="#import_android">请求样例</a>
        <li><a href="#import_1">返回值样例</a>
			</ul>
	</li>
	<li>
		<a href="#version">文档版本</a>
	</li>
</ol>


<div id="configure"></div>

##2、登录认证接口

<div id="conditions"></div>

###2.1 功能

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
<td>请求地址</td><td>http://pay.xgsdk.com:8180/account/verify-session/{xgAppId}</td>
</tr>
</table>

```
其中xgAppId是游戏在XGSDK的唯一标示，如西游伏魔是1024appid。
```

**功能描述:**
游戏服务器向XGSDK服务端发送请求，确认客户端发过来的sessionId是有效的，并获取准确的渠道账号。

<div id="steps"></div>

###2.2 请求

参数说明：
<table>
<tr>
<td>参数</td><td width="70">是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify-session</td>
</tr>
<tr>
<td>authInfo</td><td>是</td><td>String</td><td>会话验证数据，xgsdk客户端负责生成，反馈给游戏客户端，游戏客户端提交给游戏服务器后，游戏服务器拿这个参数到xgsdk服务器验证登录会话是否有效
BASE64编码后的json字符串。具体数据格式说明请参考<a href="#authInfo">authInfo数据格式</a></td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150811085930对应2015/8/11 08:59:30</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名字符串，签名算法参见签名章节，使用游戏服务端密钥，签名参数仅包括authInfo</td>
</tr>
</table>

<a name="authInfo"></a>authInfo数据格式:

<table>
<tr>
<td>参数</td><td width="70">是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>xgsdk分配给游戏的唯一AppId</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道Id</td>
</tr>
<tr>
<td>deviceId</td><td>是</td><td>String</td><td>设备Id</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>authToken</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接留空<br/>
酷派:authorization code<br/>
Vivo:authtoken<br/>
金立:AmigoToken<br/>
华为:access_token<br/>
联想:lpsust（Token）<br/>
OPPO:oauth_token<br/>
拇指玩:token<br/>
37玩:token<br/>
益玩:token<br/>
当乐:token<br/>
酷狗:Token<br/>
猎豹:mutk<br/>
PPTV:sessionid<br/>
PPS:sign<br/>
安智:sid<br/>
豌豆荚:token<br/>
UC:sid<br/>
百度:user_sessionid<br/>
小米:sessionId<br/>
iTools:sessionId<br/>
快用:tokenKey<br/>
PP助手:token_key<br/>
同步推:sessionID<br/>
91:sessionId</td>
</tr>
<tr>
<td>uId</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接留空<br/>
Vivo:openid<br/>
金立:amigoUserId<br/>
联想:realm<br/>
当乐:memberId<br/>
37玩:Userid<br/>
益玩:openid<br/>
当乐:mid<br/>
猎豹:uid<br/>
PPS:uid<br/>
安智:uid<br/>
豌豆荚:uid<br/>
百度:user_id<br/>
小米:Uid<br/>
iTools:userID<br/>
快用:guid<br/>
同步推:userID <br/>
91:loginUin<br/>
OPPO:access_token_secret</td>
</tr>
<tr>
<td>name</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接填空<br/>
Vivo:name<br/>
当乐:userName<br/>
37玩:userName<br/>
酷狗:UserName<br/>
PPTV:username<br/>
百度:user_name <br/>
iTools:userName<br/>
快用:username<br/>
猎豹:clientIp</td>
</tr>
<tr>
<td>planId</td><td>是</td><td>String</td><td>游戏发行计划编号</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏客户端密钥，签名参数为authInfo中的所有参数</td>
</tr>
</table>

<div id="import"></div>

####2.2.1 请求示例

#####2.2.1.1 构建authInfo值

######初始参数：

	authToken=61A28C6C94F8F4D37C6EE632DFA43
    channelId=mi
    deviceId=1740948824
	name=Michael
    planId=1
    uId=foo2015
    xgAppId=2001

######当前时间戳ts为：

	20150811085930

######游戏客户端密钥：

	16e532be7c4a401a903c07ef3ea10803

######authInfo生成签名前的字符串为（按Key值升序排列）:

	authToken=61A28C6C94F8F4D37C6EE632DFA43&channelId=mi&deviceId=1740948824&name=Michael&planId=1&ts=20150811085930&uId=foo2015&xgAppId=2001

######对authInfo字符串进行HmacSHA1签名的结果为:

	9150ff12a280b1c234ab4c53e9b3c53a5536dd36

######将上述签名填入到autoInfo的sign字段，填完的结果为:

	{"authToken":"61A28C6C94F8F4D37C6EE632DFA43","channelId":"mi","deviceId":"1740948824","name":"Michael","planId":"1","sign":"9150ff12a280b1c234ab4c53e9b3c53a5536dd36","ts":"20150811085930","uId":"foo2015","xgAppId":"2001"}

######最后对包含签名的authInfo字符串进行Base64编码，结果为：

	eyJhdXRoVG9rZW4iOiI2MUEyOEM2Qzk0RjhGNEQzN0M2RUU2MzJERkE0MyIsImNoYW5uZWxJZCI6Im1pIiwiZGV2aWNlSWQiOiIxNzQwOTQ4ODI0IiwibmFtZSI6Ik1pY2hhZWwiLCJwbGFuSWQiOiIxIiwic2lnbiI6IjkxNTBmZjEyYTI4MGIxYzIzNGFiNGM1M2U5YjNjNTNhNTUzNmRkMzYiLCJ0cyI6IjIwMTUwODExMDg1OTMwIiwidUlkIjoiZm9vMjAxNSIsInhnQXBwSWQiOiIyMDAxIn0=

#####2.2.1.2 构建登录验证URL

######假如当前时间戳ts为：

	20150811085930

######游戏服务端密钥为:

	aefc5134be1543dea3217144eb71e8f8

######登录验证参数为（authInfo + ts + type）：

	authInfo=eyJhdXRoVG9rZW4iOiI2MUEyOEM2Qzk0RjhGNEQzN0M2RUU2MzJERkE0MyIsImNoYW5uZWxJZCI6Im1pIiwiZGV2aWNlSWQiOiIxNzQwOTQ4ODI0IiwibmFtZSI6Ik1pY2hhZWwiLCJwbGFuSWQiOiIxIiwic2lnbiI6IjkxNTBmZjEyYTI4MGIxYzIzNGFiNGM1M2U5YjNjNTNhNTUzNmRkMzYiLCJ0cyI6IjIwMTUwODExMDg1OTMwIiwidUlkIjoiZm9vMjAxNSIsInhnQXBwSWQiOiIyMDAxIn0=&ts=20150811085930&type=verify-session

######对登录验证参数进行HmacSHA1签名的结果为：

	eeea1a2d07e258932679effea36aa0d2fe47e50e

######最终的请求URL：

	http://pay.xgsdk.com:8180//account/verify-session/2001?authInfo=eyJhdXRoVG9rZW4iOiI2MUEyOEM2Qzk0RjhGNEQzN0M2RUU2MzJERkE0MyIsImNoYW5uZWxJZCI6Im1pIiwiZGV2aWNlSWQiOiIxNzQwOTQ4ODI0IiwibmFtZSI6Ik1pY2hhZWwiLCJwbGFuSWQiOiIxIiwic2lnbiI6IjkxNTBmZjEyYTI4MGIxYzIzNGFiNGM1M2U5YjNjNTNhNTUzNmRkMzYiLCJ0cyI6IjIwMTUwODExMDg1OTMwIiwidUlkIjoiZm9vMjAxNSIsInhnQXBwSWQiOiIyMDAxIn0=&sign=eeea1a2d07e258932679effea36aa0d2fe47e50e&ts=20150811085930&type=verify-session

<div id="import_1"></div>

###2.3 返回

返回结果为JSON格式的字符串，分别有如下字段：
<table>
<tr>
<td>字段</td><td>必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>是</td><td>字符串</td><td>返回码，参见错误码章节（如：验证通过为0）</td>
</tr>
<tr>
<td>msg</td><td>是</td><td>字符串</td><td>接口调用信息提示:<br/>
- 成功<br/>
- 验证失败<br/>
- 输入错误：authToken为空<br/>
- 输入错误：uId为空<br/>
- 系统错误：渠道AppSecret为空<br/>
- 系统错误：渠道无响应<br/>
- 系统错误：渠道返回结果格式错误<br/>
</td>
</tr>
<tr>
<td>data</td><td>是</td><td>JSONObject</td><td>当code为0时候该字段才有意义，否则为空</td>
</tr>
</table>

data数据：

<table>
<tr>
<td>参数</td><td>是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道ID</td>
</tr>
<tr>
<td>sessionId</td><td>否</td><td>String</td><td>酷派:authorization code<br/>
Vivo:authtoken<br/>
金立:AmigoToken<br/>
华为:access_token<br/>
联想:ST（Token）<br/>
OPPO:oauth_token<br/>
拇指玩:token<br/>
37玩:token<br/>
益玩:token<br/>
当乐:token<br/>
酷狗:Token<br/>
猎豹:mutk<br/>
PPTV:sessionid<br/>
PPS:sign<br/>
安智:sid<br/>
豌豆荚:token<br/>
UC:sid<br/>
百度:user_sessionid<br/>
小米:sessionId<br/>
iTools:sessionId<br/>
快用:tokenKey<br/>
PP助手:token_key<br/>
同步推:sessionID<br/>
91:sessionId </td>
</tr>
<tr>
<td>uId</td><td>是</td><td>String</td><td>酷派:openid<br/>
Vivo:openid<br/>
金立:AmigoUserId<br/>
联想:AccountID<br/>
OPPO:id<br/>
拇指玩:uid<br/>
37玩:Userid<br/>
益玩:openid<br/>
当乐:mid<br/>
猎豹:uid<br/>
PPS:uid<br/>
安智:uid<br/>
豌豆荚:uid<br/>
UC:ucId<br/>
百度:user_id<br/>
小米:Uid<br/>
iTools:userID<br/>
快用:guid<br/>
PP助手:userid<br/>
同步推:userID<br/>
91:loginUin </td>
</tr>
<tr>
<td>userName</td><td>否</td><td>String</td><td>Vivo:name<br/>
联想:Username<br/>
OPPO:name<br/>
拇指玩:username<br/>
37玩:userName<br/>
当乐:username<br/>
酷狗:UserName<br/>
PPTV:username<br/>
百度:user_name<br/>
iTools:userName<br/>
快用:username<br/>
PP助手:username</td>
</tr>
<tr>
<td>nickName</td><td>否</td><td>String</td><td>酷派:nickname</td>
</tr>
<tr>
<td>state</td><td>否</td><td>String</td><td>账号状态<br/>
-1-未激活<br/>
0-正常<br/>
1-暂停<br/>
2-销户<br/>
华为:userValidStatus<br/>
联想:verfied</td>
</tr>

<tr>
<td>deviceId</td><td>否</td><td>String</td><td>联想:DeviceID</td>
</tr>
<tr>
<td>telphone</td><td>否</td><td>String</td><td>金立:AmigoUserTn<br/>
OPPO:mobile</td>
</tr>
<tr>
<td>mail</td><td>否</td><td>String</td><td>邮箱地址<br/>
Vivo:email<br/>
OPPO:email<br/>
拇指玩:mail</td>
</tr>
<tr>
<td>sex</td><td>否</td><td>String</td><td>性别:男-1,女-2,未知-0<br/>
酷派:sex<br/>
OPPO:sex<br/>
拇指玩:sex<br/>
当乐:gender</td>
</tr>
<tr>
<td>brithday</td><td>否</td><td>String</td><td>说明生日格式：YYYY-MM-DD<br/>
酷派:brithday</td>
</tr>
<tr>
<td>smallHeadIconUrl</td><td>否</td><td>String</td><td>头像小图标<br/>
酷派:highDefUrl<br/>
OPPO:profilePictureUrl<br/>
拇指玩:icon<br/>
当乐:avatar_url</td>
</tr>
<tr>
<td>bigHeadIconUrl</td><td>否</td><td>String</td><td>头像大图标<br/>
酷派:headIconUrl</td>
</tr>
<tr>
<td>constellation</td><td>否</td><td>String</td><td>星座<br/>
OPPO:constellation</td>
</tr>
<tr>
<td>balance</td><td>否</td><td>String</td><td>余额<br/>
OPPO:gameBalance</td>
</tr>
<tr>
<td>level</td><td>否</td><td>String</td><td>当乐:level</td>
</tr>
</table>

<div id="import_android"></div>


######返回示例


	{
	    "code": "0",
	    "msg": "success",
	    "data": {
	        "channelId": "mi",
	        "sessionId": "woidkljfhnav98a7fdgonqelrtnsdvaxasdfasdf",
	        "uId": "3099245"
	    }
	}

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
