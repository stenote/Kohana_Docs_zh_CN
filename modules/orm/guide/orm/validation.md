# [校验](Validation)

ORM 模型 和 [校验](Validation) 库 是紧密集成的 并且 控制器也配备了一个非常灵活的 [ORM_Validation_Exception] 来帮助你快速处理由于基本的 CRUD 操作产生的错误 [原文](# 'ORM models are tightly integrated with the [Validation] library and the module comes with a very flexible [ORM_Validation_Exception] that helps you quickly handle validation errors from basic CRUD operations.').

## 定义验证规则

验证规则可以定义到 `ORM::rules()` 方法中. 这个方法 返回一个像这样 要传递给 [校验](Validation) 对象的 一个数组 [原文](# 'Validation rules are defined in the `ORM::rules()` method. This method returns the array of rules to be added to the [Validation] object like so:'):


	public function rules()
	{
		return array(
			'username' => array(
				// 调用 Valid::not_empty($value) 方法;
				array('not_empty'),
				// 调用 Some_Class::some_method('param1', 'param2');
				array('Some_Class::some_method', array('param1', 'param2')),
				// 调用 A_Class::a_method($value);
				array(array('A_Class', 'a_method')),
				// 使用 匿名函数 并 传递字段值和校验对象
				array(function($value, Validation $object)
				{
					$object->error('some_field', 'some_error');
				}, array(':value', ':validation')),
			),
		);
	}

### 绑定值

ORM 会自动调用 `Validation::bind()` 绑定 下面的值 [原文](# 'ORM will automatically bind the following values with `Validation::bind()`:'):


- **:field** - 将会绑定这个规则字段的名字 [原文](# 'The name of the field the rule is being applied to.').
- **:value** - 将会绑定这个规则字段的值 [原文](# 'The value of the field the rule is being applied to.').
- **:model** - 将会绑定这个规则模型的接口 [原文](# 'The instance of the model that is being validated.').

## 自动校验

当模型的方法 `ORM::save()`, `ORM::update()`, 或 `ORM::create()` 被调用的时候 , 该模型会自动校验现有的数据. 所以应该提防这些方法因执行数据校验不符合 而 抛出一个 [ORM_Validation_Exception] 异常 [原文](# 'All models automatically validate their own data when `ORM::save()`, `ORM::update()`, or `ORM::create()` is called. Because of this, you should always expect these methods to throw an [ORM_Validation_Exception] when the model's data is invalid.').

	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = 'invalid username';
			$user->save();
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors();
		}
	}

## 处理校验异常

通过 [ORM_Validation_Exception] 异常可以访问到 试图保存一个模型的信息时遇到的 校验错误信息.  `ORM_Validation_Exception::errors()` 方法的工作机制 和 `Validation::errors()` 非常相似. 不传递参数将会返回校验失败的规则名. 但是和 `Validate::errors()` 不同的是 `ORM_Validation_Exception::errors()` 方法的第一个参数是一个路径. 该模型的 ORM::$_object_name 将追加到该目录 , 用来形成由 `Validation::errors()` 产生的错误 的消息文件. 第二个参数 和 `Validation::errors()` 方法 的参数 相同的 [原文](# 'The [ORM_Validation_Exception] will give you access to the validation errors that were encountered while trying to save a model's information. The `ORM_Validation_Exception::errors()` method works very similarly to `Validation::errors()`. Not passing a first parameter will return the name of the rules that failed. But unlike `Validate::errors()`, the first parameter of `ORM_Validation_Exception::errors()` is a directory path. The model's ORM::$_object_name will be appended to the directory in order to form the message file for `Validation::errors()` to use. The second parameter is identical to that of `Validation::errors()`.').

在下面的例子里, 这个错误信息将会被保存在  `application/messages/models/user.php` 文件中 [原文](# 'In the below example, the error messages will be defined in `application/messages/models/user.php`').


	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = 'invalid username';
			$user->save();
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors('models');
		}
	}

## 外部校验

如果某些信息不应由模型来校验的话，可以放在控制器里面做。诸如 防止[跨站请求伪造 CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)的令牌校验，密码核实，或者[验证码](http://en.wikipedia.org/wiki/CAPTCHA) 这些信息的校验都不应该由 模型 来做。然而，为了使用户能得到很好的使用体验 所做的多处校验和整合错误信息时非常单调乏味的。因此 [ORM_Validation_Exception] 内建支持 自动 处理多个 校验对象 和 自动命名校验信息数组。 `ORM::save()`, `ORM::update()`, 和 `ORM::create()` 都提供一个伴随模型对象的 可选的 校验对象类型的 第一参数。[尼玛这段好纠结有木有 >_<](# 'Certain forms contain information that should not be validated by the model, but by the controller. Information such as a [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery) token, password verification, or a [CAPTCHA](http://en.wikipedia.org/wiki/CAPTCHA) should never be validated by a model. However, validating information in multiple places and combining the errors to provide the user with a good experience is often quite tedius. For this reason, the [ORM_Validation_Exception] is built to handle multiple Validation objects and namespaces the array of errors automatically for you. `ORM::save()`, `ORM::update()`, and `ORM::create()` all take an optional first parameter which is a [Validation] object to validate along with the model.').



	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = $_POST['username'];
			$user->password = $_POST['password'];

			$extra_rules = Validation::factory($_POST)
				->rule('password_confirm', 'matches', array(
					':validation', ':field', 'password'
				));

			// 添加不在模型内的额外 校验规则
			$user->save($extra_rules);
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors('models');
		}
	}

因为 校验对象 已经被作为一个 参数 传递给了 模型 ，所有在 校验检查中所发现的错误 都被重新命名到了一个子数组中 键值 是 `_external`。这个返回错误的数组大概是酱紫：[原文](# 'Because the validation object was passed as a parameter to the model, any errors found in that check will be namespaced into a sub-array called `_external`. The array of errors would look something like this:')



	array(
		'username'  => 'This field cannot be empty.',
		'_external' => array(
			'password_confirm' => 'The values you entered in the password fields did not match.',
		),
	);

这样做就保证了多个 校验对象 和 模型的 校验错误信息不会相互覆盖。[原文](# 'This ensures that errors from multiple validation objects and models will never overwrite each other.')

[!!] [ORM_Validation_Exception] 也可以通过不同的方式用来合并从相关模型返回的错误 ，来瞅瞅 一些牛X的 [用例](examples)吧。[原文](# 'The power of the [ORM_Validation_Exception] can be leveraged in many different ways to merge errors from related models. Take a look at the list of [Examples](examples) for some great use cases.')

[这章翻译的挺烂。。]