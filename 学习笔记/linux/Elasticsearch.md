
```bash 

# https://www.elastic.co/downloads/past-releases/elasticsearch-7-17-17
# 下载elasticsearch
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.17-linux-x86_64.tar.gz

# 解压到opt
tar zxvf elasticsearch-7.17.17-linux-x86_64 -C /opt/

# 创建软链接（方便版本管理和切换）
sudo ln -s /opt/elasticsearch-7.17.17-linux-x86_64 /opt/elasticsearch

# 创建用户并赋权
useradd esuser

# 更新用户密码
password esuser

# 设置权限
sudo chown -R esuser:esuser /opt/elasticsearch-7.17.17-linux-x86_64

# 更新配置
vim /opt/elasticsearch/config/elasticsearch.yml

# 或者绑定所有网卡（开发测试可用）
network.host: 0.0.0.0

# 如果要使用外网访问，建议指定 HTTP 端口
http.port: 9200
# 绑定网卡到外网的话默认是生产环境,单节点集群（生产环境不要用）
discovery.type: single-node




```