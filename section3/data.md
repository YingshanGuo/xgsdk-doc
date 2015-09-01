# 西瓜SDK数据统计服务端接入文档



## 1. 数据统计服务器接入文档

此文档为数据统计服务器的接入文档，请参考本文档，严格按照文档中接口的描述、说明、格式进行服务器端的数据统计接入。

### 1.1 接口描述
<table>

<tr>
<td> 接口类型 </td>
<td> HTTP POST</td>
</tr>
<tr>
<tr>
<td>接口URL</td>
<td>http://data.xgsdk.com/bisdk/batchpush/{version}/{appid}</td>
</tr>
<tr>
<td>接口内容</td>
<td>version为接口版本，目前版本为2.0；appid为xgsdk分配给各个游戏的唯一ID；具体值可以通过xgsdk的官网页面申请** http://dev.xgsdk.com；例如A游戏申请的appid是1234，则接口URL是 http://data.xgsdk.com/bisdk/batchpush/2.0/1234** </td>
</tr>
<tr>
<td>接口使用</td>
<td>统计消息需要采取批量方式发送，可以采取每50条或每5分钟发送一次的策略</td>
</tr>
</table>

### 1.2 消息说明与格式

#### 1.2.1简介




**消息结构：**  
post的消息采取JSON结，内容由包头和包体组成：&#123;&#123;head&#125;,&#123;content&#125;&#125;。包头包含整个消息包的一些基础信息，包体由多条消息组成，每条消息的结构如下：  
**&#123;&#123;消息头&#125;&#123;&#123;消息体&#125;&#125;**        除消息头和消息体是必选，其它结构为可选，具体参考具体接口的消息结构说明。




**消息样例：**


```
{
  "head": //包头
   ｛
     "batchTimestamp":"2014-12-07 19:17:29.001",  //消息时间戳,消息发送的时间
     "datasource":"server",
     "batchDataId":"2fd92ds83asd8223sd82gasd82fg323",  //批量消息ID，全局唯一
     "sign": 0ca175b9c0f726a831d895e269332461 //消息签名，具体见签名机制说明
    ｝,
  "content":[ //包体,内部包含多条具体消息

     {
       ...//单个消息具体参考下面各种消息的样例
      },
      {
       ...//单个消息具体参考下面各种消息的样例
       }
   ]
}

```



#### 1.2.2消息格式

**包头：**

```
 "head":
  {
    "batchTimestamp":string("yyyy-MM-dd HH:mm:ss.sss"),
    "batchDataId":string,
    "datasource"：string,
    "sign":string,
    "appId": string,
    "appVersion":string
  },
```

 参数说明：
<table>
	<tr>
		<th> 参数</th>
		<th> 参数类型</th>
		<th> 最大长度 </th>
		<th> 说明</th>
		<th> 必需 </th>
	</tr>

	<tr>
		<td> batchTimestamp </td>
		<td> String </td>
		<td> 23 </td>
		<td> 消息发送时的时间戳</td>
		<td> Y </td>
	</tr>

	<tr>
		<td> datasource </td>
		<td> string </td>
		<td>10 </td>
		<td> 数据来源，client(客户端)或server(服务端) </td>
		<td> Y </td>
	</tr>

	<tr>
		<td> appId </td>
		<td> string </td>
		<td> 10 </td>
		<td> xgsdk分配的appId </td>
		<td> Y</td>
	</tr>

	<tr>
		<td> appVersion </td>
		<td> string </td>
		<td> 10 </td>
		<td> 游戏的版本信息 </td>
		<td> Y</td>
	</tr>

	<tr>
		<td> sign </td>
		<td> string </td>
		<td> 40 </td>
		<td>消息MD5签名,签名方式参考下面"签名机制" </td>
		<td> Y </td>
	</tr>
</table>



**包头：**


```
	"content":[
		{消息1},{消息2},...,{消息N}
	]

```


**attribute头：**

```
	{{attribute}}:
     "msgType":enum,
     "msgId":string, //消息ID，全局唯一
     "channel":string, //渠道ID
     "deviceId":string, //设备ID
     "os":string, //操作系统
     "network":string, //网络连接类型: wifi,gsm,3g,4g
     "clientIp":string, //客户端IP
     "sdkVersion":string, //ios,andriod sdk 版本号
     "osVersion":string, //操作系统版本号
     "cpuFreq":string, //cpu信息
     "memTotal":string, //内存大小
     "deviceBrand":string, //设备品牌：Xiaomi
     "deviceModel":string, //设备型号：MI 4LTE
     "isJailBroken":boolean, //ios是否越狱
     "isPirated":boolean, // ios是否破解
     "deviceScreen": string, //设备屏幕: 1024*768
     "carrier": "中国移动", //
     "country": "CN",  //国家
     "language": "zh", //语言
     "mac": "c4:6a:b7:a3:92:74", // mac地址
```


**参数说明：**

<table >

<tr>
<th> 参数 </th>
<th> 参数类型 </th>
<th> 最大长度 </th>
<th> 说明 </th>
<th> 必需
</th>
</tr>

<tr>
<td> msgType </td>
<td> enum </td>
<td>  </td>
<td> ("role.recharge") </td>
<td> Y
</td>
</tr>

<tr>
<td> msgId </td>
<td> string </td>
<td> 40 </td>
<td> 消息ID，需要全局唯一 </td>
<td> Y
</td></tr>
<tr>

<td> channel </td>
<td> string </td>
<td> 128 </td>
<td> 渠道ID，支持分层，以支持一级，二级，三级渠道等层次划分，示例值： xiaomi/home_page/banner1 </td>
<td> Y
</td></tr>
<tr>
<td> channelDesc </td>
<td> string </td>
<td> 128 </td>
<td> 渠道描述，支持分层，以支持一级，二级，三级渠道等层次划分，示例值： 小米/主页/广告条1 </td>
<td> N
</td></tr>
<tr>
<td> traceId </td>
<td> string </td>
<td> 40 </td>
<td> 跟踪ID，用于跟踪用户后续行为的ID </td>
<td> N
</td></tr>
<tr>
<td> ext </td>
<td> string </td>
<td> 40 </td>
<td> 扩展字段，采用key value的json map结构，用于在预定义中没有覆盖的自定义数据上报 </td>
<td> N
</td></tr>
<tr>
<td> deviceId </td>
<td> String </td>
<td> 100 </td>
<td> 设备id </td>
<td> Y
</td></tr>
<tr>
<td> os </td>
<td> string </td>
<td> 100 </td>
<td> 系统名：iOS、Android、WM等 </td>
<td> Y
</td></tr>
<tr>
<td> network </td>
<td> string </td>
<td> 20 </td>
<td> 网络类型wifi、gsm、3g等 </td>
<td> Y
</td></tr>
<tr>
<td> clientIp </td>
<td> String </td>
<td> 64 </td>
<td> 客户端IP，客户端接入统计时候可以不传，服务端接入统计时候需要传入 </td>
<td> N
</td></tr>
<tr>
<td> sessionId</td>
<td> String </td>
<td> 32</td>
<td> 会话ID，随机生成的32位字符串，用于标识一个登录，登出周期会话，用于统计在线时长使用 </td>
<td> N
</td></tr>
<tr>
<td> sdkVersion</td>
<td> String </td>
<td> 32</td>
<td> andriod,ios版本，如1.3.1 </td>
<td> N
</td></tr>
<tr>
<td> appVersion</td>
<td> String </td>
<td> 32</td>
<td> 应用自身版本号，如1.3.1 </td>
<td> N
</td></tr></table>


#### 1.2.3消息枚举类型说明


角色充值("role.recharge")  
应用场景：
<ol>
	<li>如果游戏接入XGSDK客户端，不用单独发送该消息，XGSDK充值到账后自动发送该消息</li>
	<li>如果游戏没有接入XGSDK客户端，或服务端发送该消息，可以按下面格式发送消息</li>
</ol>
影响指标：参见xgsdk数据分析页面具体菜单和指标项
<ol type='1' start='1'>
	<li>"实时概况"-"充值金额"</li>
	<li>"付费分析"-所有指标</li>
	<li>"用户周期价值"-所有指标</li>
	<li>"收入统计"-所有指标</li>
	<li>"充值统计"-所有指标</li>
	<li>"付费率"-"付费率"</li>
</ol>
格式：


```
	{
  		{{attribute}}
  		"zone":string,
  		"server":string,
  		"accountId":string,
  		"accountName":string,
  		"roleId":string,
  		"roleName":string,
  		"roleType":string,
  		"roleLevel":int,
  		"rechargeChannel":string,
  		"currency":enum，
  		"money":number,
  		"gold":int,
  		"bindingGold":int,
  		"curGold":int,
  		"curBindingGold":int,
  		"timestamp":string("yyyy-MM-dd HH:mm:ss.sss")
	}

```

参数说明：
<table>

<tr>
<th> 参数 </th>
<th> 参数类型 </th>
<th> 最大长度 </th>
<th> 说明 </th>
<th> 必需
</th></tr>
<tr>
<td> {attribute} </td>
<td>  </td>
<td>  </td>
<td>  </td>
<td> Y
</td></tr>
<tr>
<td> zone </td>
<td> string </td>
<td> 40</td>
<td> 游戏区 </td>
<td> N
</td></tr>
<tr>
<td> server </td>
<td> string </td>
<td> 40 </td>
<td> 游戏服 </td>
<td> Y
</td></tr>
<tr>
<td> accountId </td>
<td> string </td>
<td> 40 </td>
<td> 账号ID </td>
<td> Y
</td></tr>
<tr>
<td> accountName </td>
<td> string </td>
<td> 60 </td>
<td> 账号名 </td>
<td> Y
</td></tr>
<tr>
<td> roleId </td>
<td> string </td>
<td> 40 </td>
<td> 角色ID </td>
<td> Y
</td></tr>
<tr>
<td> roleName </td>
<td> string </td>
<td> 60 </td>
<td> 角色名 </td>
<td> Y
</td></tr>
<tr>
<td> roleType </td>
<td> string </td>
<td> 20 </td>
<td>  角色类型 </td>
<td> N
</td></tr>
<tr>
<td> roleLevel </td>
<td> int </td>
<td>  </td>
<td> 角色等级 </td>
<td> Y
</td></tr>
<tr>
<td> rechargeChannel </td>
<td> string </td>
<td> 40 </td>
<td> 充值渠道,例如支付宝、财付通等 </td>
<td> N
</td></tr>
<tr>
<td> currency </td>
<td> enum </td>
<td>  </td>
<td> ("CNY", "KER", "HKD", "USD", "VND", "THB", "PHP", ...),确认多币种的处理方式 </td>
<td> Y
</td></tr>
<tr>
<td> money </td>
<td> number </td>
<td>  </td>
<td> 充值金额，浮点数，入库去尾法保留2位小数 </td>
<td> Y
</td></tr>
<tr>
<td> gold </td>
<td> int </td>
<td>  </td>
<td> 充值游戏代币数量 </td>
<td> Y
</td></tr>
<tr>
<td> bindingGold </td>
<td> int </td>
<td>  </td>
<td> 充值游戏赠送代币数量 </td>
<td> N
</td></tr>
<tr>
<td> timestamp </td>
<td> string </td>
<td>  </td>
<td> 充值时间 </td>
<td> Y
</td></tr>
</table>


消息样例：


```

	{
        //"attribute"
        "msgType":"device.connect", //消息类型，具体见消息类型说明
        "msgSn":"20141207191729001", //消息序号
        "channel":"mi",//渠道ID
        "deviceId":"599a830eb0019b421ce35ec4a80e0e71",//设备ID
        "os":"android",//安卓操作系统
        "network":"wifi", //设备网络类型
  		"server":"风云争霸",
  		"accountId":"acc12345",
  		"accountName":"13422371324",
  		"roleId":"role12345",
  		"roleName":"小清新",
  		"roleLevel":23,
  		"currency":"CNY",
  		"money": 6，
  		"gold": 100， //100代币
  		"timestamp":"2015-04-07 15:52:30.250"
	}

```



#### 1.2.4签名机制

签名机制采取MD5签名，使用包体的内容(content里面的内容)加上appKey拼接后签名，其中appKey为xgsdk唯一分配给一个游戏的密钥值。

---

###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.7.30</td>
</tr>
</table>
