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
**Oops:404 Not Found**  
请等待完善


## English Version

### This guide is divided into two parts:
* Part 1: A user-friendly installation guide for supported models. (As of v0.1, this includes the SP3000 on firmware v1.51).
* Part 2: A technical explanation of the principles involved, enabling users to attempt disabling the restriction on unsupported models themselves.

### Part 1: Step-by-Step Guide for Supported Models
#### 1. Install the Module via APatch
From the Releases section of this repository, download the Modules.zip file.  
Transfer the extracted .zip file(s) to your device's internal storage.  
Install the module in the APatch app.  
During the Overlayfs RW module installation, ensure you select the following options:  
**For "Hide root", choose: root**  
**For "Mount mode", choose: Mount Overlay FS**  
Reboot your device after the installation is complete.  

#### 2. Install the modified services.jar
From the Releases section, download the Services_patched.jar file.  
Transfer this file to the root directory (/sdcard) of your device's internal storage.  
* Make the /system partition writable:
```
adb shell
su
/data/overlayfs/tmp/overlayrw -rw /system
```
If you see the output Mount RW: /system done, it means the permission has been successfully granted.
* Copy the modified services.jar into the system:  
Next, execute the following command to replace the original services.jar:
```
cp /sdcard/services.jar /system/framework/
```
* Remove the pre-compiled cache files for the original jar:  
```
rm /system/framework/oat/arm64/services.art
rm /system/framework/oat/arm64/services.odex
rm /system/framework/oat/arm64/services.vdex
```

#### 3. Enjoy It!
  
  
### Part 2: Technical Deep-Dive & Guide for Unsupported Models
**Oops: 404 Not Found**  
Please wait for this section to be completed.


## Shout-Outs!
This success wouldn't have been possible without these legends:
* The APatch Team: Your kernel-patching approach was the skeleton key that bypassed this device's final defenses. Absolute genius!
* The Overlayfs RW module: For providing a straightforward way for users to gain read-write access to the /system partition.  
* The AKBox dev [@7dollars](https://github.com/7dollars) and community: Thanks for all the encouragement and support. You were the motivation that kept me going!  
* Google Gemini: My AI partner, who provided tireless technical support and brainstorming sessions. AI-assisted reverse engineering is incredibly cool.
