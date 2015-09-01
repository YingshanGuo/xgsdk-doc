# 签名文件签名规则格式

### 1.签名文件格式
签名文件必须是以.zip为后缀的压缩文件。

### 2.签名文件内容
压缩文件中必须包含两个文件，即：.properties文件和 .keystore文件。  
-  .properties文件中必须包含三个属性：alias、storepass和keypass，如图所示。  

  <img src="img/signature.png"/>

-  .keystore是通过.properties文件中的信息生成的签名文件。

### 3.签名文件大小
签名文件的大小不能超过10K。
