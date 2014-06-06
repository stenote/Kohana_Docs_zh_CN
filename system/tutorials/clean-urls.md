# Clean URLs

将index.php移出你的urls.[原文](#Removing `index.php` from your urls.)

为了urls保持简洁,你应很想能在没有index.php中的urls浏览app(网站).按如下方法可以从URL中把index.php移除,只要二步哦.[原文](#To keep your URLs clean, you will probably want to be able to access your app without having `/index.php/` in the URL. There are two steps to remove `index.php` from the URL.)

1.编辑bootstrap文件[原文](#1. Edit the bootstrap file)
2.设置rewriting(.htaccess)[原文](#2. Set up rewriting)

## 1. Configure Bootstrap(下面开始了.)

第一件要你改变的事就是在把,在[Kohana::init]中把'index_file'设为false[原文](#The first thing you will need to change is the `index_file` setting of [Kohana::init] to false:)

    Kohana::init(array(
        'base_url'   => '/myapp/',
        'index_file' => FALSE,
    ));

这个改动将原来使用[URL::site],[URL::base],和[HTML::anchor]的链接不再包含'index.php'.所有链接会以'/myapp/'打头,而不是'/myapp/index.php/'.[原文](#This change will make it so all of the links generated using [URL::site], [URL::base], and [HTML::anchor] will no longer include "index.php" in the URL. All generated links will start with `/myapp/` instead of `/myapp/index.php/`.)

## 2. URL Rewriting

开启rewriting功能的方法不尽相同,这个取决你的服务器.(#Enabling rewriting is done differently, depending on your web server.)

Rewriting will make it so urls will be passed to index.php.

## Apache(这个阿帕奇应该都知道吧)

将'example.htaccess'改成'.htaccess',并确保'RewriteBase'后面的地址与[Kohana::init]设置中的'base_url'相匹配就OK了.[原文](#Rename `example.htaccess` to only `.htaccess` and alter the `RewriteBase` line to match the `base_url` setting from your [Kohana::init])

    RewriteBase /myapp/

剩余的'.htaccess 文件'重定向所有通index.php的请求,前提就是文件一直在你的服务器上.(css,images,favicon,什么的还依然会像原来一样载入.)绝大部分情况下,你的工作(url中去index.php)已完成鸟![原文](#The rest of the `.htaccess file` rewrites all requests through index.php, unless the file exists on the server (so your css, images, favicon, etc. are still loaded like normal).  In most cases, you are done!)

### Failed!(失败了)

如果你看到'Internal Server Error'或'No input file specified'错误,试着写上这个[原文](#If you get a "Internal Server Error" or "No input file specified" error, try changing:)

    RewriteRule ^(?:application|modules|system)\b - [F,L]

当然,我们还可以这样[原文](#Instead, we can try a slash:)

    RewriteRule ^(application|modules|system)/ - [F,L]

如果那还不行,再试下这个[原文](#If that doesn't work, try changing:)

    RewriteRule .* index.php/$0 [PT]

更简单的就这么干[原文](#To something more simple:)

    RewriteRule .* index.php [PT]

### Still Failed!(还没成功,这RP是不是也太差了)

如果你还是失败,检查下你的主机是支持URL重写.如果你能改Apache的配置,在'httpd.conf'文件中把下面的代码加上.[原文](#If you are still getting errors, check to make sure that your host supports URL `mod_rewrite`. If you can change the Apache configuration, add these lines to the the configuration, usually `httpd.conf`:)

    <Directory "/var/www/html/myapp">
        Order allow,deny
        Allow from all
        AllowOverride All
    </Directory>

同时最好也检查下Apache的日志看看能不能找到一些解决错误的线索.[原文](#You should also check your Apache logs to see if they can shed some light on the error.)

## NGINX

给出nginx的配置有点困难(一般PHP都是运行在Apache上的),但是这里还是给出个简单的[原文](#It is hard to give examples of nginx configuration, but here is a sample for a server:)

    location / {
        index     index.php index.html index.htm;
        try_files $uri index.php;
    }

    location = index.php {
        include       fastcgi.conf;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index index.php;
    }

如果你使这个正确运行有困难,开启debug level登录nginx检查下错误日志.(这个nginx,真没玩过,蛋疼)[原文](#If you are having issues getting this working, enable debug level logging in nginx and check the access and error logs.)
