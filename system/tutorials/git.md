创建新应用.(# # Creating a New Application)

[!!]运行下面的示例的前提是你的服务器已架好,并计划在<http://localhost/gitorial>新建一个应用.(#[!!] The following examples assume that your web server is already set up, and you are going to create a new application at <http://localhost/gitorial/>.)
#Git --- 傻瓜内容跟踪器,一种版本控制器.这个教程运行平台是Linux.
使用你的Git控制台,使用空目录'gitorial'运行'git init'.这就创建一个全新的git控制版本库.[原文](#Using your console, change to the empty directory `gitorial` and run `git init`. This will create the bare structure for a new git repository.)

接下来,我们要'系统'目录创建一个[子模块](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html).打开<http://github.com/kohana/core>并复制下面的'Clone URL':[原文](#Next, we will create a [submodule](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html) for the `system` directory. Go to <http://github.com/kohana/core> and copy the "Clone URL":)

![Github Clone URL](http://img.skitch.com/20091019-rud5mmqbf776jwua6hx9nm1n.png)#这张图片的Clone URL是:git//github.com/kohana/core.git

现在使用URL为'system'创建子模块:[原文](# Now use the URL to create the submodule for `system`:)

    git submodule add git://github.com/kohana/core.git system #这是命令,不翻译.

[!!]这会创建一个当前开发版本到下个稳定版的链接.开发版本差不多可以一直放心使用,它和bug不断被修复的稳定版本有着相同的API.[原文](# [!!] This will create a link to the current development version of the next stable release. The development version should almost always be safe to use, have the same API as the current stable download with bugfixes applied.)

现在可以任意添加你需要的子模块(目录).举个例子,如果你需要[数据库]模块:[原文](# Now add whatever submodules you need. For example, if you need the [Database] module:)
#运行下面的语句即可
    git submodule add git://github.com/kohana/database.git modules/database

在添加子模块后,必须初始化:[原文](# After submodules are added, they must be initialized:)

    git submodule init

现在子模块已添加完成,你可以提交他们:[原文](#Now that the submodules are added, you can commit them:)

    git commit -m 'Added initial submodules'

接下来,创建一个application目录,下面是最小核心需求(必须有):[原文](#Next, create the application directory structure. This is the bare minimum required:)

    mkdir -p application/classes/{controller,model}
    mkdir -p application/{config,views}
    mkdir -m 0777 -p application/{cache,logs}

如果运行'find application'你应看到如下所列目录:[原文](# If you run `find application` you should see this:)

    application
    application/cache
    application/config
    application/classes
    application/classes/controller
    application/classes/model
    application/logs
    application/views

我们不想控制'log'或'cache'目录,就添加'.gitignore'到各自的目录.这将无视(log,cache)所有没有隐藏的文件:[原文](#We don't want git to track log or cache files, so add a `.gitignore` file to each of the directories. This will ignore all non-hidden files:)

    echo '[^.]*' > application/{logs,cache}/.gitignore

Git 无视空目录,因此添加一个'.gitignore'文件也确保git 记录目录,但不包括目录内的文件.[原文](#[!!] Git ignores empty directories, so adding a `.gitignore` file also makes sure that git will track the directory, but not the files within it.)

现在我们需要'index.php'和'bootstrap'文件:[原文](# Now we need the `index.php` and `bootstrap.php` files:)

    wget https://github.com/kohana/kohana/raw/3.1/master/index.php --no-check-certificate
    wget https://github.com/kohana/kohana/raw/3.1/master/application/bootstrap.php --no-check-certificate -O application/bootstrap.php

提交这些有变动:[原文](# Commit these changes too:)

    git add application
    git commit -m 'Added initial directory structure'

完工鸟.现在你拥有一个使用Git来控制版本的应用(网站)了.[原文](# That's all there is to it. You now have an application that is using Git for versioning.)

## Adding Submodules(新增子模块)
完全新增一个子模块只要跟着下面的步骤:(# To add a new submodule complete the following steps:)

1.运行如下代码-git submodule为每个新增的子模块保存路径 例如:[原文](# 1. run the following code - git submodule add repository path for each new submodule e.g.:)

        git submodule add git://github.com/shadowhand/sprig.git modules/sprig

接下来,初始化并更新子模块:[原文](# 2. then init and update the submodules:)

        git submodule init
        git submodule update

## Updating Submodules(更新子模块)

在某个点上,你很可想更新你的子模块.更新所有子模块(文件)到'HEAD'(最新)版本:[原文](# At some point you will probably also want to upgrade your submodules. To update all of your submodules to the latest `HEAD` version:)

    git submodule foreach 'git checkout 3.1/master && git pull origin 3.1/master'

更新一个单独的子模块,例如,'system':[原文](# To update a single submodule, for example, `system`:)

    cd system
    git checkout 3.1/master
    git pull origin 3.1/master
    cd ..
    git add system
    git commit -m 'Updated system to latest version'

如果想更新一个单独的子模块到特定的提交(位置):[原文](# If you want to update a single submodule to a specific commit:)

    cd modules/database
    git pull origin 3.1/master
    git checkout fbfdea919028b951c23c3d99d2bc1f5bbeda0c0b
    cd ../..
    git add database
    git commit -m 'Updated database module'

需要提醒的是,你也可以检查一个已标记正式发行的某个点上的提交,例如:[原文](# Note that you can also check out the commit at a tagged official release point, for example:)

    git checkout 3.1.0

直接运行'git tag'会得到所标签的列表.[原文](# Simply run `git tag` without arguments to get a list of all tags.)

## Removing Submodules(移除子模块)
移除一个不再需要的子模块只要跟着下面的步骤:[原文](# To remove a submodule that is no longer needed complete the following steps:)

1.打开'.gitmodules'并移除子模块的参考[原文](# 1. open .gitmodules and remove the reference to the to submodule)
    It will look something like this:

        [submodule "modules/auth"]
        path = modules/auth
        url = git://github.com/kohana/auth.git

2.打开'.gif/config',移除参考的子模块[原文](# 2. open .git/config and remove the reference to the to submodule\\)

        [submodule "modules/auth"]
        url = git://github.com/kohana/auth.git

运行'git rm'--cached path/to/submodule,例如[原文](# 3. run git rm --cached path/to/submodule, e.g.)

        git rm --cached modules/auth

**注意**不要在路径结尾放斜线,否则会失败.[原文](#**Note:** Do not put a trailing slash at the end of path. If you put a trailing slash at the end of the command, it will fail.)

## Updating Remote Repository URL(更新远程库URL)

在开发项目期间,一个子模块的资源可能因各种原因而变动(创建自己的分支,服务器URL改变,库名称或路径变动等等...),但你得更新(同步)这些变化.你可以执行下面的步骤来搞定:[原文](#During the development of a project, the source of a submodule may change for any reason (you've created your own fork, the server URL changed, the repository name or path changed, etc...) and you'll have to update those changes. To do so, you'll need to perform the following steps:)

1.编辑'.gitmodules'文件,改变那些变动子模块的URL.[原文](#1. edit the .gitmodules file, and change the URL for the submodules which changed.)

2.在你的资源列表根目录运行:[原文](# 2. in your source tree's root run:)

		git submodule sync

3.运行'git init'同步新的URLs到项目库配置库:[原文](# 3. run `git init` to update the project's repository configuration with the new URLs:)

		git submodule init

收工,现在可以继续放心的玩转你的子模块.[原文](# And it's done, now you can continue pushing and pulling your submodules with no problems.)

资源: http://jtrancas.wordpress.com/2011/02/06/git-submodule-location/[原文](# Source: http://jtrancas.wordpress.com/2011/02/06/git-submodule-location/)
