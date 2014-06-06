# Sharing Kohana (共用Kohana)

因为Kohana遵循[前端控制器]模式,即所有请求都是向'index.php'发送,文件系统可配置.在'index.php'中,你可以改变'$application','$modules','$system'的路径.[原文](# Because Kohana follows a [front controller] pattern, which means that all requests are sent to `index.php`, the filesystem is very configurable.  Inside of `index.php` you can change the `$application`, `$modules`, and `$system` paths.)

[!!]每个Kohana文件在顶部都有一个安全检查防止没有使用前端控制器而被访问.因此,最好在'.htaccess'文件也保护这些文件.把'application','modules','system'目录移到不能被访问的地方与web能新增安全层相比,这个是可选的.[原文](# [!!] There is a security check at the top of every Kohana file to prevent it from being accessed without using the front controller.  Also, the `.htaccess` file should protect those folders as well.  Moving the application, modules, and system directories to a location that cannot be accessed vie web can add another layer of security, but is optional.   )

'$application'变量允许你设置包含application的目录.默认值是'application'.'$modules'变量设置了'模块'目录.'$system'变量设置了包含Kohana默认文件的目录.你可以把它的移到任意位置.[原文](# The `$application` variable lets you set the directory that contains your application files. By default, this is `application`. The `$modules` variable lets you set the directory that contains module files. The `$system` variable lets you set the directory that contains the default Kohana files. You can move these three directories anywhere.)

例如,默认的目录设置看起来是这个样子的:[原文](# For instance, by default the directories are set up like this:)

    www/
        index.php
        application/
        modules/
        system/

你也可以把目录移到web root的外面,看就起来就是这样:[原文](# You could move the directories out of the web root so they look like this:)

    application/
    modules/
    system/
    www/
        index.php

接下来,你得在'index.php'把设置改变成这样:[原文](# Then you would need to change the settings in `index.php` to be:)

    $application = '../application';
    $modules     = '../modules';
    $system      = '../system';

## Sharing system and modules(共用系统和模块)

在进一步之前,我先指定几个kohana的apps共用'system'和'modules'目录.例如:(这个仅仅是举例,位置你可任意放)[原文](# To take this a step further, we could point several kohana apps to the same system and modules folders.  For example (and this is just an example, you could arrange these anyway you want):)

	apps/
		foobar/
			application/
			www/
		bazbar/
			application/
			www/
	kohana/
		3.0.6/
		3.0.7/
		3.0.8/
	modules/

当然,你得在'index.php'中修改配置(代码如下):[原文](# And you would need to change the settings in `index.php` to be:)

	$application = '../application';
	$system      = '../../../kohana/3.0.6';
	$modules     = '../../../kohana/modules';

通过这种方式每个app能指向特定的kohana,你也可以增加新版本,你可以编辑它们的'index.php'来快速升级apps.[原文](# Using this method each app can point to a central copy of kohana, and you can add a new version, and quickly update your apps to point to the new version by editing their `index.php` files.)
