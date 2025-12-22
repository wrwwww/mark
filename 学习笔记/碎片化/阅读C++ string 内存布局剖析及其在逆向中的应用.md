[文章来源](https://mp.weixin.qq.com/s/q--3iWKyCEETOUh0l4mGnA)

文章内容

# C++ string 内存布局剖析及其在逆向中的应用

原创 渣渣物语 渣渣物语 _2025年07月01日 07:31_ _北京_

## 背景

在 iOS 逆向过程中，我们通常面对的应用主要使用 Objective-C 或 Swift 开发。然而，有些 App 也会嵌入用 C++ 编写的模块或功能，用以处理性能敏感或要求跨平台兼容的部分。C++ 逆向相比 Objective-C 逆向，确实更具挑战性。其中，std::string 作为 C++ 中最基础且常用的数据结构之一，掌握其内部实现和内存布局是 C++ 逆向分析中的必备技能。

## 实验环境

- Apple clang version 15.0.0
- Target: arm64-apple-darwin23.4.0
- C++ version: 202002

## 实验 Demo

在 Xcode 中新建一个 macOS 命令行程序，开发语言选择 C++，main 文件代码如下：

```
#include <iostream>void test_string_func(std::string arg_1){    std::cout << "c_str value: " << arg_1.c_str() << std::endl;    std::cout << "sizeof value: " << sizeof(arg_1) << std::endl;    std::cout << "size value: " << arg_1.size() << std::endl;    std::cout << "capacity value: " << arg_1.capacity() << std::endl;    std::cout << std::endl;}int main(int argc, const char * argv[]) {    std::string str_empty;    test_string_func(str_empty);
    std::string str_short = "Hello World!";    test_string_func(str_short);
    std::string str_long(23, '=');    test_string_func(str_long);
    return 0;}
```

上述代码，在 main 函数中初始化了三个字符串对象 `str_empty`、`str_short`、`str_long`，`test_string_func` 方法中使用 `sizeof` 运算符和 `c_str`、`size`、`capacity` 方法来用来帮助我们理解 std::string 的内部表示：

- `c_str()`: 返回一个指向以空字符结尾的字符数组的指针，即字符串的内容。
- `sizeof(std::string)`: 返回 std::string 对象本身的大小（字节数），通常是一个固定值。
- `size()`: 返回字符串中字符的数量。
- `capacity()`: 返回字符串当前分配的存储空间大小（字节数），可能大于 size()。

  

运行上面代码，输出结果如下：

```
c_str value: sizeof value: 24size value: 0capacity value: 22
c_str value: Hello World!sizeof value: 24size value: 12capacity value: 22
c_str value: =======================sizeof value: 24size value: 23capacity value: 31
```

从输出结果，我们可以看出：

- 不论空字符串、短字符串，还是长字符串，sizeof 返回的值都为 24， 也就是说 std::string 对象在内存中的大小为 24 字节，与字符串长度无关。
- 对于空字符串/短字符串，容量是一个固定值，这里为 22，而对于长字符串，容量会动态调整以适应内容。（后面我们会结合 size、 capacity 的汇编实现进一步分析）

## std::string 内存布局分析

在 **libc++**（LLVM 的 C++ 标准库）中，`std::string` 的内存布局采用了高度优化的设计，其核心是 **短字符串优化（SSO）**。下面我们结合 lldb 调试及汇编分析其实现

定义下面三个函数，插入在 test_string_func 函数之前，

const char* __attribute__((__noinline__))test_cstr(const std::string& str){ return str.c_str();}size_t __attribute__((__noinline__)) test_size(std::string str){ return str.size();}size_t __attribute__((__noinline__)) test_capacity(std::string str){ return str.capacity();}

在 main 函数中添加调用方法，修改后的代码如下：

```
int main(int argc, const char * argv[]) {    // 前面的代码已忽略
    std::cout << test_cstr(str_short) << std::endl;    std::cout << test_cstr(str_long) << std::endl;    std::cout << test_size(str_short) << std::endl;    std::cout << test_size(str_long) << std::endl;
    std::cout << test_capacity(str_short) << std::endl;    std::cout << test_capacity(str_long) << std::endl;
    return 0;}
```

这里使用了 `__attribute__((__noinline__))``   `修饰函数，用于指示编译器不要内联这个函数，这样整个函数汇编代码更加简短，有助于我们分析 c_str 等函数的汇编实现。

在 Xcode 编译优化选项中选择了 `-Os``   `，也是为了便于分析汇编代码，这里是个人经验值。

在上述定义的三个方法入口处设置断点，Debug -> Debug Workflow 选项中选择 "Always Show Disassembly"，运行程序。

### 字符内容存储分析（c_str）

我们首先分析 `c_str``   `函数的实现，该函数接受一个 std::string 对象的常量引用，并返回其内部 C 风格字符串的指针，其汇编代码如下：

![](1751421086623-f6da0307-ea33-423f-bd02-bdb1c65e7ae6.png)

下面逐行解释汇编代码：

- 第一行：从 std::string 对象的偏移 0x17 处加载一个字节，符号扩展到 32 位寄存器 w8。x0 是输入参数 str 的地址。
- 第二行：从 std::string 对象的起始地址（x0）加载 64 位值到 x9
- 第三行：比较 w8 和 0
- 第四行: 若 lt（Less Than）条件成立（即 w8 < 0），则 x0 = x9，否则 x0 = x0.
- 最后一行，函数返回，返回值存储在 x0 寄存器中。

## std::string 对象在内存中的大小为 24 字节，这里我们分别打印输出 str_short 与 str_long 的内存数据。  

str_short ("Hello World!") 的内存数据

![](1751421086616-c872e75c-2d6c-47e9-ae35-2b4f4b07b96d.png)

str_long("=======================") 的内存数据

![](1751421086656-5af93b54-a926-4619-a354-b45a9f7d39a9.png)

结合汇编代码与字符串内存数据，可以得知:

- string 对象的 0x17(十进制 23)偏移处存储的数据用于识别字符串类型（短字符 OR 长字符串）。这里 str_short 与 str_long 偏移 23 字节读取到的数据分别为 0x0c(与字符串长度相同) 与 0x80。

- 0x0c 符号扩展 32 位后的值为 0x0000000c，大于 0，标识为短字符串。
- 0x80 符号扩展 32 位值为 0xffffff80，小于 0，说明为长字符串。

- 对于短字符串 0x0 偏移处存储字符串内容，长字符串对象前 8 字节存储内容为数据指针，指向分配在堆上的字符串内容。

### 字符串大小分析（size）

### 接下来我们通过 test_size 的汇编实现，来分析 size 在 string 对象中如何存储的。

![](1751421086808-8a349334-1bb8-4daf-a178-3c63cfe46de4.png)

- 第一行: 从 x0 寄存器指向的地址加上偏移量 0x17（23）处加载一个有符号字节到 w8 寄存器。
- 第二行: 测试 w8 寄存器的第 31 位（0x1f）是否为 1。如果是，则表示长字符串，跳转到 0x100003090。
- 第三行: 将 x8 寄存器的低 8 位与 #0xff 进行按位与操作，结果存入 x0。这里提取了 w8 的低 8 位作为字符串长度。（返回值）
- 第五行：从 x0 寄存器指向的地址加上偏移量 0x8 处加载一个 64 位值到 x0(返回值)

  

由以上代码逻辑，我们可以得知：

- 字符串对象偏移 0x17(23) 处的值(一个字节)，其最高位(31位)，用于标记字符串类型，若最高位为 1，则为长字符串；若为 0，则为短字符串。
- 对于短字符串对象，偏移 0x17(23) 处的值(一个字节)，同时记录字符串的长度。短字符串最大长度为 0x16(十六制 22)，因为字符串对象内存大小为 24，最后一个字节存储字符串长度，字符串结尾符 '\0' 占用一个字节。
- 对于长字符对象，字符串长度存储在 0x8 偏移处，大小为 8 个字节。

### 字符串容量分析(capacity)

最后我们看下 `test_capacity 方法的汇编实现`

![](1751421086763-d75e0f65-2097-4e97-8ce7-5f9f45c3a467.png)

这里的汇编代码，与 `test_size 方法的类似，没有特别复杂的指令，不再逐行解读，若有疑问，可使用 AI 工具辅助解读。从中我们可以得知：`

- 对于短字符串对象，其容量是固定值 0x16（十进制 22）
- 长字符串对象容量值，存储在 0x10 偏移处，大小为 8 字节。该值清除最高位减 1 即为长字符串的容量大小。例如上面 str_long 内存中偏移 0x10 读取到的值为 0x8000000000000020, 与 0x7fffffffffffffff 进行按位与运算，清除最高位，之后再减去 1，得到的值为 0x1f(31)，即字符串对象的容量。

### std::string 内存布局小结

- 字符串对象大小为 24 字节，字符串对象的 最后一个字节（0x17 偏移处)的最高位，用于标识字符串类型，若为 1，则为长字符串，若是 0，则为短字符串。
- 对于短字符串，字符内容存储在字符串对象的缓冲区内，从 0x0 偏移处开始存储，以 '\0' 字符结尾，其长度最大为 22；对于较长的长字符串，字符内容存储在动态分配的内存区域。字符串对象偏移 0x0 处存储的指针，指向动态分配的内存区域。
- 短字符大小，存储在字符串对象 0x17 偏移处（大小 1字节）；长字符串大小存储在 0x8 偏移处，大小 8 字节。
- 短字符串容量为固定值 22，长字符串容量存储在 0x10 偏移处，大小为 8 字节，清除最高位减 1 即为字符串容量。

### 可用以下结构用来表示字符串内存的结构

短字符串内存结构

struct { char data[23]; // 本地缓冲区 uint8_t size; // 低7位存长度，最高位=0（短字符串标志）} __short;

长字符串内存结构

struct { char* data; // 指向堆内存的指针，大小 8 字节 size_t size; // 字符串长度，8 字节 size_t cap; // 容量（含标志位），最高位为1} __long;

## 使用 Frida 打印 string 字符内容

### 在 Frida 中打印输出 ObjC 或 C 语言字符串是非常容易的事情，但对于 C++ std::string，Frida 并未提供专门的 API。基于上述分析，我们已经掌握了 C++ string 对象的内存布局，现在我们自己实现一个方法用于读取 std::string 字符内容，代码如下：

```
function readStdString(str) {    const isLong = Memory.readU8(str.add(23)) >> 7;    if(isLong){        return str.readPointer().readUtf8String();    } else {        return Memory.readUtf8String(str);    }}
```

是的，你说得对，也问得非常关键。

---

## ✅ `readPointer()` 会固定读取 8 字节，不是 24 字节

在 **Frida 的 API** 里：

```
Memory.readPointer(address)
```

的作用是：

**从给定的内存地址读取 8 个字节（在 64 位系统中），并把它当作指针（**`void*`**）返回。**

---

## 🔍 举例说明

假设你在 Frida 脚本中有：

```
var str = ptr(0x100012340);  // 这是某个 std::string 对象的地址
var ptr_to_data = str.readPointer();  // 等价于 Memory.readPointer(str)
```

这相当于：

```
void* ptr_to_data = *(void**)(0x100012340);  // 只读取前8字节内容
```

**不会**读取整个 `std::string` 的 24 字节结构。它只读取第一个字段（偏移 0 ~ 7）。

---

## 🧠 所以你要记住：

- `Memory.readPointer(address)` 永远只读取 **8 字节**（对应 64 位系统的 `void*` 指针大小）；
- 如果你想读取整个结构体（比如 std::string 的 24 字节），你可以自己用 `readByteArray(address, 24)`；
- Frida 提供很多精细的读取函数，比如：

- `readU8`, `readU16`, `readU32`, `readU64`
- `readByteArray(ptr, length)`
- `readUtf8String(ptr)`（读 C 字符串）

---

## 🚫 如果你对短字符串（SSO）使用 `.readPointer()`

那就会出错 —— 你读取的其实是字符内容的前 8 个字节，把它误当作地址用，会访问无效内存或乱码。

所以我们在处理 `std::string` 时必须 **先判断是否是长字符串**，才安全使用 `.readPointer()`。

---

## ✅ 小结

|   |   |   |
|---|---|---|
|函数|读取字节数|说明|
|`readPointer()`|8 字节|把前 8 字节当作指针返回|
|`readUtf8String()`|直到 \0|读取 C 风格字符串|
|`readByteArray(ptr, n)`|`n` 字节|自定义长度，常用于结构体读取|

---

如果你想读取整个 `std::string` 的内存内容，可以这样做：

```
var raw = Memory.readByteArray(str, 24);
// 然后手动分析每个字段
```

需要我写一个完整例子帮你封装这个分析器也可以，随时说！

已知某 App 有以下 ObjC 方法

```
+[NSString stringWithStdString:(const void *)arg1];
```

该方法接收一个 C++ std::string 对象转换为一个 ObjC NSString 对象。

下面我们用 Frida Hook 该方法，打印 std::string 字符内容，并与转换后的 NSString 结果对比验证。

Frida hook 代码如下：

Interceptor.attach( ObjC.classes.NSString['+ stringWithStdString:'].implementation, { onEnter: function(args) { // args[2] = const void* (指向 std::string 的指针) const str = readStdString(args[2]) console.log(`std::string pointer: ${args[2]}, string value: ${str}`); }, onLeave: function(retval) { const resultString = new ObjC.Object(retval); console.log(`return value: ${resultString}`); }});

运行 App，使用 Frida 将上述脚本注入到目标程序中，可以看到以下输出结果：

![](1751421168533-57579136-413a-484e-a3a1-558b725f2933.png)

从图中可以看出，我们使用 readStdString 函数读取的字符内容与函数返回结果是一致的。

## 总结

本文将通过一个精心设计的测试 Demo，使用 LLDB 调试器，结合汇编分析，深入剖析 std::string 对象在内存中的具体布局。在此基础上，我们还探讨了在动态插桩工具 Frida 中，有效获取并打印出 C++ 字符串的内容，为 C++ 逆向提供实用的技巧。

## 二、`std::string` 的基本内存布局与 SSO

- `std::string` 对象大小固定为 **24 字节**，这在 64 位系统中自然对齐，无需额外填充。
- 采用了 **短字符串优化（SSO）** 设计，区分“短字符串”和“长字符串”两种存储模式。
- 对象内存最后一个字节（偏移 23）用于存储：

- 最高位（bit7）作为 **字符串类型标志**，0 表示短字符串，1 表示长字符串。
- 其余低 7 位表示 **短字符串长度**。

### 短字符串模式（SSO）

- 字符内容直接存储在对象的前 23 字节（偏移 0 ~ 22）。
- 最大可存储 22 个字符（预留 1 字节给 `\0` 结尾符）。
- 长度信息存储在最后一个字节的低 7 位。

### 长字符串模式（Heap）

- 对象前 8 字节存指向堆分配内存的指针。
- 偏移 8 字节处存字符串长度（64位）。
- 偏移 16 字节处存容量信息（高位为 1，需清除后减 1 得到真实容量）。
- 最后一个字节最高位标志为 1，表明为长字符串。

---

## 三、核心内存访问逻辑解析

- `c_str()` 方法：

- 读取偏移 23 字节，判断最高位；
- 若为短字符串，直接返回对象内存地址；
- 若为长字符串，返回对象首 8 字节指针。

- `size()` 方法：

- 短字符串，读取偏移 23 字节低 7 位；
- 长字符串，从偏移 8 字节读取 64 位长度。

- `capacity()` 方法：

- 短字符串固定为 22；
- 长字符串读取偏移 16 字节，清除最高位后减 1。

---

## 四、为何是偏移 23 字节？（`str.add(23)`）

- 24 字节对象索引从 0 到 23；
- 偏移 23 字节即对象最后一个字节，存储类型标志 + 长度信息；
- Frida 中读取这个字节高位，用于判断字符串存储模式。

---

## 五、设计细节与底层逻辑

- 24 字节大小设计巧妙，符合 64 位架构的自然对齐（3 × 8 字节），避免额外填充，提升访问效率。
- 利用最后一个字节高位做标志，同时用低 7 位存长度，节省空间类似“协议栈字段复用”。
- 长字符串用堆指针 + 长度 + 容量分三部分存储，结构清晰且可扩展。
- 短字符串则直接在对象内部缓存，避免堆分配，提高小字符串性能。

---

## 六、实战中的应用：动态分析与逆向

- 结合汇编观察，可以看到 `c_str()` 和 `size()` 是如何根据标志位区分访问路径的。
- 利用 Frida 脚本实现读取 `std::string`：

```
function readStdString(str) {
  const isLong = Memory.readU8(str.add(23)) >> 7;
  if (isLong) {
    return str.readPointer().readUtf8String();
  } else {
    return Memory.readUtf8String(str);
  }
}
```

- 这种方法能精准判断字符串类型，并正确读取内容，有效辅助逆向调试和内存分析。

---

## 七、常见疑问解答

|   |   |
|---|---|
|疑问|说明|
|“高位是最后一位”对吗？|不对。高位指的是一个字节的最高比特位（bit7），位于字节的最左边，而非“最后一位”。|
|`str.add(23)`<br><br>为什么是23？|因为 24 字节对象索引是 0~23，最后一个字节存标志和长度，读取它能判断字符串模式。|
|64位系统为什么设计成24字节正好对齐？|24字节等于3个8字节单位，正好满足64位系统对齐需求，无需额外填充，提升访问效率。|
|设计思想和协议栈有什么类似？|复用字段（高位做标志，低位存长度），类似协议栈中的位级编码，节省空间且表达信息完整。|

---

## 八、总结

`std::string` 内存布局设计极具匠心，兼顾性能和空间利用：

- 通过 SSO 优化，显著减少短字符串堆分配开销；
- 通过 24 字节固定大小和位复用，实现高效存储和快速判断；
- 内存对齐严谨，适合64位架构访问特性；
- 结合动态分析工具，能高效逆向、调试和实用；

理解这套设计，有助于深入掌握 C++ 标准库实现、逆向分析技术以及底层性能优化思想。

## 防止函数内联优化方便函数调试

文章中提到使用 **套壳函数（wrapper functions）并加上** `__attribute__((__noinline__))`，其目的是：

**防止编译器对函数进行内联优化，从而使我们更容易通过汇编或调试器（如 LLDB）观察函数调用过程及底层行为。**

---

### 🔍 什么是套壳函数？

就是外面再“包一层”的简单函数，用来隔离目标调用。

例如，在文章中我们看到这样写：

```
const char* __attribute__((__noinline__)) test_cstr(const std::string& str) {
    return str.c_str();
}

size_t __attribute__((__noinline__)) test_size(std::string str) {
    return str.size();
}

size_t __attribute__((__noinline__)) test_capacity(std::string str) {
    return str.capacity();
}
```

---

### 🧠 为什么要这么做？

编译器在 Release 模式或启用优化（如 `-Os`, `-O2`, `-O3`）时，会将很多函数调用 **直接内联**，也就是说函数体会直接展开到调用处，这样会：

- **省去函数调用开销**；
- **导致函数地址不再单独存在**，无法在汇编中单独调试；
- **很难在调试器中设置函数断点**（断点位置会合并到调用者中）；

如果你直接写：

```
std::cout << str.c_str();
```

编译器可能直接把 `c_str()` 的汇编实现塞进这行语句对应的汇编里，调试器完全看不到它是怎么判断是长字符串还是短字符串的。

---

### ✅ `__attribute__((__noinline__))` 的作用

这个 GNU/Clang 扩展告诉编译器：

“请 **不要内联** 这个函数，即使你觉得内联会优化性能。”

这样，函数体就会变成独立的汇编块，可以：

- 设置断点；
- 精确观察调用指令；
- 分析参数传递、寄存器变化；
- 研究 `std::string` 的底层行为（如判断 SSO、读取长度、返回 c_str 指针等）；

---

### 📐 举例对比

#### 不用 noinline 的情况：

```
std::string s = "Hello";
auto p = s.c_str();
```

👉 编译器可能直接把 `c_str()` 的实现嵌入到 `main` 中，导致调试器中你 **找不到** `c_str()` **函数体**。

---

#### 使用 noinline：

```
const char* test_cstr(const std::string& str) __attribute__((__noinline__));
const char* test_cstr(const std::string& str) {
    return str.c_str();
}
```

👉 编译后你可以在汇编中看到 `test_cstr:` 函数体，观察它是怎么判断偏移 0x17 是否小于 0、读取 pointer 或 inline 数据等。

---

### 🔧 总结用途

|   |   |
|---|---|
|目的|解释|
|防止内联优化|保留函数独立体，便于汇编调试|
|配合汇编分析底层逻辑|清楚看到调用路径、跳转条件、判断逻辑|
|逆向必备技巧|无此操作很可能会迷失在优化后的混乱指令中|

---

## 课后学习调试rust vec

```
#[inline(never)]
fn inspect_string(s: &String) {
    println!("address of &String (s): {:p}", s);
    println!("data pointer: {:p}", s.as_ptr());
    println!("len: {}", s.len());
    println!("cap: {}", s.capacity());
}

fn main() {
    let s = String::from("Hello, Rustaceans!");
  // 24
    println!("size of String: {}", std::mem::size_of::<String>());
  // 24
    println!("size of usize: {}", std::mem::size_of::<usize>());
    inspect_string(&s);

    // 阻止程序立刻退出，方便调试查看内存
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap();
}
```

String类型是对Vec<u8>的封装

```
#[derive(PartialEq, PartialOrd, Eq, Ord)]
#[stable(feature = "rust1", since = "1.0.0")]
#[lang = "String"]
pub struct String {
    vec: Vec<u8>,
}
```

所以我们从查看String的内存布局到了查看Vec<u8>,String 类型的sizeof为24，所以我们在函数入口打断点，查看24字节。

![](1751440376548-27a1597b-0d05-40d1-a687-7501999588f4.png)

在调试控制台使用命令frame

```
// 查看变量		
frame variable s
// 输出其引用地址，可以看到和上面打印出来的string的地址是一致的
(alloc::string::String *) s = 0x00000041dbb3f858
// 查看这个地址指向的内容，查看24字节，可以看到字符串.as_ptr()，指向的就是下面第二个8字节中
// 而第一个和最后一个8字节是0x00000012等于10进制的18
x/3gx 0x00000041dbb3f858
0x41dbb3f858: 0x0000000000000012 0x000001ca7099cba0
0x41dbb3f868: 0x0000000000000012
```

### 使用frida读取rust std::string

1. 获取程序的基地址，`**const base = Module.findBaseAddress(Process.name);**`
2. 需要通过其他分析工具ida，获取程序中指定函数在应用程序地址中的偏移，然后使用基地址+偏移地址找到函数地址
3. 然后hook函数的进入和离开
4. 将函数的参数传入我们写的字符串解析函数

```
const base = Module.findBaseAddress(your_rust_binary.exe	);
const offset=0x000012;
const address=base.add(offset);
Interceptor.attach(addr, {
    onEnter(args) {
      readStdString(args)
    },
    onLeave(retval) {
        // todo
    }
});
function readStdString(str){	
   let  v=str.add(8).readPointer().readUtf8String();
  console.log("v")
}
```

又或者这个程序是我们自己编译的程序，我们可以在程序中添加打印函数，打印出来函数地址和字符串地址， Frida 附加进程后，可以直接使用这个地址进行 hook。 当然也需要注入这个程序才可以

```
println!("my_func addr = {:p}", my_func as *const ());


let s = String::from("Hello");
let p = s.as_ptr();
println!("{:p}", p);
```