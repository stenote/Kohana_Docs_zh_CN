友好错误提示页[原文](# # Friendly Error Pages)

Kohana3并没有像ko2默认有方法显示友好的错误页;这个快速教程将教你怎么做.[原文](# By default Kohana 3 doesn't have a method to display friendly error pages like that)
seen in Kohana 2; In this short guide you will learn how it is done.

## Prerequisites

需要在'Kohana::init'中开启'errors'=>'TRUE',这会将PHP原生的错误转化的更容易提交.[原文](#You will need `'errors' => TRUE` passed to `Kohana::init`. This will convert PHP)
errors into exceptions which are easier to handle.

## 1. An Improved Exception Handler(三步走之第一步,一个改良的异常捕获方法)

我们常用的异常机制是原生的非友好,自身说明问题的(Kohana的异常机制代码如下,不翻译).[原文](#Our custom exception handler is self explanatory.)

	class Kohana_Exception extends Kohana_Kohana_Exception {

		public static function handler(Exception $e)
		{
			if (Kohana::DEVELOPMENT === Kohana::$environment)
			{
				parent::handler($e);
			}
			else
			{
				try
				{
					Kohana::$log->add(Log::ERROR, parent::text($e));

					$attributes = array
					(
						'action'  => 500,
						'message' => rawurlencode($e->getMessage())
					);

					if ($e instanceof HTTP_Exception)
					{
						$attributes['action'] = $e->getCode();
					}

					// Error sub-request.
					echo Request::factory(Route::get('error')->uri($attributes))
					->execute()
					->send_headers()
					->body();
				}
				catch (Exception $e)
				{
					// Clean the output buffer if one exists
					ob_get_level() and ob_clean();

					// Display the exception text
					echo parent::text($e);

					// Exit with an error status
					exit(1);
				}
			}
		}
	}

如果我们在开发环境下就过掉(无视)它,转用其他的Kohana异常处理(机制).[原文](#If we are in the development environment then pass it off to Kohana otherwise:)

*记录错误[原文](#* Log the error)
*设置路由动作函数和信息(属性).[原文](#* Set the route action and message attributes.)
*如果一个'HTTP_Exception'抛出,就重写动作,显示相关错误代码(当然,是友好).[原文](#* If a `HTTP_Exception` was thrown, then override the action with the error code.)
*抛出一个内部子请求.[原文](#* Fire off an internal sub-request.)

动作会被当作HTTP response代码.默认的是500(服务器错误),除非一个'HTTP_Response_Exception'抛出.[原文](#The action will be used as the HTTP response code. By default this is: 500 (internal 
server error) unless a `HTTP_Response_Exception` was thrown.)

因此这个(如下的代码):[原文](#So this:)

	throw new HTTP_Exception_404(':file does not exist', array(':file' => 'Gaia'));

会显示一个友好的404错误页,这个(如下代码):[原文](#would display a nice 404 error page, where:)

	throw new Kohana_Exception('Directory :dir must be writable',
				array(':dir' => Debug::path(Kohana::$cache_dir)));

将显示一个500错误页面.[原文](#would display an error 500 page.)

**The Route**(路由配置) 
	Route::set('error', 'error/<action>(/<message>)', array('action' => '[0-9]++', 'message' => '.+'))
	->defaults(array(
		'controller' => 'error_handler'
	));

## 2. The Error Page Controller (三步之第二步,错误页面控制器)

	public function before()
	{
		parent::before();

		$this->template->page = URL::site(rawurldecode(Request::$initial->uri()));

		// Internal request only!
		if (Request::$initial !== Request::$current)
		{
			if ($message = rawurldecode($this->request->param('message')))
			{
				$this->template->message = $message;
			}
		}
		else
		{
			$this->request->action(404);
		}

		$this->response->status((int) $this->request->action());
	}

1设置模板显示变量'page'是为了让用户明白自己的请求是什么.这样做的目仅为了显示.[原文](#1. Set a template variable "page" so the user can see what they requested. This
   is for display purposes only.)
2.如果只是内部请求,给'message'变量赋值并显示给用户.[原文](#2. If an internal request, then set a template variable "message" to be shown to
   the user.)
3.否则的话使用404动作.用户可以更巧妙的设置自己的信息,比如'error/404/email%20your%20login%20information%20to%20hacker%40google.com`'.[原文](#3. Otherwise use the 404 action. Users could otherwise craft their own error messages, eg:
   `error/404/email%20your%20login%20information%20to%20hacker%40google.com`)


		public function action_404()
		{
			$this->template->title = '404 Not Found';

			// Here we check to see if a 404 came from our website. This allows the
			// webmaster to find broken links and update them in a shorter amount of time.
			if (isset ($_SERVER['HTTP_REFERER']) AND strstr($_SERVER['HTTP_REFERER'], $_SERVER['SERVER_NAME']) !== FALSE)
			{
				// Set a local flag so we can display different messages in our template.
				$this->template->local = TRUE;
			}

			// HTTP Status code.
			$this->response->status(404);
		}

		public function action_503()
		{
			$this->template->title = 'Maintenance Mode';
		}

		public function action_500()
		{
			$this->template->title = 'Internal Server Error';
		}

你应注意到每个方法都是在'HTTP response' 代码后命名,并设置请求的代码.[原文](#You will notice that each example method is named after the HTTP response code
and sets the request response code.)

## 3. Conclusion (三步走之第三步,结局)

搞定!现在显示一个友好的错误页面是相当容易的啦(就像下面这样):[原文](#So that's it. Now displaying a nice error page is as easy as:)

	throw new HTTP_Exception_503('The website is down');(这会抛出503错误,内容是,站点挂了.)
