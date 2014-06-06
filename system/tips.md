# 技巧和常见误区(Tips and Common Mistakes)

这是一个你可能会遇到的技巧和常见误区或错误的集合。
(原文#This is a collection of tips and common mistakes or errors you may encounter. ）

## 切勿编辑`system`文件夹！（Never edit the `system` folder!）

你（几乎）不要修改系统文件夹。你想在system 和modules下文件的任何改变可以通过[级联文件系统](files)和[transparent extension](extension)，当你尝试更新您的Kohana的版本时，不会被破坏。
(原文#You should (almost) never edit the system folder.  Any change you want to make to files in system and modules can be made via the [cascading filesystem](files) and [transparent extension](extension) and won't break when you try to update your Kohana version.）

## 不要试图对所有东西用同一个路由（Don't try and use one route for everything）

Kohana 3的[路由](routgin)非常的强大和灵活，不要怕使用频繁，因为你需要使你的应用程序按你想要的方式运行！
（原文#Kohana 3 [routes](routing) are very powerful and flexible, don't be afraid to use as many as you need to make your app function the way you want!）

## 处理大量的路由（Handling lots of routes）

有时候你的应用程序十分复杂，有许多的路由，将它们全部放到bootstrap.php变得难以管理。如果是这样的情况，只需在APPPATH中生成一个`routes.php`文件，并将它引入到你的bootstrap：`require_once APPPATH.'routes'.EXT;`
（原文#Sometimes your application is sufficiently complex that you have many routes and it becomes unmanageable to put them all in bootstrap.php. If this is the case, simply make a `routes.php` file in APPPATH and require that in your bootstrap: `require_once APPPATH.'routes'.EXT;`）

## 异常映射（Reflection_Exception）

如果你在安装站点的时候获得一个Reflection_Exception,几乎可以肯定是因为你的[Kohana::init] 'base_url'设置出错了。如果你的base url是正确的，那么可能是你的[路由](routing)出错了。
（原文#If you get a Reflection_Exception when setting up your site, it is almost certainly because your [Kohana::init] 'base_url' setting is wrong.  If your base url is correct something is probably wrong with your [routes](routing).）
	ReflectionException [ -1 ]: Class controller_<something> does not exist
	// where <something> is part of the url you entered in your browser

### 解决办法（Solution）  {#reflection-exception-solution}

正确设置你的[Kohana::init]中的'base_url'。base url应该是你的index.php文件相对于Web服务器的文档根目录的路径。
（原文#Set your [Kohana::init] 'base_url' to the correct setting. The base url should be the path to your index.php file relative to the webserver document root.）

## ORM/Session __sleep() bug

在PHP中有一个错误，一个致命的错误后，可能会损坏你的session。当你做一些愚蠢的，并导致致命的错误的时候，生产服务器上不应有未捕获的致命错误，所以这个bug只可以在开发中发生。加载下一个页面，你会得到一个数据库连接错误，那么所有随后加载的页面将显示以下错误。
（原文#There is a bug in php which can corrupt your session after a fatal error.  A production server shouldn't have uncaught fatal errors, so this bug should only happen during development, when you do something stupid and cause a fatal error.  On the next page load you will get a database connection error, then all subsequent page loads will display the following error:）

	ErrorException [注意]: 未定义的指数：id (原文#ErrorException [ Notice ]: Undefined index: id）
	MODPATH/orm/classes/kohana/orm.php [ 1308 ]

### 解决办法 （Solution）  {#orm-session-sleep-solution}

为了解决这个问题，清除你在该域名下的cookies来重置您的session。这不应该发生在生产服务器上，这样你就不用向你的客户解释如何清除自己的cookies。你可以看更详细的关于[这个问题的讨论](http://dev.kohanaframework.org/issues/3242)。
（原文#To fix this, clear your cookies for that domain to reset your session.  This should never happen on a production server, so you won't have to explain to your clients how to clear their cookies.  You can see the [discussion on this issue](http://dev.kohanaframework.org/issues/3242) for more details.）