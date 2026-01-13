### 是什么

n8n 是一个功能强大的 **开源工作流自动化平台**。 它可以帮助你连接不同的应用程序和服务，并自动执行重复性的任务，从而节省时间和提高效率。

> 特点

- 用户通过拖拽代表特定应用或功能的节点创建复杂的工作流，即可完成复杂的数据处理逻辑。
- n8n 支持在自己的服务器（如通过 Docker）上部署。意味着所有 API 密钥和客户敏感数据都保存在本地，极大提升了安全性。
- 开发者也可以直接在节点中编写 JavaScript 代码，以满足高度自定义的需求。

### 安装

**前提条件:**

- 已安装 Docker或者Podman

**配置中文:**

1. [下载编译好的中文前端项目](https://github.com/other-blowsnow/n8n-i18n-chinese)

![[Pasted image 20251222204220.png]]

2. 解压下载的[editor-ui.tar.gz](https://github.com/other-blowsnow/n8n-i18n-chinese/releases/download/n8n%402.0.3/editor-ui.tar.gz)文件
3. 将解压后的项目目录映射到容器中的`/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist`
4. 添加环境变量N8N_DEFAULT_LOCALE=zh-CN

**执行命令:**

- 注意-v 参数后面的目录映射

```bash
podman run -it --rm  --name n8n    -p 5678:5678  -e N8N_SECURE_COOKIE=false  -v /n8n/data:/home/node/.n8n:Z  -v /n8n/dist:/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist:Z  -e N8N_DEFAULT_LOCALE=zh-CN  docker.n8n.io/n8nio/n8n
```

运行成功后, 访问5678端口便可正常访问n8n, 第一次运行需要注册并登录。登录后便可开始制作你的第一个工作流了。

![[Pasted image 20251222210046.png]]