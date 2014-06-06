[!!]  When the docs get merged these images/links should be update


# 贡献

Kohana是社区推动的，我们的文档依赖于社区的贡献。

## 指南

文档应该使用完整的句子，良好的语法并且尽可能的简洁。同时请提供丰富的示例代码并确保代码遵循了Kohana的规约和样式。

试着经常提交更新，但注意是在更改一至两个文件之后而不要单次提交大量的修改。这更有利于大家对你的提交进行反馈和合并。请确保你的提交信息表述到位而清楚。例如：差 ”文档提交“ 好 “Hello World教程的初始化草稿的提交”； 差 “修正错别字” 好 “修正搜索建立页面的错别字”。

如果你觉得某个菜单需要重新排序或者某个模块需要添加新的页面，请新建一个[错误报告](http://dev.kohanaframework.org/projects/userguide3/issues/new)来进行讨论.

## 快速方法

提交一份[错误报告](http://dev.kohanaframework.org/projects/userguide3/issues/new)以快速指出一些需要改进的地方。

如果你想要贡献一些更改，你甚至可以直接通过你的浏览器而不需要了解git！

首先在[Github](https://github.com/signup/free)上注册一个帐号。

你需要fork那个包含你希望提交改进的区域的模块。例如，fork <http://github.com/bluehawk/orm> 来提交针对 [ORM documentation](../orm) 的改进，fork <http://github.com/bluehawk/core> 来提交针对 [Kohana documentation](../kohana) 的改进等。所以，找到你希望提交改进的模块然后点击右上方的Fork按钮。

![Fork the module](contrib-github-fork.png)

用户指南部分的文件可以在 `guide/<module>/` 找到, API浏览器部分则是由源码中的评注生成。找到你想要修改的某个文件，点击文件浏览器右上方的编辑按钮即可。

![Click on edit to edit the file](contrib-github-edit.png)

做出修改后记得添加**提交细节**，而且每个你想要提交更新的文件都要重复这个操作。(注意在你没有在本地亲自动手实践过前你不会知道它是什么样的)

当你完成你的修改后，发起一个 pull request 来让你的改进被提交审察并最终可以被合并到官方文档中。

![Send a pull request](contrib-github-pull.png)

一旦你的 pull request 被接受，只要愿意，你可以随时删除你fork过来的代码仓库，而你的评注自然已经被复制到了官方文档中。

## 如果你了解git

**Bluehawk的forks都有一个 `docs` 分支。请在那些分支下操作**

为了使拉取所有的docs分支变得更容易，[http://github.com/bluehawk/kohana](http://github.com/bluehawk/kohana) 这一 "docs" 分支包含了连接到所有 "docs" 分支的一个子分支，所以你把它 clone 下来便可以很容易地 fork 所有的 "docs" 分支了。Kohana的主要文档在 [http://github.com/bluehawk/core/tree/docs/guide/kohana/] ，而其他一些模块的文档分布在 guide 目录下。(再次注意你是在 `docs` 分支中)

**简短版本**: Fork那些 bluehawk fork下来的你希望改进的模块 (例如 `git://github.com/bluehawk/orm.git` 或者 `git://github.com/bluehawk/core.git`)， 检查 `docs` 分支, 做出修改然后给 bluehawk 提交一个 pull request 。

**详细帮本**  (这同样假设你对git的运作机制有所了解，特别是子模块是怎么工作的)

 1. 在github上 Fork 那个你希望提交改进的仓库 (例如前往 http://github.com/bluehawk/core 然后点击 Fork 按钮)

 1. 为了更容易地拉取针对用户指南的更新，我创建了一个 Kohana 下的名为 docs 的分支，它包含了其他所有 doc 分支的子分支。你可以把我的这个分支手动添加到你的 Kohana 仓库中，也可以用下列命令创建一个新的 Kohana 仓库：
	
		git clone git://github.com/bluehawk/kohana
		
		#获取 docs 分支
		git checkout origin/docs
		
		#找到 system 目录和所有的模块
		git submodule init
		git submodule update

 1. 现在前往仓库中你希望贡献的文档区域并将你 fork 下来的仓库作为一个新的远程分支（romote），然后提交给它。
 
		cd system
		
		#确保你的分支是最新的
		git merge origin/docs
		(如果失败或者你无法提交，试着输入 "git checkout -b docs" 来建立一个本地分支)
		
		# 把你的仓库添加为一个新的远程分支
		git remote add <your name> git@github.com:<your name>/core.git
		
		# (对文档做一些修改)
		
		#现在提交这些更改并推到你的仓库中
		git commit
		git push <your name> docs

 1. 在 github 上提交一个 pull request。