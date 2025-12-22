## 镜像

[apache-maven-3.9.6-bin.zip](https://www.yuque.com/attachments/yuque/0/2024/zip/23135285/1713417138340-d817b3c1-c19f-4586-be28-a7ec23350085.zip)

#### 阿里云镜像配置

  

[https://developer.aliyun.com/mvn/guide](https://developer.aliyun.com/mvn/guide)  
maven 安装目录的 **conf/settings.xml** 在标签中添加 mirror 子节点:

[📎settings.xml](https://www.yuque.com/attachments/yuque/0/2023/xml/23135285/1693751512379-a1d7ac47-b46e-4956-b608-1485256184a8.xml)

```
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云公共仓库</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```