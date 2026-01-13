## 安装

### win下安装

#### 安装c++编译器

1. 下载[microsoft C++生产工具](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)
2. 勾选并下载指定工具![](学习笔记/Attachments/1725345791717-ca491e50-7dd0-408b-875c-67bf4ea26f87.png)

#### 安装rust

[官网下载](https://www.rust-lang.org/learn/get-started)

启动安装，执行默认安装就好

## 基本

### 注释

注释是给程序员看的，而不是给电脑看的。写注释是为了帮助别人理解你的代码。 这也有利于帮助你以后理解你的代码。 (很多人写了很好的代码，但后来却忘记了他们为什么要写它。)在Rust中写注释，你通常使用 `//`．

```
fn main() {
    // Rust programs start with fn main()
    // You put the code inside a block. It starts with { and ends with }
    let some_number = 100; // We can write as much as we want here and the compiler won't look at it
}
```

当你这样做时，编译器不会看`//`右边的任何东西。

还有一种注释，你用`/*`开始写，`*/`结束写。这个写在你的代码中间很有用。

```
fn main() {
    let some_number/*: i16*/ = 100;
}
```

对编译器来说，`let some_number/*: i16*/ = 100;`看起来像`let some_number = 100;`。

`/* */`形式对于超过一行的非常长的注释也很有用。在这个例子中，你可以看到你需要为每一行写`//`。但是如果您输入 `/*`，它不会停止，直到您用 `*/` 完成它。

```
fn main() {
    let some_number = 100; /* Let me tell you
    a little about this number.
    It's 100, which is my favourite number.
    It's called some_number but actually I think that... */

    let some_number = 100; // Let me tell you
    // a little about this number.
    // It's 100, which is my favourite number.
    // It's called some_number but actually I think that...
}
```

  

## 关于打印的更多信息

有时你有太多的 `"` 和转义字符，并希望 Rust 忽略所有的字符。要做到这一点，您可以在开头添加 `r#`，在结尾添加 `#`。

你可以打印二进制、十六进制和八进制。

```
fn main() {
    let number = 555;
    println!("Binary: {:b}, hexadecimal: {:x}, octal: {:o}", number, number, number);
}
```

这将打印出`Binary: 1000101011, hexadecimal: 22b, octal: 1053`。

## cargo feature

```
[package]
name = "tiger_minesweeper"
version = "1.0.7"
edition = "2024"

[features]
# 开发时使用的特性，启用动态链接以加快编译速度
dev-deps = ["bevy/dynamic_linking"]
# 默认特性为空，生产环境不使用动态链
default = []

[dependencies]
bevy = "0.16.1"
rand = "0.9.1"
thiserror = "2.0.12"
 
```

该程序定义了两个features，当我们启用features dev-deps的时候，就会开启bevy下的dynamic_linking feature，否则默认不启用feature

```
cargo run --features dev-deps
```

注意不要和 `[dev-dependencies]` 搞混，`[features]` 里的 `dev-deps` 是你自定义的一个 **feature 名**，名字可以随便取；而 `[dev-dependencies]` 是 cargo 特有的开发依赖。
## rustfmt

### rustfmt.toml



## cargo

### 相关链接

[the cargo book](https://doc.rust-lang.org/cargo/index.html)

### 换源

[相关链接](https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/)

在.cargo目录下新建文件`config.toml`

```config
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "https://mirrors.ustc.edu.cn/crates.io-index"
```



## 内存

### 字节序

对于数据 0x12345678，大地址存高字节的数据为小端,小地址存高字节数据为大端

小端存储后：0x78563412 大端存储后：0x12345678

> 查看字节序

```rust
// 2147483647,1,-2147483648,3
let nums=vec![i32::MAX,1,i32::MIN,3];
```

内存图

地址从左往右增加

![image-20231014213626444](C:\Users\wrwww\AppData\Roaming\Typora\typora-user-images\image-20231014213626444.png)

2147483647=0x7f_ff_ff_ff

### 补码,原码,反码

对于rust中i32类型的-2147483647在内存中表示为

![image-20231014214723167](C:\Users\wrwww\AppData\Roaming\Typora\typora-user-images\image-20231014214723167.png)

32类型的2147483647在内存中的二进制表示为**01111111111111111111111111111111**。这是因为i32类型使用二进制补码表示法，对于正数，其原码、补码和反码都是相同的。所以2147483647的二进制原码为01111111111111111111111111111111，由于它是正数，所以它的补码和反码也是01111111111111111111111111111111。

那么-2147483647在内存中就表示为2147483647的补码

> 负数在内存中的表示=|负数|(补码)

**是的，负数在内存中的表示通常是通过补码的形式来存储的**。具体来说，负数的补码是其绝对值转换为二进制后，按位取反再加1得到的结果。因此，负数在内存中的表示形式就是其绝对值的补码。这样做的好处是可以将加法和减法操作统一为加法操作，简化了计算机的运算过程。同时，使用补码也表示了正数和负数在内存中的对称性，便于进行溢出检测和其他操作。

### 字节对齐

i32 =4字节 在内存中就是4字节对齐

Rust中对于i32、u8、i8这些类型，它们在内存中的大小就是它们对应的字节数，并且也是按照相应的字节数进行对齐的**。具体来说，i32类型在Rust中占用4个字节，u8和i8类型各占用1个字节。内存对齐是为了提高内存访问的效率，确保数据在内存中的存储布局与硬件的访问方式相匹配，从而避免额外的性能损失。因此，Rust在内存布局上遵循了对齐规则，使得这些类型的变量在内存中的存储大小与它们的字节数一致，并按照相应的字节数进行对齐。

Rust对于复杂的数据结构通常会使用最长的类型来做数据对齐，以确保内存访问的效率和数据的正确性。这种做法是为了满足内存对齐的规则，使得数据结构的存储布局与硬件的访问方式相匹配，提高性能并减少出错的可能性。具体来说，当一个数据结构包含多种不同类型的字段时，Rust会按照其中最长的字段类型来决定整个数据结构的对齐方式。这样做可以确保数据结构中的每个字段都能以合适的对齐方式进行访问，避免了因内存访问不对齐而导致的性能问题或错误。

## 宏

### 相关链接

[宏文档](https://zjp-cn.github.io/tlborm/introduction.html)

### 概念

分为 **声明**（*Declarative*）宏(`macro_rules!`)和三种过程(*Procedural*)宏.

> 三种过程宏

- 自定义 `#[derive]` 宏在结构体和枚举上指定通过 `derive` 属性添加的代码
- 类属性（Attribute-like）宏定义可用于任意项的自定义属性
- 类函数宏看起来像函数不过作用于作为参数传递的 token

### 使用 `macro_rules!` 的声明宏用于通用元编程

> 实现一个简易的`vec[]!`宏

```rust
// 调用vec![]
let arr=vec![1,2,3];

#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

` $( $x:expr ),* `中* 表示`$(),` 零个或者多个,`,` 表示多个表达式之间使用`,`分割. 类似于正则表达式

### 使用自定义`derive`宏生成代码

> 实现类似添加`#[derive(Default)]`实现给指定类型添加方法

在给类型Xx添加属性#[derive(Default)]后,该类型会生成并添加default方法

```rust
#[derive(Default)]
pub struct Xx;

fn main() {
    let xx = Xx::default();
}
```

> 自定义trait

声明一个名为`MyTrait`的trait并添加名为`print_name`的方法

```rust
pub trait MyTrait{
    fn print_name();
}
```

> 新建自定义宏的crate包

```shell
cargo new my_macro --lib
```

在`Cargo.toml`中开启`proc-macro`和添加依赖文件

```toml
[lib]
proc-macro = true

[dependencies]
syn = "2.0.38"
quote = "1.0.33"
```

在原来的crate中的toml中引入`my_macro`

```toml
[dependencies]
hello_macro={path="../my_macro/"}
```

> 在my_macro中实现自定义PrintName的derive宏

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn;

#[proc_macro_derive(PrintName)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let gen = quote! {
        impl MyTrait for #name {
            fn print_name() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    gen.into()
}
```

`#[proc_macro_derive(PrintName)]` 中`PrintName`为自定义宏的名字,被该注解标注的函数参数为`TokenStream`,输出也为`TokenStream`,我们通过crate`syn`解析`TokenStream`为`ast`(抽象语法树),然后调用函数`impl_hello_macro`操作语法树为标注derive()macro 的类型实现MyTrait的trait ,并返回`TokenStream`

> 测试`PrintName`宏

```rust
use my_macro::PrintName;
pub trait MyTrait {
    fn print_name(); 
}
#[derive(PrintName,Default)]
pub struct Xx;

fn main() {
    let xx = Xx::default();
    Xx::print_name();.// 输出Hello, Macro! My name is Xx!    
}
```

### 类属性宏

```rust
#[bar]
fn foo(){}
```

类属性宏不同于derive宏,`derive` 只能用于结构体和枚举；属性还可以用于其它的项，比如函数。

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    todo!()
}
```

使用`#[proc_macro_attribute]`创建类属性宏,其中被标注的函数名为类属性宏的名字,其中第一个`TokenStream`为宏中传递的参数,第二个`TokenStream`为被`router`宏所标注的内容的`TokenStream`

### 类函数宏

类似于`macro_rules!`定义的声明宏一样的调用方法

```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

不同的是我们不像`macro_rules!`那样写宏,而是同其他过程宏一样对`TokenStream`操作

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    todo!()
}
```

## mini-redis

### 项目介绍

mini-redis 是一个使用 Rust Tokio 框架构建的 Redis 不完整的实现，包括服务器和客户端。

### 定义可执行文件

分别定义两个可执行文件,一个客户端一个服务端

```toml
[[bin]]
name = "server"
path = "src/bin/server.rs"
[[bin]]
name = "cli"
path = "src/bin/cli.rs"
```

> 执行指定的可执行文件

```bash
cargo run --bin server/cli -- args
```

### 库

```toml
# 异步运行时
tokio = { version = "1.28.2", features = ["full"] }
# 命令行解析程序
clap = { version = "4.3.4", features = ["derive"] }
# 高效的缓冲区结构
bytes = "1.5.0"
# 用于直接从ASCII（[u8]）解析整数，而无需先将它们编码为utf8
atoi = "2.0.0"
```

### redis协议

这里只实现了redis2的协议

| data type     | Category  | First byte | ps                                   |
| ------------- | --------- | ---------- | ------------------------------------ |
| Simple string | simple    | +          | +OK\r\n                              |
| Simple error  | simple    | -          | -Error message\r\n                   |
| Integers      | simple    | :          | :0\r\n                               |
| Bulk string   | Aggregate | $          | $5\r\nhello\r\n                      |
| arrays        | Aggregate | *          | *2\r\n$5\r\nhello\r\n$5\r\nworld\r\n |

[官方文档](https://redis.io/docs/reference/protocol-spec)

### serde
## 14.2 变体属性

### `#[serde(rename = "name")]`

如上

使用给定名称而不是 Rust 名称序列化和反序列化此变体。

允许为序列化与反序列化指定独立的名称：

- `#[serde(rename(serialize = "ser_name"))]`
    
- `#[serde(rename(deserialize = "de_name"))]`
    
- `#[serde(rename(serialize = "ser_name", deserialize = "de_name"))]`
    

### `#[serde(alias = "name")]`

`#[serde(alias = "name")]` 是一个属性，用于在序列化和反序列化时指定字段的别名。它可以用于将 Rust 结构体字段与 JSON 键或其他数据格式中的不同名称进行映射。

`#[derive(Debug, Serialize, Deserialize)]   #[serde(rename_all = "camelCase")]   struct Person2 {       #[serde(alias = "name")]       full_name: String,       age: u32,   }      fn alias() {       let person = Person2 {           full_name: "John Doe".to_owned(),           age: 30,       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);          let json = r#"{           "name": "Jane Smith",           "age": 25       }"#;          let deserialized_person: Person2 = serde_json::from_str(json).unwrap();       println!("{:?}", deserialized_person);   }      {"fullName":"John Doe","age":30}   Person2 { full_name: "Jane Smith", age: 25 }   `

在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `full_name` 和 `age` 字段。在 `full_name` 字段上，我们使用 `#[serde(alias = "name")]` 属性指定了一个别名为 "name"。这意味着在序列化和反序列化时，我们可以使用 "name" 作为键来表示 `full_name` 字段。

### `#[serde(rename_all = "...")]`

根据给定的大小写约定重命名此结构体变体的所有字段。可能的值为`"lowercase"`, `"UPPERCASE"`, `"PascalCase"`, `"camelCase"`, `"snake_case"`, `"SCREAMING_SNAKE_CASE"`, `"kebab-case"`, `"SCREAMING-KEBAB-CASE"`。

允许为序列化与反序列化指定独立的情况：

- `#[serde(rename_all(serialize = "..."))]`
    
- `#[serde(rename_all(deserialize = "..."))]`
    
- `#[serde(rename_all(serialize = "...", deserialize = "..."))]`
    

### `#[serde(skip)]`

切勿序列化或反序列化此变体。

`#[serde(skip)]` 是一个属性，用于在序列化和反序列化时跳过特定的字段。通过使用 `#[serde(skip)]` 属性，您可以指示 Serde 库在处理序列化和反序列化时忽略该字段。

以下是一个示例：

`use serde::Deserialize;   use serde::Serialize;      #[derive(Debug, Serialize, Deserialize)]   struct Person {       name: String,       age: u32,       #[serde(skip)]       secret_info: String,   }      fn main() {       let person = Person {           name: "John Doe".to_owned(),           age: 30,           secret_info: "This is a secret".to_owned(),       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);   }   {"name":"John Doe","age":30}   `

在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `name`、`age` 和 `secret_info` 字段。在 `secret_info` 字段上，我们使用 `#[serde(skip)]` 属性来指示 Serde 库在序列化和反序列化时跳过该字段。

在 `main` 函数中，我们创建了一个 `Person` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(skip)]` 属性，所以在序列化过程中，`secret_info` 字段将被忽略，不会出现在生成的 JSON 字符串中。

通过使用 `#[serde(skip)]` 属性，您可以选择性地排除某些字段，以便在序列化和反序列化过程中忽略它们。这对于保护敏感信息或排除不必要的字段很有用。

### `#[serde(skip_serializing)]`

`#[serde(skip_serializing)]` 是一个属性，用于在序列化时跳过特定的字段。通过使用 `#[serde(skip_serializing)]` 属性，您可以指示 Serde 库在处理序列化时忽略该字段，但在反序列化时仍会使用该字段。

以下是一个示例：

`use serde::Deserialize;   use serde::Serialize;      #[derive(Debug, Serialize, Deserialize)]   struct Person {       name: String,       age: u32,       #[serde(skip_serializing)]       secret_info: String,   }      fn main() {       let person = Person {           name: "John Doe".to_owned(),           age: 30,           secret_info: "This is a secret".to_owned(),       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);          let json = r#"{           "name": "Jane Smith",           "age": 25,           "secret_info": "This is a secret"       }"#;          let deserialized_person: Person = serde_json::from_str(json).unwrap();       println!("{:?}", deserialized_person);   }      {"name":"John Doe","age":30}   Person { name: "Jane Smith", age: 25, secret_info: "This is a secret" }   `

在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `name`、`age` 和 `secret_info` 字段。在 `secret_info` 字段上，我们使用 `#[serde(skip_serializing)]` 属性来指示 Serde 库在序列化时跳过该字段。

在 `main` 函数中，我们创建了一个 `Person` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(skip_serializing)]` 属性，所以在序列化过程中，`secret_info` 字段将被忽略，不会出现在生成的 JSON 字符串中。

然而，在反序列化时，`secret_info` 字段仍然会被使用。在示例中，我们使用包含 "secret_info" 键的 JSON 字符串进行反序列化，并成功地将其转换为 `Person` 结构体。

通过使用 `#[serde(skip_serializing)]` 属性，您可以选择性地在序列化过程中排除某些字段，但仍然保留它们在反序列化过程中的使用。这对于在序列化时隐藏某些字段的值，但在反序列化时仍然需要使用它们很有用。

### `#[serde(skip_deserializing)]`

`#[serde(skip_deserializing)]` 是一个属性，用于在反序列化时跳过特定的字段。通过使用 `#[serde(skip_deserializing)]` 属性，您可以指示 Serde 库在处理反序列化时忽略该字段，但在序列化时仍会使用该字段。

以下是一个示例：

`use serde::Deserialize;   use serde::Serialize;      #[derive(Debug, Serialize, Deserialize)]   struct Person {       name: String,       age: u32,       #[serde(skip_deserializing)]       secret_info: String,   }      fn main() {       let json = r#"{           "name": "John Doe",           "age": 30,           "secret_info": "This is a secret"       }"#;          let deserialized_person: Person = serde_json::from_str(json).unwrap();       println!("{:?}", deserialized_person);          let person = Person {           name: "Jane Smith".to_owned(),           age: 25,           secret_info: "This is a secret".to_owned(),       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);   }      Person6 { name: "John Doe", age: 30, secret_info: "" }   {"name":"Jane Smith","age":25,"secret_info":"This is a secret"}   `

在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `name`、`age` 和 `secret_info` 字段。在 `secret_info` 字段上，我们使用 `#[serde(skip_deserializing)]` 属性来指示 Serde 库在反序列化时跳过该字段。

在 `main` 函数中，我们使用包含 "secret_info" 键的 JSON 字符串进行反序列化，并成功地将其转换为 `Person` 结构体。由于我们使用了 `#[serde(skip_deserializing)]` 属性，所以在反序列化过程中，`secret_info` 字段将被忽略。

然而，在序列化时，`secret_info` 字段仍然会被使用。我们创建了一个 `Person` 结构体的实例，并将其序列化为 JSON 字符串。生成的 JSON 字符串中包含了 `secret_info` 字段，即使在反序列化时它被跳过了。

通过使用 `#[serde(skip_deserializing)]` 属性，您可以选择性地在反序列化过程中排除某些字段，但仍然保留它们在序列化过程中的使用。这对于在反序列化时忽略某些字段的值，但在序列化时仍然需要使用它们很有用。

### `#[serde(serialize_with = "path")]`

- `#[serde(serialize_with = "path")]` 是一个属性，用于指定在序列化过程中使用自定义的序列化函数。通过使用 `#[serde(serialize_with = "path")]` 属性，您可以告诉 Serde 库在序列化时使用指定的函数来处理特定字段。
    
    在 `#[serde(serialize_with = "path")]` 中，`path` 是一个函数路径，指定了要用于序列化的自定义函数。该函数应接受一个值作为参数，并返回一个实现了 `serde::ser::Serialize` trait 的类型。
    
    以下是一个示例：
    
    `use serde::Serialize;   use serde::Serializer;      #[derive(Debug, Serialize)]   struct Person {       name: String,       age: u32,       #[serde(serialize_with = "serialize_secret_info")]       secret_info: String,   }      fn serialize_secret_info<S>(secret_info: &String, serializer: S) -> Result<S::Ok, S::Error>   where       S: Serializer,   {       serializer.serialize_str("This is a secret")   }      fn main() {       let person = Person {           name: "John Doe".to_owned(),           age: 30,           secret_info: "Some secret".to_owned(),       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);   }   `
    
    在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `name`、`age` 和 `secret_info` 字段。在 `secret_info` 字段上，我们使用 `#[serde(serialize_with = "serialize_secret_info")]` 属性来指示 Serde 库在序列化时使用 `serialize_secret_info` 函数来处理该字段。
    
    `serialize_secret_info` 函数接受一个 `secret_info` 字符串和一个 `Serializer` 参数，并使用 `serializer.serialize_str` 方法将固定的字符串 "This is a secret" 序列化为 JSON 字符串。
    
    在 `main` 函数中，我们创建了一个 `Person` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(serialize_with = "serialize_secret_info")]` 属性，所以在序列化过程中，`secret_info` 字段将被替换为固定的字符串 "This is a secret"。
    
    通过使用 `#[serde(serialize_with = "path")]` 属性，您可以自定义特定字段的序列化过程，以便根据自己的需求进行处理。这对于对字段进行特定的转换或处理非常有用。
    
    ### 4.2. 8 `#[serde(deserialize_with = "path")]`
    
    `#[serde(serialize_with = "path")]` 是一个属性，用于指定在序列化过程中使用自定义的序列化函数。通过使用 `#[serde(serialize_with = "path")]` 属性，您可以告诉 Serde 库在序列化时使用指定的函数来处理特定字段。
    
    在 `#[serde(serialize_with = "path")]` 中，`path` 是一个函数路径，指定了要用于序列化的自定义函数。该函数应接受一个值作为参数，并返回一个实现了 `serde::ser::Serialize` trait 的类型。
    
    以下是一个示例：
    
    `use serde::Serialize;   use serde::Serializer;      #[derive(Debug, Serialize)]   struct Person {       name: String,       age: u32,       #[serde(serialize_with = "serialize_secret_info")]       secret_info: String,   }      fn serialize_secret_info<S>(secret_info: &String, serializer: S) -> Result<S::Ok, S::Error>   where       S: Serializer,   {       serializer.serialize_str("This is a secret")   }      fn main() {       let person = Person {           name: "John Doe".to_owned(),           age: 30,           secret_info: "Some secret".to_owned(),       };          let json_string = serde_json::to_string(&person).unwrap();       println!("{}", json_string);   }   {"name":"John Doe","age":30,"secret_info":"This is a secret"}   `
    
    在上面的示例中，我们定义了一个 `Person` 结构体，其中包含 `name`、`age` 和 `secret_info` 字段。在 `secret_info` 字段上，我们使用 `#[serde(serialize_with = "serialize_secret_info")]` 属性来指示 Serde 库在序列化时使用 `serialize_secret_info` 函数来处理该字段。
    
    `serialize_secret_info` 函数接受一个 `secret_info` 字符串和一个 `Serializer` 参数，并使用 `serializer.serialize_str` 方法将固定的字符串 "This is a secret" 序列化为 JSON 字符串。
    
    在 `main` 函数中，我们创建了一个 `Person` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(serialize_with = "serialize_secret_info")]` 属性，所以在序列化过程中，`secret_info` 字段将被替换为固定的字符串 "This is a secret"。
    
    通过使用 `#[serde(serialize_with = "path")]` 属性，您可以自定义特定字段的序列化过程，以便根据自己的需求进行处理。这对于对字段进行特定的转换或处理非常有用。
    
    ### 4.2.9 `#[serde(with = "module")]`
    
    `#[serde(with = "module")]` 是一个属性，用于指定在序列化和反序列化过程中使用自定义的序列化和反序列化函数。通过使用 `#[serde(with = "module")]` 属性，您可以告诉 Serde 库在处理特定字段时使用指定的模块中的函数来进行序列化和反序列化操作。
    
    在 `#[serde(with = "module")]` 中，`module` 是一个模块路径，指定了包含自定义序列化和反序列化函数的模块。
    
    以下是一个示例：
    
    `use serde::{Deserialize, Serialize};      mod custom_serialization {       pub fn serialize<S>(value: &u32, serializer: S) -> Result<S::Ok, S::Error>       where           S: serde::Serializer,       {           serializer.serialize_str(&value.to_string())       }          pub fn deserialize<'de, D>(deserializer: D) -> Result<u32, D::Error>       where           D: serde::Deserializer<'de>,       {           let s: &str = serde::Deserialize::deserialize(deserializer)?;           s.parse().map_err(serde::de::Error::custom)       }   }      #[derive(Debug, Serialize, Deserialize)]   struct Data {       #[serde(with = "custom_serialization")]       value: u32,   }      fn main() {       let data = Data { value: 42 };          let json_string = serde_json::to_string(&data).unwrap();       println!("{}", json_string);          let deserialized_data: Data = serde_json::from_str(&json_string).unwrap();       println!("{:?}", deserialized_data);   }   `
    
    在上面的示例中，我们定义了一个 `Data` 结构体，其中包含一个 `value` 字段。在 `value` 字段上，我们使用 `#[serde(with = "custom_serialization")]` 属性来指示 Serde 库在序列化和反序列化时使用 `custom_serialization` 模块中的函数。
    
    `custom_serialization` 模块中包含了 `serialize` 和 `deserialize` 函数，用于自定义 `value` 字段的序列化和反序列化操作。在 `serialize` 函数中，我们将 `value` 转换为字符串并序列化为 JSON 字符串。在 `deserialize` 函数中，我们将接收到的字符串解析为 `u32` 类型。
    
    在 `main` 函数中，我们创建了一个 `Data` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(with = "custom_serialization")]` 属性，所以在序列化过程中，`value` 字段将使用 `custom_serialization` 模块中的 `serialize` 函数进行处理。
    
    同样，我们还可以从 JSON 字符串中反序列化 `Data` 结构体。在反序列化过程中，`value` 字段将使用 `custom_serialization` 模块中的 `deserialize` 函数进行处理。
    
    通过使用 `#[serde(with = "module")]` 属性，您可以自定义特定字段的序列化和反序列化过程，以便根据自己的需求进行处理。这对于对字段进行特定的转换或处理非常有用。
    
    ### 4.2.10 `#[serde(bound = "T: MyTrait")]`
    
    如上
    
    `Serialize`和/或impls的 where 子句`Deserialize`。这将替换 Serde 为当前变体推断的任何特征范围。
    
    允许为序列化与反序列化指定独立的边界：
    
    ### 4.2.11 `#[serde(borrow)]`和`#[serde(borrow = "'a + 'b + ...")]`
    
    `#[serde(borrow)]` 和 `#[serde(borrow = "'a + 'b + ...")]` 是 Serde 库中的属性，用于指定在序列化和反序列化过程中字段的借用行为。
    
    `#[serde(borrow)]` 属性用于指定字段在序列化和反序列化时应该以借用的方式进行处理。这意味着字段的类型在序列化和反序列化期间将被借用，而不是所有权转移。
    
    以下是一个示例：
    
    `use serde::{Deserialize, Serialize};      #[derive(Debug, Serialize, Deserialize)]   struct Data {       #[serde(borrow)]       value: String,   }      fn main() {       let data = Data {           value: String::from("Hello, World!"),       };          let json_string = serde_json::to_string(&data).unwrap();       println!("{}", json_string);          let deserialized_data: Data = serde_json::from_str(&json_string).unwrap();       println!("{:?}", deserialized_data);   }   `
    
    在上面的示例中，我们定义了一个 `Data` 结构体，其中包含一个 `value` 字段。在 `value` 字段上，我们使用 `#[serde(borrow)]` 属性来指示 Serde 库在序列化和反序列化时以借用的方式处理该字段。
    
    在 `main` 函数中，我们创建了一个 `Data` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(borrow)]` 属性，所以在序列化过程中，`value` 字段将以借用的方式处理，而不是转移所有权。
    
    同样，我们可以从 JSON 字符串中反序列化 `Data` 结构体。在反序列化过程中，`value` 字段将以借用的方式处理。
    
    `#[serde(borrow = "'a + 'b + ...")]` 属性用于指定字段在序列化和反序列化时的借用约束。通过使用 `'a + 'b + ...` 语法，您可以指定字段的借用生命周期，并确保序列化和反序列化过程中的借用符合指定的生命周期约束。
    
    以下是一个示例：
    
    `use serde::{Deserialize, Serialize};      #[derive(Debug, Serialize, Deserialize)]   struct Data<'a> {       #[serde(borrow = "'a")]       value: &'a str,   }      fn main() {       let value = "Hello, World!";       let data = Data { value };          let json_string = serde_json::to_string(&data).unwrap();       println!("{}", json_string);          let deserialized_data: Data = serde_json::from_str(&json_string).unwrap();       println!("{:?}", deserialized_data);   }      {"value":"Hello, World!"}   Data { value: "Hello, World!" }   `
    
    在上面的示例中，我们定义了一个 `Data` 结构体，其中包含一个 `'a` 生命周期的引用字段 `value`。在 `value` 字段上，我们使用 `#[serde(borrow = "'a")]` 属性来指示 Serde 库在序列化和反序列化时应该将字段的借用生命周期限制为 `'a`。
    
    在 `main` 函数中，我们创建了一个 `Data` 结构体的实例，并将其序列化为 JSON 字符串。由于我们使用了 `#[serde(borrow = "'a")]` 属性，所以在序列化过程中，`value` 字段的借用生命周期限制为 `'a`。
    
    同样，我们可以从 JSON 字符串中反序列化 `Data` 结构体。在反序列化过程中，`value` 字段的借用生命周期也必须符合 `'a` 的约束。
    
    通过使用 `#[serde(borrow)]` 和 `#[serde(borrow = "'a + 'b + ...")]` 属性，您可以控制字段在序列化和反序列化过程中的借用行为，并指定借用的生命周期约束。这对于处理字段的所有权和借用非常有用，以满足特定的需求和场景。
    
    ### 4.2.12 `#[serde(other)]`
    
    `#[serde(other)]` 是 Serde 库中的一个属性，用于指定在反序列化时将未知字段存储为一个特定类型的结构体字段。
    
    当你使用 `#[serde(other)]` 属性时，Serde 将会将未知的字段序列化为指定类型的字段。这个字段将包含未知字段的名称和值。
    
    以下是一个示例：
    
    `use serde::{Deserialize, Serialize};   use serde_json::Value;      #[derive(Debug, Serialize, Deserialize)]   struct Data {       #[serde(other)]       unknown_fields: Vec<(String, Value)>,   }      fn main() {       let json_string = r#"           {               "name": "John",               "age": 30,               "city": "New York"           }       "#;          let deserialized_data: Data = serde_json::from_str(json_string).unwrap();       println!("{:?}", deserialized_data);   }   `
    
    在上面的示例中，我们定义了一个 `Data` 结构体，其中包含一个 `unknown_fields` 字段。在 `unknown_fields` 字段上，我们使用了 `#[serde(other)]` 属性来指示 Serde 库将未知字段存储为一个 `Vec<(String, Value)>` 类型的字段。
    
    在 `main` 函数中，我们使用 JSON 字符串进行反序列化，并将结果存储在 `Data` 结构体的实例中。由于我们使用了 `#[serde(other)]` 属性，所以在反序列化过程中，未知字段将被存储在 `unknown_fields` 字段中，以 `(String, Value)` 的形式表示，其中包含了未知字段的名称和值。
    
    通过使用 `#[serde(other)]` 属性，您可以处理那些在结构体中未定义的字段，以便在反序列化过程中保留它们的信息。这对于处理包含动态字段的数据非常有用，以便在后续的处理中进行进一步的分析或转换。
    
    ### 4.2.13 `#[serde(untagged)]`
    
    `#[serde(untagged)]` 是 Serde 库中的一个属性，用于指定在序列化和反序列化时如何处理无标记的变体类型（untagged variants）。
    
    当你使用 `#[serde(untagged)]` 属性时，Serde 将会以无标记的方式处理变体类型。这意味着在序列化时，变体类型的字段将被展开为外部结构体的字段，并且在反序列化时，可以接受多个字段来构造变体类型的实例。
    
    以下是一个示例：
    
    `use serde::{Deserialize, Serialize};      #[derive(Debug, Serialize, Deserialize)]   #[serde(untagged)]   enum Data {       VariantA {           field1: String,           field2: i32,       },       VariantB {           field3: bool,       },   }      fn main() {       let variant_a = Data::VariantA {           field1: String::from("Hello"),           field2: 42,       };          let variant_b = Data::VariantB {           field3: true,       };          let serialized_a = serde_json::to_string(&variant_a).unwrap();       let serialized_b = serde_json::to_string(&variant_b).unwrap();          println!("Serialized variant A: {}", serialized_a);       println!("Serialized variant B: {}", serialized_b);          let deserialized_a: Data = serde_json::from_str(&serialized_a).unwrap();       let deserialized_b: Data = serde_json::from_str(&serialized_b).unwrap();          println!("Deserialized variant A: {:?}", deserialized_a);       println!("Deserialized variant B: {:?}", deserialized_b);   }      Serialized variant A: {"field1":"Hello","field2":42}   Serialized variant B: {"field3":true}   Deserialized variant A: VariantA { field1: "Hello", field2: 42 }   Deserialized variant B: VariantB { field3: true }      `
    
    在上面的示例中，我们定义了一个 `Data` 枚举类型，其中包含两个变体：`VariantA` 和 `VariantB`。在 `Data` 枚举上，我们使用了 `#[serde(untagged)]` 属性来指示 Serde 库以无标记的方式处理这两个变体。
    
    在 `main` 函数中，我们创建了一个 `VariantA` 和一个 `VariantB` 的实例，并将它们分别序列化为 JSON 字符串。由于我们使用了 `#[serde(untagged)]` 属性，所以在序列化过程中，变体类型的字段将被展开为外部结构体的字段。
    
    我们还从 JSON 字符串中反序列化了 `VariantA` 和 `VariantB`。由于我们使用了 `#[serde(untagged)]` 属性，所以在反序列化过程中，可以接受多个字段来构造变体类型的实例。
    
    通过使用 `#[serde(untagged)]` 属性，您可以灵活地序列化和反序列化无标记的变体类型，以适应不同的数据结构和格式。这对于处理具有不确定字段结构的数据非常有用。
    

- `#[serde(bound(serialize = "T: MySerTrait"))]`
    
- `#[serde(bound(deserialize = "T: MyDeTrait"))]`
    
- `#[serde(bound(serialize = "T: MySerTrait", deserialize = "T: MyDeTrait"))]`



















