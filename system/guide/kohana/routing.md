# 路由（Routing）

Kohana提供了一个非常强大的路由系统。从本质上讲，路由提供了在url与你的controller和action之间的接口。用正确的路由，你可以做几乎所有的URL方案对应的几乎所有的控制器约定，你可以改变其中一个而不影响其他的路由。
（原文#Kohana provides a very powerful routing system.  In essence, routes provide an interface between the urls and your controllers and actions.  With the correct routes you could make almost any url scheme correspond to almost any arrangement of controllers, and you could change one without impacting the other.)

正如章节[Request Flow](flow)，一个请求被[Request]类处理，它会寻找一个匹配 [Route]，并载入适当的控制器来处理这一请求。
（原文#As mentioned in the [Request Flow](flow) section, a request is handled by the [Request] class, which will look for a matching [Route] and load the appropriate controller to handle that request.）

[!!]明白**路由是按它们被添加的顺序相匹配**是很重要的，只要一个URL匹配了一个路由，路由基本上就是"停止的",*剩下的路由不会被尝试* 。由于默认的路由匹配几乎所有的东西，包括一个空的URL ，所以新的路由必须放在它前面。
（原文# It is important to understand that **routes are matched in the order they are added**, and as soon as a URL matches a route, routing is essentially "stopped" and *the remaining routes are never tried*.  Because the default route matches almost anything, including an empty url, new routes must be place before it.）

## 创建路由（Creating routes）

如果你在`APPPATH/bootstrap.php`看看，你会看到如下的"default"路线：
（原文#If you look in `APPPATH/bootstrap.php` you will see the "default" route as follows:)

	Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));

[!!] 默认的路由只是简单地作为例子被提供，你可以删除它并用自己的路由取代它。
（原文#The default route is simply provided as a sample, you can remove it and replace it with your own routes.）

因此，这将创建一个名为`default`的路由，它将匹配`(<controller>(/<action>(/<id>)))`格式的url.
（原文#So this creates a route with the name `default` that will match urls in the format of `(<controller>(/<action>(/<id>)))`.）

让我们仔细看看在 [Route::set]的每一个参数，是`name`, `uri`，以及一个可选的数组`regex` 。
（原文#Let's take a closer look at each of the parameters of [Route::set], which are `name`, `uri`, and an optional array `regex`.）

### Name

该路由的name必须是一个**独一无二的**字符串。如果不是，相同name的路由会覆盖之前的路由。name用于创建反向路由的URL ，或检查匹配的路由。
（原文#The name of the route must be a **unique** string.  If it is not it will overwrite the older route with the same name. The name is used for creating urls by reverse routing, or checking which route was matched.）

### URI

URI是一个字符串，表示应匹配的URL的格式。用`<>`包围的表示 *key*和任何用用`()`包围的是URI*可选的*部分。`/`除了是一个字符之外没有意义，它必须匹配在URI中。通常情况下， `/`作为一个静态分隔符来使用，但只要作为正则表达式就是有意义的，这里没有限制你如何格式化你的路由。
（原文#The uri is a string that represents the format of urls that should be matched.  The tokens surrounded with `<>` are *keys* and anything surrounded with `()` are *optional* parts of the uri. In Kohana routes, any character is allowed and treated literally aside from `()<>`.  The `/` has no meaning besides being a character that must match in the uri.  Usually the `/` is used as a static seperator but as long as the regex makes sense, there are no restrictions to how you can format your routes.）

让我们再看看默认的路由，uri是 `(<controller>(/<action>(/<id>)))`。我们有三个键或者参数：controller, action 和 id。在这种情况下，整个URI是可选的，因此一个空白的URI可以匹配，默认的controller 和action (用defaults()来设置,[下面介绍](#defaults))将使得类`Controller_Welcome`被加载和方法`action_index`被调用来处理请求。
（原文#Lets look at the default route again, the uri is `(<controller>(/<action>(/<id>)))`.  We have three keys or params: controller, action, and id.   In this case, the entire uri is optional, so a blank uri would match and the default controller and action (set by defaults(), [covered below](#defaults)) would be assumed resulting in the `Controller_Welcome` class being loaded and the `action_index` method being called to handle the request.）

你可以为你的键用任何名字，但下面的键在[Request]对象中有特别的含义，并会影响被调用的controller和action ：
（原文#You can use any name you want for your keys, but the following keys have special meaning to the [Request] object, and will influence which controller and action are called:）

 * **目录** - 在`classes/controller`的子目录下寻找controller([下面介绍](#directory))。
（原文# * **Directory** - The sub-directory of `classes/controller` to look for the controller ([covered below](#directory))）
* **控制器** - 应执行请求的控制器。
（原文# * **Controller** - The controller that the request should execute.）
* **动作** - 方法调用的动作。
（原文#* **Action** - The action method to call.）

### 正则表达式（Regex）

Kohana路由系统在匹配过程中使用[Perl兼容的正则表达式](http://perldoc.perl.org/perlre.html)。默认情况下，每一个键(用`<>`包围的)都会匹配`[^/.,;?\n]++`  (或是英语中：任何一个不是斜线，句号，逗号，分号，问号，或换行符的）。您可以通过一个关联数组的键为每个键定义自己的式样，式样作为额外的第三个参数传递到Route::set中：
（原文#The Kohana route system uses [perl compatible regular expressions](http://perldoc.perl.org/perlre.html) in its matching process.  By default each key (surrounded by `<>`) will match `[^/.,;?\n]++` (or in english: anything that is not a slash, period, comma, semicolon, question mark, or newline).  You can define your own patterns for each key by passing an associative array of keys and patterns as an additional third argument to Route::set.）

在这里例子中，我们有controller在`admin`和`affiliate`这两个目录中。由于这个路由将只匹配`admin` 或 `affiliate`开头的url，所以默认路由将仍然工作在`classes/controller`中。
In this example, we have controllers in two directories, `admin` and `affiliate`.  Because this route will only match urls that begin with `admin` or `affiliate`, the default route would still work for controllers in `classes/controller`.  

	Route::set('sections', '<directory>(/<controller>(/<action>(/<id>)))',
		array(
			'directory' => '(admin|affiliate)'
		))
		->defaults(array(
			'controller' => 'home',
			'action'     => 'index',
		));

在路由中，您还可以使用一种限制性较少的regex匹配无限个参数，或忽略超出的。在这个例子中，`foobar/baz`和其他任何url将路由到`Controller_Foobar::action_baz()`，参数`"stuff"`将路由到其他任何url。如果你想用无限个参数，你可以[explode](http://php.net/manual/en/function.explode.php)它，或者你只是忽略超出的。
（原文#You can also use a less restrictive regex to match unlimited parameters, or to ignore overflow in a route. In this example, the url `foobar/baz` and anything else that is on the url would be routed to `Controller_Foobar::action_baz()` and the `"stuff"` parameter would be and anything else_that is on the url.  If you wanted to use this for unlimited parameters, you could [explode](http://php.net/manual/en/function.explode.php) it, or you just ignore the overflow.  ）
<!--You can also use a less restrictive regex to match unlimited parameters, or to ignore overflow in a route.  <!--In this example, the url `foobar/baz/and-anything/else_that/is-on-the/url` would be routed to `Controller_Foobar::action_baz()` and the `"stuff"` parameter would be `"and-anything/else_that/is-on-the/url"`.  If you wanted to use this for unlimited parameters, you could [explode](http://php.net/manual/en/function.explode.php) it, or you just ignore the overflow.  -->

	Route::set('default', '(<controller>(/<action>(/<stuff>)))', array('stuff' => '.*'))
		->defaults(array(
			'controller' => 'welcome',
			'action' => 'index',
	  ));


### 默认值（Default values）

如果一个键在路由中是可选的(或者在路由中没出现)，你可以为该键提供默认值传递一个关联数组的键和默认值到[Route::defaults]。除此之外，这可以为你的网站提供有用的一个默认的controller或action。
（原文#If a key in a route is optional (or not present in the route), you can provide a default value for that key by passing an associated array of keys and default values to [Route::defaults], chained after your [Route::set].  This can be useful to provide a default controller or action for your site, among other things.）

[!!]`controller`和`action`的键必须始终有个值，所以他们需要在你的路由中(不是在括号内)或者有一个默认值提供。
（原文#The `controller` and `action` key must always have a value, so they either need to be required in your route (not inside of parentheses) or have a default value provided.）

在默认的路由中，所有的键都是可选的，controller和action要给定一个默认值。如果我们调用一个空的url，默认值会填补它，`Controller_Welcome::action_index()`会被调用。如果我们调用`foobar`,那么默认的action会被使用，因此最后它会调用`Controller_Foobar::action_index()`，如果我们调用`foobar/baz`，那么默认的路由会被使用，`Controller_Foobar::action_baz()`将被调用。
（原文#In the default route, all the keys are optional, and the controller and action are given a default.   If we called an empty url, the defaults would fill in and `Controller_Welcome::action_index()` would be called.  If we called `foobar` then only the default for action would be used, so it would call `Controller_Foobar::action_index()` and finally, if we called `foobar/baz` then neither default would be used and `Controller_Foobar::action_baz()` would be called.）

TODO: 这里需要一个例子（need an example here）

你也可以用默认设置去设置一个不在路由中的键。
（原文#You can also use defaults to set a key that isn't in the route at all.）

TODO: 例如，可以使用不在路由中的directory或controller中的一个，但在默认情况下设置（原文#example of either using directory or controller where it isn't in the route, but set by defaults）

### 目录（Directory）

## 匿名/回调路由逻辑（Lambda/Callback route logic）

在3.1中，你可以您可以通过使用匿名路由指定高级的路由方案。代替URI ，你可以使用一个匿名函数或回调的语法来指定一个函数来处理你的路由。这里是一个简单的例子：
（原文In 3.1, you can specify advanced routing schemes by using lambda routes. Instead of a URI, you can use an anonymous function or callback syntax to specify a function that will process your routes. Here's a simple example:）

如果你想用匿名路由来使用反向路由，你必须要传递第三个参数：
（原文#If you want to use reverse routing with lambda routes, you must pass the third parameter:）

	Route::set('testing', function($uri)
		{
			if ($uri == 'foo/bar')
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar',
				);
		},
		'foo/bar'
	);

当你看到下面的路由时，反向uri的参数可能没有意义。
As you can see in the below route, the reverse uri parameter might not make sense.

	Route::set('testing', function($uri)
		{
			if ($uri == '</language regex/>(.+)')
			{
				Cookie::set('language', $match[1]);
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar'
				);
			}
		},
		'<language>/<rest_of_uri>
	);

如果你在使用php5.2，你仍然可以对这种情况使用回调函数（这个例子省略了反向路由）：
（原文#If you are using php 5.2, you can still use callbacks for this behavior (this example omits the reverse route):）

	Route::set('testing', array('Class', 'method_to_process_my_uri'));

## 例子（Examples）

这里是有无数个其他的可能性的路由。下面是一些例子：
（原文#There are countless other possibilities for routes. Here are some more examples:）

    /*
     * 快捷认证（Authentication shortcuts）
     */
    Route::set('auth', '<action>',
      array(
        'action' => '(login|logout)'
      ))
      ->defaults(array(
        'controller' => 'auth'
      ));
      
    /*
     * 多格式接口（Multi-format feeds）
     *   452346/comments.rss
     *   5373.json
     */
    Route::set('feeds', '<user_id>(/<action>).<format>',
      array(
        'user_id' => '\d+',
        'format' => '(rss|atom|json)',
      ))
      ->defaults(array(
        'controller' => 'feeds',
        'action' => 'status',
      ));
    
    /*
     * 静态页面（Static pages）
     */
    Route::set('static', '<path>.html',
      array(
        'path' => '[a-zA-Z0-9_/]+',
      ))
      ->defaults(array(
        'controller' => 'static',
        'action' => 'index',
      ));
      
    /*
     * 你不喜欢斜杠？（You don't like slashes?）
     *   EditGallery:bahamas
     *   Watch:wakeboarding
     */
    Route::set('gallery', '<action>(<controller>):<id>',
      array(
        'controller' => '[A-Z][a-z]++',
        'action'     => '[A-Z][a-z]++',
      ))
      ->defaults(array(
        'controller' => 'Slideshow',
      ));
      
    /*
     * 快速搜索（Quick search）
     */
    Route::set('search', ':<query>', array('query' => '.*'))
      ->defaults(array(
        'controller' => 'search',
        'action' => 'index',
      ));

## 请求参数（Request parameters）

`directory`,`controller`和`action`可以像这样作为公共资源从[Request]被访问：
（原文#The `directory`, `controller` and `action` can be accessed from the [Request] as public properties like so:）

	// 从一个控制器（原文#From within a controller:）
	$this->request->action();
	$this->request->controller();
	$this->request->directory();
	
	// 可以在任何地方使用（原文#Can be used anywhere:）
	Request::current()->action();
	Request::current()->controller();
	Request::current()->directory();

在路由中所有其他的键可以通过[Request::param()]被访问：
（原文#All other keys specified in a route can be accessed via [Request::param()]:）

	// 从一个控制器（原文#From within a controller:）
	$this->request->param('key_name');
	
	// 可以在任何地方使用（原文#Can be used anywhere:）
	Request::current()->param('key_name');
为防止键没有被路由设置，在[Request::param]方法用可选的第二个参数去指定一个默认的返回值。如果没有给定变量，所有的键会作为一个关联数组被返回。
（原文#The [Request::param] method takes an optional second argument to specify a default return value in case the key is not set by the route. If no arguments are given, all keys are returned as an associative array.  In addition, `action`, `controller` and `directory` are not accessible via [Request::param()].）

例如，用下面的路由：
（原文#For example, with the following route:）

	Route::set('ads','ad/<ad>(/<affiliate>)')
	->defaults(array(
		'controller' => 'ads',
		'action'     => 'index',
	));

如果一个url匹配了路由，那么`Controller_Ads::index()`将被调用。你可以用控制器的[Request]中的`param()`的方法访问参数。如果你没有用`->defaults()`，记住要定义一个默认值(通过[Request::param]的第二个可选参数。
（原文#If a url matches the route, then `Controller_Ads::index()` will be called.  You can access the parameters by using the `param()` method of the controller's [Request]. Remember to define a default value (via the second, optional parameter of [Request::param]) if you didn't in `->defaults()`.）

	class Controller_Ads extends Controller {
		public function action_index()
		{
			$ad = $this->request->param('ad');
			$affiliate = $this->request->param('affiliate',NULL);
		}
	

## 路由应该在哪里定义？（原文#Where should routes be defined?）

如果路由属于这个模块，确定的约定是将你自定义的路由放在你的模块下的`MODPATH/<module>/init.php`文件中，或着如果它们是对应用程序特定的，直接写入到`APPPATH/bootstrap.php`文件（确定把他们放入**以上**默认路由中）。当然，没有任何东西阻止你从外部文件包含他们，甚至动态生成它们。
（原文#The established convention is to either place your custom routes in the `MODPATH/<module>/init.php` file of your module if the routes belong to a module, or simply insert them into the `APPPATH/bootstrap.php` file (be sure to put them **above** the default route) if they are specific to the application. Of course, nothing stops you from including them from an external file, or even generating them dynamically.）

## 深入探讨路由如何工作（A deeper look at how routes work）

TODO: 谈如何编写路由（talk about how routes are compiled）

## 用路由创建URL和链接（Creating URLs and links using routes）

随着Kohana功能的强大，路由功能包括一些为你的路由的URI生成url的方法。你总是可以指定你的URI用[URL::site]创建一个像这样的完整的URL作为一个字符串 ：
（原文#Along with Kohana's powerful routing capabilities are included some methods for generating URLs for your routes' uris. You can always specify your uris as a string using [URL::site] to create a full URL like so:）

    URL::site('admin/edit/user/'.$user_id);

然而，Kohana也提供了一个从路由的定义生成uri的方法。如果你的路由可以减轻你回去看代码的的负担并将每一处的URI用字符串去替换,这是非常有用的。下面是一个动态生成符合`feeds`路由实例的例子：
（原文#However, Kohana also provides a method to generate the uri from the route's definition. This is extremely useful if your routing could ever change since it would relieve you from having to go back through your code and change everywhere that you specified a uri as a string. Here is an example of dynamic generation that corresponds to the `feeds` route example from above:）

    Route::get('feeds')->uri(array(
      'user_id' => $user_id,
      'action' => 'comments',
      'format' => 'rss'
    ));

比方说，你以后决定将这条路由变成`feeds/<user_id>(/<action>).<format>`使定义更详细。如果你将上面的uri生成方法写入到你的代码中，你就不用改变一行代码了！当URI的一个部分是用括号括起来的，在URI生成器没有指定值的地方和在路由中没有指定默认值的地方指定一个键，那么该部分将被从URI删除。默认路由的`(/<id>)`部分是这样的一个例子;如果没有提供id ,就不会包含到生成的URI中。
（原文#Let's say you decided later to make that route definition more verbose by changing it to `feeds/<user_id>(/<action>).<format>`. If you wrote your code with the above uri generation method you wouldn't have to change a single line! When a part of the uri is enclosed in parentheses and specifies a key for which there in no value provided for uri generation and no default value specified in the route, then that part will be removed from the uri. An example of this is the `(/<id>)` part of the default route; this will not be included in the generated uri if an id is not provided.）
 
你可能会经常使用的方法是捷径[Request::uri]，它除了假定当前的route，directory, controller和action之外，其余都和上面的一样的。如果我们当前的路由是默认的，uri是`users/list`，我们可以像下面这样生成`users/view/$id`格式的uri。
（原文#One method you might use frequently is the shortcut [Request::uri] which is the same as the above except it assumes the current route, directory, controller and action. If our current route is the default and the uri was `users/list`, we can do the following to generate uris in the format `users/view/$id`:）

    $this->request->uri(array('action' => 'view', 'id' => $user_id));
    	或者，如果在一个视图中，更好的方法是：
	（原文#Or if within a view, the preferable method is:）

    Request::instance()->uri(array('action' => 'view', 'id' => $user_id));

TODO: 除了上面的例子，还可以使用html::anchor的例子（原文#examples of using html::anchor in addition to the above examples）

## 测试路由（Testing routes）

TODO: mention bluehawk's devtools module