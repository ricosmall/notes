## 第1课 了解 SQL

SQL（Structured Query Language）是一种专门用来与数据库沟通的语言。

数据库（database）：保存有组织的数据的容器（通常是一个文件或一组文件）。

- 人们通常用数据库这个术语来代表他们使用的数据库软件，这是不正确的，也因此产生了许多混淆。确切地说，数据库软件应称为数据库管理系统（DBMS）。数据库是通过 DBMS 创建和操纵的容器，而具体它究竟是什么，形式如何，各种数据库都不一样。


表（table）：某种特定类型数据的结构化清单。

- 这里的关键一点在于，存储在表中的数据是同一种类型的数据或清单。
- 数据库中的每个表都有一个名字来标识自己。
- 使表名成为唯一的，实际上是数据库名和表名等的组合。有的数据库还使用数据库拥有者的名字作为唯一名的一部分。也就是说，虽然在相同数据库中不能两次使用相同的表名，但在不同的数据库中完全可以使用相同的表名。

模式（schema）：关于数据库和表的布局及特性的信息。

列（column）：关于数据库和表的布局及特性的信息。

- 理解列的最好办法是将数据库表想象为一个网格，就像个电子表格那样。网格中每一列存储着某种特定的信息。例如，在顾客表中，一列存储顾客编号，另一列存储顾客姓名，而地址、城市、州以及邮政编码全都存储在各自的列中。
- 数据库中每个列都有相应的数据类型。数据类型（datatype）定义了列可以存储哪些数据种类。

行（row）：表中的一个记录。

- 你可能听到用户在提到行时称其为数据库记录（record）。这两个术语多半是可以交替使用的，但从技术上说，行才是正确的术语。

主键（primary key）：一列（或一组列），其值能够唯一标识表中每一行。

- 任意两行都不具有相同的主键值； 
- 每一行都必须具有一个主键值（主键列不允许 NULL 值）； 
- 主键列中的值不允许修改或更新； 
- 主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）。

## 第2课 检索数据

### SELECT 语句

为了使用 SELECT 检索表数据，必须至少给出两条信息——想选择什么，以及从什么地方选择。

结束 SQL 语句：多条 SQL 语句必须以分号（；）分隔。多数 DBMS 不需要在单条 SQL 语句后加分号，但也有 DBMS 可能必须在单条 SQL 语句后加上分号。当然，如果愿意可以总是加上分号。事实上，即使不一定需要，加上分号也肯定没有坏处。

SQL 语句和大小写：请注意，SQL 语句不区分大小写，因此 SELECT 与 select 是相同的。同样，写成 Select 也没有关系。许多 SQL 开发人员喜欢对 SQL 关键字使用大写，而对列名和表名使用小写，这样做使代码更易于阅读和调试。不过，一定要认识到虽然 SQL 是不区分大小写的，但是表名、列名和值可能有所不同（这有赖于具体的 DBMS 及其如何配置）。

使用空格：在处理 SQL 语句时，其中所有空格都被忽略。

### 检索单个列

```shell
SELECT prod_name FROM Products;
```

### 检索多个列

```shell
SELECT prod_id, prode_name, prod_price FROM Products;
```

当心逗号：在选择多个列时，一定要在列名之间加上逗号，但最后一个列名后不加。如果在最后一个列名后加了逗号，将出现错误。

### 检索所有列

```shell
SELECT * FROM Products;
```

使用通配符：一般而言，除非你确实需要表中的每一列，否则最好别使用*通配符。虽然使用通配符能让你自己省事，不用明确列出所需列，但检索不需要的列通常会降低检索和应用程序的性能。

### 检索不同的值

```shell
SELECT DISTINCT vend_id FROM Products;
```

### 限制结果

```shell
# 检索前 5 行
SELECT prod_name FROM Products LIMIT 5;

# 从第 3 行开始检索 5 行
SELECT prod_name FROM Products LIMIT 5 OFFSET 3;

# or
SELECT prod_name FROM Products LIMIT 3, 5;
```

第 0 行：第一个被检索的行是第 0 行，而不是第 1 行。因此，LIMIT 1 OFFSET 1 会检索第 2 行，而不是第 1 行。

### 注释

- 单行注释：`# 单行注释`
- 行内注释：`SELECT prod_name FROM Products; -- 行内注释`
- 块级注释：`/* 块级注释 */`

## 第3课 排序检索数据

### 排序数据

```shell
SELECT prod_name FROM Products ORDER BY prode_name;
```

ORDER BY 子句的位置：在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中最后一条子句。如果它不是最后的子句，将会出现错误消息。

### 按多个列排序

```shell
SELECT prod_id, prod_price, prod_name FROM Products ORDER BY prod_price, prod_name;
```

### 按列位置排序

```shell
SELECT prod_id, prod_price, prod_name FROM Products ORDER BY 2, 3;
```

### 指定排序方向

```shell
SELECT prod_id, prod_price, prod_name FROM Products ORDER BY prod_price DESC;

SELECT prod_id, prod_price, prod_name FROM Products ORDER BY prod_price DESC, prod_name;
```

请注意，DESC 是 DESCENDING 的缩写，这两个关键字都可以使用。与 DESC 相对的是 ASC（或ASCENDING），在升序排序时可以指定它。但实际上，ASC 没有多大用处，因为升序是默认的（如果既不指定 ASC 也不指定 DESC，则假定为 ASC）。

## 第4课 过滤数据

### 使用 WHERE 子句

在 SELECT 语句中，数据根据 WHERE 子句中指定的搜索条件进行过滤。WHERE 子句在表名（FROM 子句）之后给出。

```shell
SELECT prod_name, prod_price FROM Products WHERE prod_price = 3.49;

SELECT prod_name, prod_price FROM Products WHERE prod_price < 10;
```

SQL 过滤与应用过滤：数据也可以在应用层过滤。为此，SQL 的 SELECT 语句为客户端应用检索出超过实际所需的数据，然后客户端代码对返回数据进行循环，提取出需要的行。 通常，这种做法极其不妥。优化数据库后可以更快速有效地对数据进行过滤。而让客户端应用（或开发语言）处理数据库的工作将会极大地影响应用的性能，并且使所创建的应用完全不具备可伸缩性。此外，如果在客户端过滤数据，服务器不得不通过网络发送多余的数据，这将导致网络带宽的浪费。

WHERE 子句的位置：在同时使用 ORDER BY 和 WHERE 子句时，应该让 ORDER BY 位于 WHERE 之后，否则将会产生错误。

### WHERE 子句操作符

- `=`：等于
- `>`：大于
- `>=`：大于等于
- `<>`：不等于
- `!=`：不等于
- `!>`：不大于
- `!<`：不小于
- `<`：小于
- `<=`：小于等于
- `BETWEEN`：在指定的两个值之间
- `IS NULL`：为 NULL 值

操作符兼容：某些操作符是冗余的（如 <> 与 != 相同，!< 相当于 >=）。并非所有 DBMS 都支持这些操作符。

### 不匹配检查

```shell
SELECT prod_name, prod_price FROM Products WHERE vend_id != 'DLL01’;
```

何时使用引号：如果仔细观察上述 WHERE 子句中的条件，会看到有的值括在单引号内，而有的值未括起来。单引号用来限定字符串。如果将值与字符串类型的列进行比较，就需要限定引号。用来与数值列进行比较的值不用引号。

### 范围值检查

```shell
SELECT prod_name, prod_price FROM Products WHERE prod_price BETWEEN 5 AND 10;
```

### 空值检查

```sql
SELECT prod_name, prod_price FROM Products WHERE prod_price IS NULL;
```
