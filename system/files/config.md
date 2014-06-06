# 配置文件
配置文件是用来存储module(模块)、class（类）或其他你想要的​​任何类型的配置。他们都是普通的PHP文件，存储在config /目录中，返回一个关联数组：
(原文#Configuration files are used to store any kind of configuration needed for a module, class, or anything else you want.  They are plain PHP files, stored in the `config/` directory, which return an associative array:）

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'setting' => 'value',
        'options' => array(
            'foo' => 'bar',
        ),
    );

如果上面的配置文件被命名为`myconf.php`，你可以这样访问它：
（原文#If the above configuration file was called `myconf.php`, you could access it using:）

    $config = Kohana::$config->load('myconf');
    $options = $config->get('options')

## 合并（Merge）

在[级联文件系统](files)的配置文件与大多数其他文件略有不同，因为它们将文件**合并**而不是覆盖。这意味着所有用相同文件路径的配置文件相组合，而产生最终的配置。最终的结果是，你可以重载*个人独特的*设置，而不是复制整个文件。
（原文#Configuration files are slightly different from most other files within the [cascading filesystem](files) in that they are **merged** rather than overloaded. This means that all configuration files with the same file path are combined to produce the final configuration. The end result is that you can overload *individual* settings rather than duplicating an entire file.）

例如，如果我们想更改或添加一个条目到配置文件中，我们就不需要从配置文件中重复所有默认的条目。
（原文#For example, if we wanted to change or add to an entry in the inflector configuration file, we would not need to duplicate all the other entries from the default configuration file.）

    // config/inflector.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'irregular' => array(
            'die' => 'dice', // 没有检测到默认的配置文件（原文#does not exist in default config file）
            'mouse' => 'mouses', // 在默认的配置文件中覆盖'mouse' => 'mice'  翻译者注#这里假设了默认的配置中有'mouse' => 'mice'这一项，现在被'mouse' => 'mouses'覆盖了（原文#overrides 'mouse' => 'mice' in the default config file）
    );


## 创建你自己的配置文件（#Creating your own config files）

比方说，我们希望有一个配置文件存储并且很容易改变一些如网站的标题或Google Analytics代码的东西。我们将创建一个配置文件，我们称之为`site.php`：
（原文#Let's say we want a config file to store and easily change things like the title of a website, or the google analytics code.  We would create a config file, let's call it `site.php`:）

    // config/site.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'title' => 'Our Shiny Website',
        'analytics' => FALSE, // 分析代码在这里，设为FALSE来关闭这个功能 （原文#analytics code goes here, set to FALSE to disable）
    );

现在我们通过访问`Kohana::$config->load('site.title')`来获取站点的名称，通过访问`Kohana::$config->load('site.analytics')` 来获取分析代码。
（原文#We could now call `Kohana::$config->load('site.title')` to get the site name, and `Kohana::$config->load('site.analytics')` to get the analytics code.）

比方说，我们想获取一些软件的版本档案，我们可以用配置文件来存储每一个版本，包括下载的链接、文档和问题追踪。
（原文#Let's say we want an archive of versions of some software.  We could use config files to store each version, and include links to download, documentation, and issue tracking.）

	// config/versions.php

	<?php defined('SYSPATH') or die('No direct script access.');
	
    return array(
		'1.0.0' => array(
			'codename' => 'Frog',
			'download' => 'files/ourapp-1.0.0.tar.gz',
			'documentation' => 'docs/1.0.0',
			'released' => '06/05/2009',
			'issues' => 'link/to/bug/tracker',
		),
		'1.1.0' => array(
			'codename' => 'Lizard',
			'download' => 'files/ourapp-1.1.0.tar.gz',
			'documentation' => 'docs/1.1.0',
			'released' => '10/15/2009',
			'issues' => 'link/to/bug/tracker',
		),
		/// ... etc ...
	);

接下来，你可以按下面的做：
（原文#You could then do the following:）

	// 在你的控制器文件中（原文#In your controller）
	$view->versions = Kohana::$config->load('versions');
	
	// 在你的视图文件中（原文#In your view:）
	foreach ($versions as $version)
	{
		// 输出一些html文件来显示每一个版本 （原文#echo some html to display each version）
	}