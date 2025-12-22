## 安装

### nvm版本管理

下载链接

相关命令

```
# node 本地版本列表
nvm list
# node 安装指定版本
nvm install 18
# 使用指定版本
nvm use 18
```

### nvm换源

[淘宝npm镜像站](https://www.npmmirror.com/?spm=a2c6h.14029880.d-5123.1.83fd3839riUZpU)

1. 命令行切换镜像源

```
# 淘宝源
npm config set registry https://registry.npmmirror.com
```

2. .npmrc文件中设置镜像源[**.npmrc**](https://pnpm.io/zh/npmrc#registry)

```
https://pnpm.io/zh/npmrc#registry
```

### pnpm安装

```
npm install -g pnpm
```

## 导入导出

现在支持es6语法,只需要在配置中设置

```
{
type:'module'
}
```

### 引入方式

```
// 默认导入,必须指定名字,名字可以随便取
// commonjs的模块引入方式
const express = require('express')
// es6的引入方式
import express from 'express'
// 加{}的导入
import {default as express} from 'express'

// 命名导入
// 方法一 别名导入
import {name as tom} from 'express';
// 方法二
import {name} from 'express'
// 方法三 全部导入 需要一个集合名 如这里的items,使用时候items.属性名来使用
import * as items from 'express'
// 方法四 同时导入默认导出和命名导出
import tom ,{ name} from 'express'
import { tom as default ,name} from 'express'
import tom , * as items from 'express'
```

对于这种不写具体路径的引入都是从node_modules当中

### 导出方式

```
// 默认导出
// 一个模块只能有一个default
// 方法一
export default 'Tom';
// 方法二
const name = 'Tom';
export default name;
// 方法三
const name = 'Tom';
export {name as default}; // 大括号必不可少

// 命名导出
// 方法一
export const name='tom';
// 方法二
const name ='tom'
export {name}//哪怕只有一个值都需要{}
// 使用别名导出
export {name as tom}
```

##   
nvm

### nvm找不到setting文件

在安装其他软件时，不小心将nvm的环境变量给删除了

在恢复了系统path中的 C:\Users\user\AppData\Roaming\nvm（nvm的安装路径） 后，使用nvm时报错

ERROR open \settings.txt: The system cannot find the file specified

对比了我与其他人的环境变量后，发现我少了%NVM_HOME%，%NVM_SYMLINK%两个变量

在path中添上两个路径

![](学习笔记/Attachments/1721899334535-45955407-dabf-48b5-b67a-876aa67afab9.png)

在系统变量中添上两个变量，变量值可以在nvm安装文件中的 settings.txt 中查看，

root：nvm_home

path: nvm_symlink

![](学习笔记/Attachments/1721899343032-0294e5c7-57b8-493d-b20e-a44e9f4d0534.png)

重启电脑，用administrator模式打开cmd，键入nvm，OK

```
@echo off
setx NVM_HOME "C:\nvm" /M
setx NVM_SYMLINK "C:\Program Files\nodejs" /M
setx Path "%NVM_HOME%;%NVM_SYMLINK%;%Path%" /M
echo 环境变量已更新，请重启终端。
```

参考链接：[https://github.com/coreybutler/nvm-windows/issues/39](https://github.com/coreybutler/nvm-windows/issues/39)

1. npm 设置国内镜像源npm 官方提供了多个国内镜像源，最常用的是淘宝的 npm 镜像。可以通过以下命令设置：使用淘宝镜像npm config set registry [https://registry.npmmirror.com/查看当前镜像源在更改之前，你可以查看当前的镜像源：npm](https://registry.npmmirror.com/查看当前镜像源在更改之前，你可以查看当前的镜像源：npm) config get registry恢复到官方源如果需要恢复到默认的 npm 官方源，可以使用以下命令：npm config set registry [https://registry.npmjs.org/](https://registry.npmjs.org/)
2. yarn 设置国内镜像源对于 yarn，可以通过命令行设置镜像源，通常也是使用淘宝镜像：使用淘宝镜像yarn config set registry [https://registry.npmmirror.com/查看当前镜像源你可以查看当前的镜像源配置：yarn](https://registry.npmmirror.com/查看当前镜像源你可以查看当前的镜像源配置：yarn) config get registry恢复到官方源若要恢复到 yarm 的默认源，可以使用：yarn config delete registry
3. pnpm 设置国内镜像源pnpm 也支持设置镜像源，同样使用淘宝镜像进行加速：使用淘宝镜像pnpm set config registry [https://registry.npmmirror.com/查看当前镜像源若想查看当前的镜像源，可以执行：pnpm](https://registry.npmmirror.com/查看当前镜像源若想查看当前的镜像源，可以执行：pnpm) get config registry恢复到官方源若需恢复到默认的官方源，可以运行：pnpm set config registry [https://registry.npmjs.org/](https://registry.npmjs.org/)非常好 👍，你说的 “npmsource 仓库安装 Node.js” 实际上指的是通过 **NodeSource 官方维护的仓库** 来在 Linux 系统上安装 Node.js。  
    这是目前在 CentOS / AlmaLinux / RHEL 等系统上 **最标准、最推荐的方式**，比系统自带版本更新且稳定。

下面我一步步教你在 **AlmaLinux 10 / CentOS / RHEL 系列** 上正确安装。

---

## 🧩 一、NodeSource 是什么？

**NodeSource** 是 Node.js 官方合作的一个第三方提供方，  
它维护了各个 Linux 发行版的 **最新稳定 Node.js 仓库源（yum/dnf）**。

使用它的好处：  
✅ 安装方便，一行命令搞定✅ 提供最新 LTS（长期支持）版本✅ 自带 npm✅ 自动配置 PATH、依赖等

---

## 🧠 二、安装步骤（适用于 AlmaLinux 8 / 9 / 10）

### 1️⃣ 更新系统包

```
sudo dnf update -y
```

---

### 2️⃣ 添加 NodeSource 仓库

NodeSource 提供的安装脚本可以一键配置好仓库：

#### 👉 以 Node.js 20（当前 LTS）为例：

```
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
```

这条命令会自动：

- 添加 NodeSource 仓库；
- 导入 GPG 密钥；
- 配置 dnf 源文件；
- 检测系统版本兼容性。

---

### 3️⃣ 安装 Node.js

```
sudo dnf install -y nodejs
```

安装完成后验证：

```
node -v
npm -v
```

输出类似：

```
v20.15.1
10.7.0
```

表示安装成功 ✅。

---

### 4️⃣（可选）安装开发工具（用于构建原生模块）

如果你要用 npm 安装带有 C/C++ 编译的依赖（如 `bcrypt`、`canvas`），执行：

```
sudo dnf install -y gcc-c++ make
```

---

## 🧰 三、可选版本（如果你想装不同版本）

|Node.js 版本|对应命令||
|---|---|---|
|22.x（最新 Current）|`curl -fsSL [https://rpm.nodesource.com/setup_22.x](https://rpm.nodesource.com/setup_22.x)|sudo bash -`|
|20.x（LTS）|`curl -fsSL [https://rpm.nodesource.com/setup_20.x](https://rpm.nodesource.com/setup_20.x)|sudo bash -`|
|18.x（旧 LTS）|`curl -fsSL [https://rpm.nodesource.com/setup_18.x](https://rpm.nodesource.com/setup_18.x)|sudo bash -`|

---

## ⚙️ 四、卸载旧版本（如系统自带 node）

有的系统自带一个旧的 node（比如 v10 或 v12），你可以先删掉：

```
sudo dnf remove -y nodejs
```

再重新安装 NodeSource 版本。

---

## 🧭 五、验证仓库来源

可以查看 NodeSource 仓库文件：

```
cat /etc/yum.repos.d/nodesource*.repo
```

你会看到类似：

```
[nodesource]
name=Node.js Packages for Enterprise Linux 10 - $basearch
baseurl=https://rpm.nodesource.com/pub_20.x/el/10/$basearch
enabled=1
gpgcheck=1
gpgkey=https://rpm.nodesource.com/pub/el/NODESOURCE-GPG-SIGNING-KEY-EL
```

---

## ✅ 六、总结命令清单（AlmaLinux 10）

```
# 1. 更新系统
sudo dnf update -y

# 2. 添加 NodeSource 仓库（以 Node.js 20 为例）
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -

# 3. 安装 Node.js
sudo dnf install -y nodejs

# 4. 验证版本
node -v
npm -v
```

---

要不要我帮你写一份针对 **AlmaLinux 10** 的完整一键安装脚本（包含 Node.js 20 + 构建依赖 + 全局 npm 配置）？  
你执行一条命令就能搞定全部。