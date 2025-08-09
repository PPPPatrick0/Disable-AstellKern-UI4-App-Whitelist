# Disable-AstellKern-UI4-App-Whitelist

**This guide will show you how to disable the APK whitelist restriction on Astell&Kern's UI4 models.**  
**本指南将告诉你如何禁用AstellKern的UI4机型的APK白名单限制。**

[Before you begin, please ensure you have already obtained root access via APatch by following this guide.](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide)  
[在开始之前，请确保你已经按照此指南获得了Apatch与Root权限](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide)

## [Chinese Version](https://github.com/PPPPatrick0/Disable-AstellKern-UI4-App-Whitelist/tree/main?tab=readme-ov-file#%E4%B8%AD%E6%96%87%E7%89%88chinese-version)
## [English Version](https://github.com/PPPPatrick0/Disable-AstellKern-UI4-App-Whitelist/blob/main/README.md#english-version-1)

## 中文版（Chinese Version）

### 本指南将分为两部分
* 第一部分，为已获得支持的机型提供傻瓜式安装指南。  
（V0.1，获得支持的有SP3000系列全部机型(SP3000 SP3000T SP3000M)）
（V0.2，SR35得到支持）
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
# 将你的设备所对应的Services_patched.jar替换为实际名称
cp /sdcard/"Services_patched.jar" /system/framework/services.jar
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
<del>
**Oops:404 Not Found**  
请等待完善
</del>

我将告诉你如何禁用白名单限制，以及它的原理

#### 1 阶段一：分析
* 线索: 在尝试进行安装白名单以外的APK时，Logcat日志明确指出，执行安装验证的进程是system_server，并且AkPackageManager.apk的源码显示，它调用了系统底层的PackageManager.getAvailableOpenAppPackages()方法来获取白名单。
* 假设: 白名单的真正逻辑，被硬编码在安卓的核心框架中，而不是在一个简单的系统App里。
* 行动:  
从设备中提取出核心框架文件：framework.jar和services.jar。  
对两者进行反编译和搜索。  
* 突破:  
在framework.jar中没有找到相关逻辑。  
在services.jar中，成功定位到了com.android.server.pm.PackageInstallerSession这个类。  
在该类的validateApkInstallLocked方法中，我们找到了与日志行为完全一致的、最终的白名单验证代码块。

#### 2 阶段二：修改
我们发现，破解的关键在于直接绕过validateApkInstallLocked方法中的if判断语句。
* 方案 (Smali修改):  
在validateApkInstallLocked方法的Smali代码中，精确定位到与if (!supportPackage)相对应的条件跳转指令 (if-eqz v0, :cond_XYZ)。  
我们的方案是，通过注释掉 (#) 这条跳转指令，来“剪断”那条通往“抛出异常、安装失败”的逻辑通路，使得无论检查结果如何，程序都会继续执行正常的安装流程。  

#### 3 阶段三：复原
重新打包并覆盖services.jar  
删除oat里的缓存serveices缓存  







## English Version

### This guide is divided into two parts:
* Part 1: A user-friendly installation guide for supported models.  
(As of v0.1, this includes the SP3000 series (SP3000 , SP3000T , SP3000M)).
(V0.2 Add SR35 support)
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
# Replace "Services_patched.jar" with the actual filename for your device
cp /sdcard/"Services_patched.jar" /system/framework/services.jar
```
* Remove the pre-compiled cache files for the original jar:  
```
rm /system/framework/oat/arm64/services.art
rm /system/framework/oat/arm64/services.odex
rm /system/framework/oat/arm64/services.vdex
```

#### 3. Enjoy It!
  
  
### Part 2: Technical Deep-Dive & Guide for Unsupported Models
<del>
**Oops: 404 Not Found**  
Please wait for this section to be completed.
</del>

I will show you how to disable the whitelist restriction, and explain the principles behind how it works.

#### Phase 1: Analysis
* Clues: When attempting to install a non-whitelisted APK.Logcat analysis clearly indicated that the installation validation process was handled by system_server. Furthermore, the source code of AkPackageManager.apk revealed that it retrieves the whitelist by calling a low-level system method: PackageManager.getAvailableOpenAppPackages().
* Hypothesis: The true whitelist logic is hardcoded within the core Android framework, not inside a simple system application.
* Action:  
Extracted the core framework files from the device: framework.jar and services.jar.  
Decompiled and searched through both files.  
* Breakthrough:  
No relevant logic was found in framework.jar.  
Successfully located the com.android.server.pm.PackageInstallerSession class within services.jar.  
Inside the validateApkInstallLocked method of this class, we found the definitive whitelist validation code block that perfectly matched the behavior seen in the logs.

#### Phase 2: Modification
We discovered that the key to the solution was to directly bypass the final if conditional check within the validateApkInstallLocked method.
* The Plan (Smali Modification):  
Pinpoint the conditional jump instruction in the Smali code of the validateApkInstallLocked method that corresponds to the if (!supportPackage) check (e.g., if-eqz v0, :cond_XYZ).  
Our strategy was to comment out (#) this jump instruction. This effectively severs the logical path that leads to throwing an exception and failing the installation, forcing the program to proceed with the normal installation flow regardless of the check's outcome.

#### Phase 3: Reassembly & Deployment
Repackage and Overwrite: Re-packaged the modified Smali code back into services.jar and used root access to overwrite the original file in /system/framework/.  
Clear Cache: Deleted the oat cache files (.art, .odex, .vdex) for services.jar to force the system to regenerate them based on our modified version upon reboot.


## Shout-Outs!
This success wouldn't have been possible without these legends:
* The APatch Team: Your kernel-patching approach was the skeleton key that bypassed this device's final defenses. Absolute genius!
* The Overlayfs RW module: For providing a straightforward way for users to gain read-write access to the /system partition.  
* The AKBox dev [@7dollars](https://github.com/7dollars) and community: Thanks for all the encouragement and support. You were the motivation that kept me going!
* [@rlw6534](https://github.com/rlw6534):	verified for SR35
* Google Gemini: My AI partner, who provided tireless technical support and brainstorming sessions. AI-assisted reverse engineering is incredibly cool.
