# Markdown 语法

用户指南中的用户指南页面、源代码中的注释(用于产生API浏览器的内容)中使用[Markdown](http://daringfireball.net/projects/markdown/) 和 [Markdown Extra](http://michelf.com/projects/php-markdown/extra/) 。这里是Markdown的一个简单总结以及Markdown extra 特征。它确实不能包含Markdown 语法的所有知识 ，也不包括全部的附加说明。

[!!] 请确定用户指南添加到markdown中的内容都符合 **[Userguide Specific Syntax](#userguide-specific-syntax)**

## 标题

    # 一级标题
	
	## 二级标题
	
	### 三级标题
	
	#### 四级标题

## 段落
~~~
普通文本将会被转换成段落。
单独的回车不会产生一个新段落， 这
是考虑到包含(特别是源代码中的注释)。

文字块之间的一个空行将会产生一个新的段落。
字符 像  > 和 &已经为你转义。

要产生一个行间隔，请在行
结尾处放置两个空格。
~~~
                         
普通文本将会被转换成段落。单独的回车不会产生一个新段落， 这是考虑到包含(特别是源代码中的注释)。

文字块之间的一个空行将会产生一个新的段落。字符像  > 和 &已经为你转义。

要产生一个行间隔，请在行结尾处放置两个空格。

## 链接
~~~
普通链接: [Kohana](http://kohanaframework.org).

带标题的链接: [Kohana](http://kohanaframework.org "The swift PHP framework")
~~~
普通链接: [Kohana](http://kohanaframework.org)

带标题的链接: [Kohana](http://kohanaframework.org "The swift PHP framework")

## 代码块

	内联代码被文字标号简单包围  `text with tick marks.`
	
内联代码被文字标号简单包围   `text with tick marks.`

	// 代码块应该缩进4个空格或者一个tab
	// 
	// 

You can also do a "fenced" code block:

	~~~
	A fenced code block has tildes
	          above and below it
	This is sometimes useful when code is near lists
	~~~
~~~
A fenced code block has tildes
		  above and below it
This is sometimes useful when code is near lists
~~~

## Unordered Lists

~~~
*  To make a unordered list, put an asterisk, minus, or + at the beginning
-  of each line, surrounded by spaces.  You can mix * - and +, but it
+  makes no difference.
~~~
*  To make a unordered list, put an asterisk, minus, or + at the beginning
-  of each line, surrounded by spaces.  You can mix * - and +, but it
+  makes no difference.

## Ordered Lists

~~~
1.  For ordered lists, put a number and a period
2.  On each line that you want numbered.
9.  It doesn't actually have to be the correct number order
5.  Just as long as each line has a number
~~~
1.  For ordered lists, put a number and a period
2.  On each line that you want numbered.
9.  It doesn't actually have to be the correct number order
5.  Just as long as each line has a number

## Nested Lists

~~~
*  To nest lists you just add four spaces before the * or number
	1. Like this
		*  It's pretty basic, this line has eight spaces, so its nested twice
	1. And this line is back to the second level
		*  Out to third level again
*  And back to the first level
~~~
*  To nest lists you just add four spaces before the * or number
	1. Like this
		*  It's pretty basic, this line has eight spaces, so its nested twice
	1. And this line is back to the second level
		*  Out to third level again
*  And back to the first level

## Italics and Bold

~~~
Surround text you want *italics* with *asterisks* or _underscores_.

**Double asterisks** or __double underscores__ makes text bold.

***Triple*** will do *both at the same **time***.
~~~
Surround text you want *italics* with *asterisks* or _underscores_.

**Double asterisks** or __double underscores__ makes text **bold**.

___Triple___ will do *both at the same **time***.

## Horizontal Rules

Horizontal rules are made by placing 3 or more hyphens, asterisks, or underscores on a line by themselves.
~~~
---
* * * *
_____________________
~~~
---
* * * *
_____________________

## Images

Image syntax looks like this:

	![Alt text](/path/to/img.jpg)
	
	![Alt text](/path/to/img.jpg "Optional title")

[!!] Note that the images in userguide are [namespaced](#namespacing).

## Tables
~~~
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell
~~~

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

Note that the pipes on the very left and very right side are optional, and you can change the text-alignment by adding a colon on the right, or on both sides for center.
~~~
| Item      | Value | Savings |
| --------- | -----:|:-------:|
| Computer  | $1600 |   40%   |
| Phone     |   $12 |   30%   |
| Pipe      |    $1 |    0%   |
~~~
| Item      | Value | Savings |
| --------- | -----:|:-------:|
| Computer  | $1600 |   40%   |
| Phone     |   $12 |   30%   |
| Pipe      |    $1 |    0%   |

# Userguide Specific Syntax

In addition to the features and syntax of [Markdown](http://daringfireball.net/projects/markdown/) and [Markdown Extra](http://michelf.com/projects/php-markdown/extra/) the following apply to userguide pages and api documentation:

## Namespacing

The first thing to note is that all links are "namespaced" to the current module.  For example, from anywhere within the Kohana core docs you do not need to include `kohana` at the beginning of a link url.  For example: `[Hello World Tutorial](tutorials/hello-world)` rather than `(kohana/tutorials/hello-world)`.

To link to a modules index page, have an empty url like: `[Kohana]()`.

To link to page in a different module, prefix your url with `../` and the module name.  For example: `[Kohana Routes](../kohana/routing)`

**Images are also namespaced**, using `![Alt Text](imagename.jpg)` would look for `media/guide/<modulename>/imagename.jpg`.

## API Links

You can make links to the api browser by wrapping any class name in brackets.  You may also include a function name, or propery name to link to that specifically.  All of the following will link to the API browser:

	[Request]  
	[Request::execute]  
	[Request::execute()]  
	[Request::$status]  

[Request]  
[Request::execute]  
[Request::execute()]  
[Request::$status]  

If you want to have parameters and have the function be clickable, only put the brackets around the class and function (not the params), and put a backslash in front of the opening parenthesis.

	[Kohana::$config]\('foobar','baz')
	
[Kohana::$config]\('foobar','baz')

## Notes

If you put `[!!]` in front of a line it will be a "note" and be placed in a box with a lightbulb.

	[!!]  This is a note

will display as:
	
[!!] This is a note

## Headers automatically get IDs

Headers are automatically assigned an id, based on the content of the header, so each header can be linked to. You can manually assign a different id using the syntax as defined in Markdown Extra. If multiple headers have the same content (e.g. more than one "Examples" header), only the first will get be automatically assigned an id, so you should manually assign more descriptive ids. For example:

	### Examples     {#more-descriptive-id}

## Including Views

If you need you may include a regular Kohana View file by placing the name of the view in double curly brackets.  **If the view is not found, no error or exception will be shown, the curly brackets and view name will simply remain there!**

	{{some/view/file}}