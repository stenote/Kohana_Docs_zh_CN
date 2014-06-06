# Configuration （配置）

默认配置文件路径为 `MODPATH/database/config/database.php`，可以复制该文件到 `APPPATH/config/database.php`并在新路径修改文件，同时需保持符合[级联文件系统](../kohana/files)规则。
[原文](# 'The default config file is located in `MODPATH/database/config/database.php`.  You should copy this file to `APPPATH/config/database.php` and make changes there, in keeping with the [cascading filesystem](../kohana/files).')

数据库配置信息文件包含一个配置信息组的数组。每一个单一的数据库配置组结构，称之为instance (单例)，例如：
[原文](# 'The database configuration file contains an array of configuration groups. The structure of each database configuration group, called an "instance", looks like this:')

    string INSTANCE_NAME => array(
        'type'         => string DATABASE_TYPE,
        'connection'   => array CONNECTION_ARRAY,
        'table_prefix' => string TABLE_PREFIX,
        'charset'      => string CHARACTER_SET,
        'profiling'    => boolean QUERY_PROFILING,
    ),
	
了解每一项配置信息是非常有必要的。	
[原文](# 'Understanding each of these settings is important.')

INSTANCE_NAME （单例名称）
:  链接名称可以设定为任何你想设定的名称，但是你需要至少设定一个名称为"default"的链接。
[原文](# ':  Connections can be named anything you want, but you should always have at least one connection called "default".')

DATABASE_TYPE （数据库类型）
:  一个已安装的数据库驱动。Kohana已包含"mysql"和"pdo"的驱动。驱动文件必须继承 Database class.
[原文](# ':  One of the installed database drivers. Kohana comes with "mysql" and "pdo" drivers.  Drivers must extend the Database class.')

CONNECTION_ARRAY (连接数组)
: 为数据库链接设定明确的驱动选项。驱动选项详见[下述说明](#connection-settings)
[原文](# ':  Specific driver options for connecting to your database. (Driver options are explained [below](#connection-settings).)')

TABLE_PREFIX (表名前缀)
: 系统会通过[查询创建器(query builder)](#query_bilding)，为所有表明增加前缀名称。 Prepared statements将**不会**使用表名前缀。
[原文](# ':  Prefix that will be added to all table names by the [query builder](#query_building). Prepared statements will **not** use the table prefix.')

QUERY_PROFILING (查询分析)
:  用于对数据库查询，开启[查询分析](../kohana/profiling)。该功能主要用于查看单一页面数据库查询次数以及分析最长加载时间。你需要开启profiler用于查看查询分析状态。
[原文](# ':  Enables [profiling](../kohana/profiling) of database queries.  This is useful for seeing how many queries each page is using, and which are taking the longest.  You must enable the profiler the view these stats.')

## Example （例子）

以下示例文件包含2个MySQL数据库连接，一个名为"local"，一个名为"remote"
[原文](# 'The example file below shows 2 MySQL connections, one local and one remote.')

    return array
    (
        'default' => array
        (
            'type'       => 'mysql',
            'connection' => array(
                'hostname'   => 'localhost',
                'username'   => 'dbuser',
                'password'   => 'mypassword',
                'persistent' => FALSE,
                'database'   => 'my_db_name',
            ),
            'table_prefix' => '',
            'charset'      => 'utf8',
            'profiling'    => TRUE,
        ),
        'remote' => array(
            'type'       => 'mysql',
            'connection' => array(
                'hostname'   => '55.55.55.55',
                'username'   => 'remote_user',
                'password'   => 'mypassword',
                'persistent' => FALSE,
                'database'   => 'my_remote_db_name',
            ),
            'table_prefix' => '',
            'charset'      => 'utf8',
            'profiling'    => TRUE,
        ),
    );

## Connections and Instances （配置集和单例集）

每个配置组可转换为一个数据库单例。每个单例可通过[Database::instance]访问到。如果你不传递参数，则会默认调用'default'配置。
[原文](# 'Each configuration group is referred to as a database instance. Each instance can be accessed by calling [Database::instance].  If you don't provide a parameter, the default instance is used.')

	// This would connect to the database defined as 'default'
    $default = Database::instance();
	
	// This would connect to the database defined as 'remote'
    $remote  = Database::instance('remote');

如果想要断开数据库，简单地销毁连接对象即可：
[原文](# 'To disconnect the database, simply destroy the object:')

    unset($default)
	
	// Or
	
	unset(Database::$instances['default']);

如果你想一次性断开所有数据库单例，可清空Database::$instances
[原文](# 'If you want to disconnect all of the database instances at once:')

    Database::$instances = array();

## Connection Settings

不同的数据库驱动需要不同的链接设置
[原文](# 'Every database driver has different connection settings.')

### MySQL

[MySQL 数据库](http://www.php.net/manual/en/book.mysql.php) 可在`connection`数组中，对如下项目进行设定：
[原文](# 'A [MySQL database](http://www.php.net/manual/en/book.mysql.php) can accept the following options in the `connection` array:')

Type      | Option     |  Description               | Default value
----------|------------|----------------------------| -------------------------
`string`  | hostname   | Hostname of the database   | `localhost`
`integer` | port       | Port number                | `NULL`
`string`  | socket     | UNIX socket                | `NULL`
`string`  | username   | Database username          | `NULL`
`string`  | password   | Database password          | `NULL`
`boolean` | persistent | Persistent connections     | `FALSE`
`string`  | database   | Database name              | `kohana`

### PDO
[PDO 数据库](http://php.net/manual/en/book.pdo.php) 可在`connection`数组中，对如下项目进行设定：
[原文](# 'A [PDO database](http://php.net/manual/en/book.pdo.php) can accept these options in the `connection` array:')

Type      | Option     |  Description               | Default value
----------|------------|----------------------------| -------------------------
`string`  | dsn        | PDO data source identifier | `localhost`
`string`  | username   | Database username          | `NULL`
`string`  | password   | Database password          | `NULL`
`boolean` | persistent | Persistent connections     | `FALSE`

[!!] 如果你使用PDO，但是并不清楚如何使用`dsn`选项，请查看[PDO::__construct](http://php.net/pdo.construct)
[原文](# 'If you are using PDO and are not sure what to use for the `dsn` option, review [PDO::__construct](http://php.net/pdo.construct).')