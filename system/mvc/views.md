#视图 [原文](# Views)

视图就是包含程序输出信息的文件(或载体).最常见有HTML,CSS,Javascript或是任何你想(需要)显示的东东,诸如XML,或是ajax的JSON输出.视图使用的目的是为了程序显示信息与逻辑相分离,使代码简洁和易(重复)用.[原文](#Views are files that contain the display information for your application. This is most commonly HTML, CSS and Javascript but can be anything you require such as XML or JSON for AJAX output. The purpose of views is to keep this information separate from your application logic for easy reusability and cleaner code.)

视图自身可以包含用来显示你传递给它的数据的代码(通常用作循环或逻辑判断).例如,循环产品信息数组,每一个元素会显示在为之新建的(表格)行中.视图仍然是PHP文件,你可以使用(或抒写)任可代码,就像你通常在其他文件中做的那样,但是,强烈建议不要那么做.让你的视图尽量只做显示的事,在控制器中获得你想要的数据,传给它就好.[原文](# Views themselves can contain code used for displaying the data you pass into them. For example, looping through an array of product information and display each one on a new table row. Views are still PHP files so you can use any code you normally would.  However, you should try to keep your views as "dumb" as possible and retreive all data you need in your controllers, then pass it to the view.)

创建视图[原文](# Creating View Files)

视图文件存储在文件系统的`views`文件夹下.你也可以在`views`下创建子文件来管理自己的视图文件.下面所列出的视图文件(注意它们的位置)都是不错的例子:(# View files are stored in the `views` directory of the [filesystem](files). You can also create sub-directories within the `views` directory to organize your files. All of the following examples are reasonable view files:)

    APPPATH/views/home.php
    APPPATH/views/pages/about.php
    APPPATH/views/products/details.php
    MODPATH/error/views/errors/404.php
    MODPATH/common/views/template.php

##载入视图[原文](Loading Views)

[视图]对象通常在[控制器](MVC模式中的C--Controller)中使用[View::factory]方法创建.接下来,视图(刚创建的)常常被赋值为[Request::$response]方法的属性(值)或是分配到其他的视图中去.[原文](# [View] objects will typically be created inside a [Controller](mvc/controllers) using the [View::factory] method. Typically the view is then assigned as the [Request::$response] property or to another view.)

    public function action_about()
    {
        $this->response->body(View::factory('pages/about'));
    }

当一个视图被赋值给[Response::body],在上面的例子中,在必要时它会自动的显示出来.获取视图显示的结果,你可以[View::render]方法或对其进行类型转换(转换成string,字符串).当视图被呈现(控制器传值到视图,C->V),视图文件被载入同时创建HTML文件(用来显示视图).[原文](# When a view is assigned as the [Response::body], as in the example above, it will automatically be rendered when necessary. To get the rendered result of a view you can call the [View::render] method or just type cast it to a string. When a view is rendered, the view file is loaded and HTML is generated.)

    public function action_index()
    {
        $view = View::factory('pages/about');

        // Render the view
        $about_page = $view->render();

        // Or just type cast it to a string
        $about_page = (string) $view;

        $this->response->body($about_page);
    }

## 视图中的变量[原文](# Variables in Views)

一旦视图被载入(显示给用户),变量可以通过[View::set]和[View::bind]的方法赋值到视图中.[原文](# Once view has been loaded, variables can be assigned to it using the [View::set] and [View::bind] methods.)

    public function action_roadtrip()
    {
        $view = View::factory('user/roadtrip')
            ->set('places', array('Rome', 'Paris', 'London', 'New York', 'Tokyo'));
            ->bind('user', $this->user);

        // The view will have $places and $user variables
        $this->response->body($view);
    }
[!!]set()和bind()赋值的愉一不同之处就是bind()方法是引用变量(赋值).如果你用bind()方法使用了一个未声明的变量,变量将被创建并默认值 等于 NULL.[原文](#[!!] The only difference between `set()` and `bind()` is that `bind()` assigns the variable by reference. If you `bind()` a variable before it has been defined, the variable will be created with a value of `NULL`.  )

你也可以直接赋值给视图对象.和使用set()是一样的.[原文](# You can also assign variables directly to the View object.  This is identical to calling `set()`;)

	public function action_roadtrip()
	{
		$view = View::factory('user/roadtrip');
            
		$view->places = array('Rome', 'Paris', 'London', 'New York', 'Tokyo');//直接赋值
        $view->user = $this->user;

        // (视图就有了$places和$user2个变量.)The view will have $places and $user variables
        $this->response->body($view);
	}

##全局变量[原文](# Global Variables)

一个程序或许有好几个视图文件但是需要获取(显示)同样的变量.例如,在模板页和详细页都显示页面标题.你可通过[View::set_global]和[View::bind_global]方法来创建可以被所有视图都可获取的变量. [原文](# An application may have several view files that need access to the same variables. For example, to display a page title in both the header of your template and in the body of the page content. You can create variables that are accessible in any view using the [View::set_global] and [View::bind_global] methods.)

    // 给所有视图都赋值$page_title(亦即,所有的视图都可以使用$page_title这个变量)[原文](# Assign $page_title to all views)
    View::bind_global('page_title', $page_title);

如果有一个程序(网站)中有三个视图被渲染到主页: `template`, `template/sidebar`, and `pages/home`.首先,创建一个抽象的模板控制器[原文](# If the application has three views that are rendered for the home page: `template`, `template/sidebar`, and `pages/home`. First, an abstract controller to create the template will be created:)

    abstract class Controller_Website extends Controller_Template {

        public $page_title;

        public function before()
        {
            parent::before();

            // 使所有视图可用$page_titl[原文](# Make $page_title available to all views)
            View::bind_global('page_title', $this->page_title);

            // 将$sidear作为一个视图文件载入模板.[原文](# Load $sidebar into the template as a view)
            $this->template->sidebar = View::factory('template/sidebar');
        }

    }

接下来,home控制器将继承Controller_Website(上例创建的模板)[原文](# Next, the home controller will extend `Controller_Website`: )

    class Controller_Home extends Controller_Website {

        public function action_index()
        {
            $this->page_title = 'Home';

            $this->template->content = View::factory('pages/home');
        }

    }

##视图中包含其他视图[原文](# Views Within Views)

如果你想在视图中包含其他视图,有2种选择.(第一种)使用[View::factory]可以沙盘化(像军事上用的沙盘模型)你包含的视图文件.这也意味着你必须使用[View::set]或 [View::bind]为视图中所有用到的变量赋值:[原文](# If you want to include another view within a view, there are two choices. By calling [View::factory] you can sandbox the included view. This means that you will have to provide all of the variables to the view using [View::set] or [View::bind]:)
	
	// In your view file:
	
    // Only the $user variable will be available in "views/user/login.php"
    <?php echo View::factory('user/login')->bind('user', $user) ?>
另一种方法是直接包含想引用的视图,当然这会使所有当前的变量在被引用的视图文件中也可使用:[原文](# The other option is to include the view directly, which makes all of the current variables available to the included view):

	// In your view file:
	
    // Any variable defined in this view will be included in "views/message.php"
    <?php include Kohana::find_file('views', 'user/login') ?>

也可在控制器中将(子一级)视图直接作为变量赋值给更高级(父一级)别的视图.例如[原文](# You can also assign a variable of your parent view to be the child view from within your controller.  For example):

	// In your controller:

	public functin action_index()
	{
		$view = View::factory('common/template);
		
		$view->title = "Some title";
		$view->body = View::factory('pages/foobar');
	}
	
	// In views/common/template.php:
	
	<html>
	<head>
		<title><?php echo $title></title>
	</head>
	
	<body>
		<?php echo $body ?>
	</body>
	</html>

当然,也可在视图内直接运行[Request][原文](# Of course, you can also load an entire [Request] within a view):

    <?php echo Request::factory('user/login')->execute() ?>

这个一个[HMVC]的例子,它使的在程序中生成和读取其他URLs成为可能.[原文](# This is an example of \[HMVC], which makes it possible to create and read calls to other URLs within your application.)