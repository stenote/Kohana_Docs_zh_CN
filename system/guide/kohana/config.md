# 配置

默认情况下Kohana在级联文件系统从[config files](files/config)安装​​加载配置的值。不管怎样，这样很容
易在别的位置和别的格式去适配配置文件去加载其他配置的值。

（原文#By default kohana is setup to load configuration values from [config files](files/config) in the
cascading filesystem.  However, it is very easy to adapt it to load config values in other 
locations/formats.）

## 源文件（Sources）

系统是围绕着用松散的方法来存储**配置文件**中配置值的思想来设计的。
（原文#The system is designed around the concept of **Config Sources**, which loosely means a method of
storing configuration values.）

你需要一个**配置读取器**来读出源文件中的配置。同样，写入配置到源文件中需要一个**配置写入器**。
（原文#To read config from a source you need a **Config Reader**. Similarly, to write config to a source
you need a **Config Writer**.）

运行下面的内容来扩展接口[Kohana_Config_Reader] / [Kohana_Config_Writer]是很容易的：
（原文#Implementing them is as simple as extending the 
[Kohana_Config_Reader] / [Kohana_Config_Writer] interfaces:）

	class Kohana_Config_Database_Reader implements Kohana_Config_Reader
	class Kohana_Config_Database_Writer extends Kohana_Config_Database_Reader implements Kohana_Config_Writer

在上面的例子中，你会注意到数据库写入器扩展数据库读取器。这是配置源文件的惯例，以此类推，如果你写一个
源文件，可能你也可以从中读取。可是，该公约不是强制的，它允许开发人员自行处理。
（原文#You'll notice in the above example that the Database Writer extends the Database Reader. 
This is the convention with config sources, the reasoning being that if you can write to a 
source chances are you can also read from it as well. However, this convention is not enforced 
and is left to the developer's discretion.）

## 群组（Groups）

为辅助结构配置值来分成逻辑上的"groups" 。例如，数据库相关的设置要在`database`组，会话相关的设置要在
`session`组。
（原文#In order to aide organisation config values are split up into logical "groups".  For example
database related settings go in a `database` group, and session related settings go in a 
`session` group.）

如何存储/组织这些结构来配置源文件。例如文件的源代码将不同的配置组放到不同的文件(`database.php`, `session.php`)，
而数据库的源代码，使用一个列来区分群组。
(原文#How these groups are stored/organised is up to the config source.  For example the file source
puts different config groups into different files (`database.php`, `session.php`) whereas
the database source uses a column to distinguish between groups.）

加载配置组可以简单地用你想加载的群组名来调用`Kohana::$config->load()`：
（原文#To load a config group simply call `Kohana::$config->load()` with the name of the group you wish to load:）

	$config = Kohana::$config->load('my_group');

`load()`将返回一个封装了配置值的[Config_Group] 实例，并确保
任何修改将被传递回配置的写入器。
（原文#`load()` will return an instance of [Config_Group] which encapsulates the config values and ensures
that any modifications made will be passed back to the config writers.）

从对象[Config_Group] 获取配置值，只需调用 [Config_Group::get]:
（原文#To get a config value from a [Config_Group] object simply call [Config_Group::get]:）

	$config = Kohana::$config->load('my_group');
	$value  = $config->get('var');

通过调用[Config_Group::set]来更改值:
(原文#To modify a value call [Config_Group::set]:）

	$config = Kohana::$config->load('my_group');
	$config->set('var', 'new_value');

### getting / setting配置的替代方法
<!-- (Alternative methods for getting / setting config)-->

除了上面所描述的方法外，你也可以使用点描述出配置值的路径，从配置组获取你想要的值：
（原文#In addition to the methods described above you can also access config values using dots to outline a path
from the config group to the value you want:）

	// 配置文件：database.php (原文#Config file: database.php）
	return array(
		'default' => array(
			'connection' => array(
				'hostname' => 'localhost'
			)
		)
	);
	
	// 需要hostname的代码（原文#Code which needs hostname:）
	$hostname = Kohana::$config->load('database.default.connection.hostname');
	

这相当于：
（原文#Which is equivalent to:）

	$config = Kohana::$config->load('database')->get('default');
	
	$hostname = $config['connection']['hostname'];

显然，这种方法是比原来更紧凑了很多，但是请记住，使用`dot.notation`  比调用 `get()`和自己遍历数组
要慢一些。如果你只需要一个特定的变量，点标记法是有用的，但除此之外，最好使用`get()` 。
（原文#Obviously this method is a lot more compact than the original, however please bear in mind that using
`dot.notation` is a lot slower than calling `get()` and traversing the array yourself.  Dot notation
can be useful if you only need one specific variable, but otherwise it's best to use `get()`.）

为[ Config_Group ]扩展[Array_Object](http://php.net/manual/en/class.arrayobject.php)你也可以使用数组
语法来获取/设置配置的值：
（原文#As [Config_Group] extends [Array_Object](http://php.net/manual/en/class.arrayobject.php) you can also use array
syntax to get/set config vars:）

	$config = Kohana::$config->load('database');
	
	// Getting the var
	$hostname = $config['default']['connection']['hostname'];
	
	// Setting the var
	$config['default']['connection']['hostname'] = '127.0.0.1';

同样，这个语法是比调用 `get()` / `set()`代价更高。
Again, this syntax is more costly than calling `get()` / `set()`.

## 配置合并（Config Merging）

配置系统的有用的功能之一是配置组合并。在级联文件系统中以相似的方法工作
，从较低一级源代码的配置分配到源栈之中被合并到上一级堆栈的源代码。（译者注#这句话的意思是将较低层级的
配置合并到它的上一级配置中，依次合并，最后得到的就是一个大的配置文件）
（原文#One of the useful features of the config system is config group merging. This works in a similar way 
to the cascading filesystem, with configuration from lower sources lower down the source stack being 
merged with sources further up the stack.）

如果有两个源代码包含相同的配置变量，那么上一级堆栈的源代码会覆盖“低一级”的源代码。但是，如果更高的
堆栈的源代码不包含一个特定的配置变量，但低一级的源代码那么从较低级别的源代码的值将被使用。
但是，如果更高级别的堆栈源代码并不只包含一个特定的配置变量，而且顺着堆栈源代码，那么从较低级别的源代码的值将被使用。
（原文#If two sources contain the same config variables then the one from the source further up the stack will
override the one from the "lower" source.  However, if the source from higher up the stack does not contain
a particular config variable but a source lower down the stack does then the value from the lower source will
be used.）

源代码在堆栈中的位置是由在你的bootstrap文件中如何加载来决定。默认情况下，当你加载一个源文件它将被推到堆栈的前端：
（原文#The position of sources in the stack is determined by how they are loaded in your bootstrap.
By default when you load a source it is pushed to the top of a stack:）

    // Stack: <empty>
	Kohana::$config->attach(new Config_File);
	// Stack: Config_File
	Kohana::$config->attach(new Config_Database);
	// Stack: Config_Database, Config_File

在上面的例子，在数据库中发现的任何配置的值将覆盖在文件系统中发现的值。
例如，使用上面所述的设置：
（原文#In the example above, any config values found in the database will override those found in the filesystem.
For example, using the setup outlined above:）

	// 在文件系统中的配置(原文#Configuration in the filesystem:）
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// 在数据库中的配置（原文#Configuration in the database:）
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// 由Kohana::$config->load('email')返回的配置：（原文#Configuration returned by Kohana::$config->load('email')）
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
			method: smtp

[!!] **注意** 上面的语法是用简单的虚拟代码来说明配置合并的概念。
**Note:** The above syntax is simply pseudo code to illustrate the concept of config merging.

在某些情况下，你可能要追加一个配置源代码到栈底，通过传递` FALSE `作为第二个参数到`attach()`可以做到这一点：
（原文#On some occasions you may want to append a config source to the bottom of the stack, to do this pass `FALSE`
as the second parameter to `attach()`:）

	// Stack: <empty>
	Kohana::$config->attach(new Config_File);
	// Stack: Config_File
	Kohana::$config->attach(new Config_Database, FALSE);
	// Stack: Config_File, Config_Database

在这个例子中，在文件系统中发现的任何值将覆盖在db中发现的。例如：
(原文#In this example, any values found in the filesystem will override those found in the db. For example:）

	// Configuration in the filesystem:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// Configuration in the database:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Configuration returned by Kohana::$config->load('email')
		email:
			sender:
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp

## 根据环境使用不同的配置源
<!--（#Using different config sources based on the environment）-->

在某些情况下，您将需要使用不同的配置值，取决于`Kohana::$environment`的状态。单元测试是这种情况的一个最好
的例子。大多数设置有两个数据库;一个正常的开发版和为单元测试单独的一个（从你的开发版中分离出测试版）。
（原文#In some situation's you'll need to use different config values depending on which state `Kohana::$environment`
is in. Unit testing is a prime example of such a situation. Most setups have two databases; one for normal
development and a separate one for unit testing (to isolate the tests from your development).)

在这种情况下，你仍然需要访问在`config`目录中存储的配置设置，因为它包含你的应用程序在任何环境所需要的
通用设置（如加密设置），取代默认的`Config_File` 源不是一个好的选择。
（原文#In this case you still need access to the config settings stored in the `config` directory as it contains generic
settings that are needed whatever environment your application is in (e.g. encryption settings),
to replacing the default `Config_File` source isn't really an option.）

为了解决这个问题，您可以附加一个单独的从 `config`的子目录中加载配置"testing"的配置文件阅读器：
（原文#To get around this you can attach a separate config file reader which loads its config from a subdir of `config` called
"testing":）

	Kohana::$config->attach(new Config_File);
	
	Kohana::$config->attach(new Config_Database);
	
	if (Kohana::$environment === Kohana::TESTING)
	{
		Kohana::$config->attach(new Config_File('config/testing'));
	}

在正常的开发中，配置源栈看起来像`Config_Database, Config_File('config')` 。然而，当`Kohana::$environment === Kohana::TESTING`
时，栈看起来像`Config_File('config/testing'), Config_Database, Config_File('config')`。

During normal development the config source stack looks like `Config_Database, Config_File('config')`.  However,
when `Kohana::$environment === Kohana::TESTING` the stack looks like `Config_File('config/testing'), Config_Database, Config_File('config')`