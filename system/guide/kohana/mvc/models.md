#模型[原文](# # Models)

来自维基百科[原文](# From Wikipedia:)

模型管理着程序的主要行为和数据[原文](# > The model manages the behavior and data of the application domain, )
响应关于状态的请求信息(通常来自于视图)[原文] (# > responds to requests for information about its state (usually from the view),)
也响应指令来改变状态(通常来源于控制器)[原文] (# > and responds to instructions to change state (usually from the controller).)

创建一个简单的模型[原文](# Creating a simple model:)

	class Model_Post extends Model
	{
		public function do_stuff()
		{
			// This is where you do domain logic...
		}
	}

如果要使用数据库,只要使用模型继承Model_Database类即可[原文](# If you want database access, have your model extend the Model_Database class:)

	class Model_Post extends Model_Database
	{
		public function do_stuff()
		{
			// This is where you do domain logic...
		}

		public function get_stuff()
		{
			// Get stuff from the database:
			return $this->db->query(...);
		}
	}

如果你要 CRUD/ORM 功能,看看[ORM Moule]((../../guide/orm)这个是数径)[原文](# If you want CRUD/ORM capabilities, see the [ORM Module](../../guide/orm))