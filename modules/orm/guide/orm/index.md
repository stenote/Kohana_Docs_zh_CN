# ORM

Kohana 的 3.x 中包括一个功能强大的对象关系映射（ORM）的模块, 使用动态记录模式和数据库的自检, 以确定模型的列信息. ORM 是和 [验证](validation) 紧密结合的库 [原文](# 'Kohana 3.x includes a powerful Object Relational Mapping (ORM) module that uses the active record pattern and database introspection to determine a model's column information. ORM is integrated tightly with the [Validation] library.').

ORM 可以像 php 对象一样来 控制 和 操纵 数据库. 只要定义一次映射关系, ORM 就可以在任何你喜欢的地方, 提取, 操纵数据库中的数据, 以及将修改后的结果存放入数据库而不需要使用SQL. 通过创建 遵循 约定由于配置 的模型之间的关系, 许多重复的查询, 创建, 读取, 更新和删除数据库中的信息可以减少或完全消除, ORM 库 可以自动处理所有的映射关系, 您就可以访问相关数据 就像访问 php 标准对象的属性一样 [原文](# 'The ORM allows for manipulation and control of data within a database as though it was a PHP object. Once you define the relationships ORM allows you to pull data from your database, manipulate the data in any way you like, and then save the result back to the database without the use of SQL. By creating relationships between models that follow convention over configuration, much of the repetition of writing queries to create, read, update, and delete information from the database can be reduced or entirely removed. All of the relationships can be handled automatically by the ORM library and you can access related data as standard object properties.').

ORM 默认包含在 Kohana 3.x 的安装包里,但是在使用之前您需要在配置里面启用该模块 [原文](# 'ORM is included with the Kohana 3.x install but needs to be enabled before you can use it.'). 

在文件 `application/bootstrap.php` 修改 [Kohana::modules] 的调用 并包含这个 ORM 模块 [原文](# ' In your `application/bootstrap.php` file modify the call to Kohana::modules and include the ORM modules.').

## 准备开始

在使用ORM之前, 我们必须在模块包含中启动它 [原文](# 'Before we use ORM, we must enable the modules required.')

	Kohana::modules(array(
		...
		'database' => MODPATH.'database',
		'orm' => MODPATH.'orm',
		...
	));


[!!] database 模块是 ORM 模块正常工作的必须前提. 所以 database 模块也必须进行配置使用现有的数据库 [原文](# 'The database module is requried for the ORM module to work. Of course the database module has to be configured to use an existing database.').

你现在就可以尝试创建你自己的 [数据模型](models) 并 [使用 ORM](using).