# Cookies 类

Kohana的提供的类，可以很容易的在cookie和session中工作。高层次的sessions和cookies都提供了相同的功能。它们允许开发者为以后取回来存储临时的或持久的特定客户端的信息，通常是为了使一些东西能在请求之间持续。
（原文#Kohana provides classes that make it easy to work with both cookies and sessions. At a high level both sessions and cookies provide the same functionality. They allow the developer to store temporary or persistent information about a specific client for later retrieval, usually to make something persistent between requests.)

[Cookies](http://en.wikipedia.org/wiki/HTTP_cookie)用来存储将持续一个相当长时间的、非私人的数据。例如存储用户的偏好或语言设置。用[Cookie]类获取或设置cookies。
（原文#[Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) should be used for storing non-private data that is persistent for a long period of time. For example storing a user preference or a language setting. Use the [Cookie] class for getting and setting cookies.）

[!!]Kohana用"已签名"的cookies。每次存储的cookie都结合了安全的hash算法处理，以防止cookie被修改。如果一个cookie在Kohana以外被修改，该hash将是不正确的，且该cookie将被删除。这个hash的生成使用[Cookie::salt()]，使用[Cookie::$salt]属性。你必须在你的bootstrap.php中定义这个设置：
（原文#Kohana uses "signed" cookies. Every cookie that is stored is combined with a secure hash to prevent modification of the cookie.  If a cookie is modified outside of Kohana the hash will be incorrect and the cookie will be deleted.  This hash is generated using [Cookie::salt()], which uses the [Cookie::$salt] property. You must define this setting in your bootstrap.php:）

	Cookie::$salt = 'foobar';

或在你的应用程序定义一个扩展的cookie类：
（原文#Or define an extended cookie class in your application:）

	class Cookie extends Kohana_Cookie
	{
		public static $salt = 'foobar';
	}

你应该设置一个安全的值给该salt。上面的例子是仅用于示范目的。
（原文#You should set the salt to a secure value. The example above is only for demonstrative purposes.）

没有什么能阻止你正常的去使用`$ _COOKIE`，但是你不能混淆使用Cookie类和常规的全局`$_COOKIE`，因为Kohana的hash用来签名cookies的情况将不会出现，Kohana将删除该cookie。
（原文#Nothing stops you from using `$_COOKIE` like normal, but you can not mix using the Cookie class and the regular `$_COOKIE` global, because the hash that Kohana uses to sign cookies will not be present, and Kohana will delete the cookie.）

## Storing, Retrieving, and Deleting Data

[Cookie] and [Session] provide a very similar API for storing data. The main difference between them is that sessions are accessed using an object, and cookies are accessed using a static class.

### Storing Data

Storing session or cookie data is done using the [Cookie::set] method:

    // Set cookie data
    Cookie::set($key, $value);

    // Store a user id
    Cookie::set('user_id', 10);

### Retrieving Data

Getting session or cookie data is done using the [Cookie::get] method:

    // Get cookie data
    $data = Cookie::get($key, $default_value);

    // Get the user id
    $user = Cookie::get('user_id');

### Deleting Data

Deleting session or cookie data is done using the [Cookie::delete] method:
    
    // Delete cookie data
    Cookie::delete($key);

    // Delete the user id
    Cookie::delete('user_id');

## Cookie 设置

cookie所有的设置和更改都是時用静态属性。你可以在`bootstrap.php`中或是通过[transparent extension](extension)来更改这些设置。在你的应用程序运行之前，务必检查这些设置，因为它们当中很多都会直接影响到你的应用程序的安全。
（原文#All of the cookie settings are changed using static properties. You can either change these settings in `bootstrap.php` or by using [transparent extension](extension).  Always check these settings before making your application live, as many of them will have a direct affect on the security of your application.）

最重要的设置是[Cookie::$salt]，这是用于安全签名的。这个值需要更改和保密：
（原文#The most important setting is [Cookie::$salt], which is used for secure signing. This value should be changed and kept secret:）

    Cookie::$salt = 'your secret is safe with me';

[!!] 更改这个值将使之前设置的所有cookies无效。
（原文#Changing this value will render all cookies that have been set before invalid.）

默认情况下，cookies存储到浏览器关闭之前。要使用指定的lifetime来更改[Cookie::$expiration]的设置：
（原文#By default, cookies are stored until the browser is closed. To use a specific lifetime, change the [Cookie::$expiration] setting:）

    // 设置cookies一周后到期（原文#Set cookies to expire after 1 week）
    Cookie::$expiration = 604800;

    // 为更清晰的表示，用原生的整数来替代（原文#Alternative to using raw integers, for better clarity）
    Cookie::$expiration = Date::WEEK;

cookie被限制访问的路径用[Cookie::$path]设置。
（原文#The path that the cookie can be accessed from can be restricted using the [Cookie::$path] setting.）

    // 只允许在/public/下的cookies(原文#Allow cookies only when going to /public/*）
    Cookie::$path = '/public/';

cookie被限制访问的域名用[Cookie::$domain]设置。
The domain that the cookie can be accessed from can also be restricted, using the [Cookie::$domain] setting.

    // 只允许在域名www.example.com下的cookies（原文#Allow cookies only on the domain www.example.com）
    Cookie::$domain = 'www.example.com';

如果你想是所有子域名的cookie能访问，在域名开始的地方用一个点号。
（原文#If you want to make the cookie accessible on all subdomains, use a dot at the beginning of the domain.）

    // 允许cookies在example.com和*.example.com下被访问（原文#Allow cookies to be accessed on example.com and *.example.com）
    Cookie::$domain = '.example.com';

只允许通过安全（HTTPS）连接访问的cookie，用[Cookie::$secure]设置。
（原文#To only allow the cookie to be accessed over a secure (HTTPS) connection, use the [Cookie::$secure] setting.）

    // 只允许cookie在安全连接下被访问（原文#Allow cookies to be accessed only on a secure connection）
    Cookie::$secure = TRUE;
    
    // 允许cookies在任何连接下被访问（原文#Allow cookies to be accessed on any connection）
    Cookie::$secure = FALSE;

要阻止用Javascript访问cookies，你可以更改[Cookie::$httponly]的设置。
（原文#To prevent cookies from being accessed using Javascript, you can change the [Cookie::$httponly] setting.）

    // 使cookie无法用Javascript访问(原文#Make cookies inaccessible to Javascript）
    Cookie::$httponly = TRUE;