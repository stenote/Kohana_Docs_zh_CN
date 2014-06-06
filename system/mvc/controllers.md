# 控制器

控制器是一个类文件，在一个应用中，它介于模型和视图之间。控制器向模型传递信息以获取数据或者改变数据。然后，控制器将从模型获取的数据传递给视图，最终输出并且呈现给用户。

控制器可以通过[Request::execute()]函数被调用，它基于通过url匹配的[Route]，一定要阅读[routing](routing)相关介绍，去理解怎么在你的应用中使用路由去映射url。

## 创建控制器

在程序编码中，一个控制器必须满足以下内容：

* 在 `classes/controller`目录 (或者子目录)
* 文件名必须是小写的，例如`articles.php`
* 类的名字必须可以映射到文件名 (用 `_` 替换 `/` ) ,并且每个单词的首字母必须大写
* 必须有 Controller这个类作为顶级父类，也就是最终要继承Controller

控制器名和文件位置的例子：

	// classes/controller/foobar.php
	class Controller_Foobar extends Controller {
	
	// classes/controller/admin.php
	class Controller_Admin extends Controller {

控制器也可以在子文件夹下面：

	// classes/controller/baz/bar.php
	class Controller_Baz_Bar extends Controller {
	
	// classes/controller/product/category.php
	class Controller_Product_Category extends Controller {
	
	
[!!] 注意，如果控制器是建在子文件夹下面，那么它将不能被默认的路由所调用。你需要定义一个包含[目录](routing#directory)参数的路由，或者需要为目录设置一个默认值。

控制器可以被其他的控制器所继承

	// classes/controller/users.php
	class Controller_Users extends Controller_Template
	
	// classes/controller/api.php
	class Controller_Api extends Controller_REST
	
[!!] [Controller_Template] 是由Kohana 提供的示例控制器

也可以让一个控制器继承另一个控制器，在其中做一些公共的事情，比如要求用户登录的处理，可以放在这里。

	// classes/controller/admin.php
	class Controller_Admin extends Controller {
		// This controller would have a before() that checks if the user is logged in
	
	// classes/controller/admin/plugins.php
	class Controller_Admin_Plugins extends Controller_Admin {
		// Because this controller extends Controller_Admin, it would have the same logged in check
		
## $this->request

每个控制器都有`$this->request`属性，它可以通过在控制器中调用[Request]对象，获取当前请求的信息。也可以通过`$this->response->body($ouput)`设置响应的内容主体。

这里有一部分`$this->request`可用的属性和方法。 它们也可以通过 `Request::instance()`调用，使用 `$this->request`只是一个快捷的方式。要了解更多的详情，可以看看[Request] 类。

属性/方法 | 作用
--- | ---
[$this->request->route()](../api/Request#property:route) | 匹配当前请求链接的 [Route] 
[$this->request->directory()](../api/Request#property:directory), <br /> [$this->request->controller](../api/Request#property:controller), <br /> [$this->request->action](../api/Request#property:action) |匹配当前路由的目录，控制器，和动作
[$this->request->param()](../api/Request#param) | 在路由中定义的其他参数
[$this->request->redirect()](../api/Request#redirect) | 将请求重定向到另一个链接

## $this->response
[$this->response->body()](../api/Response#property:body) | 请求返回的内容
[$this->response->status()](../api/Response#property:status) | 请求的http状态 (200, 404, 500, 等等)
[$this->response->headers()](../api/Response#property:headers) | 和相应返回的http头


## Actions

你可以通过定义一个以`action_`为前缀的公用方法，为你的控制器创建动作。任何不是申明为`public`，且不是以`action_`为前缀的方法，都不能通过路由被调用。

一个action方法要做什么，取决与当前的请求。它*控制*着这个应用，是否要让用户去保存提交的博客？他们是否要提交必要的字段？他们是否有权限做这些事情？控制器会通过调用其他的类，包含相关方法，完成这件事情。任何动作都要设置`$this->response->body($view)`，以便[视图文件](mvc/views)可以发送到浏览器输出。除非重定向或者在这之前，脚本已经结束了。

一个简单的action方法加载 [视图](mvc/views) 文件

	public function action_hello()
	{
		$this->response->body(View::factory('hello/world')); // This will load views/hello/world.php
	}

### 参数

参数可以通过`$this->request->param('name')` 方法进行调用。其中的 `name`是在路由中定义的name。

	// Assuming Route::set('example','<controller>(/<action>(/<id>(/<new>)))');
	
	public function action_foobar()
	{
		$id = $this->request->param('id');
		$new = $this->request->param('new');

如果参数没有设置，调用时将返回NULL。你在调用时，可以提供第二个参数，以便在没有默认值的情况下，设置一个默认值返回。

	public function action_foobar()
	{
		// 如果url中没有user参数，$id将会是false
		$id = $this->request->param('user',FALSE);

### 示例

一个product页面的的是view action

	public function action_view()
	{
		$product = new Model_Product($this->request->param('id'));

		if ( ! $product->loaded())
		{
			throw new HTTP_Exception_404('Product not found!');
		}

		$this->response->body(View::factory('product/view')
			->set('product', $product));
	}

用户登录的action.

	public function action_login()
	{
		$view = View::factory('user/login');

		if ($_POST)
		{
			// Try to login
			if (Auth::instance()->login(arr::get($_POST, 'username'), arr::get($_POST, 'password')))
			{
				Request::current()->redirect('home');
			}

			$view->errors = 'Invalid email or password';
		}

		$this->response->body($view);
	}

## Before and after

你可以在程序执行之前或者执行完之后使用 `before()` 和`after()`函数。例如，你可以检查用户是否已经登录，设置模板视图，加载一个需要的文件等等。

如果你查找`Controller_Template`，你将会看到那样的例子。

你可以通过`$this->request->action`检测正在请求的是什么action，并且做一些处理，例如在使用一个控制器时要求用户登录，除非正在使用登录的action

	// 在before中，检测用户的授权/登录，不通过的话，进行重定向

	Controller_Admin extends Controller {

		public function before()
		{
			// 如果这个用户不在admin组，并且没有登录，跳转到登录页
			if ( ! Auth::instance()->logged_in('admin') AND $this->request->action !== 'login')
			{
				$this->request->redirect('admin/login');
			}
		}
		
		public function action_login() {
			...

### 自定义构造函数

一般情况下，你不需要去改变`__construct()`构造函数，因为你可以在`before()`函数中做相同的事情。 如果你需要改变控制器的构造函数，你必须保持之前的参数，否则php将会complain。因此，请求对象可以在控制器中调用。再者，大多数情况下你都可以使用`before()`函数而不改变构造函数。但是如果你需要改的话，要像下面那样：

	// 你几乎不需要这样做，可以使用before()实现!

	// 确认 Kohana_Request在你的参数里
	public function __construct(Request $request, Response $response)
	{
		// 你必须在你的函数中调用parent::__construct
		parent::__construct($request, $response);
		
		// 然后做一些其他你想做的
	}

## 扩展其他的 controllers

TODO: 扩展其他控制器的更多描述和例子,多个扩展，等等。