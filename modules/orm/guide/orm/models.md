# 创建您自己的模型

如果要为您数据库中的 `members` 表创建模型, 可以创建文件 `application/classes/model/member.php` , 文件内容如下 [原文](# 'To create a model for the table `members` in your database, create the file `application/classes/model/member.php` with the following syntax'):

	class Model_Member extends ORM
	{
		...
	}

(更多的例子...wait..)

## 重载表名

如果你想该表数据库中使用模型的表明, 可以这样重写 `$_table_name` 这个模型属性 [原文](# 'If you wish to change the database table that a model uses, just override the `$_table_name` variable like this'):

	protected $_table_name = 'strange_tablename';

## 修改默认的主键

ORM 假设创建的 模型 和 数据表 的列中都有一个 已索引的、唯一的 列 `id`. 如果您创建的 模型 和 数据表 主键不是 `id`, 没关系~ 只需要这样重写 `$_primary_key` 这个模型属性 [原文](# 'ORM assumes each model (and database table) has an `id` column that is indexed and unique. If your primary key column isn't named `id`, that's fine - just override the `$_primary_key` variable like this'):

	protected $_primary_key = 'strange_pkey';

## 使用非默认数据库

在任何模型中, 您可以定义 ORM 使用哪个数据库进行查询操作. 如果重写了 `$_db_group` 这个模型属性, ORM 就会自动连接这个非默认数据库.例 [原文](# 'For each model, you can define which database configuration ORM will run queries on. If you override the `$_db_group` variable in your model, ORM will connect to that database. Example'):

	protected $_db_group = 'alternate';