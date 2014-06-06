# 惯例及编码风格

我们鼓励您在编码的过程中遵循和Kohana一致的编码风格，因为这样不仅增强了代码的可读性，而且便于您的代码更容易地分享和贡献出去。[原文](# 'It is encouraged that you follow Kohana's coding style. This makes code more readable and allows for easier code sharing and contributing. ')

## 类命名方式及对应文件位置

在Kohana里，类名都遵循着一个严格的约定以便于[自动加载](autoloading)。类名中需要区分的单词要用下划线来分割，同时单词的首字母须大写。下划线十分重要，因为它们直接反映了文件在文件系统中的位置。[原文](# 'Class names in Kohana follow a strict convention to facilitate [autoloading](autoloading). Class names should have uppercase first letters with underscores to separate words. Underscores are significant as they directly reflect the file location in the filesystem.')

以下惯用法适用：[原文](# 'The following conventions apply:')

1. 类名最好不要使用驼峰规则，除非你不想创建更深层次的目录。[原文](# 'CamelCased class names should not be used, except when it is undesirable to create a new directory level.')
2. 所有类的文件名和目录命都必须小写。[原文](# 'All class file names and directory names are lowercase.')
3. 所有的类文件都应该存储于 `classes`目录之下，而无论该类文件位于该[层级文件系统](files)下面的第几层。[原文](# 'All classes should be in the `classes` directory. This may be at any level in the [cascading filesystem](files).')

### 例子  {#class-name-examples}

记住，在一个类的名称里一个下划线就意味着一个新的子目录，请参考下面的例子：[原文](# 'Remember that in a class, an underscore means a new directory. Consider the following examples:')

类名                  | 文件路径
----------------------|-------------------------------
Controller_Template   | classes/controller/template.php
Model_User            | classes/model/user.php
Database              | classes/database.php
Database_Query        | classes/database/query.php
Form                  | classes/form.php

## 编码标准

为了写出高度一致性的源代码，我们要求每一个人尽量遵循编码标准。[原文](# 'In order to produce highly consistent source code, we ask that everyone follow the coding standards as closely as possible.')

### 括号

请使用[BSD/Allman 风格](http://en.wikipedia.org/wiki/Indent_style#BSD.2FAllman_style) bracketing.  

#### 花括号

花括号被置于单独的行，并且和控制语句保持同样的缩进。[原文](# 'Curly brackets are placed on their own line, indented to the same level as the control statement.')

	// 正确
	if ($a === $b)
	{
		...
	}
	else
	{
		...
	}

	// 错误
	if ($a === $b) {
		...
	} else {
		...
	}

#### 类的括号

花括号使用规则的唯一例外是，类定义时左花括号应位于类名的同一行。[原文](# 'The only exception to the curly bracket rule is, the opening bracket of a class goes on the same line.')

	// 正确
	class Foo {

	// 不正确
	class Foo
	{

#### 空括号

空括号之间不需要任何字符，留空即可。[原文](# 'Don't put any characters inside empty brackets.')

	// Correct
	class Foo {}

	// Incorrect
	class Foo { }

#### 数组的括号

数组的括号应该在一行上，或者在多行。[原文](# 'Arrays may be single line or multi-line.')

	array('a' => 'b', 'c' => 'd')
	
	array(
		'a' => 'b', 
javascript:void(0);		'c' => 'd',
	)

##### 左括号

数组的左括号应位于array关键词的同一行。[原文](# 'The opening array parenthesis goes on the same line.')

	// 正确
	array(
		...
	)

	// 不正确:
	array
	(
		...
	)

##### 右括号

###### 一维数组

由多行组成的一维数组的右括号应独占一行，并且保持与赋值语句一致的缩进。[原文](# 'The closing parenthesis of a multi-line single dimension array is placed on its own line, indented to the same level as the assignment or statement.')

	// 正确
	$array = array(
		...
	)

	// 不正确
	$array = array(
		...
		)

###### 多维数组

嵌套数组向右缩进一个制表位，其余格式则遵循一维数组的规则。[原文](# 'The nested array is indented one tab to the right, following the single dimension rules.')

	// 正确
	array(
		'arr' => array(
			...
		),
		'arr' => array(
			...
		),
	)
	
	array(
		'arr' => array(...),
		'arr' => array(...),
	)
	
##### 数组作为函数实参


	// 正确
	do(array(
		...
	))
	
	// 不正确
	do(array(
		...
		))

正如对数组的左括号的注解，单行语法在这里同样生效。[原文](# 'As noted at the start of the array bracket section, single line syntax is also valid.')

	// 正确
	do(array(...))
	
	// 实参较长时的另一种选择
	do($bar, 'this is a very long line',
		array(...));

### 命名惯例

Kohana使用下划线命名方式，而非驼峰命名规则。[原文](# 'Kohana uses under_score naming, not camelCase naming.')

#### 类

	// 控制器类，使用 Controller_ 作为前缀
	class Controller_Apple extends Controller {

	// 模型类，使用 Model_ 作为前缀
	class Model_Cheese extends Model {

	// 普通累
	class Peanut {

在创建类实例时，不要使用圆括号除非你要给构造函数传递实参。[原文](# 'When creating an instance of a class, don't use parentheses if you're not passing something on to the constructor:')

	// 正确：
	$db = new Database;

	// 不正确：
	$db = new Database();

#### 函数和方法

所有函数名都必须小写并且使用下划线来分隔单词：[原文](# 'Functions should be all lowercase, and use under_scores to separate words:')

	function drink_beverage($beverage)
	{

#### 变量

所有变量都应该小写，并且要使用下划线而不是驼峰规则来分隔变量名中的单词：[原文](# 'All variables should be lowercase and use under_score, not camelCase:')

	// 正确:
	$foo = 'bar';
	$long_example = 'uses underscores';

	// 不正确:
	$weDontWantThis = 'understood?';

### 缩进

你必须使用制表符Tab来缩进你的代码，反之使用空格来进行则是被严格禁止的。[原文](# 'You must use tabs to indent your code. Using spaces for tabbing is strictly forbidden.')

垂直间距（对多行而言）由空格来完成，而制表符Tab则不适合垂直对齐，因为不同人的制表宽度并不一致。[原文](# 'Vertical spacing (for multi-line) is done with spaces. Tabs are not good for vertical alignment because different people have different tab widths.')

	$text = 'this is a long text block that is wrapped. Normally, we aim for '
		  .'wrapping at 80 chars. Vertical alignment is very important for '
		  .'code readability. Remember that all indentation is done with tabs,'
		  .'but vertical alignment should be completed with spaces, after '
		  .'indenting with tabs.';

### 字符串连接

不要将空格置于字符串连接符的周围：[原文](# 'Do not put spaces around the concatenation operator:')

	// 正确:
	$str = 'one'.$var.'two';

	// 不正确:
	$str = 'one'. $var .'two';
	$str = 'one' . $var . 'two';

### 单行语句

单行if语句应该只在需要打断正常执行流程时使用：[原文](# 'Single-line IF statements should only be used when breaking normal execution (e.g. return or continue):')

	// 可接受的：
	if ($foo == $bar)
		return $foo;

	if ($foo == $bar)
		continue;

	if ($foo == $bar)
		break;

	if ($foo == $bar)
		throw new Exception('You screwed up!');

	// 不可接受的：
	if ($baz == $bun)
		$baz = $bar + 2;

### 比较运算符<!--:Comparison Operations-->

请使用OR和AND来做比较运算符：[原文](# 'Please use OR and AND for comparison:')

	// 正确:
	if (($foo AND $bar) OR ($b AND $c))

	// 不正确:
	if (($foo && $bar) || ($b && $c))
	
请使用elseif，而非else if：<!--: test -->[原文](# 'Please use elseif, not else if:')

	// 正确:<!--: test -->
	elseif ($bar)

	// 不正确:
	else if($bar)

### Switch结构<!--:Switch Structures-->

case，break及default都应独占一行，同时case和default区块内容均需要缩进一个tab制表符。[原文](# 'Each case, break and default should be on a separate line. The block inside a case or default must be indented by 1 tab.')

	switch ($var)
	{
		case 'bar':
		case 'foo':
			echo 'hello';
		break;
		case 1:
			echo 'one';
		break;
		default:
			echo 'bye';
		break;
	}

### 圆括号<!--:Parentheses-->

判断语句关键字和左圆括号之间应使用一个空格来分隔，字符!（惊叹号）两边都必须要有一个空格来隔开以使可读性最大化。例外的是单独的惊叹号和类型转换操作，在这两种情况下的左括号之后和右括号之前都不需要任何空格。[原文](# 'There should be one space after statement name, followed by a parenthesis. The ! (bang) character must have a space on either side to ensure maximum readability.Except in the case of a bang or type casting, there should be no whitespace after an opening parenthesis or before a closing parenthesis.')

	// 正确：
	if ($foo == $bar)
	if ( ! $foo)

	// 不正确：
	if($foo == $bar)
	if(!$foo)
	if ((int) $foo)
	if ( $foo == $bar )
	if (! $foo)

### 三元运算符<!--:Ternaries-->

所有的三元操作符都应该遵循一个标准格式。圆括号只能用于完整的表达式而不是只将变量括起来。[原文](# 'All ternary operations should follow a standard format. Use parentheses around expressions only, not around just variables.')

	$foo = ($bar == $foo) ? $foo : $bar;
	$foo = $bar ? $foo : $bar;

所有的比较和操作符必须在一组圆括号内部进行：[原文](# 'All comparisons and operations must be done inside of a parentheses group:')

	$foo = ($bar > 5) ? ($bar + $foo) : strlen($bar);

如要将复杂的三元运算表达式拆分成多行（三元表达式的第一部分超过了80个字符），则需在连续行的运算符的前面使用空格来排列这些运算符：[原文](# 'When separating complex ternaries (ternaries where the first part goes beyond ~80 chars) into multiple lines, spaces should be used to line up operators, which should be at the front of the successive lines:')

	$foo = ($bar == $foo)
		 ? $foo
		 : $bar;

### 类型转换<!--:Type Casting-->

类型转换时，需要在目标类型两边跟上空格：[原文](# 'Type casting should be done with spaces on each side of the cast:')

	// 正确:
	$foo = (string) $bar;
	if ( (string) $bar)

	// 不正确:
	$foo = (string)$bar;

如果可以，请尽量使用类型转换操作来代替三元运算操作：[原文](# 'When possible, please use type casting instead of ternary operations:')

	// 正确：
	$foo = (bool) $bar;

	// 不正确：
	$foo = ($bar == TRUE) ? TRUE : FALSE;

当类型转换成整形或布尔型时，请用短格式：[原文](# 'When casting type to integer or boolean, use the short format:')

	// 正确：
	$foo = (int) $bar;
	$foo = (bool) $bar;

	// 不正确：
	$foo = (integer) $bar;
	$foo = (boolean) $bar;

### 常量<!--:Constants-->

常量始终使用大写形式：[原文](# 'Always use uppercase for constants:')

	// 正确：
	define('MY_CONSTANT', 'my_value');
	$a = TRUE;
	$b = NULL;

	// 不正确：
	define('MyConstant', 'my_value');
	$a = True;
	$b = null;

在作比较运算时应将常量置于的表达式的右端：[原文](# 'Place constant comparisons at the end of tests:')

	// 正确：
	if ($foo !== FALSE)

	// 不正确：
	if (FALSE !== $foo)

不过这样的选择确实有一些微小的争议，所以我会来解释其中的依据。如果我们将前面的例子转换为普通的英文来表述，那么其中“正确”的列子则读作：[原文](# 'This is a slightly controversial choice, so I will explain the reasoning. If we were to write the previous example in plain English, the correct example would read:')

	if variable $foo is not exactly FALSE [参考译文:如果变量$foo并不完全就是FALSE]

而“不正确”的例子则会读作：[原文](# 'And the incorrect example would read:')

	if FALSE is not exactly variable $foo [参考译文:如果FALSE并不不全就是变量$foo]

从左到右的方向读，你会发现放在前面的常量没有任何意义。[原文](# 'Since we are reading left to right, it simply doesn't make sense to put the constant first.')

### 注释<!--:Comments-->

#### 单行注释<!--:One-line Comments-->

使用//，在你代码的上面一行进行注释，并且要与其后的起始字符相隔一个空格，字母开头的注释要大写。[原文](# 'Use //, preferably above the line of code you're commenting on. Leave a space after it and start with a capital. Never use #.')

	// Correct 

	//Incorrect
	// incorrect
	# Incorrect

### 正则表达式<!--:Regular Expressions-->

当编写正则表达式的时候，请使用PCRE的风格而不是POSIX风格。因为PCRE更强大并且更快速。[原文](# 'When coding regular expressions please use PCRE rather than the POSIX flavor. PCRE is considered more powerful and faster.')

	// 正确：
	if (preg_match('/abc/i', $str))

	// 不正确：
	if (eregi('abc', $str))

正则表达串外面应使用单引号而不是使用双引号来包围。使用单引号的字符串因为简单所以更方便。不像使用双引号的字符串那样支持插入变量和反斜线，蔽日 \n 或 \td 等等……[原文](# 'Use single quotes around your regular expressions rather than double quotes. Single-quoted strings are more convenient because of their simplicity. Unlike double-quoted strings they don't support variable interpolation nor integrated backslash sequences like \n or \t, etc.')

	// 正确：
	preg_match('/abc/', $str);

	// 不正确：
	preg_match("/abc/", $str);

当使用正则表达式来搜索和体会时，请使用向后引用$n，比起\\n这种形式来说。[原文](# 'When performing a regular expression search and replace, please use the $n notation for backreferences. This is preferred over \\n.')


	// 正确：
	preg_replace('/(\d+) dollar/', '$1 euro', $str);

	// 不正确：
	preg_replace('/(\d+) dollar/', '\\1 euro', $str);

最后，请注意到用来匹配字符串末端位置的美元符号$允许新行的字符。如果需要，则使用修正符D来解决这个需求。[源码](# 'Finally, please note that the $ character for matching the position at the end of the line allows for a following newline character. Use the D modifier to fix this if needed. [More info](http://blog.php-security.org/archives/76-Holes-in-most-preg_match-filters.html).')


	$str = "email@example.com\n";

	preg_match('/^.+@.+$/', $str);  // TRUE
	preg_match('/^.+@.+$/D', $str); // FALSE