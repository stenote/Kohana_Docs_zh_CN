# 引导程序（Bootstrap）

引导程序位于`application/bootstrap.php`。它负责设置Kohana的环境和执行的主要反应。它被包含在`index.php`(请看 [Request flow](flow))
（原文#The bootstrap is located at `application/bootstrap.php`.  It is responsible for setting up the Kohana environment and executing the main response. It is included by `index.php` (see [Request flow](flow))）

[!!] 引导程序负责你的应用程序的流程。在Kohana之前的版本中引导程序在`system`文件夹，有些内容是看不见的，且无法编辑。在Kohana 3中，引导程序需要一个更加完整和通用的作用。不管怎样，只要你认为是合适的，就不要害怕编辑和修改您的引导程序。
（原文#The bootstrap is responsible for the flow of your application.  In previous versions of Kohana the bootstrap was in `system` and was somewhat of an unseen, uneditible force.  In Kohana 3 the bootstrap takes on a much more integral and versatile role.  Do not be afraid to edit and change your bootstrap however you see fit.）

## 环境设置（Environment setup）

首先，引导程序设置时区和语言环境，增加Kohana的autoloader，那么[级联文件系统](files)可以工作了。你可以增加任何在你的应用程序中所需的所有的其他设置。
（原文#First the bootstrap sets the timezone and the locale, and adds Kohana's autoloader so the [cascading filesystem](files) works.  You could add any other settings that all your application needed here.）

~~~
// 精简过的节选于带注释的bootstrap.php的范例。（原文#Sample excerpt from bootstrap.php with comments trimmed down）

// 设置默认时区（原文#Set the default time zone.）
date_default_timezone_set('America/Chicago');
 
// 设置默认的语言环境（原文#Set the default locale.）
setlocale(LC_ALL, 'en_US.utf-8');
 
// 开启Kohana的auto-loader(原文#Enable the Kohana auto-loader.）
spl_autoload_register(array('Kohana', 'auto_load'));
 
// 为unserialization开启Kohana的auto-loader（原文#Enable the Kohana auto-loader for unserialization.）
ini_set('unserialize_callback_func', 'spl_autoload_call');
~~~

## 初始化和配置（Initialization and Configuration）

Kohana是通过调用[Kohana::init]来初始化，log和[config](files/config) reader/writers被开启。
(原文#Kohana is then initialized by calling [Kohana::init], and the log and [config](files/config) reader/writers are enabled. ）

~~~
// 精简过的节选于带注释的bootstrap.php的范例。（Sample excerpt from bootstrap.php with comments trimmed down）

Kohana::init(array('
    base_url' => '/kohana/',
	index_file => false,
));

// 将文件写入器附加到日志记录。支持多写入器。（原文#Attach the file writer to logging. Multiple writers are supported.）
Kohana::$log->attach(new Kohana_Log_File(APPPATH.'logs'));

// 将文件读取器附加到配置。支持多读取器。（原文#Attach a file reader to config. Multiple readers are supported.）
Kohana::$config->attach(new Kohana_Config_File);
~~~

你可以添加条件语句使引导程序在某些设置上有不同的值。例如，通过检查`$_SERVER['HTTP_HOST']`和设置相应的缓存、分析等，检测我们是否设置起效。这只是一个例子，有许多不同的方法来完成同样的事情。
（原文#You can add conditional statements to make the bootstrap have different values based on certain settings.  For example, detect whether we are live by checking `$_SERVER['HTTP_HOST']` and set caching, profiling, etc. accordingly.  This is just an example, there are many different ways to accomplish the same thing.）

~~~
// 摘录自http://github.com/isaiahdw/kohanaphp.com/blob/f2afe8e28b/application/bootstrap.php（原文#Excerpt from http://github.com/isaiahdw/kohanaphp.com/blob/f2afe8e28b/application/bootstrap.php
... [trimmed]）
 
/**
 * 用域名来设置环境状况（原文#Set the environment status by the domain.）
 */
if (strpos($_SERVER['HTTP_HOST'], 'kohanaphp.com') !== FALSE)
{
	// 设置起效！（We are live!）
	Kohana::$environment = Kohana::PRODUCTION;
 
	// 关闭notices和严格的errors（原文#Turn off notices and strict errors）
	error_reporting(E_ALL ^ E_NOTICE ^ E_STRICT);
}
 
/**
 * 初始化Kohana,设置默认选项（原文#Initialize Kohana, setting the default options.）
 ... [trimmed]
 */
Kohana::init(array(
	'base_url'   => Kohana::$environment === Kohana::PRODUCTION ? '/' : '/kohanaphp.com/',
	'caching'    => Kohana::$environment === Kohana::PRODUCTION,
	'profile'    => Kohana::$environment !== Kohana::PRODUCTION,
	'index_file' => FALSE,
));

... [trimmed]

~~~

[!!] 注意：默认的引导程序将设置`Kohana::$environment = $_ENV['KOHANA_ENV']`，如果设置了。在你的Web服务器的文档(例如[Apache](http://httpd.apache.org/docs/1.3/mod/mod_env.html#setenv), [Lighttpd](http://redmine.lighttpd.net/wiki/1/Docs:ModSetEnv#Options))，文件对如何提供此变量都可用。比许多其他的方法来设置`Kohana::$enviroment`，这被认为是更好的做法，因此你可以更改每台服务器的设置，而无需依靠配置选项或主机名。
（原文#Note: The default bootstrap will set `Kohana::$environment = $_ENV['KOHANA_ENV']` if set. Docs on how to supply this variable are available in your web server's documentation (e.g. [Apache](http://httpd.apache.org/docs/1.3/mod/mod_env.html#setenv), [Lighttpd](http://redmine.lighttpd.net/wiki/1/Docs:ModSetEnv#Options)). This is considered better practice than many alternative methods to set `Kohana::$enviroment`, as you can change the setting per server, without having to rely on config options or hostnames.）

## 模块（Modules）

**阅读[Modules](modules)页面获取更详细的介绍.（原文#Read the [Modules](modules) page for a more detailed description.)**

通过使用[Kohana::modules()]加载[Modules](modules)。已经包含的modules是可选的。
（原文#[Modules](modules) are then loaded using [Kohana::modules()].  Including modules is optional.  ）

数组里面的每个key应该是module的name,该值是module的路径，相对的或绝对的。
（原文#Each key in the array should be the name of the module, and the value is the path to the module, either relative or absolute.）
~~~
// 摘录自bootstrap.php的例子（原文#Example excerpt from bootstrap.php)

Kohana::modules(array(
	'database'   => MODPATH.'database',
	'orm'        => MODPATH.'orm',
	'userguide'  => MODPATH.'userguide',
));
~~~

## 路由（Routes）

**阅读[Routing](routing)页面获取更多详细的介绍和例子（原文#Read the [Routing](routing) page for a more detailed description and more examples.）**

[Routes](routing) are then defined via [Route::set()].  

~~~
// 来自Kohana 3中的默认路由（The default route that comes with Kohana 3）
Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
~~~