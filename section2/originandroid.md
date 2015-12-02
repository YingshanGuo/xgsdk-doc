# 西瓜SDK（原生Android版 ）接入文档


<div id="category" style="display:none"></div>


## 1. 文档概述

此文档为使用原生 android 引擎游戏客户端的接入文档。  
本文介绍如何在原生引擎平台下，Android 游戏客户端快速接入西瓜SDK。
文档分成三大部分:西瓜 SDK 下载，配置环境，各个接口的接入说明和样例代码。逐步细述了整个接入过程；  
同时罗列出了4种类型的接口：分别为：**用户与角色接口、充值接口、统计接口、扩展接口** ，便于游戏方的接入人员可以按照需求更加快速便捷的进行接入。


<a id="docStructure"></a>

### 1.1 文档结构

<ol type=“1”>
    <li><a href="#doc">文档概述</a>
        <ul type="disc">
		<li><a href="#docStructure">文档结构</a></li>
		<li><a href="#SDKDownload">SDK下载包</a></li>
		</ul>
    </li>
    <li>
		<a href="#configure">配置环境与快速接入简介</a>
			<ul type="disc">
				<li><a href="#conditions">开发和接入所需基本环境</a></li>
				<li><a href="#DebugWay">游戏端本地(无需打包)调试方法</a></li>
			</ul>
	</li>
	<li>
		<a href="#adjust">修改相应平台工程的配置信息</a>
			<ul type="disc">
				<li><a href="#copyJar">复制SDK的Jar包</a></li>
				<li><a href="#androidManifest">配置android权限</a></li>
				<li><a href="#splashActivity">增加闪频</a></li>
			</ul>
	</li>
    <li>
		<a href="#userInterface">用户接口</a>
			<ul type="disc">
				<li><a href="#liftcycle">接入生命周期接口</a></li>
				<li><a href="#login">登录接口（必接）</a></li>
				<li><a href="#loginCallback">登录回调</a></li>
				<li><a href="#logout">登出接口</a></li>
				<li><a href="#logoutCallback">登出回调</a></li>
				<li><a href="#exit">退出接口（必接）</a></li>
				<li><a href="#exitCallback">退出回调</a></li>
        <li><a href="#releaseResurce">释放资源（必接）</a></li>
			</ul>
	</li>
	<li>
		<a href="#payInterface">充值接口</a>
			<ul type="disc">
				<li><a href="#pay">支付接口（必接）</a></li>
				<li><a href="#payCallback">支付回调</a></li>
			</ul>
	</li>

	<li>
		<a href="#statics">统计接口</a>
			<ul type="disc">
				<li><a href="#onCreateRole">创建角色（必接）</a></li>
				<li><a href="#onRoleLevelup">角色升级（必接）</a></li>
				<li><a href="#onEnterGame">进入游戏（必接）</a></li>
                <li><a href="#onpayfinish">支付完成</a></li>
			</ul>
	</li>
	<li>
		<a href="#extraInterface">扩展接口</a>
			<ul type="disc">
				<li><a href="#switchAccount">切换账号</a></li>
				<li><a href="#onEvent">自定义事件</a></li>
				<li><a href="#onMissionBegin">任务开始</a></li>
				<li><a href="#onMissionSuccess">任务成功</a></li>
				<li><a href="#onMissionFail">任务失败</a></li>
				<li><a href="#onVirtalCurrencyPurchase">购买虚拟货币</li>
				<li><a href="#onVirtalCurrencyReward">赠送虚拟货币</li>
				<li><a href="#onVirtualCurrencyConsume">消费虚拟货币</li>
			</ul>
	</li>
    <li>
		<a href="#version">文档版本</a>
	</li>
</ol>


<a id="SDKDownload"></a>

### 1.2 SDK下载包

**渠道版 SDK 下载包包含：**
1. 西瓜 SDKV2 的 Jar 包：
   xgsdk-new-source-demo.jar,xgsdk-api.jar
2. 测试渠道包：xgsdk-test-1.0.zip  
3. 西瓜sdk（原生Android版)接入文档
<a href = "http://console.xgsdk.com/download.html">原生SDK下载</a>
4. demo运行方法 </br>
下载原生SDK后，得到native-android-demo.zip,解压此文件，
得到xgsdk-api.jar、xgsdk-new-source-demo.jar、
xgsdk-test-1.0.zip工程文件包以及originandroid.md文档文件。
解压文件xgsdk-test-1.0.zip,导入eclipse,复制xgsdk-new-source-demo.jar到工程的libs文件夹下，即可运行。  

**以上描述项目是一个demo测试项目，如果游戏商想测试自己游戏在渠道运行状况，可以用原生游戏jar替换模拟游戏xgsdk-new-source-demo.jar即可。**


<a id="SDKDownload"></a>

## 2. 配置环境与快速接入简介


<a id="conditions"></a>

### 2.1 开发和接入所需基本环境

Android 开发环境如下：  
Android 版本：Android2.2 以上  
Android 开发工具：Android SDK 和 Android Eclipse 等


<a id="DebugWay"></a>

### 2.2  游戏端本地(无需打包)调试方法
1.解压xgsdk-test-1.0.zip,并导入eclipse中 </br>
2.修改xgsdk-channel-test工程的AndroidManifest.xml文件，

        <activity
            android:name="com.xgsdk.client.testdemo.MainActivity"
            android:launchMode="singleTop"
            android:screenOrientation="landscape"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen" >
                 <intent-filter>
                <action android:name="xg.game.MAIN" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
       </activity>

  将com.xgsdk.client.testdemo.MainActivity改为自己游戏原启动Activity ，其他不变，例如

       <activity
           android:name="com.example.game.MainActivity"
           android:launchMode="singleTop"
           android:screenOrientation="landscape"
           android:theme="@android:style/Theme.NoTitleBar.Fullscreen" >
           <intent-filter>
              <action android:name="xg.game.MAIN" />
              <category android:name="android.intent.category.DEFAULT" />
           </intent-filter>
      </activity>

3.修改游戏工程的project.properties文件，添加android.library=true属性

4.打开xgsdk-channel-test的配置文件，将游戏工程作添加为lib依赖文件

5.运行xgsdk-channel-test


<a id="adjust"></a>

## 3.修改相应平台工程的配置信息
<a id="copyJar"></a>

### 3.1 复制SDK的Jar包
在游戏工程中导入lib库

<a id="androidManifest"></a>

### 3.2 配置android权限
在AndroidManifest.xml中配置android权限
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.GET_TASKS" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
```

<a id="splashActivity"></a>
### 3.3 增加闪频
1.游戏母包中AndroidManifest.xml增加 XGSplashActivity 作为启动 Activity 的声明，根据游戏朝向设定android:screenOrientation

```xml
   <activity
	 android:name="com.xgsdk.client.api.splash.XGSplashActivity"
	 android:screenOrientation="portrait"
	 android:theme="@android:style/Theme.NoTitleBar.Fullscreen" >
	 <intent-filter>
		 <action android:name="android.intent.action.MAIN" />
		 <category android:name="android.intent.category.LAUNCHER" />
	 </intent-filter>
</activity>
```

2.AndroidManifest.xml中将游戏原启动 Activity 的 intent-filter 修改为

	<intent-filter>
		 <action android:name="xg.game.MAIN" />
		 <category android:name="android.intent.category.DEFAULT" />
	 </intent-filter>

<a id="userInterface"></a>

## 4.用户接口

<a id="liftcycle"></a>

### 4.1 接入生命周期接口

在游戏各个 Activity 生命周期中调用SDK生命周期接口，样例代码如下：

```java

    @Override
    protected void onStart() {
        super.onStart();
        XGSDK.getInstance().onStart(this);
        //第一行调用父类方法，第二行调用西瓜的方法，顺序固定
		//然后再执行贵游戏的自己的代码逻辑，下面方法类同
    }

     @Override
    protected void onResume() {
        super.onResume();
        XGSDK.getInstance().onResume(this);
    }


    @Override
    protected void onPause() {
        super.onPause();
        XGSDK.getInstance().onPause(this);

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
	@Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        XGSDK.getInstance().onNewIntent(this, intent);
    }
	@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        XGSDK.getInstance().onActivityResult(this, requestCode, resultCode,
                data);
    }

	@Override
	protected void onBackPressed() {
	    super.onBackPressed();
	    XGSDK.getInstance().onBackPressed(this);

	}
	@Override
	protected void onConfigurationChanged(Configuration newConfig) {
	    super.onConfigurationChanged(newConfig);
	    XGSDK.getInstance().onConfigurationChanged(this, newConfig);
	}

    @Override
	public void onSaveInstanceState(Bundle outState,
	        PersistableBundle outPersistentState) {
	    super.onSaveInstanceState(outState, outPersistentState);
	    XGSDK.getInstance().onSaveInstanceState(this, outState);
	}
```

onCreate 生命周期方法比较特殊。  

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
        XGSDK.getInstance().onCreate(this);
        XGSDK.getInstance().setUserCallBack(new UserCallBack() {})；
	}
```
XGSDK.getInstance().setUserCallBack(new UserCallBack() {})；  
以上代码的表示的意思是实现一个匿名的 UserCallBack 对象，该对象中实现了登录成功、失败、取消，登出成功、失败，初始化失败接口，<a href="#usercallback">详见这里描述</a>

<a id="login"></a>
### 4.2 登录接口（必接）


接口定义：public void login(Activity activity, String customParams)  
接口说明：用户登录接口，传入扩展参数。  
<table>
  <tr>
  <td>参数</td>
  <td>说明</td>
  </tr>
	<tr>
	<td>customParams</td>
  <td>该参数用于扩展，传输时使用json格式，接入时若不需要直接置空即可</td>
	</tr>
</table>

<a name="usercallback" ></a>  

<a id="loginCallback"></a>
### 4.3 登录回调
在 UserCallBack 中实现登录 callback 接口（登录成功、失败、取消，登出成功、失败，初始化失败接口）  
例如登陆成功，游戏在此回调中实现登陆成功后的逻辑,其余的 callback 接口类似  
注：在登录成功 callback 中，调用 xgsdk 的 onEnterGame 接口  
样例代码：

		XGSDK.getInstance().setUserCallBack(new UserCallBack() {

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
            public void onLoginSuccess(final String authInfo) {
                Log.w(TAG, "authInfo: \n" + authInfo);
                 XGSDK.getInstance().onEnterGame(
                                        MainActivity.this, mUser, mRoleInfo,
                                        mServerInfo);
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
        });

<a id="logout"></a>
### 4.4 登出接口
接口定义：public void logout(Activity activity, String customParams)

接口说明：用户登出接口，登出传入扩展参数 customParams
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
  <tr>
  <td>customParams</td>
  <td>该参数用于扩展，传输时使用json格式，接入时若不需要直接置空即可</td>
  </tr>
</table>

<a id="logoutCallback"></a>
### 4.5 登出回调
注：登出回调接口（onLogoutFail，onLogoutSuccess）要在 UserCallBack 中实现

    XGSDK.getInstance().setUserCallBack(new UserCallBack() {
            @Override
            public void onLogoutFinish(int code, String msg) {
               ToastUtil.showToast(MainActivity.this, "logout finish." + msg);

            }
    }


<a id="exit"></a>
### 4.6 退出接口（必接）
接口定义：public void exit(Activity activity, ExitCallBack exitCallBack,String customParams)

接口说明：用户退出接口，传入 exitCallback 和扩展参数 customParams
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
  <tr>
  <td>exitCallBack</td>
  <td>退出回调</td>
  </tr>
  <tr>
  <td>customParams</td>
  <td>该参数用于扩展，传输时使用 json 格式，接入时若不需要直接置空即可</td>
  </tr>
  </table>

调用案例代码：

	findViewById(RUtil.getId(getApplicationContext(), "xg_exit"))
                .setOnClickListener(new OnClickListener() {

                    @Override
                    public void onClick(View v) {
                        XGSDK.getInstance().exit(MainActivity.this,
                                new ExitCallBack() {}，null)}}）；

<a id="exitCallback"></a>

### 4.7 退出回调
退出接口存在三个回调方法：  
1. onNoChannelExiter 使用游戏方退出框
2. onExit 直接退出
3. onCancel 取消退出

样例代码：

	new ExitCallBack() {

            @Override
            public void onNoChannelExiter() {
                Dialog dialog = new AlertDialog.Builder(
                        MainActivity.this) ).create();
                      //调用游戏方退出框
                dialog.show();

            }

            @Override
            public void onExit() {
                finish();

            }

            @Override
            public void onCancel() {
                ToastUtil.showToast(MainActivity.this,
                        "回到游戏");

            }
     }

<a id="releaseResurce"></a>

### 4.8 释放资源接口（必接）
接口定义：public void releaseResource(Activity activity, String customParams);  
接口说明：用户退出游戏时释放资源接口，传入扩展参数 customParams

<table>

<tr>
<td>参数</td>
<td>说明</td>
</tr>
  <tr>
  <td>customParams</td>
  <td>该参数用于扩展，传输时使用 json 格式，接入时若不需要直接置空即可</td>
  </tr>

</table>

调用案例代码：与退出接口类似。  


<a id="payInterface"></a>



## 5.充值接口  

<a id="pay"></a>

### 5.1 支付接口（必接）  


接口定义：public void pay(final Activity activity, PayInfo payInfo,PayCallBack payCallBack)

接口说明：充值接口  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
<tr>
<td>payInfo</td>
<td>支付信息对象，包含产品ID、名称和数量等</td>
</tr>
<tr>
<td>paycallback</td>
<td>支付回调</td>
</tr>
</table>

调用案例代码：

	//payInfo 初始化
	.......
	payment.setTotalPrice(totalPrice);
    payment.setProductUnitPrice(1);
    XGSDK.getInstance().pay(MainActivity.this, payment,
            new PayCallBack() {})

#### 支付场景
某个初级玩家，碰到了商品打折优惠，买100个元宝（价值50元）可享受8折优惠，这样其实他是用40元买了100个元宝，那么他的支付清单是：  
<table>
<tr>
<td>商品id</td>
<td>11111 </td>
</tr>
<tr>
<td>产品名称</td>
<td>100个元宝 </td>
</tr>
<tr>
<td>产品描述</td>
<td>100个元宝 </td>
</tr>
<tr>
<td>商品单位</td>
<td>元宝 </td>
</tr>
<tr>
<td>产品打折后价格</td>
<td>100 </td>
</tr>
<tr>
<td>产品打折前价格</td>
<td>100 </td>
</tr>
<tr>
<td>产品单价</td>
<td>50 </td>
</tr>
<tr>
<td>产品总量</td>
<td>1 </td>
</tr>
</table>



**关于 PayInfo 的成员说明**

<table>
<tr>
	<th>参数</th>
	<th>参数类型</th>
    <th>最大长度</th>
	<th>说明</th>
	<th>必须</th>
</tr>
<tr>
	<td>uid</td>
	<td>String</td>
    <td>128</td>
	<td>用户ID,游戏必须使用登录时西瓜服务器返回的uid</td>
	<td>Y</td>
</tr>
<tr>
	<td>productId</td>
	<td>String</td>
    <td>64</td>
	<td>产品ID</td>
	<td>Y</td>
</tr>
<tr>
	<td>productName</td>
	<td>String</td>
    <td>64</td>
	<td>产品名称</td>
	<td>Y</td>
</tr>
<tr>
	<td>productDesc</td>
	<td>String</td>
    <td>128</td>
	<td>产品描述</td>
	<td>Y</td>
</tr>
<tr>
	<td>productUnit</td>
	<td>String</td>
    <td>64</td>
	<td>商品单位，例如元宝</td>
	<td>Y</td>
</tr>
<tr>
	<td>productUnitPrice</td>
	<td>int</td>
    <td>10</td>
	<td>产品单价,单位分</td>
	<td>Y</td>
</tr>
<tr>
	<td>productQuantity</td>
	<td>int</td>
    <td>10</td>
	<td>产品数量</td>
	<td>Y</td>
</tr>
<tr>
	<td>totalAmount</td>
	<td>int</td>
    <td>10</td>
	<td>产品总额,单位分</td>
	<td>Y</td>
</tr>
<tr>
	<td>payAmount</td>
	<td>int</td>
    <td>10</td>
	<td>付费总额</td>
	<td>Y</td>
</tr>
<tr>
	<td>currencyName</td>
	<td>String</td>
    <td>64</td>
	<td>实际支付的国际标准货币代码,比如CNY(人民币)/USD(美元)</td>
	<td>Y</td>
</tr>
<tr>
	<td>roleId</td>
	<td>String</td>
    <td>32</td>
	<td>角色ID</td>
	<td>Y</td>
</tr>
<tr>
	<td>roleName</td>
	<td>String</td>
    <td>64</td>
	<td>角色名称</td>
	<td>Y</td>
</tr>
<tr>
	<td>roleLevel</td>
	<td>int</td>
    <td>32</td>
	<td>角色等级</td>
	<td>Y</td>
</tr>
<tr>
	<td>roleVipLevel</td>
	<td>String</td>
    <td>32</td>
	<td>角色vip等级</td>
	<td>Y</td>
</tr>
<tr>
	<td>serverId</td>
	<td>String</td>
    <td>32</td>
	<td>服ID</td>
	<td>Y</td>
</tr>
<tr>
	<td>zoneId</td>
	<td>String</td>
    <td>32</td>
	<td>区ID</td>
	<td>Y</td>
</tr>
<tr>
	<td>partyName</td>
	<td>String</td>
    <td>32</td>
	<td>帮会名称</td>
	<td>Y</td>
</tr>
<tr>
	<td>virtualCurrencyBalance</td>
	<td>String</td>
    <td> </td>
	<td>虚拟货币余额</td>
	<td>Y</td>
</tr>
<tr>
	<td>customInfo</td>
	<td>String</td>
    <td>2000</td>
	<td>扩展字段，订单支付成功后，透传给游戏</td>
	<td>Y</td>
</tr>
<tr>
	<td>gameTradeNo</td>
	<td>String</td>
    <td>64</td>
	<td>游戏订单ID，支付成功后，透传给游戏</td>
	<td>Y</td>
</tr>
<tr>
	<td>gameCallbackUrl</td>
	<td>String</td>
    <td>128</td>
	<td>支付回调地址，如果为空，则后台配置的回调地址</td>
	<td>N</td>
</tr>
<tr>
	<td>additionalParams</td>
	<td>String</td>
    <td> </td>
	<td>扩展参数</td>
	<td>N</td>
</tr>
</table>


#### 常用的国际货币
<table>
<tr>
	<th>国家</th>
	<th>货币中文名</th>
	<th>货币英文名</th>
	<th>货币代码</th>
</tr>
<tr>
	<td>中国</td>
	<td>人民币元</td>
	<td>RenminbiYuan</td>
	<td>CNY</td>
</tr>
<tr>
	<td>韩国</td>
	<td>韩圆</td>
	<td>Korean Won</td>
	<td>KRW</td>
</tr>
<tr>
	<td>日本</td>
	<td>日元</td>
	<td>Japanese Yen</td>
	<td>JPY</td>
</tr>
<tr>
	<td>美国</td>
	<td>美元</td>
	<td>U.S.Dollar</td>
	<td>USD</td>
</tr>
</table>

<a id="payCallback"></a>
### 5.2 支付回调
在调用 pay 接口时，需要实现 pay 支付 callback (支付的四种状态：支付成功，支付失败，支付取消,支付其他状态）

实现的案例代码

 	XGSDK.getInstance().pay(MainActivity.this, payment, new PayCallBack() {

			@Override
			public void onPaySuccess(PayInfo payInfo,PayResult payResult) {
				ToastUtil.showToastLong(MainActivity.this, "pay success, result is "+payResult.toJson());
				OrderUtils.storeOrder(MainActivity.this, GameInfo.getInstance().getUid(),payResult.getXgTradeNo(),payment);
			}

			@Override
			public void onPayFail(PayInfo payInfo,PayResult payResult) {
				ToastUtil.showToastLong(MainActivity.this, "pay failed, result is "+payResult.toJson());
			}

			@Override
			public void onPayCancel(PayInfo payInfo,PayResult payResult) {
				ToastUtil.showToastLong(MainActivity.this, "pay canceled, result is "+payResult.toJson());
			}

			@Override
			public void onPayOthers(PayInfo payInfo, PayResult payResult) {
				ToastUtil.showToastLong(MainActivity.this, "onPayOthers, result is "+payResult.toJson());
			}

			@Override
			public void onPayProgress(PayInfo payInfo, PayResult payResult) {
				ToastUtil.showToastLong(MainActivity.this, "onPayProgress, result is "+payResult.toJson());
			}
		});

<a id="statics"></a>
## 6.统计接口

<a id="onCreateRole"></a>
### 6.1 创建角色（必接）
接口定义：public void onCreateRole(RoleInfo roleInfo)  
接口说明：使用 roleInfo 来创建角色  

<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
<tr>
<td>roleInfo</td>
<td>角色信息</td>
</tr>
</table>

调用案例代码

	findViewById(RUtil.getId(getApplicationContext(), "xg_create_role"))
                .setOnClickListener(new OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        mRoleInfo.setRoleId("1112");
                        mRoleInfo.setRoleName("cuizi");
                        XGSDK.getInstance().onCreateRole(MainActivity.this,
                                mRoleInfo);
                    }
                });

关于 RoleInfo 的成员说明
<table>
<tr>
	<th>参数</th>
	<th>参数类型</th>
    <th>最大长度</th>
	<th>说明</th>
	<th>必须</th>
</tr>


<tr>
	<td>uid</td>
	<td>string</td>
	<td>128</td>
	<td>用户ID,游戏必须使用登录时西瓜服务器返回的uid</td>
    <td>Y</td>
</tr>

<tr>
	<td>serverId</td>
	<td>string</td>
	<td>32</td>
	<td>游戏服ID，示例：s1,s2</td>
    <td>Y</td>
</tr>
<tr>
	<td>serverName</td>
	<td>string</td>
	<td>64</td>
	<td>游戏服名称，示例：风云争霸</td>
    <td>Y</td>
</tr>

<tr>
	<td>zoneId</td>
	<td>string</td>
	<td>32</td>
	<td>游戏区ID,如果没有游戏区，则传入游戏服务id</td>
    <td>N</td>
</tr>
<tr>
	<td>zoneName</td>
	<td>string</td>
	<td>64</td>
	<td>游戏区名称，如果没有游戏区，则传入游戏服务名称</td>
    <td>N</td>
</tr>


<tr>
	<td>roleId</td>
	<td>string</td>
	<td>32</td>
	<td>角色ID</td>
    <td>Y</td>
</tr>

<tr>
	<td>roleName</td>
	<td>string</td>
	<td>64</td>
	<td>角色名</td>
    <td>Y</td>
</tr>

<tr>
	<td>roleType</td>
	<td>string</td>
	<td>20</td>
	<td>角色类型，如法师，道士，战士</td>
    <td>Y</td>
</tr>

<tr>
	<td>roleLevel</td>
	<td>int</td>
	<td>32</td>
	<td>角色等级</td>
    <td>Y</td>
</tr>

<tr>
	<td>roleVipLevel</td>
	<td>int</td>
	<td>32</td>
	<td>角色vip等级</td>
    <td>N</td>
</tr>

<tr>
	<td>partyName</td>
	<td>string</td>
	<td>32</td>
	<td>公会名</td>
    <td>N</td>
</tr>
<tr>
	<td>gender</td>
	<td>string</td>
    <td>枚举值：m,f;分别代表男女</td>
	<td>角色性别</td>
	<td>Y</td>
</tr>
</table>


<a id="onRoleLevelup"></a>
### 6.2 角色升级（必接）
接口定义：public void onRoleLevelup(RoleInfo roleInfo)

接口说明：角色等级接口，角色等级提升时调用
<table>
<tr>
  <td>参数</td>
  <td>说明</td>  
</tr>
<tr>
  <td>roleInfo</td>
  <td>角色信息</td>
</tr>
</table>


调用案例代码

	findViewById(RUtil.getId(getApplicationContext(), "xg_role_levelup"))
                .setOnClickListener(new OnClickListener() {

                    @Override
                    public void onClick(View v) {
                        mRoleInfo.setRoleId("1112");
                        mRoleInfo.setLevel(2);
                        XGSDK.getInstance().onRoleLevelup(mRoleInfo);
                    }
                });

<a id="onEnterGame"></a>
### 6.3 进入游戏（必接）
接口定义：public void onEnterGame(RoleInfo roleInfo)    
接口说明：进入游戏接口，进入游戏时调用

<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
<tr>
<td>RoleInfo</td>
<td>角色信息</td>
</tr>
</table>  

注：此接口在调用登录成功 callback 的时候调用
调用案例代码：

	@Override
    public void onLoginSuccess(final String authInfo) {
        Log.w(TAG, "authInfo: \n" + authInfo);
         XGSDK.getInstance().onEnterGame(mRoleInfo);
    }


<a id="onpayfinish"></a>
###6.4  支付完成
接口定义：void onPayFinish(PayInfo payInfo);
接口说明：统计接口在支付完成时上报支付的数据,需要传入类型为PayInfo的参数(改接口只在单独接入XG统计的时候需要接入)。
代码示例：
```cpp
    PayInfo payInfo;
	payInfo.uid = "123456";
    payInfo.productId = "payment017";
    payInfo.productName = "大宝剑";
    payInfo.productDesc = "倚天不出谁与争锋";
    payInfo.productUnit = "个";
    payInfo.productUnitPrice = 1;
    payInfo.productQuantity = 1;
    payInfo.totalAmount = 1;
    payInfo.payAmount = 1;
    payInfo.currencyName = "CNY";
    payInfo.roleId = "1234";
    payInfo.roleName = "yeye";
    payInfo.roleLevel = "65";
    payInfo.roleVipLevel = "8";
    payInfo.serverId = "11";
    payInfo.zoneId = "33";
    payInfo.partyName = "丐帮";
	payInfo.customInfo = "customInfo";
    payInfo.virtualCurrencyBalance = "0";
    payInfo.gameTradeNo = "12480";
    payInfo.gameCallbackUrl = "/sdkserver/receivePayResult";
    payInfo.additionalParams = "";
	mXgSdk->onPayFinish(payInfo);
```
具体限制参考5.1的PayInfo数据说明





<a id="extraInterface"></a>
## 7.扩展接口

<a id="switchAccount"></a>

### 7.1 切换账号

接口定义：public void switchAccount(Activity activity, String customParams)     
接口说明：切换账号，传入扩展参数 customParams  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>customParams</td>
		<td>该参数用于扩展，传输时使用 json 格式，接入时若不需要直接置空即可</td>
	</tr>
</table>


<a id="onEvent"></a>
### 7.2 自定义事件
接口定义：public void onEvent(RoleInfo roleInfo, String eventId, String eventDesc, int eventVal, Map<String, Object> eventBody)

接口说明：自定义事件，传入事件id以及事件内容  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
<tr>
	<td>roleInfo</td>
	<td>角色信息</td>
</tr>
<tr>
	<td>eventId</td>
	<td>事件id</td>
</tr>
<tr>
	<td>eventDesc</td>
	<td>事件描述</td>
</tr>
<tr>
	<td>eventVal</td>
	<td>事件内容</td>
</tr>
<tr>
	<td>eventBody</td>
	<td>key-value 事件体</td>
</tr>
</table>


<a id="onMissionBegin"></a>
### 7.3 任务开始
接口定义：public void onMissionBegin(RoleInfo roleInfo, String missionId, String missionName, String customParams)  

接口说明：任务开始接口  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>missionId</td>
		<td>任务ID</td>
	</tr>
	<tr>
		<td>missionName</td>
		<td>任务名称</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>该参数用于扩展，传输时使用json格式，接入时若不需要直接置空即可</td>
	</tr>
</table>

<a id="onMissionSuccess"></a>
### 7.4 任务成功
接口定义：public void onMissionSuccess(RoleInfo roleInfo, String missionId, String missionName, String customParams)

接口说明：任务执行成功接口  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>missionId</td>
		<td>任务ID</td>
	</tr>
	<tr>
		<td>missionName</td>
		<td>任务名称</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>该参数用于扩展，传输时使用json格式，接入时若不需要直接置空即可</td>
	</tr>
</table>

<a id="onMissionFail"></a>
### 7.5 任务失败

接口定义：public void onMissionFail(RoleInfo roleInfo, String missionId, String missionName, String customParams)

接口说明：任务失败  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>missionId</td>
		<td>任务ID</td>
	</tr>
	<tr>
		<td>missionName</td>
		<td>任务名称</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>该参数用于扩展，传输时使用json格式，接入时若不需要直接置空即可</td>
	</tr>
</table>

<a id="onVirtalCurrencyPurchase"></a>
### 7.6 购买虚拟货币
接口定义：public void onVirtalCurrencyPurchase(RoleInfo roleInfo, int amount, String customParams)

接口说明：充值获得的虚拟货币  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>amount</td>
		<td>数量</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>扩展参数</td>
	</tr>
  </table>

<a id="onVirtalCurrencyReward"></a>

### 7.7 赠送虚拟货币
接口定义：public void onVirtualCurrencyReward(RoleInfo roleInfo, String reason, int amount, String customParams)  

接口说明：玩家可以在任务奖励、登录奖励、成就奖励等环节获得赠送的虚拟货币  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>reason</td>
		<td>获得虚拟货币的原因(登录奖励、新手礼包等)</td>
	</tr>
	<tr>
		<td>amount</td>
		<td>数量</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>扩展参数</td>
	</tr>
</table>

<a id="onVirtualCurrencyConsume"></a>
### 7.8 消费虚拟货币

接口定义：public void onVirtalCurrencyConsume(RoleInfo roleInfo,String itemName, int amount, String customParams)  

接口说明：建议只跟踪有价值的虚拟货币，普通游戏币的消耗不建议在此跟踪  
<table>
<tr>
<td>参数</td>
<td>说明</td>
</tr>
	<tr>
		<td>roleInfo</td>
		<td>对象信息</td>
	</tr>
	<tr>
		<td>itemName</td>
		<td>消费点(比如十连抽、购买体力等)</td>
	</tr>
	<tr>
		<td>amount</td>
		<td>数量</td>
	</tr>
	<tr>
		<td>customParams</td>
		<td>扩展参数</td>
	</tr>
</table>

****

<a id="version"></a>
###文档版本说明
<table>
<tr>
<td>SDK版本</td><td>文档版本</td> <td>DK修改内容</td> <td>文档修改内容</td> <td>修改日期</td>  
</tr>
<tr>
<td>2.0.2</td><td>1.0</td> <td>初版</td> <td>初版</td> <td>2015.10.30</td>
</tr>
</table>