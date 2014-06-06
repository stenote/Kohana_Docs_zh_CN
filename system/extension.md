# 透明类扩展

这 [cascading filesystem](files) 允许透明的类扩展。例如，类 [Cookie] 在 `SYSPATH/classes/cookie.php` 里定义为：

    class Cookie extends Kohana_Cookie {}

默认的 Kohana 类以及许多其他扩展都使用了这个定义，因此如果扩展这个类的话，它们都能得到扩展。可以通过在自己的 `APPPATH/classes/cookie.php` 里添加自己的方法来透明地扩展这个类。这个方法也是用于其他所有的类。

[!!] 永远不要修改随 Kohana 发行的任何文件。只适用透明扩展来修改类以避免升级问题（所有对文件的修改都会丢失）。
[原文](# '
[!!] You should **never** modify any of the files that are distributed with Kohana. Always make modifications to classes using transparent extension to prevent upgrade issues.
')

例如，如果要使用 [Encrypt] 类设置加密过的 cookies 从而建立新的方法，应该建立文件 `application/classes/cookie.php`，它扩展了 Kohana_Cookie，并加入函数：
[原文](# '
For instance, if you wanted to create method that sets encrypted cookies using the [Encrypt] class, you would create a file at `application/classes/cookie.php` that extends Kohana_Cookie, and adds your functions:
')

    <?php defined('SYSPATH') or die('No direct script access.');

    class Cookie extends Kohana_Cookie {

        /**
         * @var  mixed  default encryption instance
         */
        public static $encryption = 'default';

        /**
         * Sets an encrypted cookie.
         *
         * @uses  Cookie::set
         * @uses  Encrypt::encode
         */
         public static function encrypt($name, $value, $expiration = NULL)
         {
             $value = Encrypt::instance(Cookie::$encrpytion)->encode((string) $value);

             parent::set($name, $value, $expiration);
         }

         /**
          * Gets an encrypted cookie.
          *
          * @uses  Cookie::get
          * @uses  Encrypt::decode
          */
          public static function decrypt($name, $default = NULL)
          {
              if ($value = parent::get($name, NULL))
              {
                  $value = Encrypt::instance(Cookie::$encryption)->decode($value);
              }

              return isset($value) ? $value : $default;
          }

    } // End Cookie


现在调用 `Cookie::encrypt('secret', $data)` 将建立一个加密的 cookie，而使用 `$data = Cookie::decrypt('secret')`. 则可以对它进行解密。
[原文](# '
Now calling `Cookie::encrypt('secret', $data)` will create an encrypted cookie which we can decrypt with `$data = Cookie::decrypt('secret')`.
')

## 工作原理

To understand how this works, let's look at what happens normally.  When you use the Cookie class, [Kohana::autoload] looks for `classes/cookie.php` in the [cascading filesystem](files).  It looks in `application`, then each module, then `system`. The file is found in `system` and is included.  Of coures, `system/classes/cookie.php` is just an empty class which extends `Kohana_Cookie`.  Again, [Kohana::autoload] is called this time looking for `classes/kohana/cookie.php` which it finds in `system`.

When you add your transparently extended cookie class at `application/classes/cookie.php` this file essentially "replaces" the file at `system/classes/cookie.php` without actually touching it.  This happens because this time when we use the Cookie class [Kohana::autoload] looks for `classes/cookie.php` and finds the file in `application` and includes that one, instead of the one in system.

## 例子: 改变 [Cookie] 设置

If you are using the [Cookie](cookies) class, and want to change a setting, you should do so using transparent extension, rather than editing the file in the system folder.  If you edit it directly, and in the future you upgrade your Kohana version by replacing the system folder, your changes will be reverted and your cookies will probably be invalid.  Instead, create a cookie.php file either in `application/classes/cookie.php` or a module (`MODPATH/<modulename>/classes/cookie.php`).

	class Cookie extends Kohana_Cookie {
	
		// Set a new salt
		public $salt = "some new better random salt phrase";
		
		// Don't allow javascript access to cookies
		public $httponly = TRUE;
		
	}

## Example: TODO: an example

Just post the code and brief description of what function it adds, you don't have to do the "How it works" like above.

## Example: TODO: something else

Just post the code and brief description of what function it adds, you don't have to do the "How it works" like above.

## More examples

TODO: Provide some links to modules on github, etc that have examples of transparent extension in use.

## Multiple Levels of Extension

If you are extending a Kohana class in a module, you should maintain transparent extensions. In other words, do not include any variables or function in the "base" class (eg. Cookie). Instead make your own namespaced class, and have the "base" class extend that one. With our Encrypted cookie example we can create `MODPATH/mymod/encrypted/cookie.php`:

	class Encrypted_Cookie extends Kohana_Cookie {

		// Use the same encrypt() and decrypt() methods as above

	}

And create `MODPATH/mymod/cookie.php`:

	class Cookie extends Encrypted_Cookie {}

This will still allow users to add their own extension to [Cookie] while leaving your extensions intact. To do that they would make a cookie class that extends `Encrypted_Cookie` (rather than `Kohana_Cookie`) in their application folder.