---
title: Active Record相关知识
id: 96
categories:
  - Mysql
  - 技术专栏
date: 2011-08-22 22:42:06
tags:
---

<div id="blog_text">

**<span style="font-size: small;">active record是rails中提供的对象关系映射(ORM)。这里我们将看到activerecord的基础知识：连接数据库，映射表，操作数据等。

active record非常接近标准的ORM模型：表对应类，行对应对象，列对应对象的属性。它和其他orm库不同的是它的配置上。通过一组默认的规 则，active record使得开发人员在配置上的工作非常小。举个例子，使用active record包装mysql中的一个表orders。找到一个特定的id记录。它更改买者的姓名，并存储结果到数据库中，改变了原始记录行。 </span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**require "rubygems"
require_gem "activerecord"
ActiveRecord::Base.establish_connection(:adapter =&gt; "mysql",
:host =&gt; "localhost", :database =&gt; "railsdb")
class Order &lt; ActiveRecord::Base
end
order = Order.find(123)
order.name = "Dave Thomas"
order.save**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">这就是全部的代码。这个例子中没有任何配置信息（除了数据库连接）。 activerecord是怎样知道我们所需要的，又是怎样知道正确地得到它呢？让我们看看这里面的原理。

<span style="color: #ff6600;">表与类</span>

</span>****<span style="font-size: small;">当你创建一个 ActiveRecord::Base的子类，实际上是包装一个数据库表。active record假定表名是类的复数形式。如果类名包含多个以大写字母开头的单词，表名会假定以下划线分隔这些单词。一些正常的复数形式有：

类名               表名
Order               orders
TaxAgency       tax_agencies
Batch              batches
Diagnosis          diagnoses
LineTtem         line_items
Person             people
Datum             data
Quantity          quantities

这些规则反应了DHH（rails的作者）的理念：类名是单数，表名是复数。如果你不喜欢这种做法，你可以在配置文件中设置一个全局变量关闭它。在 config目录下environment.rb文件中设置。
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**ActiveRecord::Base.pluralize_table_names=false**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">上述规则适用于绝大多数情况。但有时表名和类名不对应，active record允许覆盖掉生成的缺省表名,使用set_tables_name命令
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**class Sheep &lt; ActiveRecord::Base
set_table_name "sheep" # Not "sheeps"
end
class Order &lt; ActiveRecord::Base
set_table_name "ord_rev99_x" # Wrap a legacy table...
end**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;"><span style="color: #ff6600;">列和属性</span>

</span>****<span style="font-size: small;">active record对象对应数据库表中的行。对象的属性对应表中的列。你可能已经注意到我们定义的Order类并没有提到orders表中任何列。这是因为 active record是在运行期间动态决定这些类的active record为配置包装了表的类，直接反射数据库里的模式(schema)(有时这也不是很严格,如一个model存在不是schema内的属性,以后讨 论)。

我们的表orders可能是如下sql形式创建的: </span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New;">
**<span style="font-size: small;">create table orders (
id int not null auto_increment,
name varchar(100) not null,
email varchar(255) not null,
address text not null,
pay_type char(10) not null,
shipped_at datetime null,
primary key (id)
);</span>**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">我们创建包装这个表的active record类: </span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New;">
**<span style="font-size: small;">require 'rubygems'
require_gem 'activerecord'
# Connection code omitted...
class Order &lt; ActiveRecord::Base
end</span>**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">一旦我们定义了Order类，我们就可以查询到它包含的属性信息。下面的代码使 用了columns()方法，它返回一个Column对象数组。这里我们只显示orders表的每个列的名字，通过一个hash导出shipped_at 这个列的细节。
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**require 'pp'
pp Order.columns.map { |col| col.name }
pp Order.columns_hash['shipped_at']
**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">运行这段代码，我们得到以下输出： </span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New;">
**<span style="font-size: small;">["id", "name", "email", "address", "pay_type", "shipped_at"]
#&lt;ActiveRecord::ConnectionAdapters::Column:0x10e4a50
@default=nil,
@limit=nil,
@name="shipped_at",
@type=:datetime&gt;</span>**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">注意：active record决定每列的类型。在这个例子中，它已经知道数据库中的shipped_at列是一个datetime类型。这个列的值存放的是Ruby的 Time对象。我们可以通过写入一个时间字符串到这个属性，并获取内容来验证。你会发现返回的是Time对象。 </span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**order = Order.new
order.shipped_at = "2005-03-04 12:34"
pp order.shipped_at.class
pp order.shipped_at**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">它会产生：
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**Time
Fri Mar 04 12:34:00 CST 2005**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">sql类型和ruby类型的映射关系如下：
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**sql type                   ruby class
---------------------------------------
int,integer                Fixnum
decimal numeric            Float
interval,date              Date
clob,blob,text             String
float,double               Float
char,varchar,string        String
datetime,time             Time
boolean                    see text**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">这个映射关系是显而易见的。只有一个潜在的问题是decimal列。 schema设计者会使用decimal来存储带有固定小数位的数字--decimal列要求十分精确。active record把decimal映射为Float类.尽管这对大多数程序是可行的,但是浮点数字是不精确的,如果在这个类型的属性上执行一系列操作,可能会 发生四舍五入的问题。你也可能想使用多个integer列，来存储货币值的各单元，分，角，元等。另一种做法是使用聚合来构造Money对象来分离数据库 列（dollars and cents,pounds and pence,等）</span>**

**<span style="font-size: small;"><span style="color: #ff6600;">连接数据库</span>

</span>****<span style="font-size: small;">active record把数据库连接的概念抽象出来,有助于程序处理各种特殊数据库的底层细节.active record程序使用通用的调用,代理了一组数据库适配器的细节.

指定连接的一种方式是使用establish_connection()类方法。举例来说，
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**ActiveRecord::Base.establish_connection(
:adapter =&gt; "mysql",
:host =&gt; "dbserver.com",
:database =&gt; "railsdb",
:username =&gt; "railsuser",
:password =&gt; "railspw"
)**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">这是所有model类共用的缺省连接。
active record支持db2,mysql,oracle,postgres,sqlserver和sqlite数据库。连接是和model类相关的，每个类都 从父类中继承连接。ActiveRecord::base是所有active record类的基类，对于定义的所有active record类设置一个默认的连接。在需要的时侯可以重新定义，覆盖之。

下面的例子，程序的表基本上是在mysql中的online数据库。但由于某种原因，customer表在backend数据库中。
</span>**
<table width="90%" cellspacing="0" cellpadding="10">
<tbody>
<tr>
<td bgcolor="#eeeeee"><span style="font-family: Courier New; font-size: small;">**ActiveRecord::Base.establish_connection(
:adapter =&gt; "mysql",
:host =&gt; "dbserver.com",
:database =&gt; "online",
:username =&gt; "groucho",
:password =&gt; "swordfish")
class LineItem &lt; ActiveRecord::Base
# ...
end
class Order &lt; ActiveRecord::Base
# ...
end
class Product &lt; ActiveRecord::Base
# ...
end
class Customer &lt; ActiveRecord::Base
# ...
end
Customer.establish_connection(
:adapter =&gt; "mysql",
:host =&gt; "dbserver.com",
:database =&gt; "backend",
:username =&gt; "chicho",
:password =&gt; "piano")**</span></td>
</tr>
</tbody>
</table>
**<span style="font-size: small;">[size]我们还可以在config/database.yml的配置文件中 设置连接参数。对多数rails程序这是首选的做法。这样做的好处不仅在于连接信息和代码分离，而且有利于测试和分发。
你还可以结合以上两种做法。传入一个symbol给establish_connection()。rails会载database.yml的一个部分。 这样可以使得数据库连接始终在代码之外。</span>**

</div>