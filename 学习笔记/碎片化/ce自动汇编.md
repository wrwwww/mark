## 1️⃣ 基本结构

CE 的自动汇编脚本基本分两部分：

```
[ENABLE]
// 脚本启用时执行的汇编

[DISABLE]
// 脚本禁用时执行的汇编
```

常见用途：分配内存、注入代码、恢复原始代码、注册符号供 CE 使用。

---

## 2️⃣ 核心指令

### 🟢 alloc

分配一块可读写可执行的内存，一般用于放自定义代码或保存变量。

```
alloc(newmem,2048,"模块名"+偏移)
```

- `newmem` 是自定义的标签名。
- `2048` 是大小（字节数）。
- `"模块名"+偏移` 可选，表示把内存分配在接近该模块的区域，方便用短跳转。

---

### 🟢 registersymbol / unregistersymbol

注册一个符号名，让 CE 可以在地址列表或其他脚本里直接用这个名字引用。

```
registersymbol(myPtr)
unregistersymbol(myPtr)
```

注册后，你可以在 CE 里直接写 `myPtr`、`[myPtr]`、`[[myPtr]]` 访问。

---

### 🟢 label

定义标签，用于跳转或占位。

```
label(returnhere)
label(exit)
label(originalcode)
```

---

### 🟢 jmp / call / nop

- `jmp` 跳转
- `call` 调用
- `nop` 占位，不执行任何操作（用于填充原始代码剩余的空间）

---

### 🟢 db / dw / dd / dq

直接写数据到内存：

```
db 90 90 90         // 写3个字节: 90 90 90 (nop)
dw 1234             // 写2字节: 34 12 (小端)
dd 12345678         // 写4字节: 78 56 34 12
dq 123456789ABCDEF0 // 写8字节
```

---

### 🟢 aobscan / aobscanmodule

用于 AOB 扫描，找到某段指令的地址，避免硬编码偏移。

```
aobscanmodule(myAOB,GameAssembly.dll,48 8B ?? ?? ?? ?? ??)
registersymbol(myAOB)
```

之后就能用 `myAOB` 代替具体地址。

---

### 🟢 自定义变量

用 `alloc` 分配变量后就可以像普通内存一样读写。

```
alloc(myPtr,8)
mov [myPtr],rax
mov rbx,[myPtr]
```

---

## 3️⃣ 标准脚本模板

**最常用的注入模板：**

```
[ENABLE]

alloc(newmem,2048,"模块名"+偏移)
label(returnhere)
label(originalcode)
label(exit)

newmem:
  // 自定义代码
  originalcode:
  // 原始指令
  exit:
  jmp returnhere

"模块名"+偏移:
jmp newmem
nop 3      // 补齐剩余字节，避免破坏后续指令
returnhere:

[DISABLE]

"模块名"+偏移:
db 原始字节 // 恢复原始指令

dealloc(newmem)
```

---

## 4️⃣ 实战技巧

1. **确认覆盖字节数**

- 在 CE 反汇编窗口右键 → “自动汇编” → “模板 → AOB 注入”
- CE 会自动计算需要覆盖的指令长度，保证完整性。

2. **保存寄存器值**

- 用于传值到 CE 表：

```
alloc(myPtr,8)
mov [myPtr],rbx
```

在 CE 表里用 `[[myPtr]]` 访问。

3. **条件执行**

- 用 `cmp` + `jne` / `je` 来筛选执行时机。

```
cmp dword ptr [rbx],#100
jne skip
mov [myPtr],rbx
skip:
```

4. **多次 hook 时防止冲突**

- 每个 alloc 的名字要唯一。
- 可以用 `aobscanmodule` 找唯一特征码，不要写死偏移。

---

## 5️⃣ 学习路径推荐

- **第一步：用 CE 自带模板学习**

- CE 反汇编 → 右键 → 模板 → AOB 注入
- CE 自动生成的脚本非常规范，照着改就行。

- **第二步：学汇编基础**

- 熟悉 `mov / cmp / jmp / push / pop / call / ret`
- 了解寄存器：rax, rbx, rcx, rdx, rsi, rdi, rsp, rbp

- **第三步：写自己的注入逻辑**

- 试着保存基址、自动改数值、条件修改。

- **第四步：AOB 扫描 + 动态指针**

- 学会不用固定偏移，靠特征码定位。
- 学会把地址保存到 alloc 出来的变量，供表格引用。

---

要不要我帮你整理一份 **CE 自动汇编速查表**（所有常用指令和写法一页搞定），方便你随时对照？这样写脚本就不用每次查文档了。

你这段自动汇编已经非常接近正确写法了，只需要注意两点小问题就能更健壮：

---

### 1. `**nop 5**` **的长度要匹配被覆盖指令的字节数**

- `jmp newmem` 本身占用 5 字节，所以 CE 会用 `jmp` 覆盖掉原始地址处的几条指令，覆盖的总长度必须 ≥ 5 字节。
- 如果你手动写 `nop 5`，一定要确保原始地址处被覆盖的总字节数正好等于 `5+5=10`，否则会出现指令错位。
- 正确做法：让 CE 帮你自动生成模板，它会自动判断需要 nop 几个字节补齐，不建议随便写死 `nop 5`。

---

### 2. **[DISABLE] 部分恢复原始字节要完全一致**

- 你写的恢复指令 `db 48 8B 00` 是 `mov rax,[rax]`，但如果 CE 在启用时覆盖了比 3 字节更多的指令（比如 6 字节），那禁用时就只恢复了 3 字节，会导致原始代码不完整。
- 更安全的做法：把 CE 生成的 `[DISABLE]` 模板里的原始字节全部复制过来恢复。

---

### 改进后的版本（推荐写法）

```
[ENABLE]

alloc(newmem,2048,"PlayerData:get_instance"+12)
alloc(newPtr,8)
registersymbol(newPtr)

label(returnhere)
label(originalcode)

newmem:
    mov [newPtr],rax      // 保存 rax (PlayerData 实例地址)
originalcode:
    mov rax,[rax]         // 原始指令
    jmp returnhere

"PlayerData:get_instance"+12:
jmp newmem
nop 3                    // 用 CE 自动生成的 nop 数量，匹配覆盖的字节数
returnhere:

[DISABLE]

"PlayerData:get_instance"+12:
db 48 8B 00 90 90 90 90  // 恢复完整原始字节 (CE 自动生成时复制过来)

unregistersymbol(newPtr)
dealloc(newPtr)
dealloc(newmem)
```

**重点：**

- nop 数量和 db 恢复字节必须和 CE 自动生成的一模一样。
- 如果你手动改错 nop 数量或者恢复字节，就可能导致崩溃。

---

### 验证方法

1. 用 CE **右键 → Auto Assemble → Template → Code injection** 自动生成模板。
2. 看 CE 给出的 `jmp newmem` 后补了几个 nop，恢复了哪些原始字节。
3. 把你的代码插进去，保持其他部分不动。

这样几乎不会出错。

---

要不要我帮你一步一步演示 **如何用 CE 自动生成模板并确认 nop 数量和恢复字节**？这样你以后自己改脚本就不会踩坑了。

对！你完全理解对了 🎯

总结一下你刚说的，其实就是 **标准的代码注入流程**，CE 的自动汇编语法就是帮你简化了这个过程：

---

### 🔄 整个执行流的逻辑

1. **原始地址处**：  
    CE 把原来的几条指令覆盖掉 → `jmp newmem`（5 字节）  
    如果覆盖的字节数 > 5，就用 `nop` 把剩下的字节补齐，避免残缺机器码。
2. **newmem 段**：

- 先写你自己的代码（比如保存寄存器、计算、修改值等）。
- 再补回原来被覆盖掉的指令（否则程序逻辑会出错）。
- 最后 `jmp returnhere` 跳回原始流程继续执行。

3. **returnhere 标签**：  
    指向原始代码里被你覆盖的最后一条指令的下一条指令，保证能继续往下跑。

---

### 🔑 关键点

- **nop 的作用**：只负责占位，避免执行残缺指令，不会影响程序逻辑。
- **原始代码一定要补回**：否则等于把原程序的逻辑吃掉了，可能导致游戏异常。
- **跳转必须对齐**：jmp 占 5 字节，覆盖必须 >= 5 字节，否则指令残缺也会崩溃。
- **禁用脚本时要恢复完整原始字节**：不然程序下次跑到这里就会直接跳飞。

---

你现在已经抓到核心思想了 💡  
以后要做的就是：**确定 hook 点** → **申请内存** → **写新逻辑+原始逻辑** → **跳回去**，其他的就是细节问题（nop 补齐和字节恢复）。

---

要不要我帮你画一张示意图，把“原始地址 → newmem → returnhere”的执行路径画出来，直观看一遍？这样以后记忆更牢。