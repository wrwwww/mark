## 简介

### 相关阅读

1. [Apache SeaTunnel 官网](https://seatunnel.apache.org/zh-CN/)
2. [数据集成工具全面对比：Apache SeaTunnel VS. DataX、Flink CDC 和 Talend？](https://mp.weixin.qq.com/s/w-cGzb7Kd9IGKmLb2MMKRg)

## 安装

**linux**

```bash
# 下载
wget https://dlcdn.apache.org/seatunnel/2.3.12/apache-seatunnel-2.3.12-bin.tar.gz
# 解压到opt目录下
tar zxvf apache-seatunnel-2.3.12-bin.tar.gz -C /opt
sudo ln -s /opt/apache-seatunnel-2.3.12-bin /opt/seatunnel 
# 安装connectors插件
bash /opt/seatunnel/bin/install-plugin.sh

```

## 案例

###  使用mysql-cdc同步mysql到elasticsearch

1. 配置mysql开启`bin-log`,并创建用于同步的账号`repl@%`
```bash

systemctl stop mysqld
mkdir -p /var/log/mysql
sudo chown -R mysql:mysql /var/log/mysql

vim /etc/my.cnf 
# 新增或修改以下内容
# 随机生成的server-id
server-id = 5656
log-bin=/var/log/mysql/mysql-bin
binlog-format=ROW

# 创建账号并赋权
mysql -uroot -p
create user 'repl'@'%' identified by 'repl123!@#';

GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'repl'@'%';
GRANT SELECT ON excel.* TO 'repl'@'%';           -- 这个不能少！
GRANT SELECT ON information_schema.* TO 'repl'@'%';  -- 推荐加上
GRANT SELECT ON performance_schema.* TO 'repl'@'%';  -- 推荐加上

FLUSH PRIVILEGES;


```
2. 下载`mysql-connector-j-8.0.33.jar` 到`/opt/seatunnel/lib`目录下
3. 在`/opt/seatunnel/conf` 目录下创建同步任务配置文件`mysql-es-test.conf`
4. 编写同步配置文件
```json

env{
 # 并发数
 execution.parallelism = 1
 # 作业同步模式 streaming
 job.mode = "STREAMING"
 checkpoint.interval=2000
}
source {
  MySQL-CDC {
    # 取个临时表名
    result_table_name = "users"
    # mysql主节点server-id
    server-id = 5656
    # 主节点配置的复制节点权限的账号
    username = "repl"
    password = "Repl123!@#"
    # 需要同步的表，必须是数据库.表名
    table-names = ["excel.users"]
    # 默认是 INITIAL，先同步历史数据，后增量同步
    # startup.mode="INITIAL"
    # 必须指定数据库
    base-url = "jdbc:mysql://localhost:3306/excel"
  }
}
transform {
  Sql {
    source_table_name = "users"
    query = "select id,username name,email,age,created_at from users;"
  }
}
sink {
    Elasticsearch {
        hosts = ["http://127.0.0.1:9200"]
        # username = "elastic"
        # password = "ZOTFSWAv2y6IbKlVs7Dq"
        index = "index_t1"
        # CDC 实时同步 es，必须配置 primary_keys
        # 这样才能：
        # INSERT → ES 创建文档
        # UPDATE → ES 更新文档  
        # DELETE → ES 删除文档
        primary_keys = ["id"]
    }
}

```

5. 启动任务，以本地模式`/opt/seatunnel/bin/seatunnel.sh -e local --config /opt/sentunnel/config/mysql-es-test.conf`
6. 检查终端日志有没有报错
7. 登录elasticsearch查看数据同步情况
