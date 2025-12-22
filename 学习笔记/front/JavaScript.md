javaScript通过id修改html标签中的内容

```
  <input value="123" id="aa"/>
    <script>
        document.getElementById("aa").value="aaaaaaaaa";
        document.getElementById("aa").innerHtml="test";
    </script>
```

ajax异步交互

```
 function a1() {
            $.post({
                url:"${pageContext.request.contextPath}/h1/a1",
                data:{'deptno':document.getElementById("deptno").value},
                success:function (data,status) {
                    // data响应回来的数据包
                }
            });
 }
```

```
字符串的拼接
let a1=`aaa`;
let name="风吹麦浪";
let a2=`${a1}${name}`;
```

```
         let name = "zhangsan";
         let age = 100;
         let flag = false;   
         //js被骂没常量
         const PI = Math.PI;
         // 修改会报错
         //PI = 1245;
         console.log(PI)
         //var或造成变量穿透
         for(let i=0;i<5;i++){
             console.log(i);
         };
          // 存在于不属于她的作用域
         //nsole.log(i);
```

```
        var username = "张三";
        var age = 30;
        // 1: 原始的做法就是去拼接字符串
        var str = "我名字叫 " + username+",年龄是: "+age;
        console.log(str);  
        // 2:用模板字符串来拯救 注意：这里是 `（飘键） (tab键盘的上面那个键)
        // jdk1.9 
        var str2 = `我名字叫 ${username},年龄是: ${age}`;
        console.log(str2);
```

JavaScript的函数可以自带默认值

```
        // 默认参数 给参数列表设定初始值
        function add(a =100,b=100) {
            console.log(a,b);    
        }
        // 执行方法，会用默认值填充，打印出来100,200
        add();
        // 覆盖默认值打印  结果是1，2      
        add(1,2);
```

匿名函数

```
        // 1:声明式的定义
        function add (){
        };
        // 2:表达式的定义
        var add2 = function(){
        }     
        // 3:箭头函数的定义
        var sum = (a = 100,b = 300)=>{
            console.log(a+b);
        };
```

定义对象及方法

```
var person={
name : `风吹麦浪`,
  age:22,
  // 一
  say:()=>{
    console.log(this.name+this.age);
  }
  // 二
  say:funcation()=>{
    console.log(this.name+this.age);
  }
// 三
  say(){
    console.log(this.name+this.age);
  }
}


// 结构对象
// 1
var {name,age}=person;
// 2
var name=person.name;
var age=person.age;
// 3
var {name,age,language:lan}=person;
console.log(lan);

```

结构重组

```
> person
{ name: 'wrw', age: 18, language: 'cn' }
// ...person2拿到剩下的字段
> var {name,...person2}=person;
undefined
> person2
{ age: 18, language: 'cn' }

// 对象融合,若融合的对象key相同,后面的覆盖前面的
> var person3={
... ...person,
... ...person2}
undefined
> person3
{ name: 'wrw', age: 18, language: 'cn' }
>
 
```

```
> let a=[1,2,3,4,5];
undefined
> a
[ 1, 2, 3, 4, 5 ]
> a.map(a=>a+1)
[ 2, 3, 4, 5, 6 ]
```

`parseInt(value)`字符变数字

```
[ 1, 2, 3, 4, 5 ]
> a.reduce((a,b)=>a+b);
15
```

接收一个函数（必须）和一个初始值（可选），该函数接收两个参数：

- 第一个参数是上一次reduce处理的结果
- 第二个参数是数组中要处理的下一个元素  
    reduce() 会从左到右依次把数组中的元素用reduce处理，并把处理的结果作为下次reduce的第一个参数。如果是 第一次，会把前两个元素作为计算参数，或者把用户指定的初始值作为起始参数

npm JavaScript 的包管理工具

npm config set registry [http://registry.npm.taobao.org](http://registry.npm.taobao.org)

```
npm config list;//配置列表
npm config set registry http://registry.npm.taobao.org//设置淘宝源
npm init 创建项目 -y 创建默认项目

```

## javascript的引入

1. 通过script标签书写javascript代码
2. 通过script引入外部的javascript脚步

```
// 1 
<script> 
     //....
 <script>
 // 2  
 <script type="text/javascript" src=""></script>
```

### 定义变量

1. var //全局变量
2. let //局部变量
3. const //常量

### 数据类型

js的数字不区分整数小数

123//整数123  
 123.1//浮点数123.1 1.123e3//科学计数法 -99//负数 NaN //not a number Infinity // 表示无限大

**比较运算符** ！！！重要！

 =  
 1，"1"  
 == 等于（类型不一样，值一样，也会判断为true）  
 === 绝对等于（类型一样，值一样，结果为true）

这是一个JS的缺陷，坚持不要使用 == 比较  
须知：

- NaN === NaN，这个与所有的数值都不相等，包括自己
- 只能通过isNaN（NaN）来判断这个数是否是NaN

**数组**  
Java的数组必须是相同类型的对象~，JS中不需要这样

 //保证代码的可读性，尽量使用[]  
 var arr = [1,2,3,4,5,'hello',null,true];  
 //第二种定义方法  
 new Array(1,2,3,4,5,'hello');

取数字下标：如果越界了，就会 报undefined

 undefined

方法

arr.length 数组长度

arr.indexOf(2) 通过元素get它的下标

arr.slice(1,2) 创建切片从数组，可以指定开始和结束的位置

push(),pop() 存入和弹出元素，对数组尾部进行操作

unshift(),shift() 头部

unshift()将元素存入头部

shift(）将头部元素弹出

sort() 对数组元素排序

concat 数组拼接合成返回一个新数组

join(‘-’) 打印数组并使用-连接

字符串

str1.repeat(3) 一个字符串重复三次

str1.split(' ')在每次出现空格的时候拆分数组

然后可以通过join连接起来

**对象**  
对象是大括号，数组是中括号

每个属性之间使用逗号隔开，最后一个属性不需要逗号

 // Person person = new Person(1,2,3,4,5);  var person = {  name:'Tom',  age:3,  tags:['js','java','web','...'] }

`**'use strict';**`**开启js的严格模式**

#### 1、对象赋值

![](学习笔记/Attachments/1656807940719-c97cfd9b-756f-4e7c-922d-4de61aa5d85b.png)  
2、使用一个不存在的对象属性，不会报错！

#### undefined

![](学习笔记/Attachments/1656807940723-db3c5f91-bb7f-41e1-b9ef-243fbd8c7277.png)

3、动态的删减属性，通过delete删除对象的属性

#### delete![](学习笔记/Attachments/1656807940672-6f034d5b-e518-47a2-a5c6-3434ca56d881.png)

delete 删除属性通过in返回false

而如果直接undefined 来赋值给属性，通过in返回true

4、动态的添加，直接给新的属性添加值即可  
![](学习笔记/Attachments/1656807940634-1f20f5eb-87a2-41da-a89e-0d32c3f1fe71.png)  
5、判断属性值是否在这个对象中！

#### xxx in xxx

![](学习笔记/Attachments/1656807940730-b48ba563-1061-4694-bd61-24d0789c1905.png)  
6、判断一个属性是否是这个对象自身拥有的

#### hasOwnProperty()

![](学习笔记/Attachments/1656807941338-f305c3d6-a9f1-4f08-8a74-bb59e895f2e9.png)

###### 查看对象存在的属性

Object.keys(对象) 返回对象的属性的字符串数组

Object.assign(对象1，对象2) 将对象2的属性复制给对象1，如果属性名相同，同样被覆盖

### 3.4、流程控制

#### i f判断

![](学习笔记/Attachments/1656807941461-7299442b-e0ca-46f0-a629-3b6770f5f91a.png)

#### while循环，避免程序死循环

![](学习笔记/Attachments/1656807941451-bb9e5b73-51fa-4df6-9cb6-0c5152f6f5dc.png)

#### for循环

![](学习笔记/Attachments/1656807941607-3f0d8b1d-fc28-4a88-9e33-f622851b4f4f.png)

#### forEach循环

ES5.1特性

![](学习笔记/Attachments/1656807941719-efeb65f9-158e-4e01-a512-b7d1cdb49436.png)

#### for …in-------下标

#### ![](学习笔记/Attachments/1656807942420-0b024907-e646-4627-9521-37bce635d306.png)

**字符串**

大小写转换

//注意，这里是方法，不是属性了  
student.toUpperCase();  
student.toLowerCase();

student.substring(1)//从第一个字符串截取到最后一个字符串  
student.substring(1,3)//[1,3)

### 抛异常

![](学习笔记/Attachments/1656808093997-dbd41d9d-cf5d-41a8-9460-d05b9511ab91.png)

#### 唯一全局变量

![](学习笔记/Attachments/1656808225103-eac6368c-a3c6-4df8-9e61-9c548d0a979b.png)  
把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突问题~  
jQuery中就是使用的该方法：jQuery.name，简便写法：**$()**

### json

![](学习笔记/Attachments/1656808406565-dfdaf400-a1ce-41f6-881a-34c115a8d3c4.png)

![](学习笔记/Attachments/1656809063200-4fdf347f-ef22-423c-831a-b62144e8bbb0.png)

操作css

![](学习笔记/Attachments/1656809083144-0f6b68cf-f1d5-417a-a6bf-0a4ae4de330a.png)

删除节点的步骤：先获取父节点，再通过父节点删除自己  
![](学习笔记/Attachments/1656809125627-5d092098-5976-4dfd-9b19-52fad15f1d85.png)

  

### 选择器

 $('p').click();//标签选择器 $('#id1').click();//id选择器 $('.class1').click;//class选择器

**$('span').hide();隐藏span标签**

在 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/JavaScript) 中，truthy（真值）指的是在[布尔值](https://developer.mozilla.org/zh-CN/docs/Glossary/Boolean)上下文中，转换后的值为 true 的值。被定义为[假值](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)以外的任何值都为真值。（即所有除 false、0、-0、0n、""、null、undefined 和 NaN 以外的皆为真值）。

## 类型转换

parseInt() 转换为int

## 复制数据到剪切板

```
// 方法一
navigator.clipboard.writeText("数据");
// 复制数据到剪切板
// 方法二
document.querySelector("#id").select()
document.execommend("copy")
```

### 监听事件

浏览器监听键盘事件

```
getBySelection("input").addEventListener("keydown", event => {
  //  keydown 为键盘事件，submit为提交事件
  if (event.keyCode === 13) {
    search();
  }
})
```