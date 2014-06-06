# 关系

Kohana 的 ORM 支持四种类型的对象关系：**`belongs_to`，`has_many`，`has_many "through"`** 和 **`has_one`**。可以像这样使用 ‘has_many “through”’ 的对象关系的功能，如 Active Record 的 `has_many_and_belongs_to` 关系类型。

## belongs_to

如果有 一个 属于 另一个 模型时，应使用一个 **`belongs_to`** 关系。 例如 ，一个 模型 `Child` 是  模型 `Parent` 的成员 或者  模型 `Flag` 属于 模型 `Country`。

这是基于 `belongs_to` 的关系:

	protected $_belongs_to = array(
		'[alias name]' => array(
			'model'       => '[model name]'，
			'foreign_key' => '[column]'，
		),
	);

您可以省略所有在右边数组中的 key/value，在这种情况下使用默认值:

	protected $_belongs_to = array('[alias name]' => array());

该 **别名** 是用于访问代码中的相关模型。 如果您有一个 `Post` 模型，属于一个 `User` 模型，并希望使用 **`belongs_to`** 配置的默认值，那么您的代码应该像这样 ：

	protected $_belongs_to = array('user' => array());

要访问 post 的 user 模型，可以使用 `$post->user`。  由于我们以上使用的默认，那么表明就是 模型名字，并且 `post` 表的外键就是 别名 后缀以 `_id`，在当前情况下就是 `user_id`。 (您可以在模型中通过修改 `$foreign_key_suffix` 模型属性 来改变 `_id` 这个后缀。)

比方说， 您的 `Post` 数据库表的架构没有 `user_id` 列，而是具有 `author_id` 这样一个外键，存在于 数据表 `User` 的列。 您应该使用这样的代码:

	protected $_belongs_to = array(
		'user' => array(
			'foreign_key' => 'author_id'，
		),
	);

如果想通过 `$post->author` 这样的代码访问一个 post 的 author ，那么你只需要简单的修改 `author` 的值 中 `model` 的模型别名:

	protected $_belongs_to = array(
		'author' => array(
			'model'       => 'user'，
		),
	);

## has_many

标准的 **`has_many`** 就好像是从 **`belongs_to`** 关系的另一边来看。  在上面的例子中，一个 post 只隶属于一个 author 。  而从 author 角度来看，一个 author 可以有多个 post。 一个 **`has_many`** 关系可以这样定义:

	protected $_has_many = array(
		'[alias name]' => array(
			'model'       => '[model name]'，
			'foreign_key' => '[column]'，
		),
	);

同样，你可以省略，右边的数组所有键，使用默认：

	protected $_has_many = array('[alias name]' => array());

对于我们的 author 和 post 的例子，在 user 模型类似于以下:

	protected $_has_many = array('posts' => array());

承接上面的例子，可以通过 [ORM::find_all] 来获得该 author 的所有的 post 。 注意在这个例子中使用的是 [ORM::find_all] 。 **`belongs_to`** 和 **`has_one`** 关系类型， 该模型已经加载了必要的数据。 使用像 **`has_many`** 这样的关系，你可能有要限制结果的数量的需求，或者还有其他的搜索条件，这样的话您可以将这些条件添加到 [ORM::find_all] 之前 。

**The model name used by default will be the singular name of the alias using the `inflector` class。**  这个情况下，`posts` 使用 `post` 作为模型的名字。  外键的如果缺省的话，默认值为 外键隶属的模型名字后缀以 `_id` 构成。  那么，外键就是 `user_id` 并且 前提是在 `posts` 表中必须存在这样的一个列。

我们假设你先想用 别名 `stories` 来代替 `posts` ，并且依旧使用 **`belongs_to`** 例子中的 `author_id` 。 您就可以这样定义您的 **`has_many`** 关系:

	protected $_has_many = array(
		'stories' => array(
			'model'       => 'post'，
			'foreign_key' => 'author_id'，
		),
	);

## has_one

**`has_one`** 关系 和  **`has_many`** 关系 有相似之处。  **`has_one`** 是两个模型间 有且只有一个的从属关系 (而 `has_many 是 有一个 或者 多个 从属的关系 )。 如果 user 只对应一个 post 或者 story，而不是一对多的关系 那么代码可以这样设计:

	protected $_has_one = array(
		'story' => array(
			'model'       => 'post'，
			'foreign_key' => 'author_id'，
		),
	);

## has_many "through"

**`has_many "through"`**  关系可来描述 多对多 的模型间关系。  例如，让我们假设，现在我们有了一个额外的模型，称为 `Category`。  post 可能属于多个 category，每个 category 可能有多个 post 。  它们连接起来，需要表中有一个额外的列，`post_id` 和 `category_id` (有时被称为数据透视表)。  我们将其模型命名为  `Post_Category` 相应的表是 `categories_posts`。

要定义 **`has_many "through"`** 这样的关系，那么想通的语法是在 **`has_many`** 关系的标准语法上增加 **'through'** 这样一个参数。  我们假设我们是这样使用的 `Post` 模型:

	protected $_has_many = array(
		'categories' => array(
			'model'   => 'category'，
			'through' => 'categories_posts'，
		),
	);

在 Category 的 模型中:

	protected $_has_many = array(
		'posts' => array(
			'model'   => 'post'，
			'through' => 'categories_posts'，
		),
	);

若要访问 categories 和 posts，可以这样简单的使用 `$post->categories->find_all()` 和 `$category->posts->find_all()`

多对多关系的关系 有方法可以 进行 检查，添加 和 删除。 假设我们已经有了一个加载过的 $post 模型，还有一个一样加载过的 $category 模型。 您就可以调用下面的代码来检查  $post 模型 是否和 $category 模型 有关系:

	$post->has('categories'，$category);

第一个参数是用来检查的模型的别名 ( 在这个情况下 您的 post 模型有一个以上的 category 模型的关系 )  第二个就是用来检查关系的模型。

假设你要添加关系 (在categories_posts表中创建一个新的记录),可以简单的使用下面的代码:

	$post->add('categories'，$category);

下面的是删除:

	$post->remove('categories'，$category);