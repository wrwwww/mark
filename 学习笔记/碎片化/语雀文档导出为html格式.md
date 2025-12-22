## lakebook解析
1. 导出知识库为lakebook
2. 解压lakebook
3. `$meta.json` 解析知识库元数据其中主要包含知识库的目录树结构信息，及包含的文档信息。
4. 然后按照doc顺序去逐步解析文档
## lake格式解析
首先导出为html
然后修复替换其中的字符串和代码块为正常html语法
然后使用html解析工具去解析html
