# 基本用法

## 加载一个模型的接口

以下的两种方法都可以创建模型 `Model_User` 的接口 [原文](# 'To create a new `Model_User` instance, you can do one of two things'):

	$user = ORM::factory('user');
	// Or
	$user = new Model_User();

## 新增

下面这个是在数据库中新增加一条记录, 首先获得一个模型的接口 [原文](# 'To insert a new record into the database, create a new instance of the model'):

	$user = ORM::factory('user');

接下来, 首先设置模型的每个属性 [原文](# 'Then, assign values for each of the properties');

	$user->first_name = 'Trent';
	$user->last_name = 'Reznor';
	$user->city = 'Mercer';
	$user->state = 'PA';

通过执行 [ORM::save] 在数据库中插入一条新纪录 [原文](# 'Insert the new record into the database by running [ORM::save]'):

	$user->save();

[ORM::save] 方法执行时会检查插入的主键的值是否在数据库中已经存在 (缺省是`id`). 如果已有, ORM 将会 更新 这条数据的相关数值, 来代替 插入 [原文](# '[ORM::save] checks to see if a value is set for the primary key (`id` by default). If the primary key is set, then ORM will execute an `UPDATE` otherwise it will execute an `INSERT`').


## 查找模型

可以通过 [ORM::find] 这个方法查找一个模型 或者 通过传递ID到 [ORM::factory] 方法中 [原文](# 'To find an object you can call the [ORM::find] method or pass the id into the ORM constructor'):

	// 找到 user 中 id 是 20的模型
	$user = ORM::factory('user')
		->where('id', '=', 20)
		->find();
	// 或者
	$user = ORM::factory('user', 20);

## 使用 ORM 加载一条记录

[ORM::loaded] 方法可以检测 ORM 是否成功的加载了一条记录 [原文](# 'Use the [ORM::loaded] method to check that ORM successfully loaded a record').

	if ($user->loaded())
	{
		// 成功加载
	}
	else
	{
		// 出错处理
	}

## 更新记录 和 保存记录

ORM 模型 加载过之后, 你可以像这样修改 模型的 属性 [原文](# 'Once an ORM model has been loaded, you can modify a model's properties like this'):

	$user->first_name = "Trent";
	$user->last_name = "Reznor";

如果你想将更改保存到数据库中, 可以像这样使用 save() 方法 [原文](# 'And if you want to save the changes you just made back to the database, just run a `save()` call like this'):

	$user->save();



## 删除记录


要删除一个模型, 可以使用 [ORM::delete] 来删除你已经加载的 模型 [原文](# 'To delete an object, you can call the [ORM::delete] method on a loaded ORM model').

	$user = ORM::factory('user', 20);
	$user->delete();