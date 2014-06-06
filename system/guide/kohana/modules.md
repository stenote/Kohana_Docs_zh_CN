# 模块（Modules）

Modules只是一个附加的[级联文件系统](files)。一个Module可以(通过 [Kohana::find_file])增加任何类型的文件(controllers, views, classes, config files等.)到Kohana文件系统。传输应用程序的任何部分或者在不同的应用程序之间共享。例如，创建一个新的模型系统、搜索引擎、css/js管理系统等。
（原文#Modules are simply an addition to the .  A module can add any kind of file (controllers, views, classes, config files, etc.) to the filesystem available to Kohana (via [Kohana::find_file]).  This is useful to make any part of your application more transportable or shareable between different apps.  For example, creating a new modeling system, a search engine, a css/js manager, etc.）

## 哪里可以找到modules（Where to find modules）

Kolanos创建了[kohana-universe](http://github.com/kolanos/kohana-universe/tree/master/modules/),一个相当全面的modules列表，它托管在Github上面。要让你的module列在那里，可以通过Github发一条信息给他。
（原文#Kolanos has created [kohana-universe](http://github.com/kolanos/kohana-universe/tree/master/modules/), a fairly comprehensive list of modules that are available on Github. To get your module listed there, send him a message via Github.）

Mon Geslani创建了一个[很不错的网站](http://kohana.mongeslani.com/)，它允许你按activity, watchers, forks等排列Github上面的modules。它似乎没有像Kohana-universe那样的全面。
（原文#Mon Geslani created a [very nice site](http://kohana.mongeslani.com/) that allows you to sort Github modules by activity, watchers, forks, etc.  It seems to not be as comprehensive as kohana-universe.）

Andrew Hutchings创建了[kohana-modules](http://www.kohana-modules.com)，它和上面的网站很相似。
（原文#Andrew Hutchings has created [kohana-modules](http://www.kohana-modules.com) which is similar to the above sites.）

## 开启模块（Enabling modules）

Modules通过调用[Kohana::modules]和传递一个`'name' => 'path'`数组被开启。name不是很重要，但是path绝对需要。一个module的路径不一定在`MODPATH`里面，但一般是那样。你只可以调用[Kohana::modules]一次。
（原文#Modules are enabled by calling [Kohana::modules] and passing an array of `'name' => 'path'`.  The name isn't important, but the path obviously is.  A module's path does not have to be in `MODPATH`, but usually is.  You can only call [Kohana::modules] once.）

	Kohana::modules(array(
		'auth'       => MODPATH.'auth',       // 基础的身份验证（Basic authentication）
		'cache'      => MODPATH.'cache',      // 多种后台缓存（Caching with multiple backends）
		'codebench'  => MODPATH.'codebench',  // 基准工具（Benchmarking tool）
		'database'   => MODPATH.'database',   // 数据库访问（Database access）
		'image'      => MODPATH.'image',      // 图像处理（Image manipulation）
		'orm'        => MODPATH.'orm',        // 对象关系映射（Object Relationship Mapping）
		'oauth'      => MODPATH.'oauth',      // OAuth认证（OAuth authentication）
		'pagination' => MODPATH.'pagination', // 分页（Paging of results）
		'unittest'   => MODPATH.'unittest',   // 单元测试（Unit testing）
		'userguide'  => MODPATH.'userguide',  // 用户指南与API文档（User guide and API documentation）
		));

## Init.php

当一个module被激活时，如果有一个`init.php`文件在module的目录下，那么它就被包含了。理想情况下，有一个module包含使module正常运行的路由或其他初始化。Userguide和Codebench模块有init.php 文件，你可以去看看。
（原文#When a module is activated, if an `init.php` file exists in that module's directory, it is included.  This is the ideal place to have a module include routes or other initialization necessary for the module to function.  The Userguide and Codebench modules have init.php files you can look at.）

## 模块怎样工作（How modules work）

一个在已启用的module中的文件，在应用程序文件夹的同一个地方有确切的文件几乎相同。主要的区别在于，它可以通过[级联文件系统](files)在一个较高的位置（模块启用后，或在应用程序文件夹）用名称相同的文件来覆盖掉。
（原文#A file in an enabled module is virtually the same as having that exact file in the same place in the application folder.  The main difference being that it can be overwritten by a file of the same name in a higher location (a module enabled after it, or the application folder) via the [Cascading Filesystem](files).  It also provides an easy way to organize and share your code.）

## 创建你自己的模块（Creating your own module）

要创建一个module只要创建一个文件夹（通常在`DOCROOT/modules`中），将你想要成为module的文件放在那里，然后在你的bootstrap文件中激活那个module。如果要分享你的module，你可以上传它到[Github](http://github.com)。你可以看到[Kohana](http://github.com/kohana)或[其他用户](#where-to-find-modules)制作的modules的例子。
（原文#To create a module simply create a folder (usually in `DOCROOT/modules`) and place the files you want to be in the module there, and activate that module in your bootstrap.  To share your module, you can upload it to [Github](http://github.com).  You can look at examples of modules made by [Kohana](http://github.com/kohana) or [other users](#where-to-find-modules).）