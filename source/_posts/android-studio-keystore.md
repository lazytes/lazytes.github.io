---
title: Android Studio 保存打包签名配置
---

### 1. 新建配置文件
在project的根目录下新建`keystore.properties`文件,添加内容:
```
storeFile=签名文件存储的位置
storePassword=XXXXX
keyAlias=XXXXX
keyPassword=XXXXX
```
### 2. 配置build.gradle
添加配置:
```
def keystorePropertiesFile = rootProject.file("keystore.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
   ...
   signingConfigs {
       release {
          storeFile file(keystoreProperties['storeFile'])
          storePassword keystoreProperties['storePassword']
          keyAlias keystoreProperties['keyAlias']
          keyPassword keystoreProperties['keyPassword']
       }
   }
}
```
### 3. 配置VCS
在VCS中将`keystore.properties`添加到忽略文件。




