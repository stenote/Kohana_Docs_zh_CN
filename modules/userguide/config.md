# 配置

用户指南拥有下列配置选项， 在 `config/userguide.php`中。

	return array
	(
		// 开启API浏览器  TRUE or FALSE
		'api_browser'  => TRUE,
		
		// 允许下列包在API浏览器中显示。  TRUE 表示显示所有， 或者是一个字符串包之间用逗号隔开,  使用 'None' 表示一个类没有包
		// Example: 'api_packages' => 'Kohana,Kohana/Database,Kohana/ORM,None',
		'api_packages' => TRUE,
		
	);

你可以开启或者是禁止整个API浏览器， 或者仅显示特定的包。  要禁止一个模块在用户指南页面中显示， 仅仅只需要使用级联文件系统改变模块的 `enabled` 配置。例如:

	`application/config/userguide.php`
	
	return array
	(
		'modules' => array
		(
			'kohana' => array
			(
				'enabled' => FALSE,
			),
			'database' => array
			(
				'enabled' => FALSE,
			)
		)
	)
	
如果你想将自己的文档保存自己的站点，你可以使用配置让API浏览器仅仅显示你自己的模块和类。想要改变文档的样式和视图也是可以的，但是请在适当的地方给出支持信息！