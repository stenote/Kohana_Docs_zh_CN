# 类

TODO: 类的摘要介绍

[Models](mvc/models) 和 [Controllers](mvc/controllers) 也都是类，但是Kohana对待它们有些轻微的不同。请阅读各自的介绍学习更多。

## helper或者library?

在kohana3中，"helper" 类和"library"类不像之前的版本中有所不同。他们都放在`classes/`文件夹下面，并且遵循相同的命名规范。大体而言，它们之间的区别是，一个"helper"类可以被静态使用，（可以看看[helpers included in Kohana](helpers)），但是library类需要实例化，并且像对象一样使用，(像[Database query builders](../database/query/builder))。这种区别不是绝对的，自从它们被Kohana同样对待之后，就很不明显了。

## 创建一个类

要创建一个类，可以简单地把一个文件放到`classes/`目录下，在[级联文件系统](files)的任意地方，它们遵从 [类的命名规范](conventions#class-names-and-file-location).。录入，让我们创建一个`Foobar`类。

	// classes/foobar.php
	
	class Foobar {
		static function magic() {
			// Does something
		}
	}
	
现在，我们可以在任何地方调用 `Foobar::magic()` 方法，Kohana将为我们[自动加载](autoloading) 这个文件。

我们可以把类放在子目录

	// classes/professor/baxter.php
	
	class Professor_Baxter {
		static function teach() {
			// Does something
		}
	}
	
现在，我们可以在任何我们想要调用的地方调用 `Professor_Baxter::teach()` 方法。

怎么创建和使用类的例子，可以看看`system`中'classes'文件夹或者其他的模块。

## 类的命名

TODO: 讨论命名，以在你自己的classes/modules中提供清晰的扩展能力。