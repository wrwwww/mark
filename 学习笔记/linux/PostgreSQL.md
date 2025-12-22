[链接](https://blog.csdn.net/xing09268/article/details/130077597)

## 重新安装pg

操作系统：Windows

数据目录：E:\home\[https://so.csdn.net/so/search?q=PostgreSQL&spm=1001.2101.3001.7020](https://so.csdn.net/so/search?q=PostgreSQL&spm=1001.2101.3001.7020)

系统还原后，之前安装的数据库服务没了，参照网上的方法，解决过程如下：

e:

cd home\PostgreSQL\bin

pg_ctl register -N PostgreSQL-9.3 -D "E:\home\PostgreSQL\data"

上述命令中"PostgreSQL-9.3" 为注册服务名，"E\home\PostgreSQL\data"为数据文件目录

然后启动PostgreSQL，命令如下：net start postgresql-9.3

接着再登陆PostgreSQL，命令如下：psql -U postgres

转载于:https://my.oschina.net/zhuxianwen/blog/161309对，这里我说的 **“只改元数据”**，意思是：  
在 PostgreSQL 里，某些 `ALTER TABLE` 操作实际上**不用去动表里的每一行数据**，它只是修改了 **系统目录表（system catalog）** 中保存的表结构信息。

---

## 📌 什么是元数据

在数据库里，**元数据 (metadata)** = 描述数据的数据。

- 表的定义（列名、数据类型、默认值、约束、索引定义等）
- 表和索引在磁盘上的存储方式
- 统计信息（行数、列分布等）

在 PostgreSQL 中，这些都保存在 **系统表** 里，比如：

- `pg_class` → 表和索引的基本信息
- `pg_attribute` → 列的信息
- `pg_constraint` → 约束
- `pg_index` → 索引定义

---

## 📌 举例说明

### 1. 只改元数据（快）

```
ALTER TABLE users ADD COLUMN age INT;
```

- PostgreSQL 不会真的在每一行后面都追加一个 `NULL` 值。
- 它只是往 `pg_attribute` 里登记：表 `users` 有个新列 `age`，默认值是 NULL。
- 查询时如果没数据，就返回 NULL。  
    👉 所以即使表有 1 亿行，也能瞬间完成。

### 2. 必须重写表（慢）

```
ALTER TABLE users ADD COLUMN age INT DEFAULT 0;
```

- PG 11 之前：会真的在每一行填 `0`，重写整张表 ❌
- PG 11 之后：如果默认值是**常量**，优化成了元数据操作 ✅

- 系统目录里记录“默认值=0”，查询时发现没有存储，就返回默认值。
- 这样还是不用写整张表。

但如果是：

```
ALTER TABLE users ADD COLUMN created_at TIMESTAMP DEFAULT now();
```

- 默认值是函数（非常量），每一行都要执行 `now()`，所以必须写一遍全表。

---

## 📌 总结

- **元数据修改**：只改系统目录表，不动数据 → 操作快（加列、改名、常量默认值）。
- **表重写（rewrite）**：真的要遍历全表、生成新文件 → 操作慢（改复杂类型、非常量默认值）。

---

要不要我给你列一张 **PostgreSQL 各类 ALTER 操作是否只改元数据的对照表**？这样你一眼就能知道哪些操作快，哪些会重写表。