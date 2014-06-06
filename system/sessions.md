# 会话（Sessions）

Kohana的提供的类，可以很容易的在cookie和session中工作。高层次的sessions和cookies都提供了相同的功能。它们允许开发者为以后取回来存储临时的或持久的特定客户端的信息，通常是为了使一些东西能在请求之间持续。
（原文#Kohana provides classes that make it easy to work with both cookies and sessions. At a high level both sessions and cookies provide the same functionality. They allow the developer to store temporary or persistent information about a specific client for later retrieval, usually to make something persistent between requests.）

Session用来存储临时或私人的数据。非常敏感的数据需要用[Session]类的"database"或"native"适配器来存储。当使用"cookie"适配器的时候，session应始终加密。
（原文#Sessions should be used for storing temporary or private data.  Very sensitive data should be stored using the [Session] class with the "database" or "native" adapters. When using the "cookie" adapter, the session should always be encrypted.）

[!!]对于session变量的最佳做法的更多信息，请参见[7会话七宗罪](http://lists.nyphp.org/pipermail/talk/2006-December/020358.html)。
（原文#For more information on best practices with session variables see [the seven deadly sins of sessions](http://lists.nyphp.org/pipermail/talk/2006-December/020358.html).）

## 存储，取回和删除数据（原文#Storing, Retrieving, and Deleting Data）

[Cookie]和[Session]为数据存储提供了一个非常相似的API。它们之间的主要不同是sessions用对象来访问，而cookies用静态类来访问。
（原文#[Cookie] and [Session] provide a very similar API for storing data. The main difference between them is that sessions are accessed using an object, and cookies are accessed using a static class.）

用[Session::instance]方法来访问session实例:
(原文#Accessing the session instance is done using the [Session::instance] method:）

    // 获得session实例（原文#Get the session instance）
    $session = Session::instance();

当在使用sessions时，你也可以用[Session::as_array]方法获得当前的session数据：
（原文#When using sessions, you can also get all of the current session data using the [Session::as_array] method:）

    // 用数组来获取session的所有数据（原文#Get all of the session data as an array）
    $data = $session->as_array();

你也可以用此重载`$_SESSION`，用类似于标准php的方法全局的获取和设置数据：
（原文#You can also use this to overload the `$_SESSION` global to get and set data in a way more similar to standard PHP:）

    // 用session数据重载$_SESSION（原文#Overload $_SESSION with the session data）
    $_SESSION =& $session->as_array();
    
    // 设置session数据（原文#Set session data）
    $_SESSION[$key] = $value;

### 存储数据（原文#Storing Data)

存储session或cookie数据是用`set`方法：
（原文#Storing session or cookie data is done using the `set` method:)

    // 设置session数据（原文#Set session data）
    $session->set($key, $value);
	// 或（Or）
	Session::instance()->set($key, $value);

    // 存储一个用户的id（原文#Store a user id）
    $session->set('user_id', 10);

### 取回数据（原文#Retrieving Data)

获得session或cookie数据是用`get`方法：
（原文#etting session or cookie data is done using the `get` method:)

    // 获取session数据（原文#Get session data）
    $data = $session->get($key, $default_value);

    // 获取用户的id(原文#Get the user id）
    $user = $session->get('user_id');

### 删除数据（原文#Deleting Data）

删除session或cookie数据是用`delete`方法：
（原文#Deleting session or cookie data is done using the `delete` method:）

    // 删除seesion数据（原文#Delete session data）
    $session->delete($key);


    // 删除用户的id(原文#Delete the user id）
    $session->delete('user_id');

## Session的配置（原文#Session Configuration）

在你的应用程序运行之前，务必检查这些设置，因为它们当中很多都会直接影响到你的应用程序的安全。
（原文#Always check these settings before making your application live, as many of them will have a direct affect on the security of your application.）

## Session适配器（原文#Session Adapters）

当在创建或访问一个[Session]类实例的时候，你可以决定你想使用哪一个session适配器或驱动。提供给你的session适配器：
（原文#When creating or accessing an instance of the [Session] class you can decide which session adapter or driver you wish to use. The session adapters that are available to you are:）

Native
: 在你的Web服务器的默认位置存储session数据。存储位置通过`php.ini`[session.save_path](http://php.net/manual/session.configuration.php#ini.session.save-path)来定义或通过[ini_set](http://php.net/ini_set)来定义。
(原文#Stores session data in the default location for your web server. The storage location is defined by [session.save_path](http://php.net/manual/session.configuration.php#ini.session.save-path) in `php.ini` or defined by [ini_set](http://php.net/ini_set).）

Database
: 用[Session_Database]类存储session数据到数据库表。需要启用[Database]模块。
（原文#Stores session data in a database table using the [Session_Database] class. Requires the [Database] module to be enabled.）

Cookie
: 用[Cookie]类存储session数据到一个cookie。**在使用该适配器时，Sessions有一个4KB的限制，并且应该被加密。**
（原文#Stores session data in a cookie using the [Cookie] class. **Sessions will have a 4KB limit when using this adapter, and should be encrypted.**）

默认的适配器可以通过改变[Session::$default]的值来设置。默认的适配器是"native"。
（原文#The default adapter can be set by changing the value of [Session::$default]. The default adapter is "native".）

用默认的适配器来访问Session,只需简单地调用[Session::instance()]。通过传递适配器名称到`instance()`，用默认以外的东西来访问一个Session，例如：`Session::instance('cookie')`
（原文#To access a Session using the default adapter, simply call [Session::instance()].  To access a Session using something other than the default, pass the adapter name to `instance()`, for example: `Session::instance('cookie')`）


### Session适配器设置（原文#Session Adapter Settings）

通过创建一个`APPPATH/config/session.php`的session配置文件，你可以应用配置设置到每个session的适配器。下面的示例配置文件为每个适配器定义所有设置：
（原文#You can apply configuration settings to each of the session adapters by creating a session config file at `APPPATH/config/session.php`. The following sample configuration file defines all the settings for each adapter:）

[!!] 正如cookies， "lifetime"设置为"0"表示本次session将在浏览器关闭时终止。
（原文#As with cookies, a "lifetime" setting of "0" means that the session will expire when the browser is closed.）

    return array(
        'native' => array(
            'name' => 'session_name',
            'lifetime' => 43200,
        ),
        'cookie' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
        ),
        'database' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
            'group' => 'default',
            'table' => 'table_name',
            'columns' => array(
                'session_id'  => 'session_id',
        		'last_active' => 'last_active',
        		'contents'    => 'contents'
            ),
            'gc' => 500,
        ),
    );

#### Native Adapter

类型(Type)      | 设置(Setting)   | 描述(Description)                                                   | 默认值(Default)
----------------|-----------------|---------------------------------------------------------------------|-----------------
`string`        | name            | session的名字(name of the session)                                  | `"session"`
`integer`       | lifetime        | session应该持续的秒数(number of seconds the session should live for)| `0`

#### Cookie Adapter

类型(Type)      | 设置(Setting)   | 描述(Description)                                        | Default
----------------|-----------------|----------------------------------------------------------|-----------------
`string`        | name            | name of the cookie used to store the session data        | `"session"`
`boolean`       | encrypted       | 用[Encrypt]加密session数据？(encrypt the session data using [Encrypt]? )| `FALSE`
`integer`       | lifetime        | number of seconds the session should live for            | `0`

#### Database Adapter

Type      | Setting   | Description                                       | Default
----------|-----------|---------------------------------------------------|-----------
`string`  | group     | [Database::instance]组名([Database::instance] group name)| `"default"`
`string`  | table     | 存储session的表名table name to store sessions in    | `"sessions"`
`array`   | columns   | 关联数组的列别名associative array of column aliases | `array`
`integer` | gc        | 1:x的可能性会运行垃圾收集器（1:x chance that garbage collection will be run）    | `500`
`string`  | name      | name of the cookie used to store the session data | `"session"`
`boolean` | encrypted | encrypt the session data using [Encrypt]?         | `FALSE`
`integer` | lifetime  | number of seconds the session should live for     | `0`

##### 表的概要（Table Schema）

你需要在数据库中创建session存储表。这是一个默认的概要：
（原文#You will need to create the session storage table in the database. This is the default schema:）

    CREATE TABLE  `sessions` (
        `session_id` VARCHAR(24) NOT NULL,
        `last_active` INT UNSIGNED NOT NULL,
        `contents` TEXT NOT NULL,
        PRIMARY KEY (`session_id`),
        INDEX (`last_active`)
    ) ENGINE = MYISAM;

##### 表的列（Table Columns）

当连接到一个旧的session表，你可以更改列名去匹配现有的数据库模式。默认值和键值相同。
（原文#You can change the column names to match an existing database schema when connecting to a legacy session table. The default value is the same as the key value.）

session_id
: "id"列的名称（原文#the name of the "id" column）

last_active
: session最后一次进行更新的UNIX时间戳（原文#UNIX timestamp of the last time the session was updated）

contents
: 用一个序列化的字符串存储session数据，并选择性地加密（原文#session data stored as a serialized string, and optionally encrypted）