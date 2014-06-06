# 请求（Requests）

Kohana引入了灵活的HMVC请求系统。它支持与众不同的的内部请求和外部请求。
（原文#Kohana includes a flexible HMVC request system. It supports out of the box support for internal requests and external requests.）

HMVC代表`Hierarchical Model View Controller`，基本上意味着每个请求可以有从内部调用的MVC组合。
（原文#HMVC stands for `Hierarchical Model View Controller` and basically means requests can each have MVC triads called from inside each other.）

Kohana里面的请求对象适用于HTTP/1.1。
（原文#The Request object in Kohana is HTTP/1.1 compliant.）

## 创建请求（Creating Requests）

创建一个请求非常简单：
（原文#Creating a request is very easy:）

### 内部请求（Internal Requests）

内部请求是响应内部应用程序的请求。它利用[路由](routing)指向基于URI的应用程序来传递给应用程序。一个基本的内部请求看起来可能是这样：
（原文#An internal request is a request calling to the internal application. It utilizes [routes](routing) to direct the application based on the URI that is passed to it. A basic internal request might look something like:）

	$request = Request::factory('welcome');

在这个例子中，URI是'welcome'。
（原文#In this example, the URI is 'welcome'.）

#### 初始请求（The initial request）

从Kohana使用HMVC后，你可以在内部互相调用许多请求了。第一个请求（通常叫做`index.php`）调用"initial request"。你可以通过这样访问这个请求：
（原文#Since Kohana uses HMVC, you can call many requests inside each other. The first request (usually called from `index.php`) is called the "initial request". You can access this request via:）

	Request::initial();
如果你十分确定你要初始请求，你只需用这个方法。除此之外你需要用`Request::current()`方法。
（原文#You should only use this method if you are absolutely sure you want the initial request. Otherwise you should use the `Request::current()` method.）

#### 子请求（Sub-requests）

在你的应用程序中，你可以在任何时候通过`Request::factory()`语法调用一个请求。这些请求会被认为是子请求。
（原文#You can call a request at any time in your application by using the `Request::factory()` syntax. All of these requests will be considered sub-requests.）

除了这个不同之外，他们完全一样。如果该请求是一个在is_initial()方法的控制器下的子请求，你会发现：
（原文#Other than this difference, they are exactly the same. You can detect if the request is a sub-request in your controller with the is_initial() method:）

	$sub_request = ! $this->request->is_initial()

### 外部请求（External Requests）

外部请求调用第三方网站。
（原文#An external request calls out to a third party website.）

您可以使用此从远程站点拼凑HTML，或使一个REST调用第三方API ：
（原文#You can use this to scrape HTML from a remote site, or make a REST call to a third party API:）

	// 这个使用GET（原文#This uses GET）
	$request = Request::factory('http://www.google.com/');

	// 这个使用PUT（原文#This uses PUT）
	$request = Request::factory('http://example.com/put_api')->method(Request::PUT)->body(json_encode('the body'))->headers('Content-Type', 'application/json');

	// 这个使用POST(原文#This uses POST）
	$request = Request::factory('http://example.com/post_api')->method(Request::POST)->post(array('foo' => 'bar', 'bar' => 'baz'));

## 执行请求（Executing Requests）

使用`execute()`方法执行请求。这将给你一个[response](responses)对象。
To execute a request, use the `execute()` method on it. This will give you a [response](responses) object.

	$request = Request::factory('welcome');
	$response = $request->execute();

## 请求的缓存控制（Request Cache Control）

您可以快速执行缓存的请求，由factory的第二个参数传递一个缓存实例：
（原文#You can cache requests for fast execution by passing a cache instance in as the second parameter of factory:）

	$request = Request::factory('welcome', Cache::instance());

TODO