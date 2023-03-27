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

修改表主键
```sql
ALTER TABLE mytable DROP PRIMARY KEY, ADD PRIMARY KEY ( col1, col2 );
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

## 函数

### 聚合函数

| 函 数 | 说 明 |
|  ---  | ---  |
| AVG() | 平均值 |
|COUNT()| 总行数 |
| MAX() | 最大数 |
| MIN() | 最小数 |
| SUM() | 总和  |

AVG() 会忽略 NULL 行
使用 DISTINCT 可以汇总不同的值。

### 文本处理

| 函 数 | 说 明 | 使用 |
|  ---  | ---  | --- |
| LEFT(string, length) | 左边字符 | LEFT(string, 3),从左侧提取3个字符 |
| RIGHT()| 右边字符 | RIGHT(string, 3),从右侧提取3个字符 |
| LOWER() | 转换为小写字符 | LOWER('SHANGHAI') |
| UPPER() | 转换为大写字符 | UPPER('SHANGHAI') |
| LTRIM() | 去除左边的空格 | LTRIM(' SHANGHAI') |
| RTRIM() | 去除右边的空格 | RTRIM('SHANGHAI ') |
| LENGTH()| 长度 | LENGTH(string) |
| SUBSTRING() | 从字符串中提取子字符串 | SUBSTRING(string, start, length) |
| SOUNDEX() | 转换为语音值, 可以将一个字符串转换为描述其语音表示的字母数字模式。 | SOUNDEX(string) |

### 日期和时间处理

|函 数 | 说 明|
| :---: | :---: |
| ADDDATE() | 增加一个日期（天、周等）|
| ADDTIME() | 增加一个时间（时、分等）|
| CURDATE() | 返回当前日期 |
| CURTIME() | 返回当前时间 |
| DATE() |返回日期时间的日期部分|
| DATEDIFF() |计算两个日期之差|
| DATE_ADD() |高度灵活的日期运算函数|
| DATE_FORMAT() |返回一个格式化的日期或时间串|
| DAY()| 返回一个日期的天数部分|
| DAYOFWEEK() |对于一个日期，返回对应的星期几|
| HOUR() |返回一个时间的小时部分|
| MINUTE() |返回一个时间的分钟部分|
| MONTH() |返回一个日期的月份部分|
| NOW() |返回当前日期和时间|
| SECOND() |返回一个时间的秒部分|
| TIME() |返回一个日期时间的时间部分|
| YEAR() |返回一个日期的年份部分|

## 导入导出

-d 仅导出表结构

导出数据库
mysqldump -h 127.0.0.1 -u username -p --set-gtid-purget=off -p -d database > database.sql;

导出数据库表
mysqldump -h 127.0.0.1 -u username -p --set-gtid-purget=off -p -d database table1 table2 > database.sql;

导入数据库
source /database.sql;

表导入csv文件
load data local infile '/home/file.csv' into table mytable fields terminated by ',' optionally enclosed by '"' lines terminated by '\r\n' ignore 1 lines (col1, col2);

导出文件
mysql -h127.0.0.1 -uroot -p123456 -e "select * from table limit 10" database > user.log

## 权限管理

MySQL 的账户信息保存在 mysql 这个数据库中。

查询当前所有用户

```sql
USE mysql;
SELECT user FROM user;
```

**创建账户**

```sql
CREATE USER user IDENTIFIED BY 'password';
```

**修改账户**

```sql
UPDATE user SET user='newUser', password='password' WHERE user='user';
```

**删除账户**

```sql
DROP USER user;
```

**查看权限**

```sql
SHOW GRANTS FRO user;
```

**授予权限**

```sql
GRANT all privileges on database.* TO 'user' IDENTIFIED BY 'password' with grant option;
GRANT SELECT, INSERT ON database.* TO 'user';
```

**删除权限**

```sql
REVOKE SELECT, INSERT ON database.* FROM 'user';
```

**刷新权限**

```sql
FLUSH PRIVILEGES;
```