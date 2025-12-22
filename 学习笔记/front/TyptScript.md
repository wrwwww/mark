typescript是JavaScript的超集,可以编译成指定的js

## ts的配置

```json
"compilerOptions": {
  "incremental": true, // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
  "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
  "diagnostics": true, // 打印诊断信息 
  "target": "ES5", // 目标语言的版本
  "module": "CommonJS", // 生成代码的模板标准
  "outFile": "./app.js", // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
  "lib": ["DOM", "ES2015", "ScriptHost", "ES2019.Array"], // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
  "allowJS": true, // 允许编译器编译JS，JSX文件
  "checkJs": true, // 允许在JS文件中报错，通常与allowJS一起使用
  "outDir": "./dist", // 指定输出目录
  "rootDir": "./", // 指定输出文件目录(用于输出)，用于控制输出目录结构
  "declaration": true, // 生成声明文件，开启后会自动生成声明文件
  "declarationDir": "./file", // 指定生成声明文件存放目录
  "emitDeclarationOnly": true, // 只生成声明文件，而不会生成js文件
  "sourceMap": true, // 生成目标文件的sourceMap文件
  "inlineSourceMap": true, // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
  "declarationMap": true, // 为声明文件生成sourceMap
  "typeRoots": [], // 声明文件目录，默认时node_modules/@types
  "types": [], // 加载的声明文件包
  "removeComments":true, // 删除注释 
  "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
  "noEmitOnError": true, // 发送错误时不输出任何文件
  "noEmitHelpers": true, // 不生成helper函数，减小体积，需要额外安装，常配合importHelpers一起使用
  "importHelpers": true, // 通过tslib引入helper函数，文件必须是模块
  "downlevelIteration": true, // 降级遍历器实现，如果目标源是es3/5，那么遍历器会有降级的实现
  "strict": true, // 开启所有严格的类型检查
  "alwaysStrict": true, // 在代码中注入'use strict'
  "noImplicitAny": true, // 不允许隐式的any类型
  "strictNullChecks": true, // 不允许把null、undefined赋值给其他类型的变量
  "strictFunctionTypes": true, // 不允许函数参数双向协变
  "strictPropertyInitialization": true, // 类的实例属性必须初始化
  "strictBindCallApply": true, // 严格的bind/call/apply检查
  "noImplicitThis": true, // 不允许this有隐式的any类型
  "noUnusedLocals": true, // 检查只声明、未使用的局部变量(只提示不报错)
  "noUnusedParameters": true, // 检查未使用的函数参数(只提示不报错)
  "noFallthroughCasesInSwitch": true, // 防止switch语句贯穿(即如果没有break语句后面不会执行)
  "noImplicitReturns": true, //每个分支都会有返回值
  "esModuleInterop": true, // 允许export=导出，由import from 导入
  "allowUmdGlobalAccess": true, // 允许在模块中全局变量的方式访问umd模块
  "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
  "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
  "paths": { // 路径映射，相对于baseUrl
    // 如使用jq时不想使用默认版本，而需要手动指定版本，可进行如下配置
    "jquery": ["node_modules/jquery/dist/jquery.min.js"]
  },
  "rootDirs": ["src","out"], // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
  "listEmittedFiles": true, // 打印输出文件
  "listFiles": true// 打印编译的文件(包括引用的声明文件)
}
 
// 指定一个匹配列表（属于自动指定该路径下的所有ts相关文件）
"include": [
   "src/**/*"
],
// 指定一个排除列表（include的反向操作）
 "exclude": [
   "demo.ts"
],
// 指定哪些文件使用该配置（属于手动一个个指定文件）
 "files": [
   "demo.ts"
]
`
# 运行

tsc 01.ts
// 实时监听
tsc 01.ts -w

# 基础类型

## 介绍

为了让程序有价值，我们需要能够处理最简单的数据单元：数字，字符串，结构体，布尔值等。 TypeScript支持与JavaScript几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用。

## 布尔值

最基本的数据类型就是简单的true/false值，在JavaScript和TypeScript里叫做boolean（其它语言中也一样）。

let isDone: boolean = false;

## 数字

和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。

let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;

## 字符串

JavaScript程序的另一项基本操作是处理网页或服务器端的文本数据。 像其它语言里一样，我们使用 string表示文本数据类型。 和JavaScript一样，可以使用双引号（ "）或单引号（'）表示字符串。

let name: string = "bob";
//你还可以使用模版字符串，它可以定义多行文本和内嵌表达式。 
//这种字符串是被反引号包围（ `），并且以${ expr }这种形式嵌入表达式
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.
 I'll be ${ age + 1 } years old next month.`;
 
//这与下面定义sentence的方式效果相同：
let sentence: string = "Hello, my name is " + name + ".\n\n" +
     "I'll be " + (age + 1) + " years old next month.";

## 数组

TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。

// 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：
let list: number[] = [1, 2, 3];
 
// 第二种方式是使用数组泛型，Array<元素类型>：
let list: Array<number> = [1, 2, 3];
 

## 元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。

// 比如，你可以定义一对值分别为 string和number类型的元组。
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
 
// 当访问一个已知索引的元素，会得到正确的类型：
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
 
// 当访问一个越界的元素，会使用联合类型替代：
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型
 
console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString

x[6] = true; // Error, 布尔不是(string | number)类型

## 枚举

enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

enum Color {Red, Green, Blue}
let c: Color = Color.Green;
 
// 默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
 
// 或者，全部都采用手动赋值：
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
 
// 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字： 
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
 
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2

## Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量：

let notSure: any = 4;
 notSure = "maybe a string instead";
 notSure = false;
 
// 在对现有代码进行改写的时候，any类型是十分有用的，
// 它允许你在编译时可选择地包含或移除类型检查。 
// 你可能认为 Object有相似的作用，就像它在其它语言中那样。 
// 但是 Object类型的变量只是允许你给它赋任意值 - 
// 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
 
// 当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据： 
let list: any[] = [1, true, "free"];
 
list[1] = 100;

## Void

某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：

function warnUser(): void {
     console.log("This is my warning message");
 }
 
// 声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：
let unusable: void = undefined;

## Null 和 Undefined

TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。 和 void相似，它们的本身的类型用处不是很大：

// Not much else we can assign to these variables! let u: undefined = undefined; let n: null = null;

默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

然而，当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 这能避免 _很多_常见的问题。 也许在某处你想传入一个 string或null或undefined，你可以使用联合类型string | null | undefined。 再次说明，稍后我们会介绍联合类型。

注意：我们鼓励尽可能地使用--strictNullChecks，但在本手册里我们假设这个标记是关闭的。

## Never

never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

never类型是任何类型的子类型，也可以赋值给任何类型；然而，_没有_类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

下面是一些返回never类型的函数：

// 返回never的函数必须存在无法达到的终点
 function error(message: string): never {
     throw new Error(message);
 }
 
 // 推断的返回值类型为never
 function fail() {
     return error("Something failed");
 }
 
 // 返回never的函数必须存在无法达到的终点
 function infiniteLoop(): never {
     while (true) {
     }
 }
 

## Object

object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

使用object类型，就可以更好的表示像Object.create这样的API。例如：自定义对象

declare function create(o: object | null): void;
 
 create({ prop: 0 }); // OK
 create(null); // OK
 
 create(42); // Error
 create("string"); // Error
 create(false); // Error
 create(undefined); // Error
 

## 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过_类型断言_这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

// 类型断言有两种形式。 其一是“尖括号”语法： 
let someValue: any = "this is a string";
 
let strLength: number = (<string>someValue).length;

// 另一个为as语法：
let someValue: any = "this is a string";
 
let strLength: number = (someValue as string).length;
 
// 两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；
// 然而，当你在TypeScript里使用JSX时，只有 as语法断言是被允许的。

# 解构

let nums=[1,2]
let [first,second]=nums
// first=1 second=2
let nums=[1,2,3]
let [first,...rest]=nums
// ...获取其余元素
let nums=[1,2,3,4]
let [first]=nums
// first=1
let [,second,,fourth]=nums
// second=2 fourth=4

let o={
  a:"1",
  c:"3",
  b:"2",
}
let {a,b}=o
// a 由o.a组成
// b由o.c组成
let {a,...b}=o
// a 由o.a组成
// b由o剩下的部分组成

// 当属性为undefined的时候使用默认值
let { a, b = 1001 } = {nums:[11]};

## 展开

与解构相反用来组合解构

let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];

# 接口

## 可选属性

// 属性名后加?的为可选属性
interface SquareConfig {
  color?: string;
  width?: number;
}

可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

## 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 readonly来指定只读属性:

interface Point {
    readonly x: number;
    readonly y: number;
}

TypeScript具有ReadonlyArray<T>类型，它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：

let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!

# 基本项目搭建

首先初始化项目

npm init -y // 全部默认

项目需要引入的的包

npm install -D webpack webpack-cli typescript ts-loader
// -d 是开发环境 

然后是配置tsc编译的配置,webpack的配置

ts的配置可以通过`tsc -init` 初始化

{
  // 要编译包含的目录
  "include":[
    "./study/**/*"
  ],
  // 要排除的不需要编译的文件目录
  "exclude":[],
  // 配置继承其他文件的配置
   "extends":[""],
  // 手动填写要编译的文件
  "file":["xxx.ts"],
  
  // 编译的配置项
  "compilerOptions": {
    // 编译后的js版本
    "target": "es2015", 
    // 所包含的库,默认会有,document等对象都是默认导入的
    "lib": ["2015", "DOM"]
    // 使用的模块系统
    // 可选值: CommonJS、UMD、AMD、System、ES2020、ESNext
    "module": "ES2015", 
    // 编译后文件的输出目录
    "outDir": "./dist",
    // 编译为一个js
    "outFile":"./dist/app.js",
    // 对js进行编译
    "allowJs": false
    // 检查js
    "checkJs": true,
    // 编译后不保留注释
    "removeComments": true
    "esModuleInterop": true,
    "strict": true, 
    "skipLibCheck": true,
    // 严格检查模式
    "alwaysStrict": false,
    "noImplicitAny": false,
    "noImplicitThis": false,
    "strictNullChecks": false,
/**
· alwaysStrict
  用来设置编译后的文件是否使用严格模式
· noImplicitAny
  不允许隐式any类型 开启后 let x; 会报错
· noImplicitThis
  不允许不明确类型的this
· strictNullChecks
  严格的检查空值
· strictBindCallApply
  严格检查bind、call和apply的参数列表
· strictFunctionTypes
  严格检查函数的类型
· strictPropertyInitialization
  严格检查属性是否初始化
· noFallthroughCasesInSwitch
 检查switch语句包含正确的break
· noImplicitReturns
  检查函数没有隐式的返回值
· noUnusedLocals
  检查未使用的局部变量
· noUnusedParameters
**/
  }
}

在实际的开发中不可能只使用单独的编译工具还需要打包工具来帮助我们,这个时候引入webpack,并且通过ts-loader整合tsc编译器

需要引入html-webpack-plugin的webpack 插件

`npm install -d html-webpack-plugin`用来设置默认的HTML模板,不在需要手动引入编译好的js文件到HTML中

// 引入外部模块
const path =require('path')
const HtmlLoad=require('html-webpack-plugin')
// webpack的配置全部写在这个里面
module.exports={
    // webpack的工作模式
    // 启用 webpack 内置在相应环境下的优化
    // production,development,none 
    mode: 'development',
    // ts文件的入口
    entry:'./src/index.ts',
    // 编译后的文件输出目录
    output:{
        // path为导入的模块,这里是为了补全路径而已
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js',
        // 先清理文件在编译
        clean: true
    },
    // webpack 打包时候使用文件
    module:{
        // 指定要使用的
        rules:[{
            //“嘿，webpack 编译器，当你碰到「在 require()/import 语句中
            // 被解析为 '.txt' 的路径」时，在你对它打包之前，先 use(使用) raw-loader 转换一下。”
            //loader 有两个属性：
            // test 属性，识别出哪些文件会被转换。
            // use 属性，定义出在进行转换时，应该使用哪个 loader。
            test:/\.ts$/,
            // 后期还需要babel 编译器
            // ts-loader 将ts转为js文件
            // babel将对js做处理使其兼容性更好
            // 例如现代js语法在ie上可能有些库没有,
            // 这就需要babel的作用,使ie上实现一些功能
            // (()=>)为直接执行函数
            // ie 不支持所以需要关闭剪头函数在webpack中
            use: 'ts-loader',
            //排查的文件
            exclude:/node-modules/
        }]
    },
    plugins: [
        new HtmlLoad({
            // title: "web",
            // 使用已有模板
            template: "./index.html"
        })
    ],
    // 设置引用模块
    resolve: {
        extensions: ['.ts','.js']
    }
}

然后配置package.json,添加build打包,start 运行

同时输入webpack serve 用来实现实时监听修改,编译打包

  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack-dev-server"
  },