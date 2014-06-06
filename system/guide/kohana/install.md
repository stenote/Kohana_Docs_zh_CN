# 安装步骤

1. 先到[Kohana的官方网站](http://kohanaframework.org/)去下载最新 **稳定** 版。[原文](# 'Download the latest **stable** release from the [Kohana website](http://kohanaframework.org/).')
2. 将刚才下载的压缩包解压后会创建一个包含`kohana`框架的目录。[原文](# 'Unzip the downloaded package to create a `kohana` directory.')
3. 再将该目录下的文件上传至你的Web服务器。[原文](# 'Upload the contents of this folder to your webserver.')
4. 打开 `application/bootstrap.php` ，然后作如下修改：[原文](# 'Open `application/bootstrap.php` and make the following changes:')
	- 为你的应用程序设置默认[时区](http://php.net/timezones)。[原文](# 'Set the default [timezone](http://php.net/timezones) for your application.')
	- 在[Kohana::init]调用中为`base_url`设置合适的值，用以映射kohana文件夹在Web服务器上相对于网站根目录的位置。[原文](# 'Set the `base_url` in the [Kohana::init] call to reflect the location of the kohana folder on your server relative to the document root.')
6. 请确定`application/cache`和`application/logs`两个目录对web服务器进程而言具有可写权限。[原文](# 'Make sure the `application/cache` and `application/logs` directories are writable by the web server.')
7. 在你常用的浏览器里打开那个之前被你设置为了`base_url`的URL地址，以测试kohana的安装情况。[原文](# 'Test your installation by opening the URL you set as the `base_url` in your favorite browser.')

[!!] 由于zip解压过程往往依赖于你所使用的操作系统平台，而这也常常给安装过程中解压出来的子目录带来权限丢失的问题。不过，我们可以通过`find . -type d -exec chmod 0755 {} \;`将kohana下所以文件的权限都设定为755来解决该问题。[原文](# 'Depending on your platform, the installation's subdirs may have lost their permissions thanks to zip extraction. Chmod them all to 755 by running `find . -type d -exec chmod 0755 {} \;` from the root of your Kohana installation.')

你应该看看这个安装测试页面。如果它报告了任何错误，那么你就需要更正了这些错误之后再继续。[原文](# 'You should see the installation page. If it reports any errors, you will need to correct them before continuing.')

![安装页面](install.png "安装页面示例")

一旦你的安装页的报告中表明你正确地配置了安装环境，那么那就可以将kohana目录下的`install.php`重命名或者删除。这样，Kohana就被安装上了，你应该可以看到默认控制器welcome的输出：[原文](# 'Once your install page reports that your environment is set up correctly you need to either rename or delete `install.php` in the root directory. Kohana is now installed and you should see the output of the welcome controller:')

![欢迎页面](welcome.png "欢迎页面示例")

## 从Github上安装Kohana3.1

Kohana 3.1 的[源代码](http://github.com/kohana/kohana)已放在了[GitHub](http://github.com)上进行托管。要使用github上的源码来安装Kohana，你首先需要安装git。祥请访问[http://help.github.com](http://help.github.com)来了解如何在你的操作系统平台上安装git。[原文](# 'The [source code](http://github.com/kohana/kohana) for Kohana 3.1 is hosted with [GitHub](http://github.com).  To install Kohana using the github source code first you need to install git.  Visit [http://help.github.com](http://help.github.com) for details on how to install git on your platform.')

[!!] 要了解关于使用git子模块来安装Kohana的更多信息，请查看[Working with Git](tutorials/git)教程。[原文](# 'For more information on installing Kohana using git submodules, see the [Working with Git](tutorials/git) tutorial.')