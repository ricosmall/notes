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

```sql
SELECT prod_name 
FROM Products;
```

### 检索多个列

```sql
SELECT prod_id, prode_name, prod_price 
FROM Products;
```

当心逗号：在选择多个列时，一定要在列名之间加上逗号，但最后一个列名后不加。如果在最后一个列名后加了逗号，将出现错误。

### 检索所有列

```sql
SELECT * 
FROM Products;
```

使用通配符：一般而言，除非你确实需要表中的每一列，否则最好别使用*通配符。虽然使用通配符能让你自己省事，不用明确列出所需列，但检索不需要的列通常会降低检索和应用程序的性能。

### 检索不同的值

```sql
SELECT DISTINCT vend_id 
FROM Products;
```

### 限制结果

```sql
# 检索前 5 行
SELECT prod_name 
FROM Products 
LIMIT 5;

# 从第 3 行开始检索 5 行
SELECT prod_name 
FROM Products 
LIMIT 5 OFFSET 3;

# or
SELECT prod_name 
FROM Products 
LIMIT 3, 5;
```

第 0 行：第一个被检索的行是第 0 行，而不是第 1 行。因此，LIMIT 1 OFFSET 1 会检索第 2 行，而不是第 1 行。

### 注释

- 单行注释：`# 单行注释`
- 行内注释：`SELECT prod_name FROM Products; -- 行内注释`
- 块级注释：`/* 块级注释 */`

## 第3课 排序检索数据

### 排序数据

```sql
SELECT prod_name 
FROM Products 
ORDER BY prode_name;
```

ORDER BY 子句的位置：在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中最后一条子句。如果它不是最后的子句，将会出现错误消息。

### 按多个列排序

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price, prod_name;
```

### 按列位置排序

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY 2, 3;
```

### 指定排序方向

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price DESC;

SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price DESC, prod_name;
```

请注意，DESC 是 DESCENDING 的缩写，这两个关键字都可以使用。与 DESC 相对的是 ASC（或ASCENDING），在升序排序时可以指定它。但实际上，ASC 没有多大用处，因为升序是默认的（如果既不指定 ASC 也不指定 DESC，则假定为 ASC）。

## 第4课 过滤数据

### 使用 WHERE 子句

在 SELECT 语句中，数据根据 WHERE 子句中指定的搜索条件进行过滤。WHERE 子句在表名（FROM 子句）之后给出。

```sql
SELECT prod_name, prod_price 
FROM Products 
WHERE prod_price = 3.49;

SELECT prod_name, prod_price 
FROM Products 
WHERE prod_price < 10;
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

```sql
SELECT prod_name, prod_price 
FROM Products 
WHERE vend_id != 'DLL01’;
```

何时使用引号：如果仔细观察上述 WHERE 子句中的条件，会看到有的值括在单引号内，而有的值未括起来。单引号用来限定字符串。如果将值与字符串类型的列进行比较，就需要限定引号。用来与数值列进行比较的值不用引号。

### 范围值检查

```sql
SELECT prod_name, prod_price 
FROM Products 
WHERE prod_price BETWEEN 5 AND 10;
```

### 空值检查

```sql
SELECT prod_name, prod_price 
FROM Products 
WHERE prod_price IS NULL;
```

## 第5课 高级数据过滤

为了进行更强的过滤控制，SQL 允许给出多个 WHERE 子句。这些子句有两种使用方式，即以 AND 子句或 OR 子句的方式使用。

### 组合 WHERE 子句

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
WHERE vend_id = 'DLL01' AND prod_price <= 4;

SELECT prod_id, prod_price, prod_name 
FROM Products 
WHERE vend_id = 'D
```

AND: 用在 WHERE 子句中的关键字，用来指示检索满足所有给定条件的行。

OR：WHERE 子句中使用的关键字，用来表示检索匹配任一给定条件的行。

### 求值顺序

```sql
# 优先匹配 AND 左右两侧的条件
SELECT prod_id, prod_price, prod_name 
FROM Products 
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01' AND prod_price >= 10;

# 优先匹配括号内的条件
SELECT prod_id, prod_price, prod_name 
FROM Products 
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01') AND prod_price >= 10;
```

SQL 在处理 OR 操作符之前，优先处理 AND 操作符。圆括号具有更高的优先级。

### IN 操作符

IN 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。

```sql
SELECT prod_name, prod_price 
FROM Products 
WHERE vend_id IN ( 'DLL01', 'BRS01' ) 
ORDER BY prod_name;
```

IN：WHERE 子句中用来指定要匹配值的清单的关键字，功能与 OR 相当。

### NOT 操作符

WHERE 子句中 NOT 操作符有且只有一个功能，那就是否定其后所跟的任何条件。

```sql
SELECT prod_name 
FROM Products 
WHERE NOT vend_id = 'DLL01' 
ORDER BY prod_name;
```

NOT：WHERE 子句中用来否定其后条件的关键字。

## 第6课 用通配符进行过滤

### LIKE 操作符

为在搜索子句中使用通配符，必须使用 LIKE 操作符。LIKE 指示 DBMS，后跟的搜索模式利用通配符匹配而不是简单的相等匹配进行比较。

通配符（wildcard）：用来匹配值的一部分的特殊字符。

搜索模式（search pattern）：由字面值、通配符或两者组合构成的搜索条件。 

### 百分号（%）通配符

在搜索串中，% 表示任何字符出现任意次数（包括 0 个字符）。

```sql
SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE 'Fish%';

SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE '%bean bag%';

SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE 'F%y';
```

### 下划线（_）通配符

下划线的用途与 % 一样，但它只匹配单个字符，而不是多个字符。

```sql
SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE '__ inch teddy bear';
```

### 方括号（[]）通配符

方括号（[]）通配符用来指定一个字符集，它必须匹配指定位置（通配符的位置）的一个字符。

```sql
SELECT cust_contact 
FROM Customers 
WHERE cust_contact LIKE '[JM]%' ORDER BY cust_contact;

SELECT cust_contact 
FROM Customers 
WHERE cust_contact LIKE '[^JM]%' ORDER BY cust_contact;

SELECT cust_contact 
FROM Customers 
WHERE NOT cust_contact LIKE '[JM]%' ORDER BY cust_contact;
```

SQL 的通配符很有用。但这种功能是有代价的，即通配符搜索一般比前面讨论的其他搜索要耗费更长的处理时间。

## 第7课 创建计算字段

### 计算字段

存储在数据库表中的数据一般不是应用程序所需要的格式，下面举几个例子。 

- 需要显示公司名，同时还需要显示公司的地址，但这两个信息存储在不同的表列中。 
- 城市、州和邮政编码存储在不同的列中（应该这样），但邮件标签打印程序需要把它们作为一个有恰当格式的字段检索出来。 
- 列数据是大小写混合的，但报表程序需要把所有数据按大写表示出来。 
- 物品订单表存储物品的价格和数量，不存储每个物品的总价格（用价格乘以数量即可）。但为打印发票，需要物品的总价格。 
- 需要根据表数据进行诸如总数、平均数的计算。

我们需要直接从数据库中检索出转换、计算或格式化过的数据，而不是检索出数据，然后再在客户端应用程序中重新格式化。

这就是计算字段可以派上用场的地方了。与前几课介绍的列不同，计算字段并不实际存在于数据库表中。计算字段是运行时在 SELECT 语句内创建的。

### 拼接字段

```sql
SELECT vend_name + ' (' + vend_country + ')' 
FROM Vendors 
ORDER BY vend_name;

# or

SELECT vend_name || ' (' || vend_country || ')' 
FROM Vendors 
ORDER BY vend_name;
```

许多数据库（不是所有）保存填充为列宽的文本值，而实际上你要的结果不需要这些空格。为正确返回格式化的数据，必须去掉这些空格。这可以使用 SQL 的 RTRIM() 函数来完成。

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' 
FROM Vendors 
ORDER BY vend_name;
```

- `RTRIM()`：去掉字符串右边的空格。
- `LTRIM()`：去掉字符串左边的空格。
- `TRIM()`：去掉字符串两边的空格。

### 使用别名

```sql
SELECT vend_name + ' (' + vend_country + ')' AS vend_title 
FROM Vendors 
ORDER BY vend_name;
```

### 执行算术计算

```sql
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price 
FROM OrderItems 
WHERE order_num = 20008;
```

SQL 支持的算术操作符：

- `+`：加
- `-`：减
- `*`：乘
- `/`：除

## 第8课 使用函数处理数据

### 函数带来的问题

事实上，只有少数几个函数被所有主要的 DBMS 等同地支持。虽然所有类型的函数一般都可以在每个 DBMS 中使用，但各个函数的名称和语法可能极其不同。

大多数 SQL 实现支持以下类型的函数：

- 用于处理文本字符串（如删除或填充值，转换值为大写或小写）的文 本函数。 
- 用于在数值数据上进行算术操作（如返回绝对值，进行代数运算）的数值函数。 
- 用于处理日期和时间值并从这些值中提取特定成分（如返回两个日期之差，检查日期有效性）的日期和时间函数。 
- 返回 DBMS 正使用的特殊信息（如返回用户登录信息）的系统函数。

### 文本处理函数

```sql
SELECT vend_name, UPPER(vend_name) AS upper_vend_name 
FROM Vendors 
ORDER BY vend_name;

SELECT vend_name LENGTH(vend_name) AS vend_name_length 
FROM Vendors 
ORDER BY vend_name;
```

常用的文本处理函数：

- `LEFT()`：返回字符串左边的字符
- `LENGTH()`：返回字符串的长度
- `LOWER()`：将字符串转换为小写
- `LTRIM()`：去掉字符串左边的空格
- `RIGHT()`：返回字符串右边的字符
- `RTRIM()`：去掉字符串右边的空格
- `SOUNDEX()`：返回字符串的 SOUNDEX 值
- `UPPER`：将字符串转换为大写

SOUNDEX 是一个将任何文 本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX 考虑了类似的发音字符和音节，使得能对字符串进行发音比较而不是字母比较。

### 日期和时间处理函数

日期和时间采用相应的数据类型存储在表中，每种 DBMS 都有自己的特殊形式。日期和时间值以特殊的格式存储，以便能快速和有效地排序或过滤，并且节省物理存储空间。

应用程序一般不使用日期和时间的存储格式，因此日期和时间函数总是用来读取、统计和处理这些值。由于这个原因，日期和时间函数在 SQL 中具有重要的作用。遗憾的是，它们很不一致，可移植性最差。

```sql
SELECT order_num 
FROM Orders 
WHERE strftime('%Y', order_date) = '2012';
```

### 数值处理函数

常用数值处理函数：

- `ABS()`：返回一个数的绝对值
- `COS()`：返回一个角度的余弦
- `EXP()`：返回一个数的指数值
- `PI()`：返回圆周率
- `SIN()`：返回一个角度的正弦
- `SQRT()`：返回一个数的平方根
- `TAN()`：返回一个角度的正切

## 第9课 汇总数据

聚集函数（aggregate function）：对某些行运行的函数，计算并返回一个值。

SQL 聚集函数：

- `AVG()`：返回某列的平均值
- `COUNT()`：返回某列的行数
- `MAX()`：返回某列的最大值
- `MIN()`：返回某列的最小值
- `SUM()`：返回某列值之和

### AVG() 函数

AVG() 通过对表中行数计数并计算其列值之和，求得该列的平均值。AVG() 可用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

```sql
SELECT AVG(prod_price) AS avg_price 
FROM Products;

SELECT AVG(prod_price) AS avg_price 
FROM Products 
WHERE vend_id = 'DLL01';
```

### COUNT() 函数

COUNT() 函数进行计数。可利用 COUNT() 确定表中行的数目或符合特定条件的行的数目。

COUNT() 函数有两种使用方式：

- 使用 COUNT(*) 对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。
- 使用 COUNT(column) 对特定列中具有值的行进行计数，忽略 NULL 值。

```sql
SELECT COUNT(*) AS num_cust 
FROM Products;

SELECT COUNT(cust_email) AS num_cust 
FROM Customers;
```

### MAX() 函数

MAX() 返回指定列中的最大值。

```sql
SELECT MAX(prod_price) AS max_price 
FROM Products;
```

对非数值数据使用 MAX()：虽然 MAX() 一般用来找出最大的数值或日期值，但许多（并非所有）DBMS 允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，MAX() 返回按该列排序后的最后一行。

### MIN() 函数

MIN() 的功能正好与 MAX() 功能相反，它返回指定列的最小值。

```sql
SELECT MIN(prod_price) AS min_price 
FROM Products;
```

对非数值数据使用 MIN()：虽然 MIN() 一般用来找出最小的数值或日期值，但许多（并非所有）DBMS 允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，MIN() 返回该列排序后最前面的行。

### SUM() 函数

SUM() 用来返回指定列值的和（总计）。

```sql
SELECT SUM(quantity) AS items_ordered 
FROM OrderItems 
WHERE order_num = 20005;

SELECT SUM(item_price * quantity) AS total_price 
FROM OrderItems 
WHERE order_num = 20005;
```

### 聚集不同值

```sql
SELECT AVG(DISTINCT prod_price) AS avg_price 
FROM Products 
WHERE vend_id = 'DLL01;
```

### 组合聚集函数

```sql
SELECT COUNT(*) AS num_items, MIN(prod_price) AS price_min, MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg 
FROM Products;
```

## 第10课 分组数据

### 创建分组

```sql
SELECT vend_id, COUNT(*) AS num_prods 
FROM Products 
GROUP BY vend_id;
```

在使用 GROUP BY 子句前，需要知道一些重要的规定。

- GROUP BY 子句可以包含任意数目的列，因而可以对分组进行嵌套，更细致地进行数据分组。 
- 如果在 GROUP BY 子句中嵌套了分组，数据将在最后指定的分组上进 行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以 不能从个别的列取回数据）。 
- GROUP BY 子句中列出的每一列都必须是检索列或有效的表达式（但 不能是聚集函数）。如果在 SELECT 中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式。不能使用别名。 
- 大多数 SQL 实现不允许 GROUP BY 列带有长度可变的数据类型（如文本或备注型字段）。 
- 除聚集计算语句外， SELECT 语句中的每一列都必须在 GROUP BY 子句 中给出。 
- 如果分组列中包含具有 NULL 值的行，则 NULL 将作为一个分组返回。 如果列中有多行 NULL 值，它们将分为一组。 
- GROUP BY 子句必须出现在 WHERE 子句之后， ORDER BY 子句之前。

```sql
SELECT vend_id, COUNT(*) AS num_prods 
FROM Products 
GROUP BY vend_id HAVING COUNT(*) >= 2;

SELECT vend_Id, COUNT(*) AS num_prods 
FROM Products 
WHERE prod_price >= 4 
GROUP BY vend_id HAVING COUNT(*) >= 2;

SELECT order_num, COUNT(*) AS items 
FROM OrderItems 
GROUP BY order_num HAVING COUNT(*) >= 3 
ORDER BY items, order_num;
```

### SELECT 子句顺序

|子句		|说明				|是否必须使用				|
|---|---|---|
|SELECT		|要返回的列或表达式	|是						|
|FROM		|从中检索数据的表		|仅在从表选择数据时使用		|
|WHERE		|行级过滤				|否						|
|GROUP BY	|分组说明				|仅在按组计算聚集时使用		|
|HAVING		|组级过滤				|否						|
|ORDER BY	|输出排序顺序			|否						|

## 第11课 使用子查询

### 利用子查询进行过滤

```sql
SELECT cust_id 
FROM Orders 
WHERE order_num IN (
  SELECT order_num 
  FROM OrderItems 
  WHERE prod_id = 'RGAN01');

SELECT cust_name, cust_contact 
FROM Customers 
WHERE cust_id IN (
  SELECT cust_id 
  FROM Orders 
  WHERE order_num IN (
    SELECT order_num 
    FROM OrderItems 
    WHERE prod_id = 'RGAN01'));
```

只能是单列：作为子查询的 SELECT 语句只能查询单个列。企图检索多个列将返回错误。

子查询和性能：这里给出的代码有效，并且获得了所需的结果。但是，使用子查询并不总是执行这类数据检索的最有效方法。

### 作为计算字段使用子查询

```sql
SELECT cust_name, cust_state, (
  SELECT COUNT(*) 
  FROM Orders 
  WHERE Orders.cust_id = Customers.cust_id) AS orders 
FROM Customers 
ORDER BY cust_name;
```

## 第12课 联结表

### 关系表

相同的数据出现多次决不是一件好事，这是关系数据库设计的基础。关系表的设计就是要把信息分解成多个表，一类数据一个表。各表通过某些共同的值互相关联（所以才叫关系数据库）。

关系数据可以有效地存储，方便地处理。因此，关系数据库的可伸缩性远比非关系数据库要好。

### 为什么使用联结

将数据分解为多个表能更有效地存储，更方便地处理，并且可伸缩性更好。

简单说，联结是一种机制，用来在一条 SELECT 语句中关联表，因此称为联结。使用特殊的语法，可以联结多个表返回一组输出，联结在运行时关联表中正确的行。

### 创建联结

```sql
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products 
WHERE Vendors.vend_id = Products.vend_id;
```

完全限定列名：就像前一课提到的，在引用的列可能出现歧义时，必须使用完全限定列名（用一个句点分隔表名和列名）。如果引用一个没有用表名限制的具有歧义的列名，大多数 DBMS 会返回错误。

### WHERE 子句的重要性

WHERE 子句作为过滤条件，只包含那些匹配给定条件（这里是联结条件）的行。没有 WHERE 子句，第一个表中的每一行将与第二个表中的每一行配对，而不管它们逻辑上是否能配在一起。

笛卡尔积：由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

```sql
# 错误示范
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products;
```

### 内联结

目前为止使用的联结称为等值联结（equijoin），它基于两个表之间的相等测试。这种联结也称为内联结（inner join）。

```sql
SELECT vend_name, prod_name, prod_price 
FROM Vendors INNER JOIN Products 
ON Vendors.vend_id = Products.vend_id;
```

在使用这种语法时，联结条件用特定的 ON 子句而不是 WHERE 子句给出。传递给 ON 的实际条件与传递给 WHERE 的相同。

### 联结多个表

SQL 不限制一条 SELECT 语句中可以联结的表的数目。创建联结的基本规则也相同。首先列出所有表，然后定义表之间的关系。

```sql
SELECT prod_name, vend_name, prod_price, quantity 
FROM OrderItems, Products, Vendors 
WHERE Products.vend_id = Vendors.vend_id 
AND OrderItems.prod_id = Products.prod_id 
AND order_num = 20007;
```

性能考虑：DBMS 在运行时关联指定的每个表，以处理联结。这种处理可能非常耗费资源，因此应该注意，不要联结不必要的表。联结的表越多，性能下降越厉害。

多做实验：执行任一给定的 SQL 操作一般不止一种方法。很少有绝对正确或绝对错误的方法。性能可能会受操作类型、所使用的 DBMS、表中数据量、是否存在索引或键等条件的影响。因此，有必要试验不同的选择机制，找出最适合具体情况的方法。

## 第13课 创建高级联结

### 使用别名表

给列起别名的语法如下：

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' AS vend_title 
FROM Vendors 
ORDER BY vend_name;
```

给表起别名的语法：

```sql
SELECT cust_name, cust_contact 
FROM Customers AS C, Orders AS O, OrderItems AS OI 
WHERE C.cust_id = O.cust_id 
AND OI.order_num = O.order_num 
AND prod_id = 'RGAN01';
```

需要注意，表别名只在查询执行中使用。与列别名不一样，表别名不返回到客户端。

### 使用不同类型的联结

迄今为止，我们使用的只是内联结或等值联结的简单联结。现在来看三种其他联结：自联结（self-join）、自然联结（natural join）和外联结（outer join）。

#### 自联结

```sql
SELECT c1.cust_id, c1.cust_name, c1.cust_contact 
FROM Customers AS c1, Customers AS c2 
WHERE c1.cust_name = c2.cust_name 
AND c2.cust_contact = 'Jim Jones';
```

此查询中需要的两个表实际上是相同的表，因此 Customers 表在 FROM 子句中出现了两次。虽然这是完全合法的，但对 Customers 的引用具有歧义性，因为 DBMS 不知道你引用的是哪个 Customers 表。解决此问题，需要使用表别名。

用自联结而不用子查询：自联结通常作为外部语句，用来替代从相同表中检索数据的使用子查询语句。虽然最终的结果是相同的，但许多 DBMS 处理联结远比处理子查询快得多。应该试一下两种方法，以确定哪一种的性能更好。

#### 自然联结

无论何时对表进行联结，应该至少有一列不止出现在一个表中（被联结的列）。标准的联结（前一课中介绍的内联结）返回所有数据，相同的列甚至多次出现。自然联结排除多次出现，使每一列只返回一次。

自然联结要求你只能选择那些唯一的列，一般通过对一个表使用通配符（SELECT *），而对其他表的列使用明确的子集来完成。

```sql
SELECT C.*, O.order_num, O.order_date, OI.prod_id, OI.quantity, OI.item_price 
FROM Customers AS C, Orders AS O, OrderItems AS OI 
WHERE C.cust_id = O.cust_id 
AND OI.order_num = O.order_num 
AND prod_id = 'RGAN01';
```

#### 外联结

许多联结将一个表中的行与另一个表中的行相关联，但有时候需要包含没有关联行的那些行。

```sql
SELECT Customers.cust_id, Orders.order_num 
FROM Customers LEFT OUTER JOIN orders 
ON Customers.cust_id = Orders.cust_id;
```

与内联结关联两个表中的行不同的是，外联结还包括没有关联行的行。在使用 OUTER JOIN
语法时，必须使用 RIGHT 或 LEFT 关键字指定包括其所有行的表（RIGHT指出的是 OUTER JOIN 右边的表，而LEFT指出的是 OUTER JOIN左边的表）。上面的例子使用 LEFT OUTER JOIN 从 FROM 子句左边的表（Customers 表）中选择所有行。为了从右边的表中选择所有行，需要使用 RIGHT OUTER JOIN。

```sql
SELECT Customers.cust_id, Orders.order_num 
FROM Customers RIGHT OUTER JOIN Orders 
ON Orders.cust_id = Customers.cust_Id;
```

外联结的类型：有两种基本的外联结形式：左外联结和右外联结。它们之间的唯一差别是所关联的表的顺序。换句话说，调整 FROM 或 WHERE 子句中表的顺序，左外联结可以转换为右外联结。因此，这两种外联结可以互换使用，哪个方便就用哪个。

还存在另一种外联结，就是全外联结（full outer join），它检索两个表中的所有行并关联那些可以关联的行。与左外联结或右外联结包含一个表的不关联的行不同，全外联结包含两个表的不关联的行。

```sql
SELECT Customers.cust_id, Orders.order_num 
FROM Orders FULL OUTER JOIN Customers 
ON Orders.cust_id = Customers.cust_id;
```

### 使用带聚集函数的联结

```sql
SELECT Customers.cust_id, COUNT(Orders.order_num) AS num_ord 
FROM Customers INNER JOIN Orders 
ON Customers.cust_id = Orders.cust_id 
GROUP BY Customers.cust_id;
```

聚集函数也可以方便地与其他联结一起使用。

```sql
SELECT Customers.cust_id, COUNT(Orders.order_num) AS num_ord 
FROM Customers LEFT OUTER JOIN Orders 
ON Customers.cust_id = Orders.cust_id 
GROUP BY Customers.cust_id;
```

## 第14课 组合查询

### 组合查询

多数SQL查询只包含从一个或多个表中返回数据的单条 SELECT 语句。但是，SQL 也允许执行多个查询（多条 SELECT 语句），并将结果作为一个查询结果集返回。这些组合查询通常称为并（union）或复合查询（compound query）。

主要有两种情况需要使用组合查询：

- 在一个查询中从不同的表返回结构数据
- 对一个表执行多个查询，按一个查询返回数据

### 创建组合查询

可以用 UNION 操作符来组合数条 SQL 查询。

#### 使用 UNION

使用 UNION 很简单，所要做的只是给出每条 SELECT 语句，在各条语句之间放上关键字 UNION。

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

使用多条WHERE子句而不是UNION的相同查询：

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
OR cust_name = 'Fun4All';
```

对于较复杂的过滤条件，或者从多个表（而不是一个表）中检索数据的情形，使用 UNION 可能会使处理更简单。

#### UNION 规则

- UNION 必须由两条或两条以上的 SELECT 语句组成，语句之间用关键字 UNION 分隔（因此，如果组合四条 SELECT 语句，将要使用三个UNION关键字）。
- UNION 中的每个查询必须包含相同的列、表达式或聚集函数（不过，各个列不需要以相同的次序列出）。
- 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含转换的类型（例如，不同的数值类型或不同的日期类型）。

### 包含或取消重复的行

UNION 从查询结果集中自动去除了重复的行；换句话说，它的行为与一条 SELECT 语句中使用多个 WHERE 子句条件一样。

如果想返回所有的匹配行，可使用 UNION ALL 而不是 UNION。

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION ALL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

UNION ALL 为 UNION 的一种形式，它完成 WHERE 子句完成不了的工作。

#### 对组合查询结果排序

在用 UNION 组合查询时，只能使用一条 ORDER BY 子句，它必须位于最后一条 SELECT 语句之后。

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All'
ORDER BY cust_name, cust_contact;
```

## 第15课 插入数据

### 数据插入

INSERT 用来将行插入（或添加）到数据库表。插入有几种方式：

- 插入完整的行
- 插入行的一部分
- 插入某些查询的结果

插入及系统安全：使用 INSERT 语句可能需要客户端/服务器 DBMS 中的特定安全权限。在你试图使用 INSERT 前，应该保证自己有足够的安全权限。

#### 插入完整的行

把数据插入表中的最简单方法是使用基本的 INSERT 语法，它要求指定表名和插入到新行中的值。

```sql
INSERT INTO Customers
VALUES('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA', NULL, NULL);
```

存储到表中每一列的数据在 VALUES 子句中给出，必须给每一列提供一个值。如果某列没有值，则应该使用 NULL 值（假定表允许对该列指定空值）。各列必须以它们在表定义中出现的次序填充。

虽然这种语法很简单，但并不安全，应该尽量避免使用。上面的 SQL 语句高度依赖于表中列的定义次序，还依赖于其容易获得的次序信息。即使可以得到这种次序信息，也不能保证各列在下一次表结构变动后保持完全相同的次序。因此，编写依赖于特定列次序的 SQL 语句是很不安全的，这样做迟早会出问题。

编写 INSERT 语句的更安全（不过更繁琐）的方法如下：

```sql
INSERT INTO Customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact, cust_email)
VALUES('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA', NULL, NULL);
```

因为提供了列名，VALUES 必须以其指定的次序匹配指定的列名，不一定按各列出现在表中的实际次序。其优点是，即使表的结构改变，这条 INSERT 语句仍然能正确工作。

给出列名的情况下，以不同的次序填充仍然正确：

```sql
INSERT INTO Customers(cust_id, cust_contact, cust_email, cust_name, cust_address, cust_city, cust_state, cust_zip)
VALUES('1000000006', NULL, NULL, 'Toy Land', '123 Any Street', 'New York', 'NY', '11111');
```

小心使用 VALUES：不管使用哪种 INSERT 语法，VALUES 的数目都必须正确。如果不提供列名，则必须给每个表列提供一个值；如果提供列名，则必须给列出的每个列一个值。否则，就会产生一条错误消息，相应的行不能成功插入。

#### 插入部分行

使用 INSERT 的推荐方法是明确给出表的列名。使用这种语法，还可以省略列，这表示可以只给某些列提供值，给其他列不提供值。

```sql
INSERT INTO Customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
VALUES('1000000006', 'Toy Land', '123 Any Street', 'New York', 'NY', '11111', 'USA');
```

省略的列必须满足以下条件：

- 该列定义为允许 NULL 值（无值或空值）
- 在表定义中给出默认值。这表示如果不给出值，将使用默认值。

#### 插入检索出的数据

INSERT 还存在另一种形式，可以利用它将 SELECT 语句的结果插入表中，这就是所谓的INSERT SELECT。顾名思义，它是由一条 INSERT语句和一条 SELECT语句组成的。

```sql
INSERT INTO Customers(cust_id, cust_contact, cust_email, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country)
SELECT cust_id, cust_contact, cust_email, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country
FROM CustNew;
```

如果 CustNew 这个表确实有数据，则所有的数据将被插入到 Customers 表。

## 从一个表复制到另一个表

有一种数据插入不使用 INSERT 语句。要将一个表的内容复制到一个全新的表（运行中创建的表），可以使用 SELECT INTO 语句。

```sql
SELECT *
INTO CustCopy
FROM Customers;
```

要想只复制部分的列，可以明确给出列名，而不是使用 * 通配符。

在使用SELECT INTO时，需要知道一些事情：

- 任何SELECT选项和子句都可以使用，包括 WHERE和 GROUP BY
- 可利用联结从多个表插入数据
- 不管从多少个表中检索数据，数据都只能插入到一个表中

## 第16课 更新和删除数据

### 更新数据

更新（修改）表中的数据，可以使用 UPDATE 语句。有两种使用 UPDATE 的方式：

- 更新表中的特定行
- 更新表中的所有行

基本的 UPDATE 语句由三部分组成，分别是：

- 要更新的表
- 列名和它们的新值
- 确定要更新那些行的过滤条件

```sql
UPDATE Customers
SET cust_email = 'kim@thetoystore.com
WHERE cust_id = '1000000005';
```

不要省略 WHERE 子句：在使用 UPDATE 时一定要细心。因为稍不注意，就会更新表中的所有行。

更新更多列的语法稍有不同：

```sql
UPDATE Customers
SET cust_contact = 'Sam Roberts',
	cust_email = 'sam@toyland.com'
WHERE cust_id = '1000000006';
```

要删除某个列的值，可设置它为 NULL（假如表定义允许 NULL 值）。

```sql
UPDATE Customers
SET cust_email = NULL
WHERE cust_id = '1000000005'
```

其中 NULL 用来去除 cust_email 列中的值。这与保存空字符串很不同（空字符串用''表示，是一个值），而 NULL 表示没有值。

### 删除数据

从一个表中删除（去掉）数据，使用 DELETE 语句。有两种使用 DELETE 的方式：

- 从表中删除特定的行
- 从表中删除所有行

```sql
DELETE FROM Customers
WHERE cust_id = '1000000006';
```

不要省略 WHERE 子句：在使用 DELETE 时一定要细心。因为稍不注意，就会错误地删除表中所有行。

友好的外键：使用外键确保引用完整性的一个好处是，DBMS 通常可以防止删除某个关系需要用到的行。例如，要从 Products
表中删除一个产品，而这个产品用在 OrderItems 的已有订单中，那么 DELETE 语句将抛出错误并中止。这是总要定义外键的另一个理由。

### 更新和删除的指导原则

如果省略了 WHERE 子句，则 UPDATE 或 DELETE 将被应用到表中所有的行。因此许多 SQL 程序员使用 UPDATE 或 DELETE 时需要遵循以下原则：

- 除非确实打算更新和删除每一行，否则绝对不要使用不带 WHERE子句的 UPDATE 或 DELETE 语句。 
- 保证每个表都有主键（如果忘记这个内容，请参阅第 12课），尽可能像 WHERE 子句那样使用它（可以指定各主键、多个值或值的范围）。 
- 在 UPDATE 或 DELETE 语句使用 WHERE 子句前，应该先用 SELECT 进行测试，保证它过滤的是正确的记录，以防编写的 WHERE 子句不正确。 
- 使用强制实施引用完整性的数据库（关于这个内容，请参阅第12课），这样 DBMS 将不允许删除其数据与其他表相关联的行。 
- 有的 DBMS 允许数据库管理员施加约束，防止执行不带 WHERE 子句的 UPDATE 或 DELETE 语句。如果所采用的 DBMS 支持这个特性，应该使用它。

## 第17课 创建和操纵表

### 创建表

一般有两种创建表的方法：

- 多数 DBMS 都具有交互式创建和管理数据库表的工具
- 表也可以直接用 SQL 语句操纵
用程序创建表，可以使用 SQL 的 CREATE TABLE 语句。需要注意的是，使用交互式工具时实际上就是使用 SQL 语句。这些语句不是用户编写的，界面工具会自动生成并执行相应的 SQL 语句（更改已有的表时也是这样）。

#### 表创建基础

利用 CREATE TABLE 创建表，必须给出下列信息：

- 新表的名字，在关键字 CREATE TABLE 之后给出
- 表列的名字和定义，用逗号分隔
- 有的 DBMS 还要求指定表的位置

```sql
CREATE TABLE Products
(
	prod_id		CHAR(10)			NOT NULL,
	vend_id		CHAR(10)			NOT NULL,
	prod_name	CHAR(254)		NOT NULL,
	prod_price	DECIMAL(8, 2)		NOT NULL,
	prod_desc	VARCHAR(1000)	NULL
);
```

替换现有的表：在创建新的表时，指定的表名必须不存在，否则会出错。防止意外覆盖已有的表，SQL 要求首先手工删除该表（请参阅后面的内容），然后再重建它，而不是简单地用创建表语句覆盖它。

#### 使用 NULL 值

在插入或更新行时，该列必须有值。每个表列要么是 NULL列，要么是 NOT NULL 列，这种状态在创建时由表的定义规定。

```sql
CREATE TABLE Orders
(
	order_num	INTEGER		NOT NULL,
	order_date	DATETIME	NOT NULL,
	cust_id		CHAR(10)		NOT NULL
);
```

这三列都需要，因此每一列的定义都含有关键字 NOT NULL。这就会阻止插入没有值的列。如果插入没有值的列，将返回错误，且插入失败。

```sql
CREATE TABLE Vendors
(
	vend_id			CHAR(10)		NOT NULL,
	vend_name		CHAR(50)	NOT NULL,
	vend_address		CHAR(50)	,
	vend_city			CHAR(50)	,
	vend_state		CHAR(5)		,
	vend_zip			CHAR(10)		,
	vend_country		CHAR(50)
);
```

NULL 为默认设置，如果不指定 NOT NULL，就认为指定的是 NULL。

主键和 NULL 值：主键是其值唯一标识表中每一行的列。只有不允许 NULL 值的列可作为主键，允许 NULL 值的列不能作为唯一标识。

#### 指定默认值

```sql
CREATE TABLE OrderItems
(
	order_num		INTEGER			NOT NULL,
	order_item		INTEGER			NOT NULL,
	prod_id			CHAR(10)			NOT NULL,
	quantity			INTEGER			NOT NULL		DEFAULT 1,
	item_price		DECIMAL(8, 2)		NOT NULL
);
```

默认值经常用于日期或时间戳列。

### 更新表

更新表定义，可以使用 ALTER TABLE 语句。以下是使用 ALTER TABLE 时需要考虑的事情。

- 理想情况下，不要在表中包含数据时对其进行更新。应该在表的设计过程中充分考虑未来可能的需求，避免今后对表的结构做大改动
- 所有的 DBMS 都允许给现有的表增加列，不过对所增加列的数据类型（以及 NULL 和 DEFAULT 的使用）有所限制
- 许多 DBMS 不允许删除或更改表中的列
- 多数 DBMS 允许重新命名表中的列
- 许多 DBMS 限制对已经填有数据的列进行更改，对未填有数据的列几乎没有限制

使用 ALTER TABLE 更改表结构，必须给出下面的信息：

- 在 ALTER TABLE 之后给出要更改的表名（该表必须存在，否则将出错）
- 列出要做哪些更改

```sql
ALTER TABLE Vendors
ADD vend_phone CHAR(20);
```

更改或删除列、增加约束或增加键，这些操作也使用类似的语法：

```sql
ALTER TABLE Vendors
DROP COLUMN vend_phone;
```

复杂的表结构更改一般需要手动删除过程，它涉及以下步骤：

(1) 用新的列布局创建一个新表； 
(2) 使用 INSERT SELECT 语句（关于这条语句的详细介绍，请参阅第 15课）从旧表复制数据到新表。有必要的话，可以使用转换函数和计算字段； 
(3) 检验包含所需数据的新表； 
(4) 重命名旧表（如果确定，可以删除它）； 
(5) 用旧表原来的名字重命名新表； 
(6) 根据需要，重新创建触发器、存储过程、索引和外键。

小心使用 ALTER TABLE：使用 ALTER TABLE 要极为小心，应该在进行改动前做完整的备份（表结构和数据的备份）。数据库表的更改不能撤销，如果增加了不需要的列，也许无法删除它们。类似地，如果删除了不应该删除的列，可能会丢失该列中的所有数据。

### 删除表

```sql
DROP TABLE CustCopy;
```

删除表没有确认，也不能撤销，执行这条语句将永久删除该表。

使用关系规则防止意外删除：许多 DBMS 允许强制实施有关规则，防止删除与其他表相关联的表。在实施这些规则时，如果对某个表发布一条 DROP TABLE 语句，且该表是某个关系的组成部分，则DBMS将阻止这条语句执行，直到该关系被删除为止。如果允许，应该启用这些选项，它能防止意外删除有用的表。

### 重命名表

所有重命名操作的基本语法都要求指定旧表名和新表名。不过，存在 DBMS 实现差异。关于具体的语法，请参阅相应的 DBMS 文档。

## 第18课 使用视图

### 视图

视图是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索 数据的查询。

用下面的 SELECT 语句从三个表中检索数据：

```sql
SELECT cust_name, cust_contact
FROM Customers, Orders, OrderItems
WHERE Customer.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num
AND prod_id = 'RGAN01';
```

现在，假如可以把整个查询包装成一个名为 ProductCustomers 的虚拟表，则可以如下轻松地检索出相同的数据：

```sql
SELECT cust_name, cust_contact
FROM ProductCustomers
WHERE prod_id = 'RGAN01';
```

这就是视图的作用。ProductCustomers 是一个视图，它不包含任何列或数据，包含的是一个查询（与上面用以正确联结表的查询相同）。

#### 为什么使用视图

下面是视图的一些常见应用：

- 重用 SQL 语句
- 简化复杂的SQL操作。在编写查询后，可以方便地重用它而不必知道其基本查询细节。
- 使用表的一部分而不是整个表。
- 保护数据。可以授予用户访问表的特定部分的权限，而不是整个表的访问权限。
- 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

创建视图之后，可以用与表基本相同的方式使用它们。可以对视图执行 SELECT 操作，过滤和排序数据，将视图联结到其他视图或表，甚至添加和更新数据（添加和更新数据存在某些限制，关于这个内容稍后做介绍）。 

重要的是，要知道视图仅仅是用来查看存储在别处数据的一种设施。视图本身不包含数据，因此返回的数据是从其他表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。


性能问题：因为视图不包含数据，所以每次使用视图时，都必须处理查询执行时需要的所有检索。如果你用多个联结和过滤创建了复杂的视图或者嵌套了视图，性能可能会下降得很厉害。因此，在部署使用了大量视图的应用前，应该进行测试。

#### 视图的规则和限制

关于视图创建和使用的一些最常见的规则和限制：

- 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的名字）
- 对于可以创建的视图数目没有限制。
- 创建视图，必须具有足够的访问权限。这些权限通常由数据库管理人员授予。
- 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造视图。所允许的嵌套层数在不同的DBMS中有所不同（嵌套视图可能会严重降低查询的性能，因此在产品环境中使用之前，应该对其进行全面测试）。
- 许多 DBMS 禁止在视图查询中使用 ORDER BY 子句。
- 有些 DBMS 要求对返回的所有列进行命名，如果列是计算字段，则需要使用别名（关于列别名的更多信息，请参阅第7课）。
- 视图不能索引，也不能有关联的触发器或默认值。
- 有些 DBMS 把视图作为只读的查询，这表示可以从视图检索数据，但不能将数据写回底层表。详情请参阅具体的 DBMS 文档。
- 有些 DBMS 允许创建这样的视图，它不能进行导致行不再属于视图的插入或更新。例如有一个视图，只检索带有电子邮件地址的顾客。如果更新某个顾客，删除他的电子邮件地址，将使该顾客不再属于视图。这是默认行为，而且是允许的，但有的 DBMS 可能会防止这种情况发生。

### 创建视图

视图用 CREATE VIEW 语句来创建。删除视图可以用 DROP VIEW <viewname>。

#### 利用视图简化复杂的联结

```sql
CREATE VIEW ProductCustomers AS
SELECT cust_name, cust_contact, prod_id
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num;
```

在以上视图中进行检索：

```sql
SELECT cust_name, cust_contact
FROM ProductCustomers
WHERE prod_id = 'RGAN01';
```

#### 用视图重新格式化检索出的数据

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' AS vend_title
FROM Vendors
ORDER BY vend_name;
```

把此语句转换为视图：

```sql
CREATE VIEW VendorLocations AS
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' AS vend_title
FROM Vendors;
```

再检索数据：

```sql
SELECT *
FROM VendorLocations;
```

#### 用视图过滤不想要的数据

```sql
CREATE VIEW CustomerEmailList AS
SELECT cust_id, cust_name, cust_email
FROM Customers
WHERE cust_email IS NOT NULL;
```

再检索数据：

```sql
SELECT *
FROM CustomerEMailList;
```

#### 使用视图和计算字段

```sql
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

将以上查询转成视图：

```sql
CREATE VIEW OrderItemsExpanded AS 
SELECT order_num, prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM OrderItems;
```

再检索数据：

```sql
SELECT *
FROM OrderItemsExpanded
WHERE order_num = 20008;
```

## 第19课 使用存储过程

### 存储过程

简单来说，存储过程就是为以后使用而保存的一条或多条 SQL 语句。可将其视为批文件，虽然它们的作用不仅限于批处理。

### 为什么要使用存储过程

理由很多，下面给出一些主要的：

- 通过把处理封装在一个易用的单元中，可以简化复杂的操作（如前面例子所述）。
- 由于不要求反复建立一系列处理步骤，因而保证了数据的一致性。如果所有开发人员和应用程序都使用同一存储过程，则所使用的代码都是相同的。这一点的延伸就是防止错误。需要执行的步骤越多，出错的可能性就越大。防止错误保证了数据的一致性。
- 简化对变动的管理。如果表名、列名或业务逻辑（或别的内容）有变化，那么只需要更改存储过程的代码。使用它的人员甚至不需要知道这些变化。 这一点的延伸就是安全性。通过存储过程限制对基础数据的访问，减少了数据讹误（无意识的或别的原因所导致的数据讹误）的机会。
- 因为存储过程通常以编译过的形式存储，所以 DBMS 处理命令所需的工作量少，提高了性能。
- 存在一些只能用在单个请求中的 SQL 元素和特性，存储过程可以使用它们来编写功能更强更灵活的代码。

换句话说，使用存储过程有三个主要的好处，即简单、安全、高性能。

### 执行存储过程

存储过程的执行远比编写要频繁得多，因此我们先介绍存储过程的执行。执行存储过程的 SQL 语句很简单，即 EXECUTE。EXECUTE 接受存储过程名和需要传递给它的任何参数。

```sql
EXECUTE AddNewProduct('JTS01', 'Stuffed Eiffel Tower', 6.49, 'Plush stuffed toy with the text La Tour Eiffel in red white and blue'0;
```

这里执行一个名为 AddNewProduct 的存储过程，将一个新产品添加到 Products 表中。AddNewProduct 有四个参数，分别是：供应商 ID（Vendors 表的主键）、产品名、价格和描述。这 4个参数匹配存储过程中4个预期变量（定义为存储过程自身的组成部分）。此存储过程将新行添加到 Products 表，并将传入的属性赋给相应的列。

在 Products表中还有另一个需要值的列 prod_id列，它是这个表的主键。为什么这个值不作为属性传递给存储过程？要保证恰当地生成此 ID，最好是使生成此 ID 的过程自动化（而不是依赖于最终用户的输入）。

以下是存储过程所完成的工作：

- 验证传递的数据，保证所有4个参数都有值；
- 生成用作主键的唯一ID；
- 将新产品插入 Products 表，在合适的列中存储生成的主键和传递的数据。

### 创建存储过程

Oracle 版本：

```sql
CREATE PROCEDURE MailingListCount (
	ListCount OUT INTEGER
)
IS
v_rows INTEGER;
BEGIN
	SELECT COUNT(*) INTO v_rows
	FROM Customers
	WHERE NOT cust_email IS NULL;
	ListCount := v_rows;
END;
```

这个存储过程有一个名为 ListCount 的参数。此参数从存储过程返回一个值而不是传递一个值给存储过程。关键字 OUT 用来指示这种行为。Oracle支持 IN（传递值给存储过程）、OUT（从存储过程返回值，如这里）、INOUT
（既传递值给存储过程也从存储过程传回值）类型的参数。存储过程的代码括在 BEGIN 和 END 语句中，这里执行一条简单的 SELECT 语句，它检索具有邮件地址的顾客。然后用检索出的行数设置 ListCount（要传递的输出参数）。

## 第20课 管理事务处理

### 事务处理

使用事务处理（transaction processing），通过确保成批的 SQL 操作要么完全执行，要么完全不执行，来维护数据库的完整性。

关系数据库把数据存储在多个表中，使数据更容易操纵、维护和重用。不用深究如何以及为什么进行关系数据库设计，在某种程度上说，设计良好的数据库模式都是关联的。

事务处理是一种机制，用来管理必须成批执行的 SQL 操作，保证数据库不包含不完整的操作结果。利用事务处理，可以保证一组操作不会中途停止，它们要么完全执行，要么完全不执行（除非明确指示）。如果没有错误发生，整组语句提交给（写到）数据库表；如果发生错误，则进行回退（撤销），将数据库恢复到某个已知且安全的状态。

下面是关于事务处理需要知道的几个术语：

- 事务（transaction）指一组 SQL 语句；
- 回退（rollback）指撤销指定 SQL 语句的过程；
- 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），可以对它发布回退（与回退整个事务处理不同）。

可以回退哪些语句：事务处理用来管理 INSERT、UPDATE 和 DELETE 语句。不能回退 SELECT 语句（回退 SELECT 语句也没有必要），也不能回退 CREATE 或 DROP 操作。

### 控制事务处理

管理事务的关键在于将 SQL 语句组分解为逻辑块，并明确规定数据何时应该回退，何时不应该回退。

MySQL 中的标识：

```sql
START TRANSACTION
...
```

事务一直存在，直到被中断。通常，COMMIT 用于保存更改，ROLLBACK 用于撤销，详述如下。

#### 使用 ROLLBACK

```sql
DELETE FROM Orders;
ROLLBACK;
```

在此例子中，执行 DELETE 操作，然后用 ROLLBACK 语句撤销。虽然这不是最有用的例子，但它的确能够说明，在事务处理块中，DELETE 操作（与 INSERT 和 UPDATE 操作一样）并不是最终的结果。


#### 使用 COMMIT

一般的 SQL 语句都是针对数据库表直接执行和编写的。这就是所谓的隐式提交（implicit commit），即提交（写或保存）操作是自动进行的。 

在事务处理块中，提交不会隐式进行。不过，不同 DBMS 的做法有所不同。有的 DBMS 按隐式提交处理事务端，有的则不这样。

进行明确的提交，使用COMMIT语句。

Oracle 示例：

```sql
SET TRANSACTION
DELETE OrderItems WHERE order_num = 12345;
DELETE OrderItems WHERE order_num = 12345;
COMMIT;
```

#### 使用保留点

使用简单的 ROLLBACK 和 COMMIT 语句，就可以写入或撤销整个事务。但是，只对简单的事务才能这样做，复杂的事务可能需要部分提交或回退。

要支持回退部分事务，必须在事务处理块中的合适位置放置占位符。这样，如果需要回退，可以回退到某个占位符。

MySQL 和 Oracle 示例：

```sql
ROLLBACK TO delete1;
```

## 第21课 使用游标

### 游标

有时，需要在检索出来的行中前进或后退一行或多行，这就是游标的用途所在。游标（cursor）是一个存储在 DBMS 服务器上的数据库查询，它不是一条 SELECT 语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

不同的 DBMS 支持不同的游标选项和特性。常见的一些选项和特性如下：

- 能够标记游标为只读，使数据能读取，但不能更新和删除
- 能控制可以执行的定向操作（向前、向后、第一、最后、绝对位置、相对位置等）
- 能标记某些列为可编辑的，某些列为不可编辑的。
- 规定范围，使游标对创建它的特定请求（如存储过程）或对所有请求可访问。
- 指示 DBMS 对检索出的数据（而不是指出表中活动数据）进行复制，使数据在游标打开和访问期间不变化。

### 使用游标

使用游标有几个明确的步骤：

- 在使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用的SELECT语句和游标选项。
- 一旦声明，就必须打开游标以供使用。这个过程用前面定义的 SELECT 语句把数据实际检索出来。
- 对于填有数据的游标，根据需要取出（检索）各行。
- 在结束游标使用时，必须关闭游标，可能的话，释放游标（有赖于具体的DBMS）。

#### 创建游标

使用 DECLARE 语句创建游标，这条语句在不同的 DBMS 中有所不同。DECLARE 命名游标，并定义相应的 SELECT 语句，根据需要带 WHERE 和其他子句。

```sql
DECLARE CustCursor CURSOR
FOR
SELECT * FROM Customers
WHERE cust_email IS NULL;
```

#### 使用游标

```sql
OPEN CURSOR CustCursor;
```

现在可以用 FETCH 语句访问游标数据了。FETCH 指出要检索哪些行，从何处检索它们以及将它们放于何处（如变量名）。

使用 Oracle 语法从游标中检索一行：

```sql
DECLARE TYPE CustCursor IS REF CURSOR RETURN Customers%ROWTYPE
DECLARE CustRecord Customers%ROWTYPE;
BEGIN
	OPEN CustCursor;
	FETCH CustCursor INTO CustRecord;
	CLOSE CustCursor;
END;
```

#### 关闭游标

CLOSE 语句用来关闭游标。一旦游标关闭，如果不再次打开，将不能使 用。第二次使用它时不需要再声明，只需用 OPEN 打开它即可。

```sql
CLOSE CustCursor
```

## 第22课 高级SQL特性

### 约束

约束（constraint）：管理如何插入或处理数据库数据的规则。

#### 主键

主键是一种特殊的约束，用来 一组列）中的值是唯一的，而且永不改动。换句话说，表中的一列（或 多个列）的值唯一标识表中的每一行。这方便了直接或交互地处理表中的行。没有主键，要安全地 UPDATE 或 DELETE 特定行而不影响其他行会 非常困难。

表中任意列只要满足以下条件，都可以用于主键。

- 任意两行的主键值都不相同。
- 每行都具有一个主键值（即列中不允许 NULL 值）。
- 包含主键值的列从不修改或更新。（大多数 DBMS 不允许这么做，但 如果你使用的 DBMS 允许这样做，好吧，千万别！）
- 主键值不能重用。如果从表中删除某一行，其主键值不分配给新行。

一种定义主键的方法是创建它：

```sql
CREATE TABLE Vendors
(
	vend_id			CHAR(10)		NOT NULL PRIMARY KEY,
	vend_name		CHAR(50)	NOT NULL,
	vend_address		CHAR(50)	NULL,
	vend_city			CHAR(50)	NULL,
	vend_state		CHAR(5)		NULL,
	vend_zip			CHAR(10)		NULL,
	vend_country		CHAR(50)	NULL
);
```

另一种方法：

```sql
ALTER TABLE Vendors
ADD CONSTRAINT PRIMARY KEY (vend_id);
```

#### 外键

外键是表中的一列，其值必须列在另一表的主键中。外键是保证引用完 整性的极其重要部分。

定义外键的方法：

```sql
CREATE TABLE Orders
(
	order_num	INTEGER		NOT NULL PRIMARY KEY,
	order_date	DATETIME	NOT NULL,
	cust_id		CHAR(10)		NOT NULL REFERENCES Customers(cust_id)
);
```

也可以用 CONSTRAINT 来完成：

```sql
ALTER TABLE Orders
ADD CONSTRAINT
FOREIGN KEY (cust_id) REFERENCES Customers (cust_id)
```

外键有助防止意外删除：除帮助保证引用完整性外，外键还有另一个重要作用。在定义外键后，DBMS 不允许删除在另一个表中具有关联行的行。

#### 唯一约束

唯一约束用来保证一列（或一组列）中的数据是唯一的。它们类似于主 键，但存在以下重要区别。

- 表可包含多个唯一约束，但每个表只允许一个主键。
- 唯一约束列可包含 NULL 值。
- 唯一约束列可修改或更新。
- 唯一约束列的值可重复使用。
- 与主键不一样，唯一约束不能用来定义外键。

唯一约束的语法类似于其他约束的语法。唯一约束既可以用 UNIQUE 关 键字在表定义中定义，也可以用单独的 CONSTRAINT 定义。

#### 检查约束

检查约束用来保证一列（或一组列）中的数据满足一组指定的条件。检 查约束的常见用途有以下几点：

- 检查最小或最大值。例如，防止 0 个物品的订单（即使 0 是合法的数）。 
- 指定范围。例如，保证发货日期大于等于今天的日期，但不超过今天 起一年后的日期。
- 只允许特定的值。例如，在性别字段中只允许 M 或 F 。

检查 约束在数据类型内又做了进一步的限制，这些限制极其重要，可以确保插 入数据库的数据正是你想要的数据。不需要依赖于客户端应用程序或用户 来保证正确获取它，DBMS 本身将会拒绝任何无效的数据。

施加检查约束：

```sql
CREATE TABLE OrderItems
(
	order_num	INTEGER		NOT NULL,
	order_item	INTEGER		NOT NULL,
	prod_id		CHAR(10)		NOT NULL,
	quantity		INTEGER		NOT NULL CHECK (quantity > 0),
	item_price	MONEY		NOT NULL
);
```

检查名为 gender 的列只包含 M 或 F，可编写如下的 ALTER TABLE 语句：

```sql
ADD CONSTRAINT CHECK (gender LIKE '[MF]')
```

### 索引

索引用来排序数据以加快搜索和排序操作的速度。想像一本书后的索引 （如本书后的索引），可以帮助你理解数据库的索引。

假如要找出本书中所有的“数据类型”这个词，简单的办法是从第 1 页 开始，浏览每一行。虽然这样做可以完成任务，但显然不是一种好的办法。浏览少数几页文字可能还行，但以这种方式浏览整部书就不可行了。 随着要搜索的页数不断增加，找出所需词汇的时间也会增加。

这就是书籍要有索引的原因。索引按字母顺序列出词汇及其在书中的位 置。为了搜索“数据类型”一词，可在索引中找出该词，确定它出现在 哪些页中。然后再翻到这些页，找出“数据类型”一词。

使索引有用的因素是什么？很简单，就是恰当的排序。找出书中词汇的 困难不在于必须进行多少搜索，而在于书的内容没有按词汇排序。如果 书的内容像字典一样排序，则索引没有必要（因此字典就没有索引）。

数据库索引的作用也一样。主键数据总是排序的，这是 DBMS 的工作。 因此，按主键检索特定行总是一种快速有效的操作。

但是，搜索其他列中的值通常效率不高。例如，如果想搜索住在某个州的客户，怎么办？因为表数据并未按州排序，DBMS 必须读出表中所有行（从第一行开始），看其是否匹配。这就像要从没有索引的书中找出词汇一样。

解决方法是使用索引。可以在一个或多个列上定义索引，使 DBMS 保存 其内容的一个排过序的列表。在定义了索引后，DBMS 以使用书的索引类似的方法使用它。DBMS 搜索排过序的索引，找出匹配的位置，然后检索这些行。

在开始创建索引前，应该记住以下内容：

- 索引改善检索操作的性能，但降低了数据插入、修改和删除的性能。 在执行这些操作时， DBMS 必须动态地更新索引。
- 索引数据可能要占用大量的存储空间。
- 并非所有数据都适合做索引。取值不多的数据（如州）不如具有更多可能值的数据（如姓或名），能通过索引得到那么多的好处。
- 索引用于数据过滤和数据排序。如果你经常以某种特定的顺序排序数据，则该数据可能适合做索引。
- 可以在索引中定义多个列（例如，州加上城市）。这样的索引仅在以州加城市的顺序排序时有用。如果想按城市排序，则这种索引没有用处。

没有严格的规则要求什么应该索引，何时索引。大多数 DBMS 提供了可 用来确定索引效率的实用程序，应该经常使用这些实用程序。

索引用 CREATE INDEX 语句创建：

```sql
CREATE INDEX prod_name_ind
ON Products (prod_name);
```

索引必须唯一命名。

### 触发器

触发器是特殊的存储过程，它在特定的数据库活动发生时自动执行。触发 器可以与特定表上的 INSERT、UPDATE 和 DELETE 操作（或组合）相关联。

与存储过程不一样（存储过程只是简单的存储 SQL 语句），触发器与单个的表相关联。与 Orders 表上的 INSERT 操作相关联的触发器只在 Orders 表中插入行时执行。 类似地， Customers 表上的 INSERT 和 UPDATE 操作的触发器只在 Customers 表上出现这些操作时执行。

触发器内的代码具有以下数据的访问权：

- INSERT 操作中的所有新数据；
- UPDATE 操作中的所有新数据和旧数据；
- DELETE 操作中删除的数据。

下面是触发器的一些常见用途：

- 保证数据一致。例如，在 INSERT 或 UPDATE 操作中将所有州名转换 为大写。
- 基于某个表的变动在其他表上执行活动。例如，每当更新或删除一行时将审计跟踪记录写入某个日志表。
- 进行额外的验证并根据需要回退数据。例如，保证某个顾客的可用资金不超限定，如果已经超出，则阻塞插入。
- 计算计算列的值或更新时间戳。

不同 DBMS 的触发器创建语法差异很大，更详细的信息请参阅相应的文档。

Oracle 版本：

```sql
CREATE TRIGGER customer_state
AFTER INSERT OR UPDATE
FOR EACH ROW
BEGIN
UPDATE Customers
SET cust_state = Upper(cust_state)
WHERE Customers.cust_id = :OLD.cust_id
END;
```

### 数据库安全

对于组织来说，没有什么比它的数据更重要了，因此应该保护这些数据， 使其不被偷盗或任意浏览。当然，数据也必须允许需要访问它的用户访 问，因此大多数 DBMS 都给管理员提供了管理机制，利用管理机制授予 或限制对数据的访问。

任何安全系统的基础都是用户授权和身份确认。这是一种处理，通过这 种处理对用户进行确认，保证他是有权用户，允许执行他要执行的操作。 有的 DBMS 为此结合使用了操作系统的安全措施，而有的维护自己的用户及密码列表，还有一些结合使用外部目录服务服务器。

一般说来，需要保护的操作有：

- 对数据库管理功能（创建表、更改或删除已存在的表等）的访问；
- 对特定数据库或表的访问；
- 访问的类型（只读、对特定列的访问等）；
- 仅通过视图或存储过程对表进行访问；
- 创建多层次的安全措施，从而允许多种基于登录的访问和控制；
- 限制管理用户账号的能力。
