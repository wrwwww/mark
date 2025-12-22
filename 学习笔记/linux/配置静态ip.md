## 背景变化（RHEL 8 → 9 → 10）

| 版本                            | 默认网络配置方式      | 配置文件位置                                                  |
| ----------------------------- | ------------- | ------------------------------------------------------- |
| CentOS 7 / RHEL 7             | ifcfg 文件      | `/etc/sysconfig/network-scripts/ifcfg-*`                |
| CentOS 8 / RHEL 8             | 仍支持 ifcfg（过渡） | `/etc/sysconfig/network-scripts/ifcfg-*`                |
| AlmaLinux 9 / 10（RHEL 9 / 10） | 新格式（keyfile）  | `/etc/NetworkManager/system-connections/*.nmconnection` |
在 **AlmaLinux 9/10（包括 RHEL 9/10）** 里，那个经典的文件路径：
```
/etc/sysconfig/network-scripts/ifcfg-enp0s3
```
已经 **彻底被淘汰** 了。

**AlmaLinux 10 用的是 NetworkManager + keyfile 格式配置。**
也就是说：
- **network-scripts 目录和 ifcfg 文件** 已经彻底弃用；
- **NetworkManager** 是唯一官方支持的方式；
- 你应该通过 `nmcli` 或 `nmtui` 来配置静态 IP。
---
## 查看现有连接

执行：
```
nmcli connection show
```

```
NAME                UUID                                  TYPE      DEVICE
Wired connection 1  e3c7c0b5-15f1-4e5d-9a2a-01b2f367f201  ethernet  enp0s3
```

这代表 NetworkManager 正在管理 `enp0s3`。

---

## 设置静态 IP（推荐两种方法）

### 使用nmtui（图形界面）

```
sudo nmtui
```

然后选择：

1. **Edit a connection**
2. 选中你的网卡，比如 `Wired connection 1`
3. 在 **IPv4 Configuration** → 改为 `Manual`
4. 填写：
```
Address: 192.168.117.129/24
Gateway: 192.168.117.1
DNS: 8.8.8.8,114.114.114.114
```
5. 确认 → 回车保存 → 返回主界面
6. 选择 **Activate a connection** → 重启该连接。
验证：

```
ip a
ping 8.8.8.8
```
---
### 使用命令行 `nmcli`

假设你的连接名是 `Wired connection 1`：

```
nmcli connection modify "Wired connection 1" ipv4.addresses 192.168.117.129/24
nmcli connection modify "Wired connection 1" ipv4.gateway 192.168.117.1
nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 114.114.114.114"
nmcli connection modify "Wired connection 1" ipv4.method manual
nmcli connection up "Wired connection 1"
```

验证：

```
nmcli device show enp0s3
```

---

## 文件保存路径（新格式）

配置保存的位置变成：

```
/etc/NetworkManager/system-connections/Wired connection 1.nmconnection
```

你可以查看：

```
sudo cat "/etc/NetworkManager/system-connections/Wired connection 1.nmconnection"
```

内容类似：

```
[connection]
id=Wired connection 1
uuid=e3c7c0b5-15f1-4e5d-9a2a-01b2f367f201
type=ethernet
interface-name=enp0s3
autoconnect=true

[ipv4]
address1=192.168.117.129/24,192.168.117.1
dns=8.8.8.8;114.114.114.114;
method=manual

[ipv6]
method=ignore
```

---

## 总结对比

| 项目   | CentOS 7                          | AlmaLinux 10                              |
| ---- | --------------------------------- | ----------------------------------------- |
| 管理方式 | network-scripts                   | NetworkManager                            |
| 配置命令 | `vi ifcfg-enp0s3`                 | `nmtui`/ `nmcli`                          |
| 文件路径 | `/etc/sysconfig/network-scripts/` | `/etc/NetworkManager/system-connections/` |
| 重启命令 | `systemctl restart network`       | `systemctl restart NetworkManager`        |
