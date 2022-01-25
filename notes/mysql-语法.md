# SQL 语法

## 创建表
```sql
CREATE TABLE mytable (
  # int 类型，不为空，自增
  id INT NOT NULL AUTO_INCREMENT,
  # int 类型， 无符号，不为NULL，默认值为 0
  col1 int(10) unsigned NOT NULL DEFAULT 0 COMMENT 'int字段',
  # 变长字符串类型，最长为 64 个字符，可以为空，不为NULL，默认值为 空字符串
  title varchar(64) NOT NULL DEFAULT '' COMMENT '标题',
  # 日期类型，可为空
  date DATE NULL,
  # 设置主键为 id
  PRIMARY KEY (`id`));
```

## 修改表

添加列
```sql
ALTER TABLE mytable ADD COLUMN col VARCHAR(32) NOT NULL DEFAULT '' COMMENT '增加字段col' AFTER `id`;
```

修改表字段类型
```sql
ALTER TABLE mytable MODIFY COLUMN col VARCHAR(64);
```

删除列
```sql
ALTER TABLE mytable DROP COLUMN col;
```

删除表
```sql
DROP TABLE mytable;
```

## 插入

普通插入
```sql
INSERT INTO mytable (col1, col2) VALUES (val1, val2);
```

插入检索出来的数据
```sql
INSERT INTO mytable1(col1, col2) SELECT col1, col2 FROM mytable2;
```

将一个表的内容插入到一个新表
```sql
CREATE TABLE newtable AS SELECT * FROM mytable;
```

## 更新

```sql
UPDATE mytable SET col=val WHERE id=1;
```

## 删除
```sql
DELETE FROM mytable WHERE id=1;
```

清空表
```sql
TRUNCATE TABLE mytable;
```

使用更新和删除操作时一定要用 WHERE 子句，不然会把整张表的数据都破坏。可以先用 SELECT 语句进行测试，防止错误删除。

## 查询

### DISTINCT

相同值只会出现一次。它作用于所有列，也就是说所有列的值都相同才算相同。

```sql
SELECT DISTINCT col1, col2 FROM mytable;
```

### LIMIT

限制返回行数, 有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。

返回前 10 行
```sql
SELECT id FROM mytable LIMIT 10;
SELECT id FROM mytable LIMIT 0, 10;
```

返回第 11 \~ 20 行

```sql
SELECT id FROM mytable LIMIT 10, 10;
```

### ORDER BY
- ASC : 升序(默认)
- DESC: 降序

根据多个字段排序

```sql
SELECT id FROM mytable ORDER BY col1 DESC, col2 ASC; 
```

### GROUP BY

分组 : 把相同数据的行放在同一组中。

根据字段分组 求每个组中的数量

```sql
SELECT COUNT(*) FROM mytable GROUP BY col;
```

GROUP BY 自动按分组字段进行排序，ORDER BY 也可以按汇总字段来进行排序。

```sql
SELECT col, COUNT(*) AS num FROM mytable GROUP BY col ORDER BY num;
```
