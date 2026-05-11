## 相关链接

[下载地址](https://www.python.org/downloads/windows/)[python-3.12.3-amd64.exe.zip](https://www.yuque.com/attachments/yuque/0/2024/zip/23135285/1713417207923-36acddbb-4129-4a7d-9b39-b547b711137a.zip)

## pip 换源

```
# pip设置仓库地址为清华源
pip config set global.index-url  https://pypi.tuna.tsinghua.edu.cn/simple 
```

python中开启端口监听

python -m http.server 端口号
## pip

```shell
# 更新pip
python -m pip install --upgrade pip
 pip install -r ./requirements.txt
```

### 安装python库的方法

> 使用pip

```shell
# 安装指定版本
pip install urllib3=='3.0'
```

>使用源代码安装

```shell
# 下载源代码,源代码中有setup.py的文件
python setup.py install
```



## 相关链接

> anaconda

**[anaconda](https://www.anaconda.com/download)是一个为了方便使用Python进行数据科学研究而建立的软件包**。它包含了数据科学领域常见的Python库，并且自带了专门用来解决软件环境依赖问题的conda包管理系统。主要提供包管理与环境管理的功能，可以很方便地解决多版本python并存、切换以及各种第三方包安装问题。

> 阿里云镜像

[阿里镜像地址](https://developer.aliyun.com/mirror/pypi?spm=a2c6h.13651102.0.0.3e221b110CUzi4)

```shell
# pip设置仓库地址为清华源
pip config set global.index-url  https://pypi.tuna.tsinghua.edu.cn/simple 
```

> 无框架爬前程无忧

[xpath](https://zhuanlan.zhihu.com/p/547187505)

>浏览器驱动

记得选择与浏览器版本想符合的驱动版本

[chrome driver](https://chromedriver.chromium.org/downloads)

[edge driver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)

## anaconda

安装指定的python库

```python
# 搜索 jieba库
conda search jieba 
# 已经安装的库
conda list 
# 创建新的python环境
conda create --name py35 python=3.5
# 切换环境
activate py35
# 关闭anaconda py环境
deactivate py35
# 删除环境
conda remove --name py35 --all
# 安装指定课
conda install -n py35 numpy
# 删除指定环境的指定包
conda remove -n py35 numpy
```

## Scrapy框架

### 安装

```shell
pip install scrapy
```

###相关链接

[中文文档](https://scrapy-chs.readthedocs.io/zh-cn/0.24/topics/architecture.html)

### scrapy架构

Scrapy 是一个基于 Python 的开源网络爬虫框架，设计用于快速、高效地提取网站数据。下面是 Scrapy 的主要组件和架构说明：

1. **Spider（蜘蛛）：**
   - Spider 是 Scrapy 中的核心组件，用于定义爬取网站的规则和逻辑。每个 Spider 都包含了起始 URL、如何跟进链接、如何提取数据等规则。
   - 开发者需要继承 `scrapy.Spider` 类，并实现 `start_requests` 方法和/或 `parse` 方法。

2. **Request（请求）：**
   - Request 对象表示一个要发送的 HTTP 请求。它包含了 URL、回调函数、HTTP 方法等信息。
   - 在 Spider 的 `start_requests` 方法中创建起始请求，并通过回调函数处理响应。

3. **Downloader（下载器）：**
   - 下载器负责从网络中下载 HTML 页面，并将其传递给 Spider 进行解析。
   - Scrapy 默认使用 Twisted 框架提供的异步网络库来处理请求和响应，以提高爬虫的效率。

4. **Item（数据项）：**
   - Item 是表示爬取到的数据的简单容器，类似于字典。每个 Spider 可以生成多个 Item。
   - 开发者需要定义 Item 类型，以描述爬取到的数据结构。

5. **Item Pipeline（数据管道）：**
   - Item Pipeline 负责处理 Spider 提取的 Item，并进行后续的处理，例如数据清洗、存储到数据库或文件等。
   - Scrapy 提供了一些内置的 Item Pipeline，开发者也可以自定义 Item Pipeline。

6. **Middleware（中间件）：**
   - 中间件是在请求被发送到下载器之前或响应被 Spider 处理之前进行处理的组件。
   - Scrapy 提供了一系列内置的中间件，用于处理 Cookies、User-Agent、重定向等。

7. **Scheduler（调度器）：**
   - 调度器负责管理 Spider 发送的请求，确保按照优先级和顺序发送给下载器。
   - Scrapy 使用队列来存储请求，并根据队列的优先级和深度进行调度。

8. **Engine（引擎）：**
   - 引擎是 Scrapy 的核心组件，它负责协调各个组件的工作，驱动整个爬取流程。
   - 引擎接收 Spider 发来的请求，并将其发送到调度器、下载器、Spider 进行处理。

9. **Settings（设置）：**
   - 设置包含了 Scrapy 的配置信息，如爬虫的名字、下载延迟、并发请求数等。
   - 可以通过配置文件或者编程方式设置。

Scrapy 的架构是基于组件化和模块化的设计，这使得用户可以方便地扩展和定制爬虫的功能。通过上述组件的协作，Scrapy 提供了一个强大且灵活的爬虫框架，适用于各种规模的网站爬取任务。

### 爬取频率的控制

在使用Scrapy进行爬取时，通常不建议直接使用`time.sleep()`来控制爬取频率。Scrapy框架本身提供了一些机制来控制请求的频率，以及处理网站的反爬机制。

以下是一些Scrapy中用于控制爬取频率的机制：

1. **DOWNLOAD_DELAY：** 在Scrapy的设置中，你可以通过`DOWNLOAD_DELAY`参数设置下载延迟，以确保请求之间有一定的时间间隔。这可以在`settings.py`文件中进行配置：

    ```python
    DOWNLOAD_DELAY = 2  # 设置下载延迟为2秒
    ```

    这样可以帮助降低对目标网站的访问频率，减轻对服务器的压力。

2. **CONCURRENT_REQUESTS_PER_DOMAIN和CONCURRENT_REQUESTS_PER_IP：** 这两个设置可以控制对每个域名和IP的并发请求数。通过适当调整这些设置，你可以控制并发请求的数量，避免对服务器造成过大的负担。

    ```python
    CONCURRENT_REQUESTS_PER_DOMAIN = 2
    CONCURRENT_REQUESTS_PER_IP = 1
    ```

3. **AUTOTHROTTLE：** Scrapy还提供了`AUTOTHROTTLE`扩展，它能够自动调整爬取速度，避免对目标网站造成太大的负担。

    ```python
    AUTOTHROTTLE_ENABLED = True
    AUTOTHROTTLE_START_DELAY = 5.0
    AUTOTHROTTLE_MAX_DELAY = 60.0
    ```

    上述设置启用了自动限速，并设置了初始延迟和最大延迟。

总体来说，通过合理地设置上述参数，你可以在不使用`time.sleep()`的情况下有效地控制爬取频率。直接使用`time.sleep()`可能导致爬虫效率低下，并且不够灵活，因为它无法根据实际情况自动调整等待时间。

### 自定义 Item Pipeline来存储数据

> Item Pipeline负责对item进行处理清洗保存

#### 将爬取到的item保存到csv中

```shell
pip install pandas
```

#### 自定义item Pipeline

在pipelines.py中添加自定义pipline

```python
import csv
import pandas as pd

class CsvExportPipeline:
    def __init__(self):
        self.items = []

    def process_item(self, item, spider):
        self.items.append(item)
        return item

    def close_spider(self, spider):
        df = pd.DataFrame(self.items)
        df.to_csv('output_data.csv', index=False)
```

在setting.py中启用item pipeline 

```python
ITEM_PIPELINES = {
    # "ctbw.pipelines.CtbwPipeline": 300,
    "ctbw.pipelines.CsvExportPipeline": 300,
}
```

### 常用命令

```shell
# 使用模版创建一个爬虫项目
scrapy startproject xxx
# 使用模版创建爬虫文件
scrapy genspider ccc www.baidu.com
# 运行爬虫文件
scrapy runspider 
# 打开shell 调试
scrapy shell
# 下载给定的url网页并在终端输出
scrapy fetch
# 启动浏览器并查看url内容
scrapy view
# 启动指定爬虫项目
scrapy crawl 项目名称
# 列出所有爬虫项目列表
scrapy list

```



## Xpath

### 浏览器控制台中的xpath

```JavaScript
$('')
```

### 语法结构

```javascript
// 获取当前节点下有class名为value的div标签
$("//div[@class='value']")
```

### 选择器

[选择器方法](https://learnku.com/articles/50459)

> 普通的选择

```python
$("//div[@class='value']")
```

> 使用Contains()函数

```javascript
// <div type='submit'></div> 会找到包含指定字符串的标签
$("//*[contains(@type,'sub')]")
```

> 使用or 和 and 的表达式

```javascript
// or 和 and 的表达式
$("//*[@type='submit' or @name='btnReset']")
```

> Starts-with 标签的属性可能是动态变化的,可以使用这个选择器去选择那些初始内容是固定的,可能会被js改变属性内容的标签

```JavaScript
// 选择初始id为message的label标签,message变为message1也会被选择
$("//label[starts-with(@id, 'message')]")
```

> 使用text()匹配

```javascript
// 匹配节点文本内容
$("//td[text()='UserID']")
```

> follow 选择跟随在某个节点后的节点

```javascript
// type='text'这个节点后面的input标签,是多个,而不是只是后一个
$("//*[@type='text']//following::input")
```

|                   |                                                              |                              |
| ----------------- | ------------------------------------------------------------ | ---------------------------- |
| /                 | 25                                                           | 工程师                       |
| //                | 30                                                           | 设计师                       |
| .                 | 选取当前节点                                                 | 数据分析师                   |
| ..                | 选取当前节点的父节点                                         |                              |
| @                 | 选择属性                                                     | $("//div[@class='value']")   |
| 谓语（Predicates) | 选择第一个studnet                                            | /class/student[1]            |
|                   | 选择最后一个student                                          | /class/student[last()]       |
|                   | 选择倒数第二个                                               | /class/student[last()-1]     |
|                   | 选择前两个                                                   | /class/student[position()<3] |
|                   | 选择有gender属性的                                           | /class/student[@gender]      |
|                   | 选择id小于50的                                               | student[ID<50]               |
| 通配符            | 1. *   匹配任何元素节点     2. @*   匹配任何属性节点     3. node()        匹配任何类型的节点 |                              |
| string(.)         | 可以获取当前节点以及子节点的文本节点的内容                   |                              |

补充：

//input[not(@id='123')] 　　　　　　　　　　　　　　找id不为123的input

//span[substring(@name,3,5)='xxxxx'] 　　　　　　　name属性第3个字符开始的5个字符是xxxxx的

//span[sbustring-before(@class,"-")="spanclass1"]　　class属性中-字符前面的字符是spanclass1

//span[sbustring-after(@class,"-")="spanclass1"]　　　class属性中-字符后面的字符是spanclass1

//div[div[@id='xxx']]　　　　　　　　　　　　　　　　依靠子节点定位


# opencv

## 官网

[官网](https://opencv.org/releases/)

## numpy

### 相关链接

快速引导

[numpy.org](https://numpy.org/doc/stable/user/quickstart.html)

### 矩阵创建

```python
# 导入numpy
import numpy as np
```

#### 创建多维矩阵

```python
# 二维矩阵的创建
np.arange(15).reshape(3,5)

# 输出
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])

# 三维矩阵的创建
np.arange(12).reshape(4,3,1)

array([[[ 0],
        [ 1],
        [ 2]],

       [[ 3],
        [ 4],
        [ 5]],

       [[ 6],
        [ 7],
        [ 8]],
	
       [[ 9],
        [10],
        [11]]])
```

#### 创建指定数据类型的矩阵

```python
# 复数类型
c = np.array([[1, 2], [3, 4]], dtype=complex)
c
array([[1.+0.j, 2.+0.j],
       [3.+0.j, 4.+0.j]])
# 用指定值初始化矩阵
np.zeros((3, 4))
array([[0., 0., 0., 0.],
       [0., 0., 0., 0.],
       [0., 0., 0., 0.]])
# 用1
np.ones((2, 3, 4), dtype=np.int16)
array([[[1, 1, 1, 1],
        [1, 1, 1, 1],
        [1, 1, 1, 1]],

       [[1, 1, 1, 1],
        [1, 1, 1, 1],
        [1, 1, 1, 1]]], dtype=int16)
# 
np.empty((2, 3)) 
array([[3.73603959e-262, 6.02658058e-154, 6.55490914e-260],  # may vary
       [5.30498948e-313, 3.14673309e-307, 1.00000000e+000]])
```

#### 指定范围和数量的浮点数

```python
# 0~2 分9个数字
np.linspace(0, 2, 9)                   # 9 numbers from 0 to 2
array([0.  , 0.25, 0.5 , 0.75, 1.  , 1.25, 1.5 , 1.75, 2.  ])
```

### 矩阵操作

```py
# 同类型矩阵减法
a = np.array([20, 30, 40, 50])
b = np.arange(4)
b
array([0, 1, 2, 3])
c = a - b
c
array([20, 29, 38, 47])
# 布尔值矩阵
a < 35
array([0.85090352])

## 矩阵乘法
A @ B  
A.dot(B)
```













