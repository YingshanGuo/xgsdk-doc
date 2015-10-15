# 高级选项

### 1. 使用自定义AndroidManifest参数

#### 1.1 使用场景
在AndroidManifest.xml文件中，可能我们针对每个渠道会有一些不同的配置项，例如JPUSH插件，每个渠道都有不同的APPKEY值。这时候为了使用同一个游戏母包进行打包操作，游戏开发者需要在母包的AndroidManifest.xml中使用一个通配符来标识这个配置项，然后通过Portal后台配置这个通配符在各个渠道的值，西瓜SDK就可以在打包阶段正确地打出每一个渠道包。

目前西瓜SDK支持10个AndroidManifest自定义参数。分别是：
- xg_param1
- xg_param2
- xg_param3
- xg_param4
- xg_param5
- xg_param6
- xg_param7
- xg_param8
- xg_param9
- xg_param10

这些KEY值目前是固定的。value值可以在Portal-高级配置中进行指定。

#### 1.2 操作
以配置JPUSH_APPKEY为例，JPUSH_APPKEY的值在每个渠道都是不一样的，我们这里使用xg_param1这个自定义参数值（当然你也可以选择其他的）：

1. 在母包的AndroidMinifest.xml文件内容：

		<meta-data android:name="JPUSH_APPKEY" android:value="$xg_param1$" />

2. 依次在【高级配置】 -> 【发布计划】 -> 【渠道】，进行参数值的配置。如下图所示，我们配置一个UC渠道的JPUSH_APPKEY的值。如有多个渠道请每个渠道分别配置对应的JPUSH_APPKEY的值。

	![AndroidManifest_custom_properties.png](AndroidManifest_custom_properties.png)

3. 使用西瓜提供的打包工具（含命令行工具）进行打包之后，最终生成的渠道包apk中的AndroidManifest.xml对应的配置将被替换成：

		<meta-data android:name="JPUSH_APPKEY" android:value="23ocd36n2dx345dsxx73d" />


#### 1.3 注意

1. 目前的KEY是固定的，支持10个自定义参数，可以自由选择使用哪一个。

2. 母包中如果使用了自定义参数，则必须在Portal中对应的发布计划和渠道必须配置对应的参数值，否则无法打包成功。

3. 如果同一个发布计划中有一部分渠道不需要自定义参数，比如计划【游戏内测发布计划】中有三个渠道，分别是：【360】、【UC】、【爱游戏】，其中【360】和【UC】两个渠道有自定义参数，【爱游戏】没有自定义参数，那么请使用带自定义参数和不带自定义参数的两个母包，并且在实际打渠道包的时候针对【360】和【UC】使用带参数母包，针对【爱游戏】使用不带参数的母包。

3. 自定义渠道参数的值不支持为空的替换。例如【爱游戏】渠道不需要配置**JPUSH_KEY**，则需要提供一个AndroidManifest.xml中不含有**JPUSH_KEY**的母包，而不能将【爱游戏】中的**JPUSH_KEY**的自定义参数值设置为""（空字符串）。


### 2. 使用自定义渠道资源

#### 2.1 使用场景
某些渠道需要在apk包中放置一些必要的资源文件，例如【爱游戏】需要将feeInfo.dat文件放置在assets目录下。

我们在Portal中的高级配置下可以指定渠道上传自定义游戏资源，该资源将在打渠道包的过程中，被拷贝到apk根目录下。

#### 2.2 操作
以【爱游戏】配置feeInfo.dat资源为例，该文件需要放置在assets目录下。

1. 在本地创建assets目录，将feeInfo.dat放置于assets目录中。

2. 将assets目录压缩成zip包，文件名可自由定义，例如叫**custom_res.zip**。此zip包的结果如下：

		custom_res.zip
            |_ assets
                |_ feeInfo.dat

3. 导航到【高级配置】 -> 【发布计划】 -> 【渠道】，点击上传按钮，选择刚刚生成的**custom_res.zip**包上传，如下图所示：

	![AndroidManifest_custom_resource.png](AndroidManifest_custom_resource.png)
