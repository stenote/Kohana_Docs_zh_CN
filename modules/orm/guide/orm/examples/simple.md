# 简单的示例

这是一个 单模型的 ORM 使用范例, 没有实用关系, 但是用了字段的验证. 

## 数据表结构

	CREATE TABLE IF NOT EXISTS `members` (
	  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(32) NOT NULL,
	  `first_name` varchar(32) NOT NULL,
	  `last_name` varchar(32) NOT NULL,
	  `email` varchar(127) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;

## 模型
	
	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Model_Member extends ORM {

		public function rules()
		{
			return array(
				'username' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'first_name' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'last_name' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'email' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 127)),
					array('email'),
				),
			);
		}
	}

[!!] 当你使用 `ORM::save()` 时, 将触发 `ORM::rules()`，`ORM::rules()` 将一个数组结果 传递到一个 [Validation] 对象来对字段进行验证 . 

[!!] 请注意 在模型中定义主键 `id` 并不是必须的。 并且 数据库中的表名 是 **复数** 和 模型的名称 是 **奇异**。

## 控制器

	<?php defined('SYSPATH') or die('No direct access allowed.');
	
	class Controller_Member extends Controller_Template {
		
		public function action_index()
		{
			/**
			 * 范例一
			 */
			
			// 创建一个 `member` 的模型接口
			$members = ORM::factory('member');
			
			// 获得所有 `first name` 是 "Peter" 的结果
			// find_all() 意思就是找到所有符合条件的结果。
			$members->where('first_name', '=', 'Peter')->find_all();

			// 获得 $members 对象中的 记录数量
			$members->count_all();
			
			/**
			 * 范例二
			 */
			
			// 创建一个 `member` 的模型接口
			$member = ORM::factory('member');
			
			// 获得一个 `user name` 是 "bongo" 的 成员记录 find() 的意思是
			// 我们只获得第一个匹配的记录.
			$member->where('username', '=', 'bongo')->find();
			
			/**
			 * 范例三
			 */
			
			// 创建一个 `member` 的模型接口
			$member = ORM::factory('member');
			
			// 执行 一个 `INSERT` 查询
			$member->username = 'bongo';
			$member->first_name = 'Peter';
			$member->last_name = 'Smith';
			$member->save();
			
			/**
			 * 范例四
			 */
			
			// 创建一个 `member`表中的 "id" 列的值 等于 1 的模型接口
			$member = ORM::factory('member', 1);
			
			// 执行 更新 操作
			$member->username = 'bongo';
			$member->first_name = 'Peter';
			$member->last_name = 'Smith';
			$member->save();
		}
	}

[!!] 如果要访问 模型的列值 ，可以想访问 PHP 对象一样使用 $member->first_name 。