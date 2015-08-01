#西瓜SDK 原生Android版 客户端接入文档

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
****
<!--

###文档信息
	文档名：原生Android版 客户端接入文档
	作者：周海兵
	SDK版本：2.0
	文档版本：1.0
	日期：2015.8.1
-->

###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>SDK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0 </td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.8.1</td>
</tr>
</table>

****

##1. 西山居渠道版SDK概述

	此文档为使用原生android引擎游戏客户端的接入文档。详细说明了接入渠道版SDK需要的资料和开发步骤

###1.1 SDK下载包
	渠道版SDK下载包包含：
	1. 西瓜SDKV2的Jar包：xgsdk-core.jar，xgsdk-data.jar，xgsdk-lib.jar，xgsdk-api.jar
	2. xgsdk-demo
	3. XGSDK 原生Android版 客户端接入文档

##2. 配置环境与快速接入简介

###2.1 开发和接入所需基本环境
Android开发环境：

	Android版本：Android2.2 以上
	Android开发工具：Android SDK和Android Eclipse等

###2.2 接入步骤简介
	1.配置android权限(AndroidManifest.xml文件)
	2.增加闪屏
	3.接入生命周期接口
	4.接入必接接口

###2.3 快速接入
#####2.3.1. 配置AndroidManifest.xml文件<br />

	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />


#####2.3.2. 增加闪屏

#####2.3.3.接入生命周期接口<br />
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

#####2.3.4. 接入必接接口
#####支付接口<br />
pay(final Activity activity, PayInfo payInfo,PayCallBack payCallBack)

	参数说明：
		payment:

		paycallback:

#####登入接口<br />
login(Activity activity, String customParams)

	参数说明：
		customParams：

#####登出接口<br />
logout(Activity activity, String customParams)

	参数说明：
		customParams：

#####退出接口<br />
exit(Activity activity, ExitCallBack exitCallBack,String customParams)

	参数说明：
		exitCallBack：
		customParams：

#####进入游戏接口<br />
onEnterGame(Activity activity, XGUser user, RoleInfo roleInfo,GameServerInfo serverInfo)

	参数说明：
		user:

		RoleInfo:
		serverInfo:

#####角色等级接口<br />
onRoleLevelup(Activity activity, RoleInfo roleInfo)

	参数说明：
		roleInfo：

#####创建角色接口<br />
onCreateRole(Activity activity, RoleInfo roleInfo)

	参数说明：
		roleInfo:
