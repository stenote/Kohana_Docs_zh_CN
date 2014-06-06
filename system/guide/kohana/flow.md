# 请求流（Request Flow）

每个应用程序都遵循相同的流程：
（原文#Every application follows the same flow:）

1. 应用程序从`index.php`开始（原文#Application starts from .）
	1. 设置application，module，system的路径。(`APPPATH`, `MODPATH`, and `SYSPATH`)（原文#The application, module, and system paths are set. ）
	2. 设置Error reporting的等级（原文#Error reporting levels are set.）
	3. 如果存在安装文件，就加载它。（原文#Install file is loaded, if it exists.）
	4. 引导文件，`APPPATH/bootstrap.php`已经引入。（原文#The bootstrap file, `APPPATH/bootstrap.php`, is included.）
2. 一旦我们在`bootstrap.php`：（原文#Once we are in `bootstrap.php`:）
	6. [Kohana]类已经加载。（原文#The [Kohana] class is loaded.）
	7. [Kohana::init]被调用来设置错误处理,缓存和日志记录。（原文#[Kohana::init] is called, which sets up error handling, caching, and logging.）
	8. 附加[Kohana_Config]的阅读器和[Kohana_Log]的写入器。（原文#[Kohana_Config] readers and [Kohana_Log] writers are attached.）
	9. 调用[Kohana::modules]来开启更多的模块（原文#[Kohana::modules] is called to enable additional modules.）
	    * 模块路径添加到级联文件系统（原文#Module paths are added to the [cascading filesystem](files).）
		* 如果每个模块的`init.php`文件存在就引入它（原文#Includes each module's `init.php` file, if it exists. ）
	    * `init.php`文件可以执行额外的环境设置，包括添加路由。（原文#The `init.php` file can perform additional environment setup, including adding routes.）
	10. 多次调用[Route::set]来定义[应用程序的路由](routing)（原文#[Route::set] is called multiple times to define the [application routes](routing).）
	11. 调用[Request::instance]来开始出来请求。（原文#[Request::instance] is called to start processing the request.）
		1. 检测每一个路由的设置，直到路由被匹配。（原文#Checks each route that has been set until a match is found.）
		2. 创建controller的实例，并传递请求给它。（原文#Creates the controller instance and passes the request to it.）
		3. 调用[Controller::before]（原文#Calls the [Controller::before] method.）
		4. 调用能产生请求响应的controller的action。（原文#Calls the controller action, which generates the request response.）
		5. 调用[Controller::after]方法。（原文#Calls the [Controller::after] method.）
		    * 当你使用[HMVC sub-requests](requests)的时候，上面5个步骤可以被重复多次。（原文#The above 5 steps can be repeated multiple times when using [HMVC sub-requests](requests).）
3. 应用程序的流程返回到index.php（原文#Application flow returns to index.php）
	12. 显示主[请求]的响应（原文#The main [Request] response is displayed）