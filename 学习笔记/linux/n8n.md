### 配置中文

1. [下载编译好的中文前端项目](https://github.com/other-blowsnow/n8n-i18n-chinese)
2. 将解压后的项目目录映射到容器中的`**/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist**`
3. 添加环境变量N8N_DEFAULT_LOCALE=zh-CN

```
podman run -it --rm  --name n8n    -p 5678:5678  -e N8N_SECURE_COOKIE=false  -v n8n_volume:/home/node/.n8n  -v /root/software/n8n/dist:/usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/dist:Z  -e N8N_DEFAULT_LOCALE=zh-CN  docker.n8n.io/n8nio/n8n
```