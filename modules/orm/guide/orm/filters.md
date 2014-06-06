# 过滤器(Filters)

Filters 在 ORM 中的功能 就好像他们是 的 3.0.x [Validation] 类的一部分，但是为了配合在 在 3.1.x [Validation] 的灵活的语法规则 进行了修改. Filters 可以用来在你的模型字段被赋值的时候触发运行 并且 可以用来在数据被插入之前对数据进行格式化.

定义自己的过滤器和定义规则方法相同: 使用 [ORM::filters] 方法返回一个像下面示例数组:

	public function filters()
	{
		return array(
			'username' => array(
				array('trim'),
			),
			'password' => array(
				array(array($this, 'hash_password')),
			),
			'created_on' => array(
				array('Format::date', array(':value', 'Y-m-d H:i:s')),
			),
		);
	}

[!!] 定义过滤器时, 你可以使用 `:value`, `:field`, 和 `:model` 参数分别来指定字段值, 字段名字, 和模型的接口.