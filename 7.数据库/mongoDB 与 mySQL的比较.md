 -|Mongodb |Mysql
--|--|--
数据库模型 | 非关系型 | 关系型 
存储方式 | 虚拟内存+持久化 | 不同引擎有不同的存储方式
查询语句 | 独特的Mongodb查询方式 | 传统sql语句
架构特点 | 通过副本集以及分片实现高可用 | 常见有单点，M-S，MHA，MMM等架构方式
数据处理方式 | 基于内存，将热数据存在物理内存中，从而达到高速读写 | 不同引擎拥有自己的特点
成熟度 | 新兴数据库，成熟度较低 | 拥有较为成熟的体系，成熟度较高
广泛度 | Nosql数据库中mongodb是较为完善的DB之一，使用人群也在不断增长 | 开源数据库的份额在不断增加，mysql 的份额也在持续增长

SQL术语/概念|MongoDB术语/概念 |解释/说明
--|--|--
database | database | 数据库
table | collection | 数据库表/集合
row | document | 数据记录行/文档
column | field | 数据字段/域
index | index | 索引
table joins | | 表连接，MongoDB 不支持
primary key | primarykey | 主键，MongoDB自动将_id字段设置为主键

