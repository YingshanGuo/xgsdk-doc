# 西瓜SDK（原生 IOS 版 ）接入文档


<div id="category" style="display:none"></div>


## 1. 文档概述

此文档为使用原生 IOS 游戏客户端的接入文档。  
本文介绍如何在原生平台下，IOS 游戏客户端快速接入西瓜SDK。
文档分成两大部分:主动调用接口和回调函数接口。
包括了5种类型的接口，分别为：** 必选接口、可选接口、统计接口、分享接口、推送接口 ** ，便于游戏方的接入人员可以按照需求更加快速便捷的进行接入。




## 2. 快速接入简介

<ol type=“1”>
<li><a href="#permission">主动调用接口</a></li>
      <ul>
	    <li><a href="#splash">必选接口</a></li>
	    <li><a href="#pay">可选接口</a></li>
    	<li><a href="#liftcyle">统计接口</a></li>
	    <li><a href="#userAndRole">分享接口</a></li>
	    <li><a href="#statistics">推送接口</a></li>
      </ul>
	<li><a href="#extend">回调函数接口</a></li>
  <ul>
  <li><a href="#splash1">初始化回调</a></li>
  <li><a href="#pay1">登录回调</a></li>
  <li><a href="#liftcyle1">登出回调</a></li>
  <li><a href="#userAndRole1">支付回调</a></li>
  </ul>
</ol>

## 3. 接入说明
<a id="permission"></a>
### 3.1 主动调用接口
<a id="splash"></a>
#### 3.1.1 必选接口
- 初始化  
接口说明：应用加载结束后，即：  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {}中调用。  
参数说明：<table>
<tr>
<td>customInfo</td>
<td>扩展字段，暂时不用传值</td>
</tr>
<tr>
<td>xgsdkDelegate</td>
<td>回调接收类。请先实现XgsdkDelegate中的方法</td>
</tr>
</table>
-(void)XGInitWith:(NSString *)customInfo xgsdkDelegate:(id<XgsdkDelegate>) xgsdkDelegate;

- 登录  
接口说明：登陆时调用，将拉起登录界面。  
参数说明：<table>
<tr>
<td>customInfo</td>
<td>扩展字段，暂时不用传值</td>
</tr>
</table>  -(void)XGLoginWith:(NSString *)customInfo;

- 进入游戏  
接口说明：登陆成功后，正式进入游戏时调用。  
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
</table>  -(void) onEnterGame:(XGRoleInfo*) roleInfo;

- 登出  
接口说明：退出游戏时调用。  
参数说明：<table>
<tr>
<td>customInfo</td>
<td>扩展字段，暂时不用传值</td>
</tr>
</table>
-(void)XGLogout:(NSString *)customInfo;

- 支付  
接口说明：购买时调用。将拉起支付界面。  
参数说明：<table>
<tr>
<td>buyInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
</table>  -(void)XGPaymentWithBuyInfo:(XGBuyInfo*)buyInfo;

<a id="pay"></a>
#### 3.1.2 可选接口
- 获取渠道ID  
接口说明：需要得到当前渠道id时调用。  
参数说明：无  
-(NSString *)XGgetChannelId;

- 绑定账号  
接口说明：仅小米渠道需要调用此接口，绑定账号用。  
参数说明：<table>
<tr>
<td>customInfo</td>
<td>扩展字段，暂时不用传值</td>
</tr>
</table>  -(void)XGBindAccount:(NSString *) customInfo;

- 进入用户中心  
接口说明：需要在游戏过程中，打开sdk的用户中心界面，调用此接口。  
参数说明：<table>
<tr>
<td>customInfo</td>
<td>扩展字段，暂时不用传值</td>
</tr>
</table>  -(void)XGUserCenter:(NSString *)customInfo;

- 是否需要绘制用户中心按钮  
接口说明：少量的sdk强制要求绘制用户中心按钮，该接口用于区分。  
参数说明：无  
-(BOOL) isShowUserCenterAvailable;


<a id="liftcyle"></a>
#### 3.1.3 统计接口
- 等级提升  
接口说明：角色提升时调用。  
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
</table>  -(void) onRoleLevelUp:(XGRoleInfo*) roleInfo;

- 自定义  
接口说明：使用自定义事件方式上报。  
参数说明：无  
-(void) onEvent:(XGRoleInfo*) roleInfo eventId:(NSString*) eventID enventDesc:(NSString*)eventDesc eventValue:(NSString*)eventValue eventBody:(NSString*) eventContent;

- 任务开始  
接口说明：进入某个任务或关卡时调用。  
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>missionId</td>
<td>任务号</td>
</tr>
<tr>
<td>missionName</td>
<td>任务名称</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onMissionBegin:(XGRoleInfo*) roleInfo missionId:(NSString*) missionId missionName:(NSString*)missionName customParams:(NSString*)customParams;

- 任务成功  
接口说明：任务或关卡成功时调用。
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>missionId</td>
<td>任务号</td>
</tr>
<tr>
<td>missionName</td>
<td>任务名称</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onMissionSuccess:(XGRoleInfo*) roleInfo missionId:(NSString*) missionId missionName:(NSString*)missionName customParams:(NSString*)customParams;

- 任务失败  
接口说明：任务或关卡失败时调用。
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>missionId</td>
<td>任务号</td>
</tr>
<tr>
<td>missionName</td>
<td>任务名称</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onMissionFail:(XGRoleInfo*) roleInfo missionId:(NSString*) missionId missionName:(NSString*)missionName customParams:(NSString*)customParams;

- 虚拟货币购买  
接口说明：购买虚拟货币（如元宝）成功时调用。  
参数说明：<table>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>amount</td>
<td>数量</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onVirtualCurrencyPurchase:(XGRoleInfo*) roleInfo amount:(NSString*)amount customParams:(NSString*)customParams;


- 虚拟货币获赠  
接口说明：获得奖励时调用。  
参数说明：<table>
<tr>
<td>reason</td>
<td>获得奖励的原因</td>
</tr>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>amount</td>
<td>数量</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onVirtualCurrencyReward:(XGRoleInfo*) roleInfo reason:(NSString*)reason amount:(NSString*)amount customParams:(NSString*)customParams;


- 虚拟货币消费  
接口说明：消费虚拟货币（如元宝）时调用。  
参数说明：<table>
<tr>
<td>itemName</td>
<td>消费项目</td>
</tr>
<tr>
<td>roleInfo</td>
<td>值对象，参考xgsdk.h中其属性定义即可</td>
</tr>
<tr>
<td>amount</td>
<td>数量</td>
</tr>
<tr>
<td>customParams</td>
<td>自定义参数（json格式字符串）</td>
</tr>
</table>
-(void) onVirtualCurrencyConsume:(XGRoleInfo*) roleInfo itemName:(NSString*)itemName amount:(NSString*)amount customParams:(NSString*)customParams;

- 测试网速  
接口说明：需要测试网速时调用，调用此接口后，西瓜sdk的统计平台可展示玩家的手机端与被测试服务端的网络连接情况。  
参数说明：<table>
<tr>
<td>serverHost</td>
<td>待测试的url，如：www.baidu.com </td>
</tr>
</table>
-(void) pingServer:(NSString*) serverHost;

<a id="userAndRole"></a>
#### 3.1.4 分享接口
若游戏选择接入xgsdk的分享功能，则需要调用本接口。  
-(void)openShare:(NSString*)mediaObj;  
-(void)directShare:(int)channelValue mediaObj:(NSString*)mediaObj;


<a id="statistics"></a>
#### 3.1.4 推送接口
若游戏选择接入xgsdk的推送功能，则需要调用本接口。  
-(void)setPushActivityBlock:(void (^)(NSString* activity ,NSString* jsonParam))block;


<a id="extend"></a>
### 3.2 回调函数接口
初始化，登陆，登出，支付是异步方法，游戏方需实现回调函数协议类XgsdkDelegate。

<a id="splash1"></a>
#### 3.2.1 初始化回调
参数说明：
<table>
<tr>
<td>code</td>
<td>0为成功，其他为失败</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
</table>

-(void) onInitFinishWithResultCode:(NSInteger) code resultMsg: (NSString*) msg;

<a id="pay1"></a>
#### 3.2.2 登录回调

- 登录成功  
参数说明：<table>
<tr>
<td>code</td>
<td>登陆成功结果码，固定为0</td>
</tr>
<tr>
<td>authInfo</td>
<td>用于做登陆验证的加密串</td>
</tr>
</table>
-(void) onLoginSuccessWithResultCode:(NSInteger) code authInfo: (NSString*) authInfo;

- 登录失败
参数说明：<table>
<tr>
<td>code</td>
<td>登陆失败结果码，固定为1</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
<tr>
<td>channelCode</td>
<td>渠道返回的登陆错误码</td>
</tr>
</table>
-(void) onLoginFailWithResultCode:(NSInteger) code resultMsg: (NSString*) msg channelCode:(NSString*) channelCode;

- 登录取消
参数说明：<table>
<tr>
<td>code</td>
<td>登陆取消结果码，固定为2</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
</table>
-(void) onLoginCancelWithResultCode:(NSInteger) code resultMsg:(NSString*) msg;

<a id="liftcyle1"></a>
#### 3.2.3 登出回调

参数说明：
<table>
<tr>
<td>code</td>
<td>登出结果码，固定为0</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>

</table>  

-(void) onLogoutFinishWithResultCode:(NSInteger) code resultMsg:(NSString*) msg;


<a id="userAndRole1"></a>
#### 3.2.4 支付回调
- 支付成功  
参数说明：
<table>
<tr>
<td>code</td>
<td>支付成功结果码，固定为0</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
<tr>
<td>gameTradeNo</td>
<td>游戏订单号</td>
</tr>
<tr>
<td>xgTradeNo</td>
<td>西瓜sdk订单号</td>
</tr>
</table>
-(void) onPaySuccessWithResultCode:(NSInteger) code resultMsg:(NSString*) msg gameTradeNO:(NSString*) gameTradeNo xgTradeNO:(NSString*) xgTradeNo;


- 支付取消  
参数说明：
<table>
<tr>
<td>code</td>
<td>支付取消结果码，固定为1</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
<tr>
<td>gameTradeNo</td>
<td>游戏订单号</td>
</tr>
<tr>
<td>channelCode</td>
<td>渠道结果码</td>
</tr>
<tr>
<td>channelMsg</td>
<td>渠道结果信息</td>
</tr>
</table>
-(void) onPayCancelWithResultCode:(NSInteger) code resultMsg:(NSString*) msg gameTradeNO:(NSString*) gameTradeNo channelCode:(NSString*) channelCode channelMsg:(NSString*) channelMsg;


- 支付失败  
参数说明：<table>
<tr>
<td>code</td>
<td>支付失败结果码，固定为2</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
<tr>
<td>gameTradeNo</td>
<td>游戏订单号</td>
</tr>
<tr>
<td>channelCode</td>
<td>渠道结果码</td>
</tr>
<tr>
<td>channelMsg</td>
<td>渠道结果信息</td>
</tr>
</table>
-(void) onPayFailWithResultCode:(NSInteger) code resultMsg:(NSString*) msg gameTradeNO:(NSString*) gameTradeNo channelCode:(NSString*) channelCode channelMsg:(NSString*) channelMsg;

- 支付正在进行  
参数说明：<table>
<tr>
<td>code</td>
<td>支付失败结果码，固定为5</td>
</tr>
<tr>
<td>msg</td>
<td>消息描述</td>
</tr>
<tr>
<td>gameTradeNo</td>
<td>游戏订单号</td>
</tr>
<tr>
<td>channelCode</td>
<td>渠道结果码</td>
</tr>
<tr>
<td>channelMsg</td>
<td>渠道结果信息</td>
</tr>
</table>

 ** 备注：此时说明支付过程尚未完成，需等待。建议游戏给玩家以提示，并限制玩家的再次购买请求。**  

 -(void) onPayProgressWithResultCode:(NSInteger) code resultMsg:(NSString*) msg gameTradeNO:(NSString*) gameTradeNo channelCode:(NSString*) channelCode channelMsg:(NSString*) channelMsg;


 - 支付完成、结果未知  
 参数说明：<table>
 <tr>
 <td>code</td>
 <td>支付失败结果码，固定为3</td>
 </tr>
 <tr>
 <td>msg</td>
 <td>消息描述</td>
 </tr>
 <tr>
 <td>gameTradeNo</td>
 <td>游戏订单号</td>
 </tr>
 <tr>
 <td>channelCode</td>
 <td>渠道结果码</td>
 </tr>
 <tr>
 <td>channelMsg</td>
 <td>渠道结果信息</td>
 </tr>
 </table>

 -(void) onPayOthersWithResultCode:(NSInteger) code resultMsg:(NSString*) msg gameTradeNO:(NSString*) gameTradeNo channelCode:(NSString*) channelCode channelMsg:(NSString*) channelMsg;
****

### 文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.8.1</td>
</tr>
</table>
