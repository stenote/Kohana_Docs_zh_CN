# 类加载

Kohana 利用了PHP的[autoloading](http://php.net/manual/language.oop5.autoload.php). 这样就不需要在使用类之前去[include](http://php.net/include) 或 [require](http://php.net/require) 它们了。在使用类的时候 Kohana 将寻找和包含所需要的类文件。使用类或其静态[Cookie::set](http://www.kofans.org/3.2/guide/api/Cookie#set) 方法时，直接调用即可：

    Cookie::set('mycookie', 'any string value');

或者调用 [Encrypt] 实例,只需调用 [Encrypt::instance]:

    $encrypt = Encrypt::instance();

类是通过 [Kohana::auto_load] 方法载入的，这个方法把类名简单地转换为文件名：

1. 类放在[filesystem](files)里的 `classes/` 目录下。
2. 类名里的下划线将被替换为斜线。
2. 文件名全部小写。

当调用一个尚未载入的类时 (例如: `Session_Cookie`), Kohana 将使用 [Kohana::find_file] 在文件系统里搜索一个名为 `classes/session/cookie.php` 的文件。

如果类没有遵守这个命名惯例，它们就不能被 Kohana 自动加载。这时就必须手动包含所需要的文件，或添加自己的 [autoload function.](http://us3.php.net/manual/en/function.spl-autoload-register.php) 函数。

## 自定义的自动加载器

Kohana 的默认自动加载器在 `application/bootstrap.php` 用 [spl_autoload_register](http://php.net/spl_autoload_register) 激活:

    spl_autoload_register(array('Kohana', 'auto_load'));

This allows [Kohana::auto_load] to attempt to find and include any class that does not yet exist when the class is first used.

### 通过自动加载器使用 Zend 库

如果其他库有自己的 autoloader，就能很容易地访问它们了。例如，下面将介绍如何激活 Zend 的 autoloader 从而在 Kohana 应用程序里可以使用 Zend 库。

#### 下载及安装 Zend 框架库

- [下载最新版本的 Zend 框架文件](http://framework.zend.com/download/latest).
- 在 `application/vendor` 目录下建立一个 `vendor` 子目录。这能让第三方软件独立于应用程序类。
- 把包含 Zend 框架的Zend 目录移动到 `application/vendor/Zend`。


#### 在引导代码中包含 Zend 自动加载器

在 `application/bootstrap.php`, 的某个地方，拷贝下列代码：

	/**
	 * Enable Zend Framework autoloading
	 */
	if ($path = Kohana::find_file('vendor', 'Zend/Loader'))
	{
	    ini_set('include_path',
	    ini_get('include_path').PATH_SEPARATOR.dirname(dirname($path)));
	
	    require_once 'Zend/Loader/Autoloader.php';
	    Zend_Loader_Autoloader::getInstance();
	}
	
#### 用法举例

现在就能从 Kohana 应用程序内部自动载入任何 Zend 框架类了：

	if ($validate($_POST))
	{
		$mailer = new Zend_Mail;
		
		$mailer->setBodyHtml($view)
			->setFrom(Kohana::$config->load('site')->email_from)
			->addTo($email)
			->setSubject($message)
			->send();
	}