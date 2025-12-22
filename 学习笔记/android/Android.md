  

## Activity的生命周期

![](学习笔记/Attachments/1741677609179-78f6a284-493f-429b-bff6-20322cf18678.png)

### onCreate()

您`必须实现`此回调，它会在系统首次创建 Activity 时触发。Activity 会在创建后进入“已创建”状态。在`[onCreate()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onCreate\(android.os.Bundle\))`中 方法，执行基本的应用启动逻辑， 在 activity 的整个生命周期内仅发生一次。

您的活动不会保留在“已创建”的 状态。`onCreate()` 方法完成执行后，activity 会进入 _Started_ 状态，系统会调用 `[onStart()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onStart\(\))` 和 `[onResume()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onResume\(\))` 方法 连续调用。

### onStart()

当 Activity 进入“已启动”状态时，系统会调用 `[onStart()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onStart\(\))`。使Activity从不可见到可见

### onResume()

这是应用与用户互动的状态。应用会一直保持这种状态，直到 使焦点远离应用，例如接听来电的设备、用户 导航到其他 activity，或设备屏幕关闭。

当发生中断事件时，活动会进入_已暂停_状态 并且系统会调用 `[onPause()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onPause\(\))` 回调。

如果 Activity 返回到 从“onPause()”状态转换为“onResume()”状态，系统会再次调用 `onResume()` 方法结合使用。因此，请实施 `onResume()`，用于初始化您在发布期间释放的组件 `onPause()`，以及执行任何其他 每次活动进入“onResume”状态时都必须进行的初始化状态。

### onPause()

系统会将此方法作为用户离开的第一个 但这并不总是意味着 activity 会被销毁。 它表明 activity 不再位于前台，但它处于 在用户处于多窗口模式时仍然可见。 有几种原因可能会导致 activity 进入 此状态：

- 中断应用执行的事件，如“ [onResume()](https://developer.android.google.cn/guide/components/activities/activity-lifecycle?hl=zh-cn#onresume) 回调会暂停当前 Activity。这是最常见的情况。
- 在多窗口模式下，只有一个应用获得焦点 而系统会暂停所有其他应用。
- 打开新的半透明 activity（例如对话框） 会暂停其涵盖的活动。只要 该 activity 部分可见但未获得焦点， 保持暂停状态。

### onStop()

当您的 activity 不再对用户可见时，它会进入 _已停止_状态，系统将调用 `[onStop()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onStop\(\))` 回调。当新启动的 activity 覆盖整个屏幕时，可能会发生这种情况。

### onDestroy()

销毁 Ativity 之前，系统会先调用 [onDestroy()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#onDestroy\(\))。系统调用此回调的原因有两种：

1. 由于用户完全关闭了 activity，因此 activity 正在结束 或由于 [finish()](https://developer.android.google.cn/reference/android/app/Activity?hl=zh-cn#finish\(\))正在 调用该方法。
2. 由于配置，系统暂时销毁 activity 更改，例如设备旋转或进入多窗口模式。

## Activity之间的跳转和数据传递

### 命令行启动应用

```
adb shell am start com.xxx.x/.MainActivity
```

### 跳转

#### 1. 显式跳转

显式跳转是指明确指定目标Activity的类名进行跳转。

```
// 显式跳转 
// CurrentActivity.this：当前Activity的上下文。
// TargetActivity.class：目标Activity的类。
val intent = Intent(CurrentActivity.this, TargetActivity::class.java)
// 跳转
activity.startActivity(intent)
```

#### 2. 隐式跳转

隐式跳转是通过指定Action、Category等信息，让系统根据Intent的配置找到合适的Activity进行跳转。

```
Intent intent = new Intent();
intent.setAction("com.example.ACTION_START");
intent.addCategory("com.example.CATEGORY_DEFAULT");
startActivity(intent);
```

- `setAction`：设置Intent的Action。
- `addCategory`：添加Category（可选）。

### 通信

可以通过 `Intent` 在Activity之间传递数据。

```
// 传递数据
Intent intent = new Intent(CurrentActivity.this, TargetActivity.class);
intent.putExtra("key1", "value1");
intent.putExtra("key2", 123);
startActivity(intent);

// 在目标Activity中接收数据
Intent intent = getIntent();
String value1 = intent.getStringExtra("key1");
int value2 = intent.getIntExtra("key2", 0); // 0是默认值
```

- `putExtra`：用于传递数据，支持多种数据类型（如String、int、boolean等）。
- `getStringExtra`、`getIntExtra`：用于接收数据。

### 接受跳转类返回的结果

如果希望从目标Activity返回结果给源Activity，可以使用 `startActivityForResult`。

```
// 在当前Activity中启动目标Activity并等待结果
Intent intent = new Intent(CurrentActivity.this, TargetActivity.class);
startActivityForResult(intent, REQUEST_CODE); // REQUEST_CODE是请求码

// 在目标Activity中设置返回结果
Intent resultIntent = new Intent();
resultIntent.putExtra("result_key", "result_value");
setResult(RESULT_OK, resultIntent); // RESULT_OK是结果码
finish();

// 在当前Activity中接收返回结果
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_CODE && resultCode == RESULT_OK) {
        String result = data.getStringExtra("result_key");
        // 处理返回结果
    }
}
```

- `startActivityForResult`：启动目标Activity并等待结果。
- `setResult`：在目标Activity中设置返回结果。
- `onActivityResult`：在源Activity中接收返回结果。

#### 例子

##### 跳转浏览器并打开指定网页

```
val intent = Intent()
intent.setAction(Intent.ACTION_VIEW)
intent.data= Uri.parse("https://www.baidu.com")
activity.startActivity(intent)
```

### logcat

### 使用adb命令行过滤

```
adb logcat --pid=`adb shell pidof -s com.example.packagename`
或者使用包名直接过滤（Android 7.0+）：
adb logcat --pid=$(adb shell pidof com.example.packagename)
```

# Frida Hook vs 直接修改Smali：比较与分析

## 主要区别概述

|特性|Frida Hook|直接修改Smali|
|---|---|---|
|修改方式|运行时动态注入|静态修改|
|开发效率|高 - 即时生效，不需反复打包|低 - 需反复反编译/回编译/签名|
|灵活性|非常灵活，可动态调整|固定修改|
|技术要求|需JavaScript/Python知识|需Smali语法知识|
|对原包影响|无需修改原包|需修改原包|
|检测风险|容易被运行时检测|修改后较难检测|
|适用范围|适合快速分析、动态调试|适合永久性修改|

## Frida Hook的优势

1. **动态性**：

```
// 示例：Hook某个方法的调用
Java.perform(function() {
  var TargetClass = Java.use("com.example.Target");
  TargetClass.targetMethod.implementation = function() {
    console.log("方法被调用了!");
    return this.targetMethod();
  };
});
```

- 可以在运行时动态修改应用行为
- 修改立即生效，无需重启应用

2. **开发效率高**：

- 无需反编译/回编译过程
- 脚本修改后立即重新注入即可测试

3. **灵活性**：

- 可以针对不同场景编写不同脚本
- 条件性Hook，根据运行时状态决定行为

## 直接修改Smali的优势

1. **持久性**：

- 修改永久生效，不需每次运行时注入
- 适合最终发布版本的修改

2. **隐蔽性**：

- 不会被运行时检测工具发现
- 修改后应用行为与原应用无异

3. **性能**：

- 无运行时Hook开销
- 执行效率与原代码一致

## 典型使用场景对比

**适合Frida的场景**：

- 快速逆向分析与调试
- 动态测试不同修改方案
- 需要频繁调整的测试阶段
- 无法获取或修改APK的情况(如系统应用)

**适合修改Smali的场景**：

- 需要永久性修改应用功能
- 发布破解/修改版应用
- Frida难以实现的复杂结构性修改
- 需要考虑反检测的场景

## 技术细节对比

### Frida Hook技术特点

1. 基于ptrace实现进程注入
2. 提供完整的Java运行时访问能力
3. 支持多种语言编写脚本(JS/Python等)
4. 可以Hook native(C/C++)和Java层

### Smali修改技术特点

1. 需要理解Dalvik字节码
2. 需要处理回编译问题
3. 可能需要修复签名校验
4. 可以修改任何可反编译的代码部分

## 选择建议

对于**逆向分析、调试和快速原型**，优先考虑Frida Hook；对于**发布修改版应用或长期修改**，则应该选择直接修改Smali。在实际工作中，两者经常结合使用 - 用Frida快速定位需要修改的点，然后用Smali实施最终修改。

# 反编译

## 工具

## frida

### 安装

### 电脑端

```
-- 电脑端需要使用python3 安装frida-tools工具包
pip install frida-tools
```

  

查看devices的架构

```
// x86_64,arm64-v8a,x86,armeabi-v7a,armeabi
adb shell getprop ro.product.cpu.abi
```

根据设备的架构下载指定版本的[frida-server](https://github.com/frida/frida/releases),注意tools的版本和server版本要一致

下载后安装到设备端

1. android

下载文件解压并移动到设备目录`**/data/local/tmp/**`下

```
unxz frida-server.xz
adb root
adb push frida-server /data/local/tmp/
adb shell "chmod 755 /data/local/tmp/frida-server"
adb shell "/data/local/tmp/frida-server &"
```

### 查看设备的进程

```
frida-ps -U  #运行中的进程
frida-ps -Ua #use设备上运行的应用app
frida-ps -Uai # use设备上安装的应用
```

### apktool

[apktool](http://ibotpeaches.github.io/Apktool/install/)

Windows安装

1. 下载适用于windows的[脚本](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/windows/apktool.bat)
2. 下载最新版本的[apktool](https://bitbucket.org/iBotPeaches/apktool/downloads)
3. 解压到自定义目录
4. 将目录添加到环境变量然后保存
5. 打开终端执行`apktool.bat -version`,正确输出版本号即安装完成
6. **注意文件和文件夹的名称使用英文**

使用

```
// 反编译 test.apk
apktool.bat ./test.apk
// 对修改后的文件进行编译
apktool.bat b -o ./output.apk ./test
```

## 使用android studio打开反编译

### 安卓游戏逆向修改自动化流程精要

#### 一、项目初始化

1. 新建空白Android项目，包名/版本号/API等参数需与原APK完全一致
2. 使用Apktool反编译后：

- 替换`assets`、`res`、`AndroidManifest.xml`
- 将`libs`重命名为`jniLibs`存放于`app/src/libs/`
- 移动`public.xml`至`app/src/libs/`

#### 二、核心配置

1. 转换`classes.dex`为`classes.jar`并放入`app/src/libs/`
2. 修改`build.gradle`：

```
android {
    sourceSets {
        main {
            jniLibs.srcDirs = ['src/libs/jniLibs']
        }
    }
    aaptOptions {
        additionalParameters "--stable-ids", "$buildDir/generated/ids.txt"
        additionalParameters "--emit-ids", "$buildDir/generated/ids.txt"
    }
}
dependencies {
    implementation files('src/libs/classes.jar')
}
```

3. 添加自动生成ID的Gradle task（代码见原文）

#### 三、代码修改方案

**Java层修改：**

1. 继承重写：新建类继承目标类，修改后需更新Manifest
2. 完全重写：删除`classes.jar`中的类，利用AS智能补全重新实现

**Native层修改：**

- 使用MsHookFramework实现：

- 函数Hook：替换函数头跳转到自定义实现
- 内存修改：通过GamePatch修改指定地址HEX值

#### 四、优势说明

- 开发体验接近正向开发（AS智能补全支持）
- 自动化构建流程替代手动解包/安装
- 资源ID稳定性通过`public.xml`保持

提示：配合Jadx等逆向工具可提升重写效率

## adb(Android debug bridge)

### 简介

这是计算机技术中最常提到的 ADB，是 Google 为 Android 系统提供的一种命令行工具。 
- **功能**：它充当了电脑与 Android 设备（手机、平板或模拟器）之间的“桥梁”，允许用户通过电脑对移动设备进行调试和控制。
- **主要用途**：
    - **安装/卸载应用**：通过命令安装本地 APK 文件。
    - **文件传输**：在电脑和手机之间复制文件（Pull/Push）。
    - **运行 Shell 命令**：直接进入手机的 Unix shell 环境执行指令。
    - **系统调试**：查看设备日志（logcat）、重启进入 Recovery 模式或刷机。
- **结构**：由三个部分组成：运行在电脑上的**客户端**、在后台运行的**服务器**，以及在手机端运行的**守护进程 (adbd)**。
### 常用命令

```bash
1. adb install name.apk
2. adb devices
3. adb push 00.txt /stcard
4. adb pull 00.txt /stcard
5. adb shell pm list packages 查看手机全部软件的包名
6. adb shell dumpsys battery 电池信息
7. adb shell ps进程
8. adb shell top CPU
9. adb connect 127.0.0.1:58526连接wifi调试
10. adb shell pm list packages -3第三方应用
```
