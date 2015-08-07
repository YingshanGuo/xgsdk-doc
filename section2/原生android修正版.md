#西瓜SDK（原生Android版 ）接入文档

****
<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css" />

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


##1. 文档概述

<font face="微软雅黑">此文档为使用原生android引擎游戏客户端的接入文档。</br>
本文介绍如何在原生引擎平台下，Android游戏客户端快速接入西瓜SDK。
文档分成三大部分:接入环境下载/搭建，西瓜SDK接口说明以及参考代码。逐步细述了整个接入过程；同时罗列出了4种类型的接口，
分别为：用户中心接口、充值接口、统计接口、扩展接口，便于游戏方的接入人员可以按照需求更加快速便捷的进行接入。
</font>


###1.1 SDK下载包
	渠道版SDK下载包包含：
	1. 西瓜SDKV2的Jar包：xgsdk-core.jar，xgsdk-data.jar，xgsdk-lib.jar，xgsdk-api.jar
	2. xgsdk-demo.jar
	3. XGSDK 原生Android版 客户端接入文档

##2. 配置环境与快速接入简介

###2.1 开发和接入所需基本环境
Android开发环境：

	Android版本：Android2.2 以上
	Android开发工具：Android SDK和Android Eclipse等

###2.2 接入步骤简介
	1.配置android权限(AndroidManifest.xml文件)
	2.增加闪屏
	3.接入生命周期接口（必接）
	3.接入登录接口（必接）
	4.接入充值接口（必接）
	5.接入统计接口（部分必接）
	6.接入扩展接口

###2.3 快速接入
####2.3.1. 配置AndroidManifest.xml文件<br />

	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <uses-permission android:name="android.permission.FLASHLIGHT" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.READ_SMS" />
    <uses-permission android:name="android.permission.RECEIVE_SMS" />


####2.3.2. 增加闪屏

####2.3.3 接入生命周期接口（必接）<br />
在游戏各个Activity生命周期中调用SDK生命周期接口<br />



    @Override
    protected void onPause() {
        super.onPause();
        XGSDK.getInstance().onPause(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        XGSDK.getInstance().onResume(this);
    }

    @Override
    protected void onStart() {
        super.onStart();
        XGSDK.getInstance().onStart(this);
    }

    @Override
    protected void onStop() {
        super.onStop();
        XGSDK.getInstance().onStop(this);
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        XGSDK.getInstance().onRestart(this);
    }

	@Override
    protected void onDestroy() {
        super.onDestroy();
        XGSDK.getInstance().onDestory(this);
    }

onCreate生命周期方法比较特殊<br />
<font color="red">注：setUserCallBack在init之前调用</font>

	@Override
    protected void onCreate(Bundle savedInstanceState) {
        XGSDK.getInstance().onCreate(this);
        XGSDK.getInstance().setUserCallBack(new UserCallBack() {}；
		XGSDK.getInstance().init(this);
	}


####2.3.4 接入登录接口（必接）

#####登入接口<br />
login(Activity activity, String customParams)

	参数说明：
		customParams：登录参数

在UserCallBack中实现登录callback接口（登录成功，登录失败，登录取消）<br />
<font color="red">注：在登录成功callback中，调用xgsdk的onEnterGame接口</font>

		@Override
        public void onLogoutSuccess(String msg) {
            ToastUtil.showToast(MainActivity.this, "logout success." + msg);
        }

        @Override
        public void onLogoutFail(int code, String msg) {
            ToastUtil.showToast(MainActivity.this, "logout fail." + code
                    + " " + msg);
        }

        @Override
        public void onLoginSuccess(String authInfo) {
            Log.w(TAG, "authInfo: \n" + authInfo);
            XGUser user = AuthUtil.auth(MainActivity.this, authInfo);
            XGSDK.getInstance().onEnterGame(MainActivity.this, user,
                    mRoleInfo, mServerInfo);
        }

        @Override
        public void onLoginFail(int code, String msg) {
            ToastUtil.showToast(MainActivity.this, "login fail." + code
                    + " " + msg);
        }

        @Override
        public void onInitFail(int code, String msg) {
            ToastUtil.showToast(MainActivity.this, "init fail." + code
                    + " " + msg);
        }

        @Override
        public void onLoginCancel(String msg) {
            ToastUtil.showToast(MainActivity.this, "login cancel." + msg);
        }

#####登出接口<br />
logout(Activity activity, String customParams)

	参数说明：
		customParams：登出参数

#####退出接口<br />
exit(Activity activity, ExitCallBack exitCallBack,String customParams)

	参数说明：
		exitCallBack：退出回调
		customParams：退出参数
####2.3.5 接入充值接口（必接）
#####支付接口<br />
pay(final Activity activity, PayInfo payInfo,PayCallBack payCallBack)

	参数说明：
		payInfo: 支付信息对象，包含产品ID、名称和数量等
		paycallback:支付回调

在调用pay接口时，需要实现pay支付callback(支付成功，支付失败，支付取消）

 	@Override
    public void onSuccess(String msg) {
        ToastUtil.showToast(MainActivity.this,
                "pay success." + msg);

    }

    @Override
    public void onFail(int code, String msg) {
        ToastUtil.showToast(MainActivity.this,
                "pay fail." + code + " " + msg);
    }

    @Override
    public void onCancel(String msg) {
        ToastUtil.showToast(MainActivity.this,
                "pay cancel." + msg);

    }

#####2.3.6 接入统计接口（部分必接）
#####进入游戏接口（必接）
注：此接口在调用登录成功callback的时候调用<br />
onEnterGame(Activity activity, XGUser user, RoleInfo roleInfo,GameServerInfo serverInfo)

	参数说明：
		user: 用户对象
		RoleInfo:角色信息
		serverInfo:服务器信息

#####角色等级接口（必接）<br />
onRoleLevelup(Activity activity, RoleInfo roleInfo)

	参数说明：
		roleInfo：角色信息

#####创建角色接口（必接）<br />
onCreateRole(Activity activity, RoleInfo roleInfo)

	参数说明：
		roleInfo:角色信息

####2.3.6 接入扩展接口
#####创建新的意图接口
	@Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        XGSDK.getInstance().onNewIntent(this, intent);
    }



#####切换活动页面接口
	@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        XGSDK.getInstance().onActivityResult(this, requestCode, resultCode,
                data);
    }

#####切换账号接口
switchAccount(Activity activity, String customParams)

	参数说明：
		customParams：切换账号参数

#####个人中心接口
openUserCenter(Activity activity, String customParams)

	参数说明：
		customParams：个人中心参数


****

###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.8.1</td>
</tr>
</table>
