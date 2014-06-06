# 错误/异常处理（Error/Exception Handling）

Kohana同时提供了异常处理和错误处理，通过PHP的[ErrorException](http://php.net/errorexception)类，将errors转换到exceptions。许多细节的错误和应用程序的内部状态将通过handler来显示。
（原文#Kohana provides both an exception handler and an error handler that transforms errors into exceptions using PHP's [ErrorException](http://php.net/errorexception) class. Many details of the error and the internal state of the application is displayed by the handler:）

1. 异常类（Exception class）
2. 错误等级（Error level）
3. 错误消息（Error message）
4. 错误代码行高亮的错误代码（Source of the error, with the error line highlighted）
5. 执行流程的调试追踪（A [debug backtrace](http://php.net/debug_backtrace) of the execution flow）
6. 引入文件，加载扩展，和全局变量（Included files, loaded extensions, and global variables）

## 例子（Example）

点击任何一个链接来切换显示更多的信息：
（原文#Click any of the links to toggle the display of additional information:）

<div>{{userguide/examples/error}}</div>

## 关闭错误/异常处理（Disabling Error/Exception Handling）

如果你不想用完整的错误处理，在你调用[Kohana::init]的时候，可以关闭它（我们强烈反对）：
（原文#If you do not want to use the internal error handling, you can disable it (highly discouraged) when calling [Kohana::init]:）

    Kohana::init(array('errors' => FALSE));

## 错误报告（Error Reporting）

默认情况下，Kohana显示所有错误，包括严格模式的warnings。可以用[error_reporting](http://php.net/error_reporting)来设置：
（原文#By default, Kohana displays all errors, including strict mode warnings. This is set using [error_reporting](http://php.net/error_reporting):）

    error_reporting(E_ALL | E_STRICT);

当你的应用程序在生产下运行，推荐用较为保守的设置，如无视notices：
（原文#When you application is live and in production, a more conservative setting is recommended, such as ignoring notices:）

    error_reporting(E_ALL & ~E_NOTICE);

如果你在触发错误时得到白屏，你的主机可能已禁用了显示错误。你可以通过在调用`error_reporting`后增加这一行再次开启它。
（原文#If you get a white screen when an error is triggered, your host probably has disabled displaying errors. You can turn it on again by adding this line just after your `error_reporting` call:）

    ini_set('display_errors', TRUE);

即使在​​生产中，错误也应该**总是**显示，因为它允许你在错误发生时，用[exception and error handling](debugging.errors)去生成一个漂亮的错误页面，而不是一个空白的屏幕。
（原文#Errors should **always** be displayed, even in production, because it allows you to use [exception and error handling](debugging.errors) to serve a nice error page rather than a blank white screen when an error happens.）

## HTTP异常处理（HTTP Exception Handling）

Kohana自带一个强大的处理HTTP错误的系统。他包含每个http状态码的异常类。在你的应用程序中触发一个404（最常见的场景）：
（原文#Kohana comes with a robust system for handing http errors. It includes exception classes for each http status code. To trigger a 404 in your application (the most common scenario):）

	throw new HTTP_Exception_404('File not found!');

在Kohana中，没有默认的方法处理这些错误。建议您设置一个异常处理程序（并注册）来处理这些类型的错误。这里有一个简单的例子在*/application/classes/foobar/exception/handler.php*。
There is no default method to handle these errors in Kohana. It's recommended that you setup an exception handler (and register it) to handle these kinds of errors. Here's a simple example that would go in */application/classes/foobar/exception/handler.php*:

	class Foobar_Exception_Handler
	{
		public static function handle(Exception $e)
		{
			switch (get_class($e))
			{
				case 'Http_Exception_404':
					$response = new Response;
					$response->status(404);
					$view = new View('error_404');
					$view->message = $e->getMessage();
					$view->title = 'File Not Found';
					echo $response->body($view)->send_headers()->body();
					return TRUE;
					break;
				default:
					return Kohana_Exception::handler($e);
					break;
			}
		}
	}

把一些这样的东西放到你的bootstrap里面来注册这个handler。
（原文#And put something like this in your bootstrap to register the handler.）

	set_exception_handler(array('Foobar_Exception_Handler', 'handle'));

 > *注意*确定将`set_exception_handler()`放到` Kohana引导程序的`Kohana::init()`**之后**，否则无法正常工作。
（原文#*Note:* Be sure to place `set_exception_handler()` **after** `Kohana::init()` in your bootstrap, or it won't work.）
 
 > 如果你收到*Fatal error: Exception thrown without a stack frame in Unknown on line 0*，这意味着有一个在异常处理程序中的错误。如果用上面的例子，要确定*404.php*存在于*/application/views/error/*下。
（原文#If you receive *Fatal error: Exception thrown without a stack frame in Unknown on line 0*, it means there was an error within your exception handler. If using the example above, be sure *404.php* exists under */application/views/error/*.）