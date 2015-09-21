# 西瓜SDK session验证文档



## 1. 文档概述

此文档是西瓜SDK服务端接入登录验证文档。介绍游戏服务器如何验证用户登录信息，游戏客户端在接收到西瓜登录成功的回调后，
将对应的信息发送到游戏服务器，游戏服务器使用登录认证接口向西瓜登录服务器验证并获取用户登录信息。  
** 注意：** 登录认证接口为登录流程必接接口。



### 1.1 文档结构


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
				<li><a href="#import_android">请求示例</a>
        <li><a href="#import_1">返回值样例</a>
			</ul>
	</li>
	<li>
		<a href="#version">文档版本</a>
	</li>
</ol>


<div id="configure"></div>

## 2. 登录认证接口

<div id="conditions"></div>

### 2.1 功能

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
<td>请求地址</td><td>http://a2.xgsdk.com/account/verify-session/{xgAppId}</td>
</tr>
</table>

```
其中xgAppId是游戏在XGSDK的唯一标示，如西游伏魔是1024appid。
```

**功能描述:**
游戏服务器向XGSDK服务端发送请求，确认客户端发过来的sessionId是有效的，并获取准确的渠道账号。

<div id="steps"></div>

### 2.2 输入

参数说明：
<table>
<tr>
<td>参数</td><td >是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify-session</td>
</tr>
<tr>
<td>authInfo</td><td>是</td><td>String</td><td>会话验证数据，xgsdk客户端负责生成，通过onLoginSuccess回调反馈给游戏客户端，游戏客户端提交给游戏服务器后，游戏服务器拿这个参数到xgsdk服务器验证登录会话是否有效</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法采用HMACSHA1，使用游戏服务端密钥，签名参数仅包括authInfo和ts，可参考<a href="#import_android">请求样例</a>章节</td>
</tr>
</table>

<!--
<a name="authInfo"></a>authInfo数据格式:

<table>
<tr>
<td>参数</td><td >是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>xgsdk分配给游戏的ID</td>
</tr>
<tr>
<td>planId</td><td>是</td><td>String</td><td>发布计划编号</td>
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
<td>authToken</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接留空  
酷派:authorization code  
Vivo:authtoken  
金立:AmigoToken  
华为:access_token  
联想:lpsust（Token）  
OPPO:oauth_token  
拇指玩:token  
37玩:token  
益玩:token  
当乐:token  
酷狗:Token  
猎豹:mutk  
PPTV:sessionid  
PPS:sign  
安智:sid  
豌豆荚:token  
UC:sid  
百度:user_sessionid  
小米:sessionId  
iTools:sessionId  
快用:tokenKey  
PP助手:token_key  
同步推:sessionID  
91:sessionId</td>
</tr>
<tr>
<td>uId</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接留空  
Vivo:openid  
金立:amigoUserId  
联想:realm  
当乐:memberId  
37玩:Userid  
益玩:openid  
当乐:mid  
猎豹:uid  
PPS:uid  
安智:uid
豌豆荚:uid  
百度:user_id  
小米:Uid  
iTools:userID  
快用:guid  
同步推:userID  
91:loginUin  
OPPO:access_token_secret  
</tr>
<tr>
<td>name</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接填空  
Vivo:name  
当乐:userName  
37玩:userName  
酷狗:UserName  
PPTV:username  
百度:user_name
iTools:userName  
快用:username  
猎豹:clientIp</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏客户端密钥，签名参数为authInfo中的所有参数</td>
</tr>
</table>

-->

<div id="import"></div>


### 2.3 输出
返回结果为JSON格式的字符串，分别有如下几个字段：
<table>
<tr>
<td>字段</td><td >必选</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>code</td><td>是</td><td>字符串</td><td>返回码，参见错误码章节</td>
</tr>
<tr>
<td>msg</td><td>是</td><td>字符串</td><td>接口调用信息提示</td>
</tr>
<tr>
<td>data</td><td>是</td><td>JSONObject</td><td>当Code为0时候该字段才有意义，否则为空</td>
</tr>
</table>

**data数据：**
<table>
<tr>
<td>参数</td><td >是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>xgAppId</td><td>是</td><td>String</td><td>xgsdk分配给游戏的ID</td>
</tr>
<tr>
<td>planId</td><td>是</td><td>String</td><td>发布计划编号</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道Id</td>
</tr>
<tr>
<td>deviceId</td><td>是</td><td>String</td><td>设备Id</td>
</tr>
<tr>
<td>sessionId</td><td>否</td><td>String</td><td>
<!--酷派:authorization code  
Vivo:authtoken  
金立:AmigoToken  
华为:access_token  
联想:ST（Token）  
OPPO:oauth_token  
拇指玩:token  
37玩:token  
益玩:token  
当乐:token  
酷狗:Token  
猎豹:mutk  
PPTV:sessionid  
PPS:sign  
安智:sid  
豌豆荚:token  
UC:sid  
百度:user_sessionid  
小米:sessionId  
iTools:sessionId  
快用:tokenKey  
PP助手:token_key  
同步推:sessionID  
91:sessionId  </td>
</tr>
<tr>
<td>uId</td><td>是</td><td>String</td><td>酷派:openid  
Vivo:openid  
金立:AmigoUserId  
联想:AccountID  
OPPO:id  
拇指玩:uid  
37玩:Userid  
益玩:openid  
当乐:mid  
猎豹:uid  
PPS:uid  
安智:uid  
豌豆荚:uid  
UC:ucId  
百度:user_id  
小米:Uid  
iTools:userID  
快用:guid  
PP助手:userid  
同步推:userID  
91:loginUin
-->
会话编号
</td>
</tr>
<tr>
<td>uid</td><td>是</td><td>String</td><td>用户编号</td>
</tr>
<tr>
<td>userName</td><td>否</td><td>String</td><td>用户名 </td>
</tr>
<tr>
<td>nickName</td><td>否</td><td>String</td><td>账用户昵称</td>
</tr>
<tr>
<td>state</td><td>否</td><td>String</td><td>账号状态  
-1-未激活  
0-正常  
1-暂停  
2-销户</td>
</tr>
<tr>
<td>deviceId</td><td>否</td><td>String</td><td>设备编号</td>
</tr>
<tr>
<td>telphone</td><td>否</td><td>String</td><td>用户手机号</td>
</tr>
<tr>
<td>mail</td><td>否</td><td>String</td><td>性邮箱地址</td>
</tr>
<tr>
<td>sex</td><td>否</td><td>String</td><td>性别  
1-男  
2-女  
0-未知</td>
</tr>
<tr>
<td>brithday</td><td>否</td><td>String</td><td>生日
格式：YYY-MM-DD</td>
</tr>
<tr>
<td>smallHeadIconUrl</td><td>否</td><td>String</td><td>头像小图标url</td>
</tr>
<tr>
<td>bigHeadIconUrl</td><td>否</td><td>String</td><td>头像大图标url</td>
</tr>
<tr>
<td>constellation</td><td>否</td><td>String</td><td>星座</td>
</tr>
<tr>
<td>balance</td><td>否</td><td>String</td><td>渠道币余额</td>
</tr>
<tr>
<td>level</td><td>否</td><td>String</td><td>渠道用户等级</td>
</tr>
</table>

<div id="import_android"></div>

### 2.4 请求示例

<!--
#### 2.4.1 authInfo结构

**初始参数**
xgAppId=1024appid  
channelId=mi  
authToken=authToken  
uid=uid  
name=name  
planId=1  
deviceId=deviceId  

**当前时间戳ts为：**
20150723150028

**游戏客户端密钥：** 123456

**authInfo生成签名前的源串为:**

authToken=authToken&channelId=mi&deviceId=deviceId&name=name&planId=1&ts=20150723150028&uid=uid&xgAppId=1024appid

**最终HmacSHA1签名为：**  
fa34381dc584f631a87a0436e49ef4d3a71ee55d


**Base64编码前的authInfo数据为：**
{"authToken":"authToken","channelId":"mi","deviceId":"deviceId","name":"name","planId":"1","xgAppId":"1024appid","sign":"fa34381dc584f631a87a0436e49ef4d3a71ee55d","ts":"20150723150028","uid":"uid"}

**Base64编码后的authInfo数据为：**
eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsImRldmljZUlkIjoiZGV2aWNlSWQiLCJuYW1lIjoibmFtZSIsInBsYW5JZCI6IjEiLCJ4Z0FwcElkIjoiMTAyNGFwcGlkIiwic2lnbiI6ImZhMzQzODFkYzU4NGY2MzFhODdhMDQzNmU0OWVmNGQzYTcxZWU1NWQiLCJ0cyI6IjIwMTUwNzIzMTUwMDI4IiwidWlkIjoidWlkIn0=
-->

**客户端上报的authInfo：**
eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsImRldmljZUlkIjoiZGV2aWNlSWQiLCJuYW1lIjoibmFtZSIsInBsYW5JZCI6IjEiLCJ4Z0FwcElkIjoiMTAyNGFwcGlkIiwic2lnbiI6ImZhMzQzODFkYzU4NGY2MzFhODdhMDQzNmU0OWVmNGQzYTcxZWU1NWQiLCJ0cyI6IjIwMTUwNzIzMTUwMDI4IiwidWlkIjoidWlkIn0=

**当前时间戳ts为：**
20150723150028

**游戏服务端密钥为：** 654321

**登录验证参数签名前的源串：**
authInfo=eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsImRldmljZUlkIjoiZGV2aWNlSWQiLCJuYW1lIjoibmFtZSIsInBsYW5JZCI6IjEiLCJ4Z0FwcElkIjoiMTAyNGFwcGlkIiwic2lnbiI6ImZhMzQzODFkYzU4NGY2MzFhODdhMDQzNmU0OWVmNGQzYTcxZWU1NWQiLCJ0cyI6IjIwMTUwNzIzMTUwMDI4IiwidWlkIjoidWlkIn0=&ts=20150723150028&type=verify-session

**对应HmacSHA1签名为：**  
10b1cdc8e4259b780a4336b725137a5579f84129

**请求样例：**  
http://p2.xgsdk.com/account/verify-session/1024appid?authInfo=eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsImRldmljZUlkIjoiZGV2aWNlSWQiLCJuYW1lIjoibmFtZSIsInBsYW5JZCI6IjEiLCJ4Z0FwcElkIjoiMTAyNGFwcGlkIiwic2lnbiI6ImZhMzQzODFkYzU4NGY2MzFhODdhMDQzNmU0OWVmNGQzYTcxZWU1NWQiLCJ0cyI6IjIwMTUwNzIzMTUwMDI4IiwidWlkIjoidWlkIn0=&sign=10b1cdc8e4259b780a4336b725137a5579f84129&ts=20150723150028&type=verify-session




<div id="import_1"></div>

### 2.5 返回值样例
```
{
    "code": "0",
    "msg": "success",
    "data": {
        "xgAppId": "1024appid",
        "planId": "1",
        "channelId": "mi",
        "deviceId": "deviceId",
        "sessionId": "woidkljfhnav98a7fdgonqelrtnsdvaxasdfasdf",
        "uid": "3099245"
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
