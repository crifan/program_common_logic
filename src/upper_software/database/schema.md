# schema模式

之前在数据库领域内就看到和提到了schema

[MongoDB vs. Redis Comparison](https://db-engines.com/en/system/MongoDB;Redis)
[architecture - When to Redis? When to MongoDB? - Stack Overflow](https://stackoverflow.com/questions/5400163/when-to-redis-when-to-mongodb)

想要去搞清楚，什么是schema

什么是schema

[在数据库中，schema、catalog分别指的是什么？ - 知乎](https://www.zhihu.com/question/20355738)
> 对于mysql，schema和database可以理解为等价的.
> 
> As defined in the MySQL Glossary:
> In MySQL, physically, a schema is synonymous with a database. You can substitute the keyword SCHEMA instead of DATABASE in MySQL SQL syntax, for example using CREATE SCHEMA instead of CREATE DATABASE.
> 
> Some other database products draw a distinction. For example, in the Oracle Database product, a schema represents only a part of a database: the tables and other objects owned by a single user.
> 
> schema就是数据库对象的集合，这个集合包含了各种对象如：表、视图、存储过程、索引等。
> 
> 如果把database看作是一个仓库，仓库很多房间（schema），一个schema代表一个房间，table可以看作是每个房间中的储物柜，user是每个schema的主人，有操作数据库中每个房间的权利，就是说每个数据库映射user有每个schema（房间）的钥匙。
> 
> schema是对一个数据库的结构描述。在一个关系型数据库里面，schema定义了表、每个表的字段，还有表和字段之间的关系。
catalog是由一个数据库实例的元数据组成的，包括基本表，同义词，索引，用户等等。”

[Difference Between Schema / Database in MySQL - Stack Overflow](https://stackoverflow.com/questions/11618277/difference-between-schema-database-in-mysql)
> Depends on the database server. MySQL doesn't care, its basically the same thing.
Oracle, DB2, and other enterprise level database solutions make a distinction. Usually a schema is a collection of tables and a Database is a collection of schemas.

[Schema（数据库中的Schema）_百度百科](https://baike.baidu.com/item/Schema/15286221)
> A schema is a collection of database objects (used by a user.).
> schema objects are the logical structures that directly refer to the database’s data.
> A user is a name defined in the database that can connect to and access objects.
> schemas and users help database administrators manage database security.”

[数据库中Schema（模式）概念的理解](https://www.biaodianfu.com/database-schema.html)
> 在学习数据库时，会遇到一个让人迷糊的Schema的概念。实际上，schema就是数据库对象的集合，这个集合包含了各种对象如：表、视图、存储过程、索引等。
> 
> 如果把database看作是一个仓库，仓库很多房间（schema），一个schema代表一个房间，table可以看作是每个房间中的储物柜，user是每个schema的主人，有操作数据库中每个房间的权利，就是说每个数据库映射的user有每个schema（房间）的钥匙。
>
> 默认情况下一个用户对应一个集合，用户的schema名等于用户名，并作为该用户缺省schema。所以schema集合看上去像用户名。访问一个表时，如果没有指明该表属于哪个schema，系统会自动加上缺省的schema。一个对象的完整名称为schema.object，而不属user.object。
> 
> 在MySQL中创建一个Schema和创建一个Database的效果好像是一样的，但是在SQL Server和Oracle数据库中效果又是不同的。

