# 信息(Messages)

Kohana有一个基于查询系统的强大的键，你可以定义系统消息。
（原文#Kohana has a robust key based lookup system so you can define system messages.）

## 获取一条信息(Getting a message)

用Kohana::message()的方法来获取一条信息的键
（原文#Use the Kohana::message() method to get a message key:）

	Kohana::message('forms', 'foobar');

键`foobar`看起来是在文件`messages/forms.php` 中：
（原文#This will look in the `messages/forms.php` file for the `foobar` key:）

	<?php
	
	return array(
		'foobar' => 'Hello, world!',
	);

你还可以让它们看起来在子文件夹和子键中：
（原文#You can also look in subfolders and sub-keys:）

	Kohana::message('forms/contact', 'foobar.bar');

键`[foobar][bar]`看起来是在文件`messages/forms/contact.php`中：
（原文#This will look in the `messages/forms/contact.php` for the `[foobar][bar]` key:）

	<?php
	
	return array(
		'foobar' => array(
			'bar' => 'Hello, world!',
		),
	);

## Notes

 * Don't use __() in your messages files, as these files can be cached and will not work properly.
 * Messages are merged by the cascading file system, not overwritten like classes and views.