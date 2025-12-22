## 概念

### 流程图（简略）：

```
 工作区（Working Directory） ---> 暂存区（Staging Area） ---> 本地仓库（Repository）
       编辑文件                     git add                    git commit

```

```
echo hello > a.txt      # 创建文件
git status              # Git 说这是个未跟踪（untracked）文件
git add a.txt           # 加入暂存区，准备提交
git commit -m "add a.txt"  # 正式提交，从这时起开始版本管理
```

所以真正的“开始管理”是 `git commit`，而不是 `git add`。

### 那什么时候才“开始管理”一个文件？

Git 会**从你首次提交它（**`git commit`**）的时候起开始正式版本管理它**。

---

## HEAD

### 命令介绍

在 Git 中，`HEAD` 是一个非常关键的**指针**，它指向你当前所在的提交（通常是某个分支的最新提交）。可以理解为：**你当前工作目录的“快照”版本**。

---

### HEAD 的类型

#### 1. 指向分支的 HEAD（正常情况）

这是最常见的情况，`HEAD` 指向当前分支的最新提交。

例如：

```
HEAD → main → 提交对象（commit）
```

#### 2. 游离的 HEAD（detached HEAD）

当你使用如下命令时：

```
git checkout <某个 commit-id>
```

此时你**不在任何分支上**，而是处于某个具体提交的状态，`HEAD` 指向这个提交对象，而不是分支。

```
HEAD → 提交对象（不是分支）
```

这种状态叫做“游离 HEAD（detached HEAD）”。

---

### 常见用法

```
git log HEAD                 # 查看当前 HEAD 所在的提交历史
git show HEAD                # 查看 HEAD 指向的提交详情
git checkout HEAD~1          # 切换到上一个提交
git reset --hard HEAD        # 丢弃当前更改并回退到 HEAD
git reflog                   # 查看 HEAD 的历史变动记录
git checkout c1^             # c1的父节点 相对定位
git checkout c1~1            # c1的父节点
```

---

### 示例说明

#### 查看 HEAD 当前指向

```
cat .git/HEAD
```

输出示例：

```
ref: refs/heads/main
```

表示 `HEAD` 当前指向的是 `main` 分支。

---

#### 游离 HEAD 示例

```
git checkout a1b2c3d
```

此时你会看到警告信息：

```
You are in 'detached HEAD' state.
```

意味着你现在不在任何分支上，只是查看某一次提交。

你可以继续查看、构建测试代码，但如果你此时做了提交，这些提交可能会“丢失”，除非你创建新分支：

```
git switch -c temp-test-branch
```

---

### 总结理解

|状态|HEAD 指向|是否在分支|
|---|---|---|
|正常工作状态|当前分支|是|
|检出指定提交|提交 ID|否（游离）|
|新建分支后恢复提交|新分支上的 HEAD|是|

---

如果你还想继续深入，比如 `HEAD` 与 `rebase/reset/checkout` 的关系、`.git/HEAD` 文件的格式解析、HEAD vs ORIG_HEAD vs FETCH_HEAD，我可以进一步展开。是否需要？

  

## 学习网站

[https://oschina.gitee.io/](https://oschina.gitee.io/)

---

## git init

### 命令介绍

用于初始化一个新的 Git 仓库，会创建 `.git` 隐藏目录，用于追踪版本信息。

### 常见用法

```
git init
```

### 示例说明

```
mkdir my-project
cd my-project
git init
```

这将在 `my-project` 目录下初始化一个空的 Git 仓库。

---

## git status

### 命令介绍

用于查看当前工作区与暂存区的状态，比如哪些文件被修改、哪些文件未跟踪等。

### 常见用法

```
git status
```

### 示例说明

```
git status
```

输出显示哪些文件被修改、哪些文件未被 `add`、哪些已经准备提交。

---

## git add

### 命令介绍

将工作区中的修改添加到暂存区，准备提交。是 `commit` 的前置操作。

### 常见用法

```
git add <文件名>      # 添加指定文件
git add .             # 添加当前目录下所有更改
git add -A            # 添加所有（包括删除）更改
git add -u            # 仅添加已追踪文件的更改
```

### 示例说明

```
git add index.html
git add .
git add -A
```

将文件或全部变更加入暂存区，准备下一步 `commit`。

---

## git commit

### 命令介绍

将暂存区的内容作为一次快照保存到本地版本历史中。每次提交都包含一个提交信息说明。

### 常见用法

```
git commit -m "说明"      # 常规提交
git commit -am "说明"     # 提交已追踪文件的修改（省略 add）
git commit --amend        # 修改上一条提交
```

### 示例说明

```
git commit -m "新增注册页面"
git commit -am "修复样式"
git commit --amend -m "修正提交说明"
```

---

## git log

### 命令介绍

查看提交历史，包括提交哈希、作者、时间、说明等。

### 常见用法

```
git log              # 完整日志
git log --oneline    # 一行显示每条记录
git log --graph      # 图形化显示提交历史
```

### 示例说明

```
git log
git log --oneline
git log --oneline --graph
```

查看最近的提交历史记录，便于回顾或选择版本。

---

## git diff

### 命令介绍

比较文件的差异，用于查看尚未提交的修改内容。

### 常见用法

```
git diff              # 查看工作区与暂存区差异
git diff --cached     # 查看暂存区与最近一次提交差异
git diff <文件名>     # 查看指定文件差异
```

### 示例说明

```
git diff
git diff --cached
git diff main.css
```

---

## git restore

### 命令介绍

撤销工作区或暂存区的变更，用于恢复文件。

### 常见用法

```
git restore <文件>              # 撤销工作区修改
git restore --staged <文件>     # 撤销 add（暂存区）
```

### 示例说明

```
git restore index.html
git restore --staged index.html
```

---

## git rm

### 命令介绍

删除文件，同时将删除操作加入暂存区（即提交时会删除该文件）。

### 常见用法

```
git rm <文件>              # 删除文件并提交变更
git rm --cached <文件>     # 停止跟踪文件，但保留文件本身
```

### 示例说明

```
git rm config.json
git rm --cached secret.txt
```

---

## git branch

### 命令介绍

查看、创建、删除本地分支。

### 常见用法

```
git branch               # 查看所有本地分支
git branch <分支名>      # 创建新分支
git branch -d <分支名>   # 删除本地分支
```

### 示例说明

```
git branch
git branch feature/login
git branch -d feature/old
```

---

## git checkout

### 命令介绍

切换到指定分支或恢复文件，也可用于创建新分支。

### 常见用法

```
git checkout <分支名>           # 切换分支
git checkout -b <分支名>        # 创建并切换新分支
git checkout <提交ID> -- <文件> # 恢复指定版本的文件
```

### 示例说明

```
git checkout main
git checkout -b feature/home
git checkout HEAD~1 -- index.html
```

---

## git switch

### 命令介绍

Git 推荐用于替代 `checkout` 的分支切换命令，更加语义清晰。

### 常见用法

```
git switch <分支名>        # 切换分支
git switch -c <分支名>     # 创建并切换分支
```

### 示例说明

```
git switch develop
git switch -c feature/user
```

---

## git merge

### 命令介绍

将另一个分支的更改合并到当前分支，生成一个新的合并提交。

### 常见用法

```
git merge <分支名>
```

### 示例说明

```
git checkout main
git merge feature/login
```

---

## git pull

### 命令介绍

从远程仓库拉取代码并与本地分支合并，等价于 `git fetch` + `git merge`。

### 常见用法

```
git pull
git pull origin <分支名>
```

### 示例说明

```
git pull
git pull origin develop
```

---

## git push

### 命令介绍

将本地分支的提交推送到远程仓库，通常用于分享代码或备份。

### 常见用法

```
git push
git push origin <分支名>
git push -u origin <分支名>   # 设置默认上游
```

### 示例说明

```
git push
git push origin main
git push -u origin feature/ui
```

---

## git clone

### 命令介绍

克隆一个远程 Git 仓库到本地，复制其所有历史和文件。

### 常见用法

```
git clone <仓库地址>
```

### 示例说明

```
git clone https://github.com/yourname/project.git
```

---

## git stash

### 命令介绍

临时保存当前工作区和暂存区的更改，让工作区恢复干净，稍后可以再恢复这些改动。

### 常见用法

```
git stash                   # 保存当前更改
git stash list              # 查看所有 stash
git stash apply             # 应用最近一次 stash，不删除记录
git stash pop               # 应用并删除最近的 stash
git stash drop              # 删除最近一次 stash
git stash clear             # 清空所有 stash
```

### 示例说明

```
git stash
git stash list
git stash apply
git stash pop
```

---

## git rebase

### 命令介绍

把一个分支的更改“重新应用”到另一个分支的基础上，通常用于整理提交历史，使其更线性。使得两个分支的功能看起来像是按顺序开发，但实际上它们是并行开发的

### 常见用法

```
git rebase <分支名>           # 当前分支变基到目标分支
git rebase -i HEAD~3          # 交互式 rebase，整理最近 3 次提交
git rebase --abort            # 取消 rebase 操作
git rebase --continue         # 解决冲突后继续 rebase
```

### 示例说明

```
git checkout feature
git rebase main
```

将 feature 分支的修改重新建立在 main 最新提交之上。

---

## git tag

### 命令介绍

创建标签用于标记某次特定的提交，常用于版本发布。

### 常见用法

```
git tag                     # 查看所有标签
git tag <标签名>            # 创建标签
git tag -a <标签名> -m "说明"  # 创建带注解的标签
git push origin <标签名>    # 推送单个标签
git push origin --tags      # 推送所有标签
```

### 示例说明

```
git tag v1.0
git tag -a v1.1 -m "版本发布 1.1"
git push origin v1.1
```

---

## git reset

### 命令介绍

用于重置当前 HEAD 到指定状态。可选择是否保留工作区或暂存区改动。

### 常见用法

```
git reset --soft HEAD~1     # 保留暂存区，撤销 commit
git reset --mixed HEAD~1    # 撤销 commit 和暂存（默认）
git reset --hard HEAD~1     # 完全回退，包括工作区
```

### 示例说明

```
git reset --soft HEAD~1
git reset --mixed HEAD~2
git reset --hard HEAD~1
```

---

### git revert

**背景：** 在多人协作的项目中，某次提交引入了错误。为了安全地撤销这个提交，避免破坏提交历史，需要使用 `git revert` 命令。

---

**步骤：**

1. 查看提交历史，找到需要撤销的提交 ID：

```
git log --oneline
```

2. 使用 `git revert` 生成一个新的提交，撤销指定的提交：

```
git revert <commit-id>
```

3. Git 会自动打开编辑器，让你确认或修改撤销提交的说明，保存退出后，新提交就完成了。

4. 推送变更到远程仓库：

```
git push origin <分支名>
```

---

**说明：**

- `git revert` 通过生成一个新的“反向提交”来撤销目标提交，**不会删除或改写历史**，保证版本库完整性。

- 新的提交会将内容更新到当前分支，恢复到撤销目标提交前的状态。

- 适合已经推送到远程的分支，能安全回滚错误改动，避免历史混乱。

  

---

## git cherry-pick

### 命令介绍

将其他分支中的某个提交“摘取”过来，复制到当前分支。

### 常见用法

```
git cherry-pick <commit-id>
```

### 示例说明

```
git cherry-pick 3a4f9d7
```

将该 commit 的内容应用到当前分支。

---

## git fetch

### 命令介绍

从远程仓库获取最新更新，但不自动合并。常与 `git merge` 或 `git rebase` 连用。

### 常见用法

```
git fetch                  # 拉取所有远程更新
git fetch origin           # 拉取 origin 的所有更新
git fetch origin main      # 拉取远程 main 分支
```

### 示例说明

```
git fetch
git fetch origin
```

---

## git remote

### 命令介绍

管理远程仓库的连接地址（如 GitHub、GitLab 等）。

### 常见用法

```
git remote -v                      # 查看远程地址
git remote add origin <url>       # 添加远程仓库
git remote remove origin          # 删除远程仓库
git remote set-url origin <url>   # 修改远程仓库地址
```

### 示例说明

```
git remote -v
git remote add origin https://github.com/user/repo.git
```

---

## git config

### 命令介绍

配置 Git 的行为，如用户名、邮箱、换行符等。

### 常见用法

```
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --list                 # 查看所有配置
```

### 示例说明

```
git config --global user.name "Alice"
git config --global user.email "alice@example.com"
git config --global user. email ' 2435960154@qq.com ' # 初始化邮箱
git config --global user. name 'wrw' # 初始化用户名
```

---

## 案例

### 克隆远程仓库并开始开发

**背景：** 新开发者加入项目，需要从 GitHub 上获取项目源码并切换到开发分支。

**步骤：**

1. 克隆仓库

```
git clone https://github.com/org/project.git
cd project
```

2. 创建新分支并切换

```
git switch -c feature/user-auth
```

3. 开发并提交

```
git add .
git commit -m "实现用户认证接口"
```

4. 推送到远程

```
git push -u origin feature/user-auth
```

  

---

### 合并功能分支到主分支并解决冲突

**背景：** 功能开发完成，需要合并到主分支，但期间主分支已更新，产生冲突。

**步骤：**

1. 切换主分支并拉取最新代码

```
git switch main
git pull
```

2. 合并分支

```
git merge feature/user-auth
```

3. 解决冲突（Git 会提示冲突文件）

- 编辑冲突文件

- 删除 `<<<<<<<`, `=======`, `>>>>>>>` 等标记

- 保留正确内容

4. 暂存并提交

```
git add .
git commit -m "合并用户认证功能"
```

  

---

### 使用 stash 保存临时修改并切换分支

**背景：** 正在开发某功能时，突然需要切换到其他分支处理紧急问题。

**步骤：**

1. 保存当前修改

```
git stash
```

2. 切换到其他分支

```
git switch fix/hot-bug
```

3. 处理并提交 bug 修复后，切换回来

```
git switch feature/user-auth
```

4. 恢复临时修改

```
git stash pop
```

  

---

### 发布稳定版本并打标签

**背景：** 当前主分支的代码已完成验收，需要发布为 `v1.0`。

**步骤：**

1. 切换并拉取主分支最新代码

```
git switch main
git pull
```

2. 创建标签

```
git tag -a v1.0 -m "版本 1.0 发布"
```

3. 推送标签

```
git push origin v1.0
```

  

---

### 回退错误提交

**背景：** 某次提交引入了严重问题，需要撤回。

#### 方法一：用 `git revert` 撤销（推荐）

```
git revert <commit-id>
```

这会生成一个新的提交用于“抵消”旧的提交，不破坏历史。

#### 方法二：用 `git reset` 回退（危险）

```
git reset --hard HEAD~1
```

这个操作会强制回退，请谨慎使用，尤其是多人协作时。

---

### 交互式压缩多次提交

**背景：** 本地提交太多不合理的历史，想在 push 之前合并为一个干净的提交记录。

**步骤：**

1. 执行交互式 rebase（如合并最近 3 次提交）

```
git rebase -i HEAD~3
```

2. 把后两条提交前的 `pick` 改为 `squash` 或 `s`

```
pick a1b2c3 修复样式
squash d4e5f6 调整颜色
squash f7g8h9 统一命名
```

3. 修改合并后的提交说明，保存退出

  

---

### 撤销变更

git reset 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

虽然在你的本地分支中使用 git reset 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的哦！

为了撤销更改并分享给别人，我们需要使用 git revert。应该就是创建一个新的提交,使其可以到撤销之前

```
本地
git reset c1
远程
git revert c1
```

---

### 多分支协作

#### 新建分支

```
# 新建test分支
git branch test
# 切换分支
git checkout test
```

#### 修改内容

```
# test分支添加变量a
fn main (){
    let a=10;
}
```

```
# master分支添加变量b
fn main (){
    let b=110;
}
```

#### 提交分支

```
# 切换分支
git checkout test;
# 提交
git commit -a -m "添加变量a"
git checkout master;
git commit -a -m "添加变量b";
```

#### 合并两个分支

```
git checkout master;
git merge test;
```

这个时候会出现冲突,可以手动修改文件解决冲突

```
fn main (){
<<<test
    let a=10
>>> master
    let b=110;
}
// 冲突解决后
fn main (){
 
    let a=10
    let b=110;
}
```

解决冲突后提交

```
git commit -a -m "合并冲突"
git checkout test
git merga master
```

#### 删除分支

```
git checkout master;
git branch -d test;
```

---

### 问答

#### 如果我head到某次提交，然后修改了文件，add 后commit 之后会发生什么？

1. 你当前在某分支（比如 `main`），然后执行：

```
git checkout HEAD^
```

或者

```
git switch --detach HEAD^
```

这会把 `HEAD` 指向**上一个提交**（`HEAD^`），进入游离 HEAD 状态（detached HEAD）。

2. 你修改了文件，执行了：

```
git add <文件>
git commit -m "提交说明"
```

##### 1. 你处于游离 HEAD 状态

- `HEAD` 不再指向任何分支，而是指向某个具体提交（上一个提交）。

- 此时你提交会生成一个新的提交，但这个提交没有分支指向它。

##### 2. 你的提交会产生“悬空”的新提交

- 这个提交存在，但没有分支名称引用它。

- 如果你没有创建新分支来保存它，这个提交**很可能会被 Git 垃圾回收**（丢失）。

##### 3. 你修改并提交后，HEAD 会自动指向这个新提交

- 但仍是游离 HEAD，没有分支指向。

如果你想保留这个提交，最好创建新分支：

```
git switch -c new-branch
```

这样新提交就在 `new-branch` 分支下，不会丢失。

##### 总结

|操作|结果|
|---|---|
|`git checkout HEAD^`|进入游离 HEAD 状态，指向上一个提交|
|修改、`git add`、`git commit`|生成悬空的新提交，HEAD 指向新提交|
|不创建分支|新提交易丢失|
|创建分支保存新提交|提交被安全保存|

### 接着如果不想要原来的分支，如何将这个 变主分支？

好的！你现在是在游离 HEAD 状态，做了新的提交，想**用这条新的提交替代原来的主分支（比如** `main`**）**，让主分支指向这个新提交，也就是“把主分支切换到这个新提交上”。

---

#### 如何操作？

##### 1. 确认当前 HEAD 指向你想要的提交（游离 HEAD 状态下）

```
git log -1
```

确认当前提交是你新提交的那个。

---

##### 2. 强制让主分支指向当前提交

```
git branch -f main HEAD
```

这条命令会把 `main` 分支指针强制移动到当前 `HEAD` 所指的提交。

---

##### 3. 切换回主分支

```
git switch main
```

现在你的主分支 `main` 就是基于新提交的最新状态了。

---

##### 4. （可选）推送到远程仓库（如果需要覆盖远程）

```
git push -f origin main
```

**注意：** 强制推送会覆盖远程的主分支，请确保你确认覆盖是安全且需要的。

  

##### “无引用”的提交变成了“悬空提交”

- 当你用 `git branch -f main HEAD` 强制移动了 `main` 分支指针，原来 `main` 指向的旧提交就**没有任何分支引用**了。

- 这些“无引用”的提交变成了“悬空提交”（dangling commits）。

- Git 不会立刻删除它们，而是会在一段时间内（默认 90 天）保留，方便误操作恢复。

- 这段时间过后，Git 的垃圾回收机制（`git gc`）会自动清理这些悬空提交，彻底删除它们。

---

#### 总结

|命令|作用|
|---|---|
|`git branch -f main HEAD`|强制将 main 分支指向当前 HEAD|
|`git switch main`|切回 main 分支|
|`git push -f origin main`|强制推送覆盖远程 main 分支|

---

## github

GitHub Actions 工作流是一种自动化工具，可以帮助你为 GitHub 仓库创建 CI/CD（持续集成和持续交付）流水线。你可以通过它自动构建、测试和部署项目，提升开发效率并减少人工操作。以下是一个简要的 GitHub Actions 工作流概述：

### GitHub Actions 工作流的组成部分：

1. **工作流（Workflow）**  
    工作流是由多个任务（Job）组成的自动化过程，通常位于 `.github/workflows/` 目录下的 YAML 文件中。每个工作流文件可以包含一个或多个作业。

![](lake%20import/学习笔记/Attachments/1745896812119-867780d0-b7fe-4229-b40f-2f1f623910b2.png "null")

2. **作业（Job）**  
    作业是一组运行在同一环境中的步骤。一个作业可以运行在不同的操作系统环境（例如 Ubuntu、Windows 或 macOS）中。

3. **步骤（Step）**  
    步骤是工作流中的一个任务，可以是运行命令、调用 Actions 或执行脚本。

4. **事件（Event）**  
    工作流的触发器，可以是某些操作的发生，比如 `push`、`pull_request`，或者时间触发。

5. **Action**  
    GitHub 提供了一些官方或社区贡献的预构建 Actions，这些可以帮助你简化常见任务，比如部署、构建和测试。

### 触发工作流程

工作流程触发器是导致工作流程运行的事件。 这些事件可以是：

- 工作流程存储库中发生的事件

- 在 GitHub 外部发生并在 GitHub 上触发 `repository_dispatch` 事件的事件

- 预定时间

- 手动

例如，您可以将工作流程配置为在推送到存储库的默认分支、创建发行版或打开议题时运行。

有关详细信息，请参阅“[触发工作流程](https://docs.github.com/zh/actions/using-workflows/triggering-a-workflow)”；有关事件的完整列表，请参阅“[触发工作流的事件](https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows)”。

### 工作流语法

工作流是使用 YAML 定义的。 有关用于创作工作流的 YAML 语法的完整参考，请参阅“[GitHub Actions 的工作流语法](https://docs.github.com/zh/actions/using-workflows/workflow-syntax-for-github-actions#about-yaml-syntax-for-workflows)”。

有关管理工作流运行（例如重新运行、取消或删除工作流运行）的详细信息，请参阅“[管理工作流运行和部署](https://docs.github.com/zh/actions/managing-workflow-runs)”。

必须将工作流文件存储在存储库的 `.github/workflows` 目录中。

#### name

工作流的名称。 GitHub 在存储库的“操作”选项卡下显示工作流的名称。如果省略 `name`，GitHub 会显示相对于存储库根目录的工作流文件路径。

#### run-name

run-name 是在 工作流运行列表（也就是 GitHub 仓库的 Actions 页签里）显示的名字。

如果你在工作流 YAML 文件里设置了 run-name，那工作流运行时就用你指定的名字。

如果你不设置 run-name，或者它的内容是空格，GitHub 会自动根据触发事件的信息来生成一个名字。比如：

如果是 push 触发的，就用推送的 提交信息。

如果是 pull_request 触发的，就用 PR 的 标题。

#### on

若要自动触发工作流，请使用 `on` 定义哪些事件可以触发工作流运行。 有关可用事件的列表，请参阅“[触发工作流的事件](https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows)”。

可以定义单个或多个可以触发工作流的事件，或设置时间计划。 还可以将工作流的执行限制为仅针对特定文件、标记或分支更改。 后续部分将介绍这些选项。

#### [使用单个事件](https://docs.github.com/zh/actions/writing-workflows/workflow-syntax-for-github-actions#%E4%BD%BF%E7%94%A8%E5%8D%95%E4%B8%AA%E4%BA%8B%E4%BB%B6)

例如，当推送到工作流存储库中的任何分支时，将运行具有以下 `on` 值的工作流：

```
on: push
```

#### [使用多个事件](https://docs.github.com/zh/actions/writing-workflows/workflow-syntax-for-github-actions#%E4%BD%BF%E7%94%A8%E5%A4%9A%E4%B8%AA%E4%BA%8B%E4%BB%B6)

可以指定单个事件或多个事件。 例如，当推送到存储库中的任何分支或有人创建存储库的分支时，将运行具有以下 `on` 值的工作流：

```
on: [push, fork]
```

如果指定多个事件，仅需发生其中一个事件就可触发工作流。 如果触发工作流的多个事件同时发生，将触发多个工作流运行。

#### jobs

工作流运行由一个或多个 `jobs` 组成，默认情况下并行运行。 若要按顺序运行作业，可以使用 `jobs.<job_id>.needs` 关键字定义对其他作业的依赖关系。

每个作业在 `runs-on` 指定的运行器环境中运行。

在工作流程的使用限制之内可运行无限数量的作业。 有关详细信息，请参阅针对 GitHub 托管运行程序的“[使用限制、计费和管理](https://docs.github.com/zh/actions/learn-github-actions/usage-limits-billing-and-administration)”和针对自托管运行程序使用限制的“[自托管运行器的使用限制](https://docs.github.com/zh/actions/hosting-your-own-runners/managing-self-hosted-runners/usage-limits-for-self-hosted-runners)”。

jobs.id

使用 `jobs.<job_id>` 为作业提供唯一标识符。 键 `job_id` 是一个字符串，其值是作业配置数据的映射。 必须将 `<job_id>` 替换为对于 `jobs` 对象的唯一字符串。 `<job_id>` 必须以字母或 `_` 开头，并且只能包含字母数字字符、`-` 或 `_`。

##### [示例：创建作业](https://docs.github.com/zh/actions/writing-workflows/workflow-syntax-for-github-actions#%E7%A4%BA%E4%BE%8B%E5%88%9B%E5%BB%BA%E4%BD%9C%E4%B8%9A)

在此示例中，已创建两个作业，其 `job_id` 值为 `my_first_job` 和 `my_second_job`。使用 `jobs.<job_id>.name` 设置作业名称，该名称显示在 GitHub UI 中。

```
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

##### job.needs

使用 `jobs.<job_id>.needs` 标识运行此作业之前必须成功完成的所有作业。 它可以是一个字符串，也可以是字符串数组。 如果某个作业失败或跳过，则所有需要它的作业都会被跳过，除非这些作业使用让该作业继续的条件表达式。 如果运行包含一系列相互需要的作业，则故障或跳过将从故障点或跳过点开始，应用于依赖项链中的所有作业。 如果希望某个作业在其依赖的作业未成功时也能运行，请在 `jobs.<job_id>.if` 中使用 `always()` 条件表达式。

##### [示例：要求成功的依赖项作业](https://docs.github.com/zh/actions/writing-workflows/workflow-syntax-for-github-actions#%E7%A4%BA%E4%BE%8B%E8%A6%81%E6%B1%82%E6%88%90%E5%8A%9F%E7%9A%84%E4%BE%9D%E8%B5%96%E9%A1%B9%E4%BD%9C%E4%B8%9A)

job2必须在job1结束才运行，而job3是必须要job1和job2都成功执行，job4不是必须要job1和job2成功

```
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
  job4:
  if: ${{ always() }}
  needs: [job1, job2]
```

### 简单的工作流示例

以下是一个 GitHub Actions 工作流的例子，它将在每次推送代码到 `main` 分支时运行，进行构建和测试。

```
name: CI Pipeline
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

### 解析

- `name`：工作流的名称。

- `on`：指定工作流的触发事件，此处为推送到 `main` 分支。

- `jobs`：定义了一个作业，名为 `build`，该作业会在 `ubuntu-latest` 的环境中运行。

- `steps`：该作业包括多个步骤，如代码检出、设置 Node.js 环境、安装依赖和运行测试。

### 常见的 GitHub Actions 用例

- 自动化构建和测试（例如，Node.js、Java、Python 项目）。

- 自动部署应用到云平台（如 AWS、Heroku、Vercel）。

- 代码质量检查和报告（如 Lint、Test Coverage）。

- 定时任务（使用 `schedule` 事件定期执行工作流）。

GitHub Actions 可以帮助你轻松地自动化开发流程，提高团队的协作效率。你可以根据需要自定义工作流，满足不同的需求。如果你有特定的工作流场景，欢迎告诉我，我可以帮你设计一个更具体的配置。

```
name: Rust CI and Release

on:
  push:
    branches: [ "package" ]
  pull_request:
    branches: [ "package" ]
  # 新增：打 tag 时触发发布
  push:
    tags:
      - 'v*.*.*'  # 匹配 v1.0.0 这样的语义化版本标签

env:
  CARGO_TERM_COLOR: always

jobs:
  build-and-test:
    strategy:
      matrix:
        platform: [x86_64-pc-windows-msvc, x86_64-unknown-linux-gnu]
    runs-on: ${{ matrix.platform }}
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Install Rust
      uses: actions-rs/toolchain@v1
      with:
        toolchain: stable
        components: rustfmt, clippy
        profile: minimal
        override: true
    
    - name: Build
      run: cargo build --verbose
    
    - name: Run tests
      run: cargo test --verbose

    # 将构建产物上传为工作流制品
    - name: Upload artifacts
      uses: actions/upload-artifact@v3
      with:
        name: binaries-${{ matrix.platform }}
        path: |
          target/debug/your-program-name*
          target/debug/deps/your-program-name*

  # 新增发布任务（仅在打 tag 时运行）
  release:
    needs: build-and-test  # 依赖构建任务
    if: startsWith(github.ref, 'refs/tags/')  # 仅在 tag 推送时运行
    runs-on: ubuntu-latest
    
    steps:
    - name: Download all artifacts
      uses: actions/download-artifact@v3
      with:
        path: artifacts

    - name: Create Release
      uses: softprops/action-gh-release@v1
      with:
        # 从 tag 名提取版本号
        tag_name: ${{ github.ref }}
        name: Release ${{ github.ref_name }}
        body: |
          Auto-generated release for ${{ github.ref_name }}
          ### 构建包含以下平台的二进制文件：
          - Windows (x86_64)
          - Linux (x86_64)
        draft: false
        prerelease: false
        files: |
          artifacts/binaries-x86_64-pc-windows-msvc/*
          artifacts/binaries-x86_64-unknown-linux-gnu/*
```

根据您提供的GitHub Actions工作流配置学习需求，以下是基于搜索结果的CICD配置核心参数解析与编写指南（相关引用来源已标注）：

---

### **一、YAML文件基础结构**

```
name: CI Pipeline Name  # 工作流名称，标识用途
on:                     # 触发条件定义
  push:                 # 代码推送事件
    tags:               # 标签触发
      - 'v*'            # 以v开头的标签触发（如v1.0）
  pull_request:         # PR合并触发
    branches: [ main ]  # 指定分支
  schedule:             # 定时触发
    - cron: '0 0 * * *' # 每天UTC 0点执行

jobs:                   # 任务集合
  build:                # 任务名称
    runs-on: ubuntu-latest  # 运行环境（Windows/macOS可选）
    steps:              # 执行步骤
    - name: Checkout code
      uses: actions/checkout@v4  # 拉取代码（[[1]](https://blog.csdn.net/FNAspectOL/article/details/113914816)）
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 18.x
    - run: npm install && npm build  # 直接执行命令
```

（来源：[[33]](https://segmentfault.com/a/1190000044967110?sort=votes)）

---

### **二、关键配置模块详解**

#### 1. **触发器（Triggers）**

|触发条件|配置示例|适用场景|
|---|---|---|
|标签推送|`tags: ['v*']`|版本发布自动化（[[3]](https://cloud.tencent.com/developer/article/1939118)）|
|分支推送|`branches: [main, dev]`|日常开发测试|
|定时任务|`schedule: cron: '0 0 * * *'`|定期构建或测试（[[39]](https://segmentfault.com/a/1190000011881435)）|
|手动触发|`workflow_dispatch:`|紧急修复或特殊场景|

#### 2. **环境与依赖管理**

- **运行环境选择**

```
runs-on: [windows-latest, ubuntu-22.04, macos-13]  
```

跨平台构建需配置多环境矩阵（[[36]](https://www.cnblogs.com/huangshiyi/p/17045583.html)）。

- **工具链安装**

```
- uses: actions/setup-python@v4
  with:
    python-version: '3.10'
- uses: dtolnay/rust-toolchain@stable  # Rust工具链
```

- **缓存加速**

```
- uses: actions/cache@v3
  with:
    path: |
      ~/.cargo
      node_modules
    key: ${{ runner.os }}-build-${{ hashFiles('**/Cargo.lock') }}
```

缓存依赖目录显著提升构建速度（[[8]](https://blog.csdn.net/hahahhahahahha123456/article/details/103126368)）。

---

#### 3. **构建与发布步骤**

```
steps:
  - name: Compile Release
    run: cargo build --release  # 编译Release版本
  - name: Create Artifacts
    run: |
      mkdir release
      cp target/release/app.exe release/
  - name: Upload Release
    uses: softprops/action-gh-release@v1
    with:
      files: release/app.exe
      token: ${{ secrets.GITHUB_TOKEN }}
```

（示例来源：[[1]](https://blog.csdn.net/FNAspectOL/article/details/113914816)）

---

### **三、扩展功能配置**

#### 1. **多阶段流水线（Stages）**

```
jobs:
  build:
    stage: build  # 阶段名称
    steps: [编译步骤]
  test:
    needs: build  # 依赖前置任务
    stage: test
    steps: [测试步骤]
  deploy:
    needs: test
    stage: deploy
    steps: [部署步骤]
```

（引用：[[37]](https://cakepanit.com/forward/ff7280bd.html)）

#### 2. **安全与权限**

- **敏感信息处理**  
    使用 `secrets` 存储密钥：

```
env:
  AWS_ACCESS_KEY: ${{ secrets.AWS_KEY }}
```

#### 3. **通知集成**

```
- name: Slack Notification
  uses: rtCamp/action-slack-notify@v2
  with:
    channel: '#ci-alerts'
    status: ${{ job.status }}
```

---

### **四、最佳实践建议**

1. **模块化复用**  
    通过 `workflow_call` 事件拆分通用步骤，减少重复代码（[[33]](https://segmentfault.com/a/1190000044967110?sort=votes)）。

2. **错误处理**  
    使用 `continue-on-error: true` 允许非关键步骤失败（[[36]](https://www.cnblogs.com/huangshiyi/p/17045583.html)）。

3. **日志与调试**  
    启用 `ACTIONS_STEP_DEBUG: true` 环境变量输出详细日志。

---

### **完整配置模板参考**

```
name: Rust Build & Release

on:
  push:
    tags: ['v*']

jobs:
  build:
    runs-on: windows-latest
    steps:
    - uses: actions/checkout@v4
    - uses: dtolnay/rust-toolchain@stable
    - uses: actions/cache@v3
      with:
        path: |
          ~/.cargo/registry
          target
        key: ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
    - run: cargo build --release
    - run: zip -r release.zip target/release/
    - uses: softprops/action-gh-release@v1
      with:
        files: release.zip
```

如需更详细的参数说明，可参考 [GitHub Actions官方文档](https://docs.github.com/actions)。

当然可以。下面是一份整理清晰、分条归类的《GitHub Actions 配置错误总结与解决方案文档》，适合你日后参考或团队分享复用：

---

# 🧩 GitHub Actions 常见错误总结与解决方案（以 Windows Rust 构建为例）

## 🔧 1. 文件路径错误（Pattern not match）

**错误提示：**

```
Pattern 'installer\tiger_minesweeper-windows-x64-setup.exe' does not match any files.
```

**原因：**

- 使用了 Windows 风格路径 `\`，但 YAML 中需要统一使用 `/`

- 编译/拷贝阶段文件没有真正生成，导致找不到

**解决方案：**

- 路径统一使用 `/`：  
    ✅ `installer/tiger_minesweeper-windows-x64-setup.exe`

- 添加调试命令确认文件存在：

```
- run: dir installer
```

---

## 🔐 2. GitHub Token 权限不足（403）

**错误提示：**

```
"message": "Resource not accessible by integration"
```

**原因：**

- 默认的 `${{ secrets.GITHUB_TOKEN }}` 没有足够的权限（尤其是在 fork 项目或权限收紧仓库中）

**解决方案：**

🔹 **方法一（推荐）**：启用默认 token 写权限  
进入仓库设置：

```
Settings → Actions → General → Workflow permissions
✅ 勾选 “Read and write permissions”
```

🔹 **方法二**：使用自定义 Personal Access Token（PAT）

1. GitHub → Settings → Developer Settings → Tokens → Generate token (classic)

2. 勾选 `repo` 权限 → 创建

3. 存入仓库 Secrets，例如命名为：`GH_PAT`

4. 修改 workflow：

```
env:
  GITHUB_TOKEN: ${{ secrets.GH_PAT }}
```

---

## 📦 3. Release 文件未生成

**常见现象：**

- 构建成功，但上传 Release 的文件匹配失败

- Release 自动创建了，但没有附带任何文件

**可能原因：**

- 构建或 copy 阶段失败（如路径错、权限错）

- 目录未创建、命令没有递归 copy 文件夹

**推荐验证：**

- 加 `dir` 命令确认阶段输出：

```
- run: dir release
- run: dir installer
```

---

## 🏗️ 4. 分支和 tag 不同步（tag 悬空）

**现象：**

- tag 推上去了，触发了 CI/CD

- 但 GitHub 仓库代码页面显示的不是你刚修改的内容

**原因：**

- 你只推了 tag，没有 push commit 到主分支

- 导致主分支和 tag 断裂，可能会混淆其他开发者

**解决方案：**  
推 tag 前先推主分支或使用 `--follow-tags`：

```
git push origin main
git push origin v1.2.3
# 或一次性推送
git push origin main --follow-tags
```

---

## 🚧 5. `makensis` 编译失败（图标或路径问题）

**错误示例：**

```
Error while loading icon from "assets\branding\bevy_bird_dark.png": invalid icon file
```

**常见原因：**

- 使用了 `.png` 作为图标，但 NSIS 仅支持 `.ico`

- 使用错误路径或资源缺失

**解决方案：**

- 将 `.png` 图标转换为 `.ico`（可用工具如 IcoConvert.com）

- 路径用双反斜线或统一用 `/`，并确保资源存在

---

## 🧠 6. Rust 构建缓存未命中

**原因：**

- 缓存 key 改变（比如 `Cargo.lock` 改动）

- 不同架构（如 `x86_64-pc-windows-msvc` vs `wasm32`）未做独立缓存

- 默认目录不匹配（如用不同 `CARGO_HOME`）

**推荐写法：**

```
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: |
      ~/.cargo/registry
      ~/.cargo/git
      target
    key: ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
    restore-keys: |
      ${{ runner.os }}-cargo-
```

---

## ✅ 7. 使用 `actions/checkout` 默认行为说明

默认会根据触发事件（如 tag push）**checkout 当前 tag 指向的 commit**：

```
- uses: actions/checkout@v4
```

如果你想切换回某个分支，需要显式指定：

```
- uses: actions/checkout@v4
  with:
    ref: main
```

---

# ✅ 建议的发布流程（脚本 + tag）

1. 修改代码 → commit

2. 修改 `Cargo.toml` 版本

3. 打 tag（如 `v1.2.3`）

4. 推送主分支和 tag：

```
git push origin main
git push origin v1.2.3
```

5. GitHub Actions 触发 → 构建 → 上传 Release

---

## Android build

```
# This is a basic workflow to help you get started with Actions

name: push_and_release

# Controls when the workflow will run
on:
  # git tags v1.0.1
  # git push v1.0.1
  push:
    tags:
      - "v*"


# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      #  拉取版本
      - uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: 17
          
      - name: Decode keystore
        run: |
          echo "${{ secrets.SIGNING_KEYSTORE_BASE64 }}" | base64 -d > keystore.jks

      - name: Make gradlew executable
        run: chmod +x ./gradlew

      - name: Build signed release APK
        env:
          CI: true
          SIGNING_KEY_ALIAS: ${{ secrets.SIGNING_KEY_ALIAS }}
          SIGNING_KEY_PASSWORD: ${{ secrets.SIGNING_KEY_PASSWORD }}
          SIGNING_STORE_PASSWORD: ${{ secrets.SIGNING_STORE_PASSWORD }}
        run: ./gradlew assembleRelease


      - name: Create Release
        uses: softprops/action-gh-release@v2
        with:
          files: |
            app/build/outputs/apk/release/app-release.apk
          generate_release_notes: true
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### github 环境变量

`${{ secrets.SIGNING_KEYSTORE_BASE64 }}`通过这种语法去获取github的环境变量，

在github中配置环境变量的值 setting->Actions secrets and variables->

![](lake%20import/学习笔记/Attachments/1750837737325-47c6f944-a072-46b8-8d02-58d3acd72370.png "null")

![](lake%20import/学习笔记/Attachments/1750837788062-6b654426-d290-444d-a914-223bae01a811.png "null")

![](lake%20import/学习笔记/Attachments/1750837837598-5acc2157-c440-422e-a2df-89953e0a055b.png "null")

在build.gradle.kts中配置签名

```
...
android {
    ...
    defaultConfig {...}
    signingConfigs {
        create("release") {
            storeFile = file("myreleasekey.keystore")
            storePassword = "password"
            keyAlias = "MyReleaseKey"
            keyPassword = "password"
        }
    }
    buildTypes {
        getByName("release") {
            ...
            signingConfig = signingConfigs.getByName("release")
        }
    }
}
```

- 每个 buildType（如 `release`、`debug`）可以指定一个 `signingConfig`

- `signingConfig` 决定了是否签名、如何签名

- 如果 `signingConfig` 是空的，Gradle 就**不会签名该 APK**

`secrets.SIGNING_KEYSTORE_BASE64` 是你**在 GitHub 仓库的 Secrets 中设置的一个值**，它包含的是你 Android 签名文件 `.jks` 的 **base64 编码内容**，用于在 GitHub Actions 中还原出原始的 `.jks` 文件以进行 APK 签名。

---

## ✅ 为什么要用 base64？

因为 GitHub Secrets 不支持直接上传二进制文件，所以我们把 `.jks` 签名文件编码成文本上传，然后在 Actions 中解码还原。

---

## ✅ 操作步骤详解

### 第一步：生成 keystore（如果还没有）

```
keytool -genkey -v -keystore my-release-key.jks \
  -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
```

---

### 第二步：将 `.jks` 文件 base64 编码

#### Linux/macOS：

```
base64 my-release-key.jks > my-release-key.jks.base64
```

#### Windows (PowerShell)：

```
[Convert]::ToBase64String([IO.File]::ReadAllBytes("my-release-key.jks")) > my-release-key.jks.base64
```

然后打开生成的 `.base64` 文件，把里面的内容复制。

---

### 第三步：添加到 GitHub Secrets

进入你的 GitHub 仓库：

1. 打开仓库页面

2. 点击右上角 ➜ **Settings**

3. 左侧菜单点击 ➜ **Secrets and variables → Actions**

4. 点击 ➜ **New repository secret**

5. 填写：

- Name: `SIGNING_KEYSTORE_BASE64`

- Value: 粘贴刚刚复制的 base64 内容（可能会很长）

- 点击 **Add secret**

---

## ✅ 在 GitHub Actions 中如何用？

工作流里这段代码：

```
- name: Decode keystore
  run: echo "${{ secrets.SIGNING_KEYSTORE_BASE64 }}" | base64 -d > keystore.jks
```

它会把 secret 里的 base64 内容解码，并生成一个 `keystore.jks` 文件，供后续 Gradle 使用进行签名。

---

## ✅ 补充：还有这些 Secrets 一起设置

|Secret 名|说明|
|---|---|
|`SIGNING_KEYSTORE_BASE64`|base64 编码的 `.jks` 文件|
|`SIGNING_KEY_ALIAS`|`keytool` 里设置的 alias|
|`SIGNING_KEY_PASSWORD`|生成密钥的密码|
|`SIGNING_STORE_PASSWORD`|keystore 的密码（通常相同）|

---

如果你希望我帮你检查生成的 `jks` 或 `base64` 是否正确，也可以将命令输出贴过来我协助确认（不包括私密内容）。