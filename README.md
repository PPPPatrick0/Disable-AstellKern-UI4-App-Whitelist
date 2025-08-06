# Disable-AstellKern-UI4-App-Whitelist

**This guide will show you how to disable the APK whitelist restriction on Astell&Kern's UI4 models.**
**本指南将告诉你如何禁用AstellKern的UI4机型的APK白名单限制。**

[Before you begin, please ensure you have already obtained root access via APatch by following this guide.](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide)  
[在开始之前，请确保你已经按照此指南获得了Apatch与Root权限](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide)

## [Chinese Version](https://github.com/PPPPatrick0/Disable-AstellKern-UI4-App-Whitelist/tree/main?tab=readme-ov-file#%E4%B8%AD%E6%96%87%E7%89%88chinese-version)
## [English Version](https://github.com/PPPPatrick0/Disable-AstellKern-UI4-App-Whitelist/blob/main/README.md#english-version-1)

## 中文版（Chinese Version）

### 本指南将分为两部分
* 第一部分，为已获得支持的机型提供傻瓜式安装指南。（截至V0.1，获得支持的有SP3000(V1.51)）
* 第二部分，为尚未获得支持的机型提供原理说明，使自己尝试禁用限制成为可能。

### 第一部分
#### 1 通过Apatch安装模块  
从Release中下载 Modules.zip ，并将文件传送至您的设备。  
在Apatch中进行安装  
Overlayfs RW安装时  
**Hide root选root**  
**Mount mode选Mount Overlay FS**  
随后重启

#### 2 安装service.jar
从Release中下载Services_patched.jar  
移动至设备的内部存储的根目录  
*执行如下命令，赋予system目录读写权限：
```
adb shell
su
/data/overlayfs/tmp/overlayrw -rw /system
```
显示Mount RW: /system done，证明权限赋予成功。  
*接着执行如下命令，写入修改过的services.jar：
```
cp /sdcard/services.jar /system/framework 
```
*然后删除原有jar包的预编译文件：
```
rm /system/framework/oat/arm64/services.art
rm /system/framework/oat/arm64/services.odex
rm /system/framework/oat/arm64/services.vdex
```

#### 3 重启
至此你已成功解除白名单限制

### 第二部分
Oops:404 Not Found  
请等待完善


## English Version


感谢列表
Overlayfs RW
