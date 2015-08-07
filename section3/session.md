#西瓜SDK session验证文档
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font face="微软雅黑">此文档是西瓜SDK服务端接入登录验证文档。介绍游戏服务器如何验证用户登录信息，游戏客户端在接受到西瓜登录成功的回调后，
将对应的信息发送到游戏服务器，游戏服务器使用登录认证接口向西瓜登录服务器验证并获取用户登录信息。</font>
<font face="微软雅黑" color="FF0000">注意</font>：<font face="微软雅黑">登录认证接口为登录流程必接接口。</font>

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

<!--
###文档信息

	渠道SDK服务端接入文档
	作者：林立
	SDK版本：2.0
	文档版本：1.0
	日期：2015.7.31
-->

##一、登录认证接口

###1、功能

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
<td>请求地址</td><td>http://pay.xgsdk.com:8180/account/verify_session/{sdkAppid}</td>
</tr>
</table>

```
其中sdkAppid是游戏在XGSDK的唯一标示，如西游伏魔是1024appid。
```

**功能描述:**
游戏服务器向XGSDK服务端发送请求，确认客户端发过来的sessionId是有效的，并获取准确的渠道账号。

###2、输入

参数说明：
<table>
<tr>
<td>参数</td><td width="70">是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>type</td><td>是</td><td>String</td><td>接口类型，固定为verify_session</td>
</tr>
<tr>
<td>authInfo</td><td>是</td><td>String</td><td>会话验证数据，xgsdk客户端负责生成，反馈给游戏客户端，游戏客户端提交给游戏服务器后，游戏服务器拿这个参数到xgsdk服务器验证登录会话是否有效
BASE64编码后的json字符串</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏服务端密钥，签名参数仅包括authInfo和ts</td>
</tr>
</table>

authInfo数据:

<table>
<tr>
<td>参数</td><td width="70">是否必需</td><td>类型</td><td>说明</td>
</tr>
<tr>
<td>sdkAppid</td><td>是</td><td>String</td><td>xgsdk分配给游戏的ID</td>
</tr>
<tr>
<td>channelId</td><td>是</td><td>String</td><td>渠道ID</td>
</tr>
<tr>
<td>deviceId</td><td>是</td><td>String</td><td>设备id</td>
</tr>
<tr>
<td>ts</td><td>是</td><td>String</td><td>当前时间戳，秒级，如20150723150028对应2015/7/23 15:00:28</td>
</tr>
<tr>
<td>authToken</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接填空<br/>
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
91:sessionId  </td>
</tr>
<tr>
<td>uId</td><td>否</td><td>String</td><td>以下渠道按说明填写,没有的直接填空<br/>
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
<td>sign</td><td>是</td><td>String</td><td>签名，签名算法参见签名章节，使用游戏客户端密钥，签名参数为authInfo中的所有参数</td>
</tr>
</table>

###3、输出

返回结果为JSON格式的字符串，分别有如下几个字段：
<table>
<tr>
<td>字段</td><td>必选</td><td>类型</td><td>说明</td>
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
<tr>
<td>参数</td><td>是否必需</td><td>类型</td><td>说明</td>
</tr>
</table>

###4、请求样例

####4.1 authInfo结构

**初始参数：**<br/>

	sdkAppid=1024appid<br/>
	channelId=mi<br/>
	authToken=authToken<br/>
	uId=uId<br/>
	name=name<br/>

**当前时间戳ts为：**<br/>

	20150723150028

**游戏客户端密钥：**<br/>

	123456

**authInfo生成签名前的源串为:**

	authToken=authToken&channelId=mi&name=name&sdkAppid=1024appid&ts=20150723150028&uId=uId123456

**最终SHA256签名为:**<br/>

	390d743c09d2428c3dde6fcae3a8166f66fd452a9c9cdb0e567f3018269e343d

**Base64编码前的authInfo数据为:**

	{"authToken":"authToken","channelId":"mi","name":"name",sdkAppid":"1024appid","sign":"390d743c09d2428c3dde6fcae3a8166f66fd452a9c9cdb0e567f3018269e343d","ts":"20150723150028","uId":"uId"}

**Base64编码后的authInfo数据为：**

	eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsIm5hbWUiOiJuYW1lIixzZGtBcHBpZCI6IjEwMjRhcHBpZCIsInNpZ24iOiIzOTBkNzQzYzA5ZDI0MjhjM2RkZTZmY2FlM2E4MTY2ZjY2ZmQ0NTJhOWM5Y2RiMGU1NjdmMzAxODI2OWUzNDNkIiwidHMiOiIyMDE1MDcyMzE1MDAyOCIsInVJZCI6InVJZCJ9

####4.2 登录验证参数结构

**当前时间戳ts为**

	20150723150028

**游戏服务端密钥为:**

	654321

**登录验证参数签名前的源串：**

	authInfo=eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsIm5hbWUiOiJuYW1lIixzZGtBcHBpZCI6IjEwMjRhcHBpZCIsInNpZ24iOiIzOTBkNzQzYzA5ZDI0MjhjM2RkZTZmY2FlM2E4MTY2ZjY2ZmQ0NTJhOWM5Y2RiMGU1NjdmMzAxODI2OWUzNDNkIiwidHMiOiIyMDE1MDcyMzE1MDAyOCIsInVJZCI6InVJZCJ9&ts=20150723150028&type=verify_session654321

**对应SHA256签名为：**

	d068f342e04926a0fcbd19db0685984d1f531bacbcc94ecfd4abf57fe7418c1a

**请求样例：**

	http://pay.xgsdk.com:8180/account/login/1024appid?authInfo=eyJhdXRoVG9rZW4iOiJhdXRoVG9rZW4iLCJjaGFubmVsSWQiOiJtaSIsIm5hbWUiOiJuYW1lIixzZGtBcHBpZCI6IjEwMjRhcHBpZCIsInNpZ24iOiIzOTBkNzQzYzA5ZDI0MjhjM2RkZTZmY2FlM2E4MTY2ZjY2ZmQ0NTJhOWM5Y2RiMGU1NjdmMzAxODI2OWUzNDNkIiwidHMiOiIyMDE1MDcyMzE1MDAyOCIsInVJZCI6InVJZCJ9&sign=d068f342e04926a0fcbd19db0685984d1f531bacbcc94ecfd4abf57fe7418c1a&ts=20150723150028&type=verify_session

###5、返回值样例


		{
	    "code": "0",
	    "msg": "success",
	    "data": {
	        "channelId": "mi",
	        "sessionId": "woidkljfhnav98a7fdgonqelrtnsdvaxasdfasdf",
	        "uId": "3099245"
	    }
	}

###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>DK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.7.31</td>
</tr>
</table>
