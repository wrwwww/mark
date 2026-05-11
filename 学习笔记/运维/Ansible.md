## 安装
### Ubuntu/Debian（apt）
```bash
sudo apt update
sudo apt install -y ansible
```

### CentOS/RHEL（EPEL + yum/dnf）
```bash
sudo yum install -y epel-release
sudo yum install -y ansible
```

### Pip
```bash
python3 -m pip install --user ansible
```

---

## 配置（`ansible.cfg`）
常用：`inventory`、`remote_user`、`forks`、`timeout`、`host_key_checking`，以及 `module_name`（`ansible` 命令未写 `-m` 时的默认模块）。

```properties
[defaults]
# inventory = ./inventory.ini
# remote_user = root
# forks = 20
# timeout = 30
# host_key_checking = False

# ansible 命令（非 playbook）不写 -m 时默认模块
module_name = command
```

---

## 快速上手（最小闭环）
### 1）准备 Inventory
```properties
[web]
10.0.0.11 ansible_user=root
10.0.0.12 ansible_user=root
```

### 2）连通性验证（Ad-hoc）
```bash
ansible -i inventory.ini web -m ping
```

### 3）跑一个最小 Playbook（带 handler）
```yaml
---
- name: Deploy nginx
  hosts: web
  become: true
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

```bash
ansible-playbook -i inventory.ini site.yml
```

---

## Ansible 主机清单
### 一、 核心地位与文件位置
#### 1.1 核心地位
主机清单是 Ansible 操作**不可跳过**的强制环节。任何远程主机若未定义在清单中，Ansible 将拒绝对其执行任务。

#### 1.2 文件位置与优先级
+ **系统级配置**：`/etc/ansible/hosts`
+ **项目级配置**：当前目录下的 `./hosts` 文件
+ **优先级规则**：当前目录下的 `hosts` 文件优先级**高于**系统级配置。

#### 1.3 主机条目属性
支持在主机条目后附加连接与控制属性，例如：

+ 登录用户名、密码
+ SSH 端口、私钥
+ Python 解释器路径
+ 自定义主机别名

---

### 二、 主机清单语法详解
#### 2.1 精确主机标识
直接指定目标主机的 IP 地址或 FQDN（完全限定域名）。

+ **FQDN 解析依赖**：本地 DNS 服务器或 `/etc/hosts` 文件。
+ **FQDN 结构**：`主机名.二级域.顶级域.根域` (例：`www.example.com`)

#### 2.2 范围表示法
用于批量定义具有连续规律的主机名或 IP。

+ **主机名范围**：`web[01:06]` (匹配 `web01` 至 `web06`，需注意数字位数)
+ **IP 网段**：`10.0.1.0/24`
+ **字母范围**：`server[a:d]`

#### 2.3 分组管理
通过 INI 格式将主机逻辑归类。

+ **基本组定义**：

```properties
[webservers]
web01
web02
```

+ **嵌套子组**：使用 `:children` 后缀包含其他组。

```properties
[production:children]
webservers
dbservers
```

#### 2.4 特殊组名
Ansible 预留的隐式分组，无需手动定义即可直接调用：

+ `all`：清单中存在的**所有主机**。
+ `ungrouped`：未分配至任何显式组的主机。
+ `localhost`：特指 Ansible 控制节点自身。

---

### 三、 常用主机连接属性速查表
除 Python 解释器外，以下变量常用于定义主机的具体连接方式：

| 属性变量 | 作用描述 | 配置示例 |
| :--- | :--- | :--- |
| `ansible_host` | **别名指向真实 IP** (生产环境常用) | `web01 ansible_host=192.168.1.10` |
| `ansible_user` | 指定 SSH 连接的用户名 | `ansible_user=ubuntu` |
| `ansible_port` | 指定 SSH 服务的监听端口 | `ansible_port=2222` |
| `ansible_ssh_private_key_file` | 指定 SSH 私钥文件路径 | `ansible_ssh_private_key_file=/path/key.pem` |
| `ansible_python_interpreter` | 指定远程主机 Python 解释器路径 | `ansible_python_interpreter=/usr/bin/python3` |


---

### 四、 主机匹配逻辑与通配符
在使用 `ansible` 命令或 `--limit` 限定执行范围时，可使用逻辑运算符（**需加引号防止 Shell 误解析**）。

#### 4.1 逻辑运算符
+ `:`** (OR - 并集)**：匹配组 A 或组 B 中的所有主机。
    - 示例：`ansible 'web:storage' --list`
+ `:&`** (AND - 交集)**：匹配同时属于组 A **且**属于组 B 的主机。
    - 示例：`ansible 'web:&mysql' --list`
+ `:!`** (NOT - 补集)**：匹配属于组 A **但不在**组 B 中的主机。
    - 示例：`ansible 'web:!mysql' --list`

#### 4.2 通配符
+ `*`：匹配任意长度任意字符。
+ **注意**：必须使用引号包裹模式，防止通配符被本地 Shell 优先展开。

---

### 五、 Python 解释器兼容性处理
#### 5.1 问题现象
Ansible 默认寻找 `python` 命令，若远程主机仅安装 `python3` 或存在多版本共存，可能导致执行警告或报错。

#### 5.2 解决方案与优先级
按优先级从高到低排列（高优先级覆盖低优先级）：

1. **命令行参数 (Extra Vars)**：

```bash
ansible-playbook site.yml -e "ansible_python_interpreter=/usr/bin/python3"
```

2. **主机清单条目**：

```properties
web01 ansible_host=10.0.0.1 ansible_python_interpreter=/usr/bin/python3
```

3. **Playbook 内 **`vars`** 字段**：

```yaml
vars:
  ansible_python_interpreter: /usr/bin/python3
```

4. **全局配置文件 (**`ansible.cfg`**)**：

```properties
[defaults]
interpreter_python = /usr/bin/python3
```

---

### 六、 常见问题排查与规范
#### 6.1 命名规范警告
+ **禁止**：主机名或组名中包含短横线 `-`。
+ **原因**：Ansible 内部 Jinja2 引擎会将短横线识别为减号运算符，导致变量解析失败 (`Invalid character`)。
+ **规范**：请使用下划线 `_` 替代短横线（例：`web_servers`）。

#### 6.2 范围写法易错点
+ **写法 **`web[01:06]`：仅匹配恰好为两位数字的主机 (`web01` ... `web06`)。
+ **写法 **`web[1:6]`：匹配 `web1` 至 `web6` (不补零)。

#### 6.3 逻辑匹配加引号对照
+ **错误写法**：`ansible web:&mysql --list` (Shell 会把 `&` 当作后台运行符号)。
+ **正确写法**：`ansible "web:&mysql" --list` 或 `ansible 'web:&mysql' --list`。





## 一、 跨主机免密认证（SSH Key）配置
前置说明：Ansible 依赖 SSH 协议与远程主机通信。若未配置免密登录，每次执行任务都需交互式输入密码，会阻断自动化流程。

### 1.1 操作步骤
#### 第一步：在控制节点生成密钥对
```bash
ssh-keygen -t rsa -b 4096 -C "ansible-control-node"
```

#### 第二步：分发公钥至目标主机
```bash
ssh-copy-id root@192.168.1.100
```

#### 第三步：验证免密登录
```bash
ssh root@192.168.1.100 "hostname"
```

### 1.2 批量分发公钥脚本示例
```bash
#!/usr/bin/env bash

while read -r host; do
  [[ -z "$host" || "$host" =~ ^\\[.*\\]$ ]] && continue
  ssh-copy-id root@"$host"
done < hosts
```

> 更推荐做法：使用 Ansible 的 `authorized_key` 模块分发公钥，做到幂等与可重复执行。
>

### 1.3 故障排查要点
+ 权限：`~/.ssh` 目录权限建议 `700`，`authorized_keys` 文件权限建议 `600`
+ SELinux：启用时可执行 `restorecon -R -v ~/.ssh` 恢复安全上下文

## 二、 特权升级（sudo）配置
场景说明：当使用普通用户（非 root）通过 SSH 登录后，需要执行安装软件、管理服务或访问受限目录等操作时，必须临时提升权限。

### 2.1 目标主机端配置（推荐方式）
使用 `visudo` 编辑 sudoers：

```latex
%wheel ALL=(ALL) NOPASSWD: ALL
```

配置含义（简表）：

| 字段 | 含义 |
| --- | --- |
| `%wheel` | 目标为 `wheel` 用户组 |
| `ALL=(ALL)` | 可在任意主机以任意身份运行命令 |
| `NOPASSWD: ALL` | 关键：执行所有命令无需交互输入密码 |


### 2.2 Ansible 端配置（Playbook 或清单变量）
方式一：在 Inventory 中定义：

```properties
[webservers]
web01 ansible_host=10.0.0.1 ansible_user=deploy ansible_become=yes ansible_become_method=sudo
```

方式二：在 Playbook 中全局启用：

```yaml
---
- name: 更新系统软件包
  hosts: all
  become: yes
  become_method: sudo
  tasks:
    - name: 安装 nginx
      apt:
        name: nginx
        state: present
```

### 2.3 命令行临时提权
```bash
ansible all -m ping --become --become-method=sudo
```

## 模块化实践
### 一、 模块概述与幂等性原理
#### 1.1 模块生态与数量
+ **发展趋势**：Ansible 模块数量增长迅猛。
    - 2015年：约270个
    - 2016年：540个
    - 2018年：1378个
    - 2025年5月：超过 **10000+** 个。
+ **学习策略**：**按需检索**。无需死记硬背，遇到默认模块无法解决的场景时，通过 [Ansible 官方文档](https://docs.ansible.com/ansible/latest/collections/index_module.html) 精准查找，不建议在命令行盲目搜索。

#### 1.2 幂等性原理
+ **定义**：同一操作执行一次与执行多次的最终结果完全一致。
+ **底层逻辑**：**条件判断 + 状态检查**。
    - 执行前检查目标状态（如用户是否存在）。
    - **存在** → 跳过操作，输出 `ok` 或 `changed=0`。
    - **不存在** → 执行创建/修改操作，输出 `changed=1`。

---

### 二、 Command 模块 vs Shell 模块
#### 2.1 Command 模块（默认模块）
+ **特点**：安全、简单，但**功能受限**。
+ **三大局限性**：
    1. **不支持命令别名**：例如 `ll` 无法识别，需写作 `ls -l`。
    2. **不支持环境变量**：`export` 设置无效（Python执行环境隔离）。
    3. **不支持管道符 **`|`：无法进行数据流传递（如 `cat file | grep text`）。
+ **常用参数**：`chdir`（执行前切换目录）。

#### 2.2 Shell 模块
+ **特点**：功能强大，完美替代 Command。
+ **优势**：
    - ✅ 支持别名（`ll`）。
    - ✅ 支持环境变量（需用分号连接，如 `export VAR=1; cmd`）。
    - ✅ 支持管道符、重定向等 Shell 特性。
+ **用法**：仅需将 `-m command` 替换为 `-m shell`。

#### 2.3 默认模块切换技巧
+ **场景**：规避 Command 模块的默认缺陷。
+ **方法**：修改 `ansible.cfg` 配置文件。

```properties
[defaults]
module_name = shell
```

---

### 三、 常用系统管理模块
| 模块 | 核心功能 | 关键参数 | 关键实践/警示 |
| :--- | :--- | :--- | :--- |
| **hostname** | 设置主机名 | `name` | **高风险**：批量执行会覆盖所有目标主机名，建议单机或使用变量。 |
| **user** | 用户管理 | `name`, `state`, `group`, `uid`, `system`, `password` | `state=present` 创建，`state=absent` 删除；`system=yes` 创建系统用户 (UID<1000)。 |
| **group** | 用户组管理 | `name`, `state`, `system`, `gid` | `state=absent` 删除组；`system=yes` 创建系统组。 |
| **cron** | 定时任务 | `name`, `job`, `minute/hour...` | 注意幂等性，建议配合 `name` 使用以支持修改和删除。 |
| **sysctl** | 内核参数调优 | `name`, `value`, `reload` | 用于修改 `/etc/sysctl.conf`，提升系统性能。 |
| **setup** | 收集目标主机信息 | `filter` | **大规模优化**：超过百台主机建议设置 `gather_facts: false` 或开启缓存。 |


---

### 四、 文件管理模块详解
#### 4.1 Copy 模块（推送）
+ **必选参数**：`src`（本地源）、`dest`（远程目标路径+文件名）。
+ **高级用法**：
    - **直接生成内容**：`content="This is text"`（替代 `echo`）。
    - **备份机制**：`backup=yes`（覆盖前备份原文件）。
    - **权限设置**：`owner`, `group`, `mode`（如 `mode='0644'`）。

#### 4.2 Fetch 模块（拉取）
+ **功能**：从远程主机拉取文件到控制机。
+ **路径规则**：文件会自动存储为 `dest目录/远程IP/原始绝对路径` 结构，**有效防止多主机文件覆盖**。

#### 4.3 File 模块（属性与状态管理）
+ **状态控制 **`state`：
    - `directory`：创建目录。
    - `touch` / `file`：创建空文件。
    - `link`：创建软链接（需配合 `src`）。
    - `absent`：删除文件/目录/链接。
+ **权限说明**：默认受系统 `umask` 影响，目录默认 `0755`，文件默认 `0644`。

---

### 五、 关键实践建议与优先级
#### 5.1 必练核心模块 (TOP 3)
1. **hostname**：主机标识配置。
2. **setup**：资产信息自动采集（CMDB雏形）。
3. **copy**：配置文件统一分发。

#### 5.2 进阶场景模块
+ **源码编译部署**：`user`、`group`（创建专属运行账户，如 nginx/mysql）。
+ **日常维护**：`cron`（定时清理日志/备份）、`sysctl`（内核参数优化）。

#### 5.3 通用原则总结
+ **底层逻辑映射**：所有 Ansible 模块本质是 **Linux 原生命令的封装**。理解命令原理是掌握模块的捷径。
+ **统一状态范式**：绝大多数模块通过 `state=present/absent` 控制资源生命周期，这是 Ansible 声明式自动化的核心特征。



### 一、 压缩包管理模块：Archive
#### 1.1 功能定位
+ **<font style="color:rgb(15, 17, 21);">作用</font>**<font style="color:rgb(15, 17, 21);">：将</font>**<font style="color:rgb(15, 17, 21);">本地</font>**<font style="color:rgb(15, 17, 21);">压缩包传输至远程主机并</font>**<font style="color:rgb(15, 17, 21);">一步解压</font>**<font style="color:rgb(15, 17, 21);">。</font>
+ **<font style="color:rgb(15, 17, 21);">典型场景</font>**<font style="color:rgb(15, 17, 21);">：远程主机源码编译安装前的软件包解压准备。</font>

#### 1.2 核心参数与逻辑
| 参数 | 说明 | 注意事项 |
| --- | --- | --- |
| `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">src</font>` | <font style="color:rgb(15, 17, 21);">控制节点上的</font>**<font style="color:rgb(15, 17, 21);">本地</font>**<font style="color:rgb(15, 17, 21);">压缩包路径</font> | <font style="color:rgb(15, 17, 21);">支持 tar.gz, zip 等格式</font> |
| `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">dest</font>` | <font style="color:rgb(15, 17, 21);">远程主机的</font>**<font style="color:rgb(15, 17, 21);">目标目录</font>** | <font style="color:rgb(15, 17, 21);">需确保远程目录存在，且需匹配压缩包内</font>**<font style="color:rgb(15, 17, 21);">顶层目录结构</font>** |
| `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">copy</font>` | <font style="color:rgb(15, 17, 21);">默认为</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">yes</font>` | <font style="color:rgb(15, 17, 21);">启用 SCP 传输功能</font> |


#### 1.3 常见坑点与替代方案
+ **<font style="color:rgb(15, 17, 21);">问题</font>**<font style="color:rgb(15, 17, 21);">：若</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">dest</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">目录不存在，或压缩包内路径与远程目录结构不匹配，解压失败。</font>
+ **<font style="color:rgb(15, 17, 21);">替代方案</font>**<font style="color:rgb(15, 17, 21);">（Ansible 版本不支持远程解压时）：</font>
    1. <font style="color:rgb(15, 17, 21);">先使用</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">copy</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">模块传输压缩包。</font>
    2. <font style="color:rgb(15, 17, 21);">再使用</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">shell: tar -xf package.tar.gz -C /target/dir</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">命令解压。</font>

---

### 二、 文件内容编辑模块
#### 2.1 选型依据
+ **<font style="color:rgb(15, 17, 21);">不推荐</font>************<font style="color:rgb(15, 17, 21);"> </font>**`**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">sed</font>**`**<font style="color:rgb(15, 17, 21);"> </font>************<font style="color:rgb(15, 17, 21);">命令</font>**<font style="color:rgb(15, 17, 21);">：Ansible 执行 Shell 中的</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">sed</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">容易遇到特殊字符转义和编码问题。</font>
+ **<font style="color:rgb(15, 17, 21);">推荐方案</font>**<font style="color:rgb(15, 17, 21);">：</font>
    - **<font style="color:rgb(15, 17, 21);">lineinfile</font>**<font style="color:rgb(15, 17, 21);">：处理</font>**<font style="color:rgb(15, 17, 21);">整行</font>**<font style="color:rgb(15, 17, 21);">的增、删、改。</font>
    - **<font style="color:rgb(15, 17, 21);">replace</font>**<font style="color:rgb(15, 17, 21);">：处理</font>**<font style="color:rgb(15, 17, 21);">行内部分内容</font>**<font style="color:rgb(15, 17, 21);">的查找替换。</font>

#### 2.2 Lineinfile 模块（整行编辑）
+ **<font style="color:rgb(15, 17, 21);">核心逻辑</font>**<font style="color:rgb(15, 17, 21);">：基于正则匹配行首/行尾/整行，进行替换或删除。</font>
+ **<font style="color:rgb(15, 17, 21);">关键参数</font>**<font style="color:rgb(15, 17, 21);">：</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">path</font>`<font style="color:rgb(15, 17, 21);">：目标文件路径。</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">regexp</font>`<font style="color:rgb(15, 17, 21);">：用于定位行的正则表达式（如</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">^#Comment</font>`<font style="color:rgb(15, 17, 21);">）。</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">line</font>`<font style="color:rgb(15, 17, 21);">：要替换成的</font>**<font style="color:rgb(15, 17, 21);">完整新行内容</font>**<font style="color:rgb(15, 17, 21);">。</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state</font>`<font style="color:rgb(15, 17, 21);">：</font>
        * `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">present</font>`<font style="color:rgb(15, 17, 21);">（默认）：确保行存在。</font>
        * `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">absent</font>`<font style="color:rgb(15, 17, 21);">：删除匹配到的行。</font>
+ **<font style="color:rgb(15, 17, 21);">示例</font>**<font style="color:rgb(15, 17, 21);">：</font>
    - <font style="color:rgb(15, 17, 21);">修改监听端口：</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">regexp='^listen' line='listen 80;'</font>`
    - <font style="color:rgb(15, 17, 21);">删除所有注释行：</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">regexp='^#' state=absent</font>`

#### 2.3 Replace 模块（局部替换）
+ **<font style="color:rgb(15, 17, 21);">核心逻辑</font>**<font style="color:rgb(15, 17, 21);">：类似</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">sed -i 's/old/new/g'</font>`<font style="color:rgb(15, 17, 21);">，仅替换匹配的部分字符，不改变整行结构。</font>
+ **<font style="color:rgb(15, 17, 21);">关键参数</font>**<font style="color:rgb(15, 17, 21);">：</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">path</font>`<font style="color:rgb(15, 17, 21);">：目标文件路径。</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">regexp</font>`<font style="color:rgb(15, 17, 21);">：需要被替换的</font>**<font style="color:rgb(15, 17, 21);">内容模式</font>**<font style="color:rgb(15, 17, 21);">。</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">replace</font>`<font style="color:rgb(15, 17, 21);">：替换后的</font>**<font style="color:rgb(15, 17, 21);">新内容</font>**<font style="color:rgb(15, 17, 21);">。</font>
    - <font style="color:rgb(15, 17, 21);">支持反向引用</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">\1</font>`<font style="color:rgb(15, 17, 21);">。</font>
+ **<font style="color:rgb(15, 17, 21);">示例</font>**<font style="color:rgb(15, 17, 21);">：</font>
    - `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">regexp='defaults.*' replace='defaults rw'</font>`<font style="color:rgb(15, 17, 21);">：将</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">defaults</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">后跟任意内容替换为</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">defaults rw</font>`<font style="color:rgb(15, 17, 21);">。</font>

---

### 三、 软件包管理模块：Apt & Yum
#### 3.1 模块适配性
| 操作系统家族 | 对应模块 | 核心参数 |
| --- | --- | --- |
| <font style="color:rgb(15, 17, 21);">Debian / Ubuntu</font> | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">apt</font>` | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">name</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state</font>` |
| <font style="color:rgb(15, 17, 21);">RHEL / CentOS / Rocky</font> | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">yum</font>` | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">name</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state</font>` |


#### 3.2 安装与卸载实践
| 操作 | 参数写法 | 行为特征与风险 |
| --- | --- | --- |
| **<font style="color:rgb(15, 17, 21);">安装</font>** | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state=present</font>` | <font style="color:rgb(15, 17, 21);">自动处理依赖；</font>**<font style="color:rgb(15, 17, 21);">Ubuntu 系默认自动启动服务</font>**<font style="color:rgb(15, 17, 21);">（Rocky 系需额外配置）。</font> |
| **<font style="color:rgb(15, 17, 21);">卸载</font>** | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state=absent</font>` | **<font style="color:rgb(15, 17, 21);">仅移除主包</font>**<font style="color:rgb(15, 17, 21);">。残留</font>**<font style="color:rgb(15, 17, 21);">配置文件</font>**<font style="color:rgb(15, 17, 21);">（Config）和</font>**<font style="color:rgb(15, 17, 21);">不再需要的依赖</font>**<font style="color:rgb(15, 17, 21);">（Deps）。</font> |


#### 3.3 深度清理（替代方案）
+ **<font style="color:rgb(15, 17, 21);">场景</font>**<font style="color:rgb(15, 17, 21);">：需要彻底清除软件包及其配置（类似</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">purge</font>`<font style="color:rgb(15, 17, 21);">）。</font>
+ **<font style="color:rgb(15, 17, 21);">方法</font>**<font style="color:rgb(15, 17, 21);">：回归</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">shell</font>`<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">模块执行原生命令。</font>
    - **<font style="color:rgb(15, 17, 21);">Debian 系</font>**<font style="color:rgb(15, 17, 21);">：</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">shell: apt purge -y nginx</font>`
    - **<font style="color:rgb(15, 17, 21);">RedHat 系</font>**<font style="color:rgb(15, 17, 21);">：</font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">shell: yum remove --setopt=clean_requirements_on_remove=True package</font>`

---

### 四、 服务管理模块：Service
#### 4.1 核心能力矩阵
模块通过两个核心维度控制服务：

| 维度 | 参数 | 可选值示例 | 作用 |
| --- | --- | --- | --- |
| **<font style="color:rgb(15, 17, 21);">运行状态</font>** | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">state</font>` | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">started</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">stopped</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">restarted</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">reloaded</font>` | <font style="color:rgb(15, 17, 21);">控制当前进程是否运行</font> |
| **<font style="color:rgb(15, 17, 21);">持久化自启</font>** | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">enabled</font>` | `<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">yes</font>`   <font style="color:rgb(15, 17, 21);">,</font><font style="color:rgb(15, 17, 21);"> </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">no</font>` | <font style="color:rgb(15, 17, 21);">控制开机是否自启动</font> |


#### 4.2 典型运维场景命令
```bash
# 1. 设置开机自启
ansible web -m service -a "name=nginx enabled=yes"

# 2. 修改配置文件后重载（平滑生效，不中断连接）
ansible web -m service -a "name=nginx state=reloaded"

# 3. 重启服务
ansible web -m service -a "name=nginx state=restarted"
```

---

### 五、 辅助调试与生态扩展
#### 5.1 常用辅助模块
| 模块 | 功能 | 示例 |
| --- | --- | --- |
| **setup** | 获取主机信息，常用于 Playbook 条件判断 | `filter=ansible_distribution`（判断系统类型） |
| **debug** | 打印变量或消息，用于调试 | 配合 `register` 打印命令执行结果 |


## Ansible Playbook 核心编程笔记
## 一、 模块学习策略与核心思维
### 1.1 模块优先级回顾
| 模块分类 | 代表模块 | 应用场景与备注 |
| :--- | :--- | :--- |
| **基础信息** | `hostname`, `setup` | 环境定制与资产采集 |
| **包管理** | `apt`, `yum` | 软件安装与卸载 |
| **服务控制** | `service` | 进程启停与自启管理 |
| **文件编辑** | `lineinfile`, `replace` | 特定配置修改（生产环境通常由 `template` 替代） |


### 1.2 核心学习心法
+ **降维打击思维**：所有 Ansible 功能均可映射到 **Linux 原生命令**。掌握底层命令逻辑，即可触类旁通 Ansible 模块。
+ **进阶路径**：初学者可先用 `command` 模块快速验证功能，再逐步替换为专用模块。

---

## 二、 Playbook 基础概念与执行机制
### 2.1 定义与类比
+ **本质**：Ansible 的 **声明式自动化脚本**。
+ **类比**：类似 Linux Shell 脚本，但不能直接粘贴原生命令（如 `apt install`），需转换为 Ansible 模块语法。

### 2.2 两种执行方式对比
| 执行方式 | 命令格式 | 适用场景 |
| :--- | :--- | :--- |
| **Ad-hoc 命令** | `ansible <host> -m <module> -a "<args>"` | 单次临时操作、快速验证 |
| **Playbook 执行** | `ansible-playbook <playbook.yml>` | 复杂编排、批量部署、版本控制 |


> **底层逻辑**：Playbook 解析器会将 YAML 文件拆解为多条 Ad-hoc 命令依次执行。
>

### 2.3 Playbook 四要素（核心骨架）
| 要素 | 对应字段 | 功能说明 |
| :--- | :--- | :--- |
| **1. 目标** | `hosts` | 定义要在哪些主机上执行（对应 Inventory 中的组名/IP）。 |
| **2. 任务** | `tasks` | 定义具体执行的操作列表，每个任务必须包含 `name` 和 `module`。 |
| **3. 变量** | `vars` | 预设环境变量或自定义值（如 Python 解释器路径）。 |
| **4. 触发器** | `handlers` | 类似钩子函数，当任务状态为 `changed` 时触发（常用于重启服务）。 |


---

## 三、 YAML 语法规范（Ansible 限定版）
### 3.1 基础语法铁律
| 规则项 | 要求 | 反面教材 |
| :--- | :--- | :--- |
| **大小写** | **敏感** | `Hosts` ≠ `hosts` |
| **缩进** | 仅允许**空格**，禁止 `Tab` | 混用 Tab 会导致解析失败 |
| **层级** | 同级属性必须**左对齐** | `name` 和 `apt` 未对齐则报错 |
| **列表** | 使用短横线 `- ` 开头 | `- item1` (注意空格) |


### 3.2 Ansible 特有约束
+ **文档分隔符 **`---`：
    - ✅ 允许：文件开头第一个 `---` 作为起始标识。
    - ❌ **禁止**：在单个 YAML 文件中使用多个 `---` 定义多个 Playbook（Ansible 不支持单文件多 Play，会报错）。

### 3.3 数据格式生态定位
+ **运维/配置**：首选 **YAML**（人类可读性强）。
+ **K8s/API传输**：底层转为 **JSON**。
+ **开发/程序**：首选 **JSON**。

---

## 四、 实战案例：Playbook 部署 Apache
### 4.1 需求拆解
+ **目标**：在 Rocky Linux 主机安装 Apache (httpd)。
+ **状态**：安装成功且开机自启。

### 4.2 完整 Playbook 代码
```yaml
---
- hosts: 10.0.0.13
  remote_user: root
  gather_facts: true   # 默认开启，收集主机信息
  
  tasks:
    - name: 01 - Install Apache2 (httpd) package
      apt:
        name: apache2
        state: present

    - name: 02 - Ensure Apache2 is running and enabled
      service:
        name: apache2
        state: restarted
        enabled: yes
```

### 4.3 执行与验证
+ **执行命令**：`ansible-playbook install_apache.yml`
+ **验证命令**：`ansible 10.0.0.13 -m shell -a "netstat -tnulp | grep :80"`
+ **注意**：默认包含 `Gathering Facts` 步骤，若主机量过大且无需系统信息，可在 Playbook 中设置 `gather_facts: false` 提升速度。

---

## 五、 高级特性与调试技巧
### 5.1 并发与目标控制
| 需求 | 实现方式 | 示例 |
| :--- | :--- | :--- |
| **调整并发数** | 修改 `ansible.cfg` 中的 `forks` | `forks = 10` |
| **限定单机执行** | 使用 `-l` 参数 | `ansible-playbook site.yml -l 10.0.0.13` |


### 5.2 调试与日志级别
| 参数 | 详细程度 | 用途 |
| :--- | :--- | :--- |
| `-v` | 简略输出 | 查看任务执行结果 |
| `-vv` | 详细输出 | 查看具体命令和返回数据 |
| `-vvv` | 极详细输出 | 排查连接与认证问题 |


### 5.3 关键避坑指南
+ **模块覆盖原则**：在单个 Task 中定义多个模块时，遵循 **Last Defined** 原则（仅最后一个模块生效）。**严禁在一个 Task 中混用多模块**。
+ **幂等性校验**：利用 `validate` 参数（如 `validate: nginx -t`）在修改配置后自动进行语法检查，防止配置错误导致服务宕机。

---

## 六、 综合案例：Nginx 环境定制化部署
### 6.1 需求清单
1. 创建系统用户 `nginx_test`（指定 UID）。
2. 建立项目发布目录。
3. 拷贝定制化 Nginx 配置文件。
4. 安装并启动 Nginx 服务。

### 6.2 涉及模块矩阵
| 步骤 | 模块 | 关键参数 |
| :--- | :--- | :--- |
| 用户创建 | `user` | `name`, `uid`, `system`, `create_home` |
| 目录创建 | `file` | `path`, `state=directory`, `owner`, `mode` |
| 配置分发 | `copy` / `template` | `src`, `dest`, `backup=yes` |
| 软件安装 | `apt` / `yum` | `name`, `state` |
| 服务启动 | `service` | `name`, `state`, `enabled` |


### 6.3 清理原则（逆序操作）
+ **顺序**：停服务 → 卸载软件 → 删文件 → 删用户。
+ **原因**：防止因依赖关系导致删除失败或留下残留文件。

### 6.4 安全最佳实践
+ **权限最小化**：普通用户执行 Playbook 时，需配置 `NOPASSWD` 免密 `sudo`。
+ **杜绝明文密码**：敏感信息使用 `ansible-vault` 加密或变量文件引用。



## Playbook 进阶：异常处理、Handlers、Tags、变量
### 一、 异常中断处理机制
### 1.1 核心概念
+ **异常中断定义**：任务正常执行过程中，因特殊原因被迫中止的现象。
+ **设计原则**：辅助性检测任务（如健康检查、状态探测）的失败不应导致整体业务编排崩溃。

### 1.2 两种容错处理方案对比
| 处理方式 | 参数配置 | 行为表现 | 适用场景 |
| :--- | :--- | :--- | :--- |
| **强制成功** | `failed_when: false` | 命令实际执行失败，但 Playbook **标记为成功**（绿色输出）。 | 探测性命令，不关心结果只关心流程继续。 |
| **忽略错误** | `ignore_errors: true` | 命令执行失败，显示**红色错误信息**，但流程**继续执行**后续任务。 | 允许失败但不阻断流程，需保留错误记录供排查。 |


### 1.3 实操示例
```yaml
tasks:
  # 示例1：强制成功（无论命令是否报错，都认为任务 OK）
  - name: 探测性检查（结果不影响后续）
    shell: cat /tmp/may_not_exist.txt
    failed_when: false

  # 示例2：忽略错误（报红字但不停止）
  - name: 尝试重启服务（失败也继续）
    service:
      name: nginx
      state: restarted
    ignore_errors: true
```

---

### 二、 任务依赖与触发机制：Notify & Handler
### 2.1 引入场景
+ **痛点**：配置文件变更后，需要**自动、按需**重启服务，而不是无条件地每次执行重启任务。
+ **解决思路**：解耦"变更检测"与"响应动作"。

### 2.2 机制原理
| 组件 | 作用 | 位置 |
| :--- | :--- | :--- |
| `notify` | 声明触发器。当任务状态为 `changed` 时，发送通知。 | 位于 `tasks` 中的具体任务内。 |
| `handlers` | 定义响应动作。是一组特殊的任务列表。 | 位于 Play 顶层，与 `tasks` **同级**。 |


### 2.3 语法结构与示例
```yaml
---
- hosts: webservers
  
  tasks:
    - name: 01 - 更新 Nginx 配置文件
      copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
      notify:                   # 触发点
        - restart nginx         # 必须与 handler 的 name 完全一致
        - check config status

  handlers:                     # 与 tasks 对齐
    - name: restart nginx
      service:
        name: nginx
        state: restarted

    - name: check config status
      shell: nginx -t
```

### 2.4 关键特性
+ **默认不执行**：`handlers` 中的任务仅在收到 `notify` 通知时才运行。
+ **单次汇聚**：即使多个任务都 `notify` 同一个 `handler`，该 `handler` 在整个 Play 执行结束后**仅运行一次**。
+ **严格匹配**：`notify` 的值必须与 `handler` 的 `name` 字符串**完全一致**（含空格、大小写）。

---

### 三、 标签分组管理：Tags
### 3.1 引入动机
+ **痛点**：单个 Playbook 文件需同时支持"全新安装"和"仅更新配置"两种独立场景。
+ **目标**：避免全量执行无关任务，实现**按需执行**。

### 3.2 标签作用机制
1. **打标签**：在 `tasks` 中为不同任务添加 `tags` 属性。
2. **指定执行**：运行时通过 `--tags` 或 `--skip-tags` 参数筛选任务。

### 3.3 实操示例
```yaml
tasks:
  - name: 安装 Apache 软件包
    apt: name=apache2 state=present
    tags:
      - install              # 归类为安装任务

  - name: 推送自定义配置文件
    copy: src=httpd.conf dest=/etc/apache2/
    tags:
      - config_change        # 归类为配置变更任务

  - name: 重启 Apache 服务
    service: name=apache2 state=restarted
    tags:
      - config_change
```

**执行命令**：

+ 仅执行安装任务：`ansible-playbook site.yml --tags install`
+ 仅更新配置：`ansible-playbook site.yml --tags config_change`

### 3.4 标签适用边界
| 场景 | 建议 |
| :--- | :--- |
| 多个场景**共用**大量相同任务 | ✅ 使用 `tags` 分类，单文件管理。 |
| 不同场景任务**完全独立**无交集 | ❌ 不建议滥用标签，应**拆分为多个独立 Playbook** 文件。 |


---

### 四、 变量进阶与优先级体系
### 4.1 变量六种实现方式
1. **Facts**（系统采集信息，如 `ansible_fqdn`）
2. **Inventory 主机变量**（直接定义在主机名后）
3. **Inventory 组变量**（定义在 `[组名:vars]` 下）
4. **Playbook **`vars`（Play 级别变量）
5. `vars_files`（外部变量文件）
6. **命令行 **`-e`** 变量**（Extra vars）

### 4.2 变量优先级金字塔（从低到高）
```plain
┌─────────────────────────────┐
│ 6. 命令行 -e 变量 (最高)    │
├─────────────────────────────┤
│ 5. Playbook vars_files      │
├─────────────────────────────┤
│ 4. Playbook vars 定义       │
├─────────────────────────────┤
│ 3. Inventory 组变量         │
├─────────────────────────────┤
│ 2. Inventory 主机变量       │
├─────────────────────────────┤
│ 1. Facts 采集信息 (最低)    │
└─────────────────────────────┘
```

### 4.3 覆盖验证案例
**Inventory 文件 (inventory.ini)**：

```properties
[web]
vm01 host_id=master    # 主机变量

[web:vars]
host_id=node           # 组变量
```

+ **结果**：执行后 `vm01` 生效 `host_id=master`。
+ **结论**：**主机变量优先级 > 组变量优先级**。

### 4.4 模板语法规范
| 场景 | 变量引用格式 | 示例 |
| :--- | :--- | :--- |
| **Ansible Playbook / Jinja2 模板** | `{{ variable_name }}` | `path: /var/log/{{ ansible_fqdn }}.log` |
| **Shell 脚本** | `$variable` | `echo $PATH` |
| **AWK 命令** | 直接使用变量名 | `awk '{print $1}'` |


> **注意**：Ansible 模板严格使用双大括号 `{{ }}` 包裹变量，不可与 Shell 或 AWK 语法混淆。
>



## 一、 变量优先级体系深度验证
### 1.1 优先级金字塔（由低到高）
Ansible 变量存在多层级覆盖机制，理解优先级顺序是避免配置混乱的关键。

| 优先级层级 | 变量来源 | 定义位置/方式 | 覆盖能力 |
| :--- | :--- | :--- | :--- |
| **第1级（最低）** | Inventory 主机/组变量 | `inventory` 文件中直接定义 | 被所有上层变量覆盖 |
| **第2级** | `vars_files` 外部文件 | 通过 `vars_files` 关键字导入 | 覆盖 Inventory，被 Playbook vars 覆盖 |
| **第3级** | Playbook `vars` 定义 | Playbook 内部 `vars:` 区块 | 覆盖前两级，被命令行覆盖 |
| **第4级（最高）** | 命令行 `-e` 参数 | `ansible-playbook -e "key=value"` | **最高优先级，覆盖一切** |


### 1.2 各层级实操详解
#### 1.2.1 命令行参数（最高优先级）
+ **命令格式**：`ansible-playbook playbook.yml -e "key=value"`
+ **优先级数值**：3（本视频定义体系中的最高级）
+ **典型场景**：临时覆盖测试、敏感信息（密码）运行时传入、CI/CD 流水线动态注入。
+ **示例**：

```bash
ansible-playbook deploy.yml -e "nginx_port=8080 env=production"
```

#### 1.2.2 Playbook 中 `vars` 定义（中等优先级）
+ **定义位置**：Playbook 文件中，与 `hosts`、`tasks` 同级。
+ **优先级数值**：2
+ **语法示例**：

```yaml
---
- hosts: webservers
  vars:
    head: "3W"
    host_id: "node"
    tae: "ANSIBLE.TOP"
    ng_port: 8080
  tasks:
    - name: Print variable
      debug:
        msg: "{{ ng_port }}"
```

+ **注意**：此处定义的变量会覆盖 `vars_files` 和 Inventory 中的同名变量。

#### 1.2.3 `vars_files` 导入（次高优先级）
+ **功能**：将变量定义从 Playbook 主体剥离，存放于独立 YAML 文件中。
+ **优先级数值**：1
+ **路径规则**：默认使用**相对路径**，与 Playbook 文件位于同一目录。
+ **语法示例**：

```yaml
# Playbook 中引用
vars_files:
  - "vars.yml"
  - "secret_vars/credentials.yml"

# vars.yml 内容
ng_port: 8081
app_user: webapp
```

+ **覆盖关系**：`vars_files` 变量会覆盖 Inventory 变量，但会被 Playbook `vars` 区块覆盖。

#### 1.2.4 主机清单中定义变量（基础优先级）
+ **优先级**：最低，作为默认基线值存在。
+ **定义方式**：

```properties
# 单主机变量
[vm04]
10.0.0.4 ansible_host=10.0.0.4 ng_port=8080

# 主机组变量
[web:vars]
ng_port=80
web_root=/var/www
```

+ **覆盖验证**：若上述所有层级均定义了 `ng_port`，最终生效的是命令行 `-e` 传入的值。

---

## 二、 模板文件（Jinja2）应用详解
### 2.1 模板文件规范
| 规范项 | 要求 | 说明 |
| :--- | :--- | :--- |
| **存放目录** | 必须位于 `templates/` 目录下 | Ansible 默认搜索路径，与 Playbook 同级或项目根目录 |
| **文件后缀** | 推荐使用 `.j2` | 如 `nginx.conf.j2`，便于识别模板文件 |
| **内容语法** | Jinja2 模板语法 | 支持变量、条件、循环等编程结构 |


### 2.2 `template` 模块 vs `copy` 模块
| 对比项 | `copy` 模块 | `template` 模块 |
| :--- | :--- | :--- |
| **处理方式** | 原样拷贝，不做任何解析 | 拷贝过程中**动态解析 Jinja2 语法**并注入变量值 |
| **适用文件** | 静态文件（二进制、已渲染好的配置） | 动态配置文件（需根据环境替换变量） |
| **语法示例** | `src: files/nginx.conf` | `src: templates/nginx.conf.j2` |


**标准用法**：

```yaml
- name: Deploy nginx config with template
  template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: '0644'
  notify: restart nginx
```

### 2.3 Jinja2 基础语法速查表
| 语法类型 | 格式 | 示例 | 说明 |
| :--- | :--- | :--- | :--- |
| **变量输出** | `{{ 变量名 }}` | `listen {{ ng_port }};` | 输出变量值到文件 |
| **条件判断** | `{% if 条件 %}...{% endif %}` | `{% if ng_port is defined %}port {{ ng_port }}{% endif %}` | 按条件渲染内容块 |
| **循环迭代** | `{% for item in list %}...{% endfor %}` | `{% for upstream in backends %}server {{ upstream }};{% endfor %}` | 遍历列表生成重复配置块 |
| **注释** | `{# 注释内容 #}` | `{# 这是自动生成的配置，请勿手动修改 #}` | 模板注释，不渲染到目标文件 |


---

## 三、 条件判断（`when` 语句）深度应用
### 3.1 基础语法结构
`when` 语句用于控制任务是否执行，基于条件表达式求值结果决定。

**标准流程模式**：

```yaml
- name: Step 1 - 检测当前状态
  command: pgrep nginx | wc -l
  register: nginx_count
  ignore_errors: true   # 防止 pgrep 无结果时报错中断

- name: Step 2A - 已安装则跳过
  debug:
    msg: "NGINX service is already installed on {{ inventory_hostname }}"
  when: nginx_count.stdout != "0"

- name: Step 2B - 未安装则执行安装
  yum:
    name: nginx
    state: present
  when: nginx_count.stdout == "0"
```

### 3.2 支持的条件表达式全集
| 条件类型 | 语法格式 | 使用场景 |
| :--- | :--- | :--- |
| **相等判断** | `when: ansible_distribution == "RedHat"` | 根据操作系统类型执行不同任务 |
| **多条件与** | `when: (ansible_distribution == "RedHat" and ansible_distribution_major_version == "7")` | 限定特定系统版本执行 |
| **多条件或** | `when: ansible_distribution == "RedHat" or ansible_distribution == "Debian"` | 兼容多种操作系统 |
| **正则匹配** | `when: ansible_facts['distribution'] is match(".*[Rr]ed.*")` | 模糊匹配系统名称（RedHat/CentOS/Rocky） |
| **变量存在性** | `when: nginx_port is defined` | 变量未定义时跳过任务 |
| **空值判断** | `when: nginx_port is not none` | 变量不为空时才执行 |
| **比较运算** | `when: ansible_memtotal_mb > 2048` | 根据硬件配置决定行为 |
| **文件存在** | `when: ansible_facts['os_family'] == "Debian" and check_file.stat.exists` | 结合 `stat` 模块判断文件状态 |


### 3.3 综合实践案例：动态检测与分支执行
#### 3.3.1 环境准备
+ **VM013**：已安装 NGINX（通过 `netstat -tlnp | grep nginx` 确认监听 80 端口）。
+ **VM016**：未安装 NGINX。

#### 3.3.2 Playbook 实现
```yaml
---
- hosts: all
  gather_facts: true
  tasks:
    - name: 检测 NGINX 进程数量
      shell: pgrep nginx | wc -l
      register: nginx_count
      ignore_errors: true
      changed_when: false

    - name: 输出已安装提示
      debug:
        msg: "【跳过】主机 {{ inventory_hostname }} 已安装 NGINX，无需重复安装"
      when: nginx_count.stdout != "0"

    - name: 执行 NGINX 安装
      yum:
        name: nginx
        state: present
      when: 
        - nginx_count.stdout == "0"
        - ansible_os_family == "RedHat"

    - name: 启动并启用 NGINX
      service:
        name: nginx
        state: started
        enabled: yes
      when: nginx_count.stdout == "0"
```

#### 3.3.3 幂等性保障验证
+ **首次执行**：VM016 触发安装任务（`changed=1`），VM013 仅输出提示信息（`skipping`）。
+ **二次执行**：两台主机均跳过安装任务，VM016 的 `yum` 模块因软件已存在而返回 `changed=0`。
+ **结论**：通过 `when` 条件判断实现了完全的幂等性，重复执行不产生副作用。

## 一、 迭代管理：核心概念与语法演进
### 1.1 迭代的本质定义
| 概念 | 说明 |
| :--- | :--- |
| **迭代含义** | 对**同一任务**重复执行多次，每次使用不同的输入参数 |
| **与条件判断的区别** | `when` 决定任务**是否执行**；迭代决定任务**执行多少次** |
| **Python 类比** | `with_items` 等价于 `for item in [list]` 结构 |


### 1.2 单值列表循环
#### 1.2.1 基础语法
```yaml
- name: 批量安装软件包
  apt:
    name: "{{ item }}"
    state: present
  with_items:
    - NGX
    - REDIS-server
    - MT-24
```

#### 1.2.2 执行逻辑拆解
上述 Playbook 会展开为三条独立的 Ad-hoc 命令：

```bash
apt install NGX state=present
apt install REDIS-server state=present
apt install MT-24 state=present
```

#### 1.2.3 关键特性
+ **临时变量**：`item` 是循环内部的默认变量名，每次迭代被赋予列表中的一个值。
+ **原子性**：每个 `item` 对应一次完整的模块调用，相互独立。
+ **适用场景**：批量安装软件、批量创建文件、批量启动服务等**单一参数**的重复操作。

### 1.3 多值传递：字典结构循环
#### 1.3.1 单值列表的局限性
当任务需要同时传递**多个关联参数**时，纯列表无法表达键值关系。

+ 示例需求：创建用户 `user1` 并加入 `group1`，创建 `user2` 并加入 `group2`。
+ 纯列表 `[user1, user2]` 无法携带对应的组信息。

#### 1.3.2 字典式多值结构
```yaml
- name: 批量创建用户并指定组
  user:
    name: "{{ item.name }}"
    group: "{{ item.group }}"
    state: present
  with_items:
    - { name: "user1", group: "group1" }
    - { name: "user2", group: "group2" }
    - { name: "user3", group: "wheel" }
```

#### 1.3.3 属性提取语法
| 取值方式 | 语法 | 示例值 |
| :--- | :--- | :--- |
| 直接取值 | `{{ item }}` | `{ name: "user1", group: "group1" }`（整个字典） |
| **属性提取** | `{{ item.name }}` | `"user1"` |
| **属性提取** | `{{ item.group }}` | `"group1"` |


#### 1.3.4 嵌套属性扩展
支持更深层级的字典嵌套：

```yaml
with_items:
  - { name: "app1", config: { port: 8080, host: "localhost" } }
  - { name: "app2", config: { port: 8081, host: "0.0.0.0" } }
# 取值：{{ item.config.port }}
```

### 1.4 新旧语法对比与选择策略
| 对比维度 | 旧语法 `with_items` | 新语法 `loop` |
| :--- | :--- | :--- |
| **引入版本** | Ansible 早期版本 | Ansible 2.5+ |
| **语法形式** | `with_items: [list]` | `loop: "{{ list }}"` |
| **设计目标** | 功能分散（`with_items`、`with_dict`、`with_fileglob` 等） | **统一入口**，降低记忆负担 |
| **兼容性** | 全版本支持 | 部分特殊场景（如动态变量解析）仍需回退旧语法 |


**语法对比示例**：

```yaml
# 旧语法（讲师推荐，习惯优先）
with_items:
  - item1
  - item2

# 新语法（官方推荐）
loop:
  - item1
  - item2
```

**实践建议**：

+ 初学者可从 `with_items` 入手，互联网上存量 Playbook 大量使用该语法。
+ 新项目建议逐步迁移至 `loop`，享受统一语法的简洁性。
+ 遇到复杂查找需求时，查阅官方文档确认 `loop` 是否完全支持。

---

## 二、 模板流程管理：Jinja2 条件渲染（`if`）
### 2.1 语法结构速查
| 结构类型 | 语法格式 | 说明 |
| :--- | :--- | :--- |
| **单分支** | `{% if 条件 %}...{% endif %}` | 条件为真时输出内容块 |
| **双分支** | `{% if 条件 %}...{% else %}...{% endif %}` | 条件为真/假分别输出不同内容 |
| **多分支** | `{% if 条件1 %}...{% elif 条件2 %}...{% else %}...{% endif %}` | 多个条件分支判断 |


### 2.2 条件表达式支持类型
| 表达式类型 | 示例 | 说明 |
| :--- | :--- | :--- |
| **变量存在性** | `{% if port is defined %}` | 变量已定义时为真 |
| **变量非空** | `{% if port %}` | 变量非空字符串/非零时为真 |
| **相等比较** | `{% if env == "production" %}` | 精确匹配 |
| **逻辑组合** | `{% if port and name %}` | 多条件与关系 |


### 2.3 核心用途：多主机差异化配置生成
**问题场景**：三台主机配置需求不同：

+ **主机13**：需配置 `listen 80` 和 `server_name app1.com`
+ **主机16**：仅需配置 `listen 80`
+ **主机19**：仅需配置 `server_name app3.com`

**模板实现**（`nginx.conf.j2`）：

```nginx
server {
    {% if port is defined %}
    listen {{ port }};
    {% endif %}
    
    {% if name is defined %}
    server_name {{ name }};
    {% endif %}
    
    root /var/www/html;
}
```

**渲染结果对比**：

| 主机 | Inventory 变量 | 生成配置 |
| :--- | :--- | :--- |
| 主机13 | `port=80 name=app1.com` | `listen 80;`   `server_name app1.com;` |
| 主机16 | `port=80` | `listen 80;` |
| 主机19 | `name=app3.com` | `server_name app3.com;` |


**核心价值**：一套模板适配多套环境，消除为每台主机手工维护配置文件的痛点。

---

## 三、 模板流程管理：Jinja2 循环渲染（`for`）
### 3.1 语法结构
```plain
{% for 变量名 in 列表 %}
    # 重复输出的内容块
    {{ 变量名.属性 }}
{% endfor %}
```

### 3.2 嵌套逻辑：`for` + `if` 组合
实际场景中，循环体内常需配合条件判断处理可选属性。

```plain
{% for vhost in vhosts %}
server {
    {% if vhost.port is defined %}
    listen {{ vhost.port }};
    {% endif %}
    
    {% if vhost.name is defined %}
    server_name {{ vhost.name }};
    {% endif %}
}
{% endfor %}
```

### 3.3 实践案例：单主机生成多虚拟主机配置
**Inventory 变量定义**：

```yaml
vhosts:
  - { port: 80, name: "app1.com" }
  - { port: 8080 }                    # 仅端口，无域名
  - { name: "app3.com" }              # 仅域名，无端口
```

**渲染输出**：

```nginx
server {
    listen 80;
    server_name app1.com;
}

server {
    listen 8080;
}

server {
    server_name app3.com;
}
```

**执行逻辑解析**：

| 迭代轮次 | `vhost` 值 | 条件判断结果 | 输出内容 |
| :--- | :--- | :--- | :--- |
| 第1轮 | `{port:80, name:"app1.com"}` | 两个属性均存在 | `listen 80;` + `server_name app1.com;` |
| 第2轮 | `{port:8080}` | 仅 `port` 存在 | 仅输出 `listen 8080;` |
| 第3轮 | `{name:"app3.com"}` | 仅 `name` 存在 | 仅输出 `server_name app3.com;` |


---

## 四、 核心概念辨析与定位总结
### 4.1 任务控制 vs 内容控制
| 控制维度 | 实现机制 | 作用对象 | 核心问题 |
| :--- | :--- | :--- | :--- |
| **任务控制** | `when`、`with_items`、`loop` | Ansible **任务执行流程** | 任务是否执行？执行多少次？ |
| **内容控制** | Jinja2 `{% if %}`、`{% for %}` | 模板**文件内容生成** | 配置文件中输出什么内容？ |


### 4.2 语法层级关系全景图
```plain
Ansible Playbook 编程体系
├── 任务执行控制（面向操作）
│   ├── 条件判断：when
│   └── 迭代循环
│       ├── with_items（单值/字典，旧语法）
│       ├── with_dict（字典专用，旧语法）
│       └── loop（统一语法，新推荐）
│
└── 模板内容控制（面向文本）
    ├── 条件分支：{% if %}...{% endif %}
    └── 循环展开：{% for %}...{% endfor %}
```

### 4.3 实践学习路径建议
| 阶段 | 学习重点 | 方法建议 |
| :--- | :--- | :--- |
| **入门** | `with_items` + `if`/`for` 组合 | 从单一功能 Playbook 开始，逐步叠加复杂度 |
| **进阶** | `loop` 新语法 + 复杂嵌套模板 | 在 GitHub 搜索 `ansible nginx deployment` 等真实案例模仿学习 |
| **精通** | 自定义 Filter、宏（Macro）、模板继承 | 阅读 Ansible 官方文档 Jinja2 章节及社区最佳实践 |


## 一、 Role 核心定义与本质
### 1.1 Role 的定义
| 概念 | 说明 |
| :--- | :--- |
| **官方定义** | Role 是将松散的 Playbook 拆解为多个独立的功能片段，并在体系化目录结构中重新编排，形成唯一入口的组织方式。 |
| **通俗理解** | Role 是对 Playbook 的**模块化封装**，将原本平铺直叙的任务列表重构为可复用、可维护的标准化组件。 |


### 1.2 Role 的本质
+ **本质论断**：Role **本质上仍是 Playbook**，只是以目录结构形式对 Playbook 进行了**肢解与重组**。
+ **核心逻辑**：将原本集中在一个 YAML 文件中的所有内容，按功能类型打散到不同子目录中，再通过入口文件重新整合。

### 1.3 Role 解决的核心问题
| 问题场景 | Playbook 直接编写 | Role 方式 |
| :--- | :--- | :--- |
| **任务臃肿** | 单个文件包含数十个任务，难以阅读 | 按功能拆分为独立文件 |
| **复用困难** | 不同项目间复制粘贴代码片段 | Role 作为独立单元被引用 |
| **协作冲突** | 多人修改同一文件易产生冲突 | 各司其职，修改独立目录 |
| **维护成本** | 变量、模板、文件混在一起 | 分类存放，定位清晰 |


---

## 二、 Role 目录结构规范与划分依据
### 2.1 标准目录结构全景
```plain
roles/
└── <role_name>/                 # 角色根目录
    ├── tasks/                   # 任务定义目录
    │   └── main.yml             # 任务入口文件（必须）
    ├── handlers/                # 触发器定义目录
    │   └── main.yml             # 触发器入口文件
    ├── templates/               # Jinja2 模板目录
    │   └── *.j2                 # 模板文件
    ├── files/                   # 静态文件目录
    │   └── *                    # 永不变化的静态文件
    ├── vars/                    # 项目专用变量目录
    │   └── main.yml             # 高优先级变量
    ├── defaults/                # 默认变量目录
    │   └── main.yml             # 低优先级默认变量
    ├── meta/                    # 元数据目录（极少使用）
    │   └── main.yml             # 角色依赖、作者信息等
    └── tests/                   # 测试目录（可选）
        └── test.yml
```

### 2.2 各目录功能详解
| 目录 | 存放内容 | 文件特点 | 使用频率 |
| :--- | :--- | :--- | :--- |
| `tasks/` | 任务定义片段（如 `user.yml`、`install.yml`） | `main.yml` 作为入口，通过 `include_tasks` 加载其他文件 | ★★★★★ |
| `handlers/` | 触发器定义（如 `restart nginx`） | `main.yml` 集中存放，被 `notify` 调用 | ★★★★☆ |
| `templates/` | Jinja2 模板文件（`.j2` 后缀） | 包含 `{{ variable }}` 和 `{% if %}` 的动态内容 | ★★★★☆ |
| `files/` | 静态文件（源码包、基准配置、证书） | **内容永不变更**，原样拷贝到目标主机 | ★★★★☆ |
| `vars/` | 项目专用变量 | 优先级较高，覆盖 `defaults` | ★★★☆☆ |
| `defaults/` | 默认变量 | 优先级最低，作为兜底值 | ★★★☆☆ |
| `meta/` | 角色元数据 | 定义角色间依赖关系，**实践中极少使用** | ★☆☆☆☆ |


### 2.3 目录划分的核心理据
+ **依据对象类型划分**：目录严格对应 Playbook 中的固有对象类型。
+ **Task 对应 **`tasks/`：所有 `name: xxx` 的任务片段。
+ **Handler 对应 **`handlers/`：所有 `notify` 触发的响应动作。
+ **Template 对应 **`templates/`：所有需要动态渲染的 `.j2` 文件。
+ **File 对应 **`files/`：所有静态不变的依赖文件。
+ **Variable 对应 `vars/` 和 **`defaults/`：所有变量定义。

---

## 三、 Role 构建逻辑与工程实践原则
### 3.1 拆分粒度的自主性原则
| 原则 | 说明 |
| :--- | :--- |
| **无强制标准** | 拆分数量由项目复杂度决定，可拆为 5 个、10 个或 20 个子文件 |
| **类比微服务** | 拆得越细，复用性越强，但维护复杂度也相应增加 |
| **平衡建议** | 单个任务文件控制在 50 行以内，功能高内聚 |


### 3.2 目录创建的按需性原则
```plain
重要原则：仅当 Playbook 实际使用某类功能时，才创建对应目录

示例：
- 未使用模板功能 → 不创建 templates/ 目录
- 未定义 handlers → 不创建 handlers/ 目录
- 仅有静态文件 → 仅保留 files/ 目录即可
```

### 3.3 整合的双层机制
#### 第一层：子目录内整合（`include_tasks`）
```yaml
# roles/nginx_pro/tasks/main.yml
---
- name: 创建系统用户和组
  include_tasks: group.yml

- name: 创建应用程序用户
  include_tasks: user.yml

- name: 安装 NGINX 软件包
  include_tasks: package.yml

- name: 部署配置文件
  include_tasks: config.yml

- name: 启动服务
  include_tasks: service.yml
```

#### 第二层：Role 级整合（`roles` 关键字）
```yaml
# site.yml（项目入口）
---
- hosts: web
  roles:
    - nginx_pro          # 自动加载 roles/nginx_pro/tasks/main.yml
    - mysql_pro          # 可同时调用多个角色
    - redis_pro
```

### 3.4 文件搜索路径机制
| 模块 | 默认搜索路径 | 说明 |
| :--- | :--- | :--- |
| `copy`（使用 `files/`） | `roles/<role>/files/` | 直接写文件名即可，无需完整路径 |
| `template`（使用 `templates/`） | `roles/<role>/templates/` | 直接写模板名，如 `src: nginx.conf.j2` |
| `include_tasks` | `roles/<role>/tasks/` | 直接写文件名，如 `include_tasks: user.yml` |


---

## 四、 Role 实战：NGINX 部署项目重构
### 4.1 原始 Playbook 结构分析
```yaml
# 原始单文件 Playbook
---
- hosts: web
  tasks:
    - name: 创建 nginx 用户组
      group: name=nginx system=yes
    - name: 创建 nginx 用户
      user: name=nginx group=nginx system=yes
    - name: 安装 nginx
      apt: name=nginx state=present
    - name: 推送配置文件
      copy: src=nginx.conf dest=/etc/nginx/nginx.conf
    - name: 启动服务
      service: name=nginx state=started enabled=yes
```

### 4.2 Role 目录结构搭建
```plain
roles/
└── nginx_pro/
    ├── tasks/
    │   ├── main.yml          # 入口：按序 include_tasks
    │   ├── group.yml         # 创建用户组
    │   ├── user.yml          # 创建用户
    │   ├── package.yml       # 安装软件包
    │   ├── config.yml        # 部署配置
    │   └── service.yml       # 管理服务
    ├── handlers/
    │   └── main.yml          # 定义 restart nginx
    ├── files/
    │   └── nginx.conf        # 静态配置文件
    └── templates/            # 暂未使用，后续可迁移 .j2 文件
```

### 4.3 各文件内容详解
`tasks/group.yml`：

```yaml
---
- name: 创建 NGINX 系统用户组
  group:
    name: nginx
    system: yes
    state: present
```

`tasks/user.yml`：

```yaml
---
- name: 创建 NGINX 系统用户
  user:
    name: nginx
    group: nginx
    system: yes
    shell: /sbin/nologin
    create_home: no
    state: present
```

`tasks/package.yml`：

```yaml
---
- name: 安装 NGINX 软件包
  apt:
    name: nginx
    state: present
    update_cache: yes
```

`tasks/config.yml`：

```yaml
---
- name: 部署 NGINX 主配置文件
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: '0644'
  notify: restart nginx
```

`tasks/service.yml`：

```yaml
---
- name: 启动并启用 NGINX 服务
  service:
    name: nginx
    state: started
    enabled: yes
```

`handlers/main.yml`：

```yaml
---
- name: restart nginx
  service:
    name: nginx
    state: restarted
```

### 4.4 项目入口与执行
`site.yml`：

```yaml
---
- hosts: web
  become: yes
  roles:
    - nginx_pro
```

**执行命令**：

```bash
ansible-playbook site.yml
```

**验证结果**：

```bash
curl http://<目标IP>:10086
# 输出：Welcome to nginx!
```

---

## 五、 Role 高级扩展与变量传递机制
### 5.1 多 Role 项目整合
```plain
项目根目录/
├── site.yml                     # 主入口
└── roles/
    ├── nginx_pro/               # NGINX 角色
    ├── mysql_pro/               # MySQL 角色
    └── redis_pro/               # Redis 角色
```

`site.yml`** 多角色调用**：

```yaml
---
- hosts: web
  roles:
    - nginx_pro
    - mysql_pro
    - redis_pro
```

### 5.2 变量注入扩展：第六种变量来源
| 变量来源 | 定义位置 | 优先级层级 |
| :--- | :--- | :--- |
| 命令行 `-e` | 运行时传入 | 最高 |
| Playbook `vars` | `site.yml` 中 `vars:` 区块 | 第2级 |
| `vars_files` | 外部 YAML 文件 | 第3级 |
| **Role **`vars/main.yml` | `roles/<role>/vars/main.yml` | **第4级（新增）** |
| **Role **`defaults/main.yml` | `roles/<role>/defaults/main.yml` | **第5级（新增，最低优先级）** |
| Inventory 变量 | `inventory` 文件 | 基础级 |


**示例**：

```yaml
# roles/nginx_pro/defaults/main.yml（默认值，易被覆盖）
nginx_port: 80
nginx_user: www-data

# roles/nginx_pro/vars/main.yml（项目专用，优先级较高）
nginx_port: 10086
nginx_user: nginx
```

### 5.3 进阶改造路径建议
| 阶段 | 改造内容 | 目的 |
| :--- | :--- | :--- |
| **第一阶段** | 完成基础 Role 结构拆分 | 实现功能模块化，验证可运行 |
| **第二阶段** | 添加 `tags` 标签 | 支持按场景选择性执行 |
| **第三阶段** | 添加 `when` 条件判断 | 处理多操作系统/多版本差异 |
| **第四阶段** | 完善 `notify` + `handlers` | 实现配置变更自动触发 |
| **第五阶段** | 引入变量层级（`defaults`/`vars`） | 支持多环境配置覆盖 |


**核心原则**：遵循"**先完成基础 Role 结构，再逐项增强**"的演进策略，避免一开始过度设计。

## 一、 课程定位与项目概述
### 1.1 课程定位
| 项目 | 说明 |
| :--- | :--- |
| **课程性质** | Ansible 系列课程的**最后一讲**，综合性最强 |
| **核心任务** | 将多个独立 Playbook 整合为**单一复杂 Role**，实现 LNMP + WordPress 全栈自动化部署 |
| **项目代号** | LNMP = Linux + Nginx + MySQL + PHP |


### 1.2 部署前置条件
| 前置项 | 要求 | 原因 |
| :--- | :--- | :--- |
| **环境清理** | 彻底清理目标主机上已存在的 Nginx/PHP/MySQL 环境 | 避免端口冲突、配置覆盖、版本不一致导致的异常 |
| **跨主机免密** | 配置 Web 节点与 DB 节点间的 SSH 免密登录 | 实现跨主机自动化操作（如远程执行 MySQL 初始化脚本） |
| **版本校验** | 目标主机 PHP 版本必须为 **8.3-fpm** | Role 中硬编码了该依赖，需提前确认 |


### 1.3 部署逻辑四阶段
```plain
第一阶段：软件安装
├── Nginx 安装
├── PHP 8.3-fpm 安装
└── MySQL 安装

第二阶段：数据库环境准备
├── 启动 MySQL 服务
├── 执行初始化脚本（建库、建用户、授权）
└── 验证 3306 端口监听

第三阶段：WordPress 配置
├── 下载 WordPress 源码包
├── 解压至 Web 根目录
├── 渲染 Nginx 虚拟主机配置（模板）
└── 修正文件权限归属

第四阶段：服务启动与验证
├── 批量启动 Nginx、PHP-FPM、MySQL
├── 访问 Web 安装向导完成配置
└── 发布测试文章验证全栈链路
```

---

## 二、 Role 目录结构与设计哲学
### 2.1 项目结构全景
```plain
lnmp-wordpress/
├── lnmp.yml                    # 主入口：部署 Nginx/PHP/WordPress
├── mysql.yml                   # 辅助入口：单独初始化 MySQL
├── inventory/                  # 主机清单目录
│   └── hosts
└── roles/
    ├── nginx/                  # Nginx Role
    │   └── tasks/
    │       ├── main.yml
    │       ├── group.yml
    │       ├── user.yml
    │       └── install.yml
    ├── php/                    # PHP Role
    │   └── tasks/
    │       ├── main.yml
    │       ├── group.yml
    │       ├── user.yml
    │       └── install.yml
    ├── mysql/                  # MySQL Role
    │   ├── tasks/
    │   │   ├── main.yml
    │   │   ├── group.yml
    │   │   ├── user.yml
    │   │   ├── install.yml
    │   │   └── init.yml
    │   └── files/
    │       └── mysql_init.sh   # 数据库初始化脚本
    ├── wordpress/              # WordPress Role
    │   ├── tasks/
    │   │   └── main.yml
    │   └── templates/
    │       └── domain.conf.j2  # Nginx 虚拟主机模板
    └── service/                # Service Role（服务批量管理）
        └── tasks/
            └── main.yml
```

### 2.2 单一职责设计哲学
| 原则 | 说明 | 示例 |
| :--- | :--- | :--- |
| **原子操作** | 每个 Role 仅执行**三项原子操作**：创建组 → 创建用户 → 安装软件 | `nginx/` 目录仅包含 `group.yml`、`user.yml`、`install.yml` |
| **功能解耦** | 杜绝模块间功能耦合，配置管理移交至独立 Role | Nginx Role 不包含配置部署，由 WordPress Role 接管 |
| **按需存在** | 仅当实际使用时才创建 `templates/` 或 `files/` 目录 | PHP Role 无配置文件，故无 `templates/` 目录 |


### 2.3 目录清理规范
```plain
清理原则：删除所有未被 main.yml 引用的 files/ 和 templates/ 子目录及文件

检查方法：
1. 查看各 Role 的 tasks/main.yml
2. 确认是否使用 template 或 copy 模块
3. 无引用则删除对应目录
```

---

## 三、 各组件 Role 实现细节
### 3.1 Nginx Role
```yaml
# roles/nginx/tasks/main.yml
---
- name: 创建 Nginx 系统用户组
  include_tasks: group.yml

- name: 创建 Nginx 系统用户
  include_tasks: user.yml

- name: 安装 Nginx 软件包
  include_tasks: install.yml
```

**关键说明**：

+ **不包含配置文件部署**：Nginx 虚拟主机配置完全交由 **WordPress Role** 通过 `template` 模块动态生成。
+ **无 **`templates/`** 或 **`files/`** 目录**：所有操作均通过包管理器完成。

### 3.2 PHP Role
```yaml
# roles/php/tasks/install.yml
---
- name: 安装 PHP 8.3-fpm 及相关扩展
  apt:
    name:
      - php8.3-fpm
      - php8.3-mysql
      - php8.3-curl
      - php8.3-gd
      - php8.3-mbstring
    state: present
    update_cache: yes
```

**版本硬编码警示**：

| 风险项 | 说明 | 规避方法 |
| :--- | :--- | :--- |
| **版本锁定** | 代码中硬编码 `php8.3-fpm` | 部署前需执行 `apt list --installed |
| **源依赖** | 需确保 APT 源包含 PHP 8.3 | 可预先添加 `ondrej/php` PPA |


### 3.3 MySQL Role
`tasks/init.yml`：

```yaml
---
- name: 执行 MySQL 初始化脚本
  script: mysql_init.sh
  args:
    executable: /bin/bash
  register: mysql_init_result
  changed_when: mysql_init_result.rc == 0
```

`files/mysql_init.sh`（数据库初始化脚本）：

```bash
#!/bin/bash
mysql -u root <<EOF
CREATE DATABASE IF NOT EXISTS wordpress;
CREATE USER IF NOT EXISTS 'wp_user'@'%' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;
EOF
```

**脚本存放要求**：

+ 必须**预先放入** `roles/mysql/files/` 目录。
+ Ansible 执行时自动上传至远程主机 `/tmp` 并执行。

### 3.4 WordPress Role
**核心任务序列**：

```yaml
# roles/wordpress/tasks/main.yml
---
- name: 下载 WordPress 最新中文版
  get_url:
    url: https://cn.wordpress.org/latest-zh_CN.tar.gz
    dest: /tmp/wordpress.tar.gz

- name: 解压 WordPress 至 Web 根目录
  unarchive:
    src: /tmp/wordpress.tar.gz
    dest: /var/www/html/
    remote_src: yes
    owner: www-data
    group: www-data

- name: 修正 WordPress 目录权限
  file:
    path: /var/www/html/wordpress
    owner: www-data
    group: www-data
    recurse: yes

- name: 部署 Nginx 虚拟主机配置
  template:
    src: domain.conf.j2
    dest: /etc/nginx/sites-available/wordpress.conf
  notify: restart nginx

- name: 启用虚拟主机站点
  file:
    src: /etc/nginx/sites-available/wordpress.conf
    dest: /etc/nginx/sites-enabled/wordpress.conf
    state: link
  notify: restart nginx
```

`templates/domain.conf.j2`（Nginx 虚拟主机模板）：

```nginx
server {
    listen 80;
    server_name {{ wp_domain }};
    root {{ wp_root }};

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass {{ php_fpm_socket }};
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

**模板变量说明**：

| 变量名 | 示例值 | 说明 |
| :--- | :--- | :--- |
| `{{ wp_domain }}` | `wordpress.example.com` | 网站域名 |
| `{{ wp_root }}` | `/var/www/html/wordpress` | Web 根目录路径 |
| `{{ php_fpm_socket }}` | `unix:/run/php/php8.3-fpm.sock` | PHP-FPM 套接字路径 |


### 3.5 Service Role
```yaml
# roles/service/tasks/main.yml
---
- name: 批量启动 LNMP 核心服务
  service:
    name: "{{ item }}"
    state: started
    enabled: yes
  with_items:
    - nginx
    - php8.3-fpm
    - mysql
```

---

## 四、 主 Playbook 与入口编排
### 4.1 双入口设计逻辑
| 入口文件 | 调用 Roles | 目的 | 执行时机 |
| :--- | :--- | :--- | :--- |
| `mysql.yml` | `mysql`, `service`（部分） | 单独部署并初始化 MySQL | **先执行**，确保数据库环境就绪 |
| `lnmp.yml` | `nginx`, `php`, `wordpress`, `service` | 部署 Web 层与 WordPress | **后执行**，依赖 MySQL 已就绪 |


`mysql.yml`** 示例**：

```yaml
---
- hosts: db_servers
  become: yes
  roles:
    - mysql
    - service
```

`lnmp.yml`** 示例**：

```yaml
---
- hosts: web_servers
  become: yes
  vars:
    wp_domain: "blog.ansible.top"
    wp_root: "/var/www/html/wordpress"
    php_fpm_socket: "unix:/run/php/php8.3-fpm.sock"
  roles:
    - nginx
    - php
    - wordpress
    - service
```

### 4.2 跨主机免密认证配置
```bash
# 在 Web 节点（13）上执行
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id root@10.0.0.19   # 复制公钥至 DB 节点（19）

# 验证免密登录
ssh root@10.0.0.19 "mysql -e 'SHOW DATABASES;'"
```

### 4.3 依赖关系说明
```plain
mysql.yml 执行完成 → MySQL 服务启动 + 数据库/用户创建完成
                              ↓
lnmp.yml 执行 → WordPress 安装向导可成功连接数据库
```

---

## 五、 部署验证与效果确认
### 5.1 端口监听检查
```bash
# Web 节点验证 Nginx
netstat -tnulp | grep :80
# 预期输出：nginx 进程监听 0.0.0.0:80

# DB 节点验证 MySQL
netstat -tnulp | grep :3306
# 预期输出：mysqld 进程监听 0.0.0.0:3306
```

### 5.2 WordPress 安装流程
| 步骤 | 操作 | 关键参数 |
| :--- | :--- | :--- |
| 1 | 访问 `http://10.0.0.16` | 自动跳转至安装向导 |
| 2 | 填写数据库连接信息 | 数据库名：`wordpress`   用户名：`wp_user`   密码：`123456`   主机：`10.0.0.19` |
| 3 | 填写站点信息 | 站点标题、管理员用户名/密码/邮箱 |
| 4 | 完成安装并登录后台 | 发布测试文章验证 |


### 5.3 最终效果验证
+ 成功发布测试文章：《ANSIBLE很好很好》
+ 前台页面正常显示文章内容
+ 后台管理功能正常
+ **结论**：LNMP + WordPress 全栈链路贯通，自动化部署成功。

---

## 六、 Ansible 大规模运维优化策略
### 6.1 小规模优化（<100台）
| 优化项 | 配置方式 | 效果 |
| :--- | :--- | :--- |
| **提高并发** | 修改 `ansible.cfg`：`forks = 20` | 同时操作更多主机，缩短总执行时间 |
| **禁用 Fact 收集** | Playbook 中设置 `gather_facts: false` | 跳过系统信息采集，速度提升 30%~50% |


### 6.2 中等规模优化（100~1000台）
| 优化项 | 配置方式 | 说明 |
| :--- | :--- | :--- |
| **启用 Fact 缓存** | `ansible.cfg` 配置：   `fact_caching = jsonfile`   `fact_caching_connection = /tmp/ansible_cache` | 首次收集后持久化，后续执行直接读取缓存 |
| **分批执行** | Playbook 中设置 `serial: 10` | 每次仅操作 10 台，避免控制节点过载 |


### 6.3 大规模优化（>5000台）
| 优化项 | 实现方式 | 适用场景 |
| :--- | :--- | :--- |
| **异步执行** | `async: 3600` + `poll: 0` | 长耗时任务（如大文件下载、数据库备份） |
| **迁移至专业平台** | AWX / Ansible Tower | 提供 Web UI、RBAC、调度、审计等企业级功能 |
| **错误处理强化** | `ignore_errors: true`   `failed_when: 自定义条件`   `until: 成功条件` + `retries: 5` | 容忍网络抖动、服务启动延迟等瞬时故障 |
| **日志持久化** | `ansible.cfg` 配置：`log_path = /var/log/ansible.log` | 关键任务日志强制留存，便于审计回溯 |


### 6.4 稳定性保障措施清单
+ **主机角色精细化**：区分 DB、Cache、Web、App 等角色，针对性配置。
+ **备份恢复策略**：关键文件（配置、数据库）操作前自动备份。
+ **超时与重试机制**：所有网络相关任务必须配置 `timeout` 和 `retries`。
+ **灰度发布**：结合 `serial` 和 `delegate_to` 实现分组滚动更新。

---

## 七、 核心方法论总结
### 7.1 两种 Role 设计思路对比
| 设计视角 | 组织方式 | 优点 | 缺点 |
| :--- | :--- | :--- | :--- |
| **操作逻辑视角** | 按软件栈分层（Nginx → PHP → MySQL → WP） | 结构清晰，易于理解 | 任务间存在隐式依赖，修改影响面大 |
| **业务逻辑视角** | 每个 Role 为独立业务单元（如“建库用户”、“部署SSL”） | 高度解耦，复用性强 | 需显式编排依赖，编排复杂度高 |


**实践建议**：两种思路可并存，小型项目优先操作逻辑视角，复杂项目可混合使用。

### 7.2 根本能力要求
```plain
Ansible 自动化能力 = 模块熟练度 × Linux 命令理解深度 × 持续练习

核心模块必会清单：
├── 包管理：apt / yum
├── 文件操作：copy / template / get_url / unarchive / file
├── 服务管理：service / systemd
├── 命令执行：command / shell / script
└── 流程控制：include_tasks / import_role / when / loop
```

**强调**：Playbook 编写能力建立在大量实践之上，"**多练**"为第一要务。

