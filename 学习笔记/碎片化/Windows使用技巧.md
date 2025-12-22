# 1. win脚本执行策略

```
PS D:\code\wrwwww.github.io> npm install -g pnpm

added 1 package in 11s

1 package is looking for funding
  run `npm fund` for details
PS D:\code\wrwwww.github.io> pnpm -v
pnpm : 无法加载文件 C:\Users\admin\AppData\Roaming\npm\pnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http
s:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ pnpm -v
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS D:\code\wrwwww.github.io> pnpm
pnpm : 无法加载文件 C:\Users\admin\AppData\Roaming\npm\pnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http
s:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ pnpm
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

  

原因是： 首次在计算机上启动 Windows PowerShell 时，现用执行策略很可能是 Restricted（默认设置）。Restricted 策略不允许任何脚本运行。  
解决办法:powershell使用管理员执行

  

```
set-ExecutionPolicy RemoteSigned
```

# 2. win输入法使用小鹤双拼

微软默认输入法是没有小鹤双拼的，可以通过修改注册表启用

## 2.1. 注册表

1. 打开注册表
2. 搜索注册表 `\HKEY_CURRENT_USER\Software\Microsoft\InputMethod\Settings\CHS`
3. 新建字符串值，`UserDefinedDoublePinyinScheme0`值`小鹤双拼*2*^*iuvdjhcwfg^xmlnpbksqszxkrltvyovt`
4. 进入输入法,启用方案

## 2.2. 批处理命令

通过批处理命令启用小鹤双拼

```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\SOFTWARE\Microsoft\InputMethod\Settings\CHS]
"Enable Double Pinyin"=dword:00000001
"DoublePinyinScheme"=dword:0000000a
"UserDefinedDoublePinyinScheme0"="小鹤双拼*2*^*iuvdjhcwfg^xmlnpbksqszxkrltvyovt"
```

  

关闭小鹤双拼

  

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\InputMethod\Settings\CHS]
"Enable Double Pinyin"=dword:00000000
"DoublePinyinScheme"=dword:00000000
"UserDefinedDoublePinyinScheme0"=-
```

# 3. powershell

## 3.1. 永久添加别名

```
notepad $profile.AllUsersAllHosts
```

在文件中添加文本

```
set-alias -name pn -value pnpm
将pnmp 简写为 pn
```

# 4. wsl

  

## 4.1. 启用linux特性

  

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

  

## 4.2. 启用虚拟化

  

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

  

## 4.3. 设置wsl版本

  

```
wsl --set-default-version 2
```

  

## 4.4. 安装linux

  

```
wsl --install kali-linux
```

  

## 4.5. 从c盘迁移linux

  

```
wsl --export kali-linux D:\software\kali.tar
```

  

## 4.6. 注销linux

  

```
wsl --unregister kali-linux
```

  

## 4.7. 导入linux

  

```
wsl --import kali-linux D:\software\kali\ D:\software\kali.tar --version 2
```

# 5. 添加桌面右键菜单

`计算机\HKEY_CLASSES_ROOT\*\shell`桌面右键

![](lake%20import/学习笔记/Attachments/1656895051592-c2c0cf34-f14c-44c2-800e-28bc1b513f99.png)

添加图标

![](lake%20import/学习笔记/Attachments/1656895174839-ddc5d5db-5de0-44af-8e2c-8b8ed813c3c6.png)

![](lake%20import/学习笔记/Attachments/1656895656088-c994bf4f-4116-47ba-a265-4d5cf7d8e4fc.png)

在子项command项中添加shell命令

![](lake%20import/学习笔记/Attachments/1656895719480-b2abf4d1-e07c-4966-9238-501174690d3c.png)
## 设置vim

jk键映射到esc

```
{
    "files.autoSave": "onFocusChange",
    "git.openRepositoryInParentFolders": "never",
    "vim.useSystemClipboard": true,
    "[rust]": {
        "editor.defaultFormatter": "rust-lang.rust-analyzer"
    },
    "vim.commandLineModeKeyBindings": [

    ],
    "vim.insertModeKeyBindings": [
             {
            "before":["j","j"],
            "after":["<Esc>"]
        },
    ]
}
```