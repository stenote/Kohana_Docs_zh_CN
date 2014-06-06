#调试

Kohana框架自带了一些工具，可以帮助进行调试你的应用

最常用的方法是 [Debug::vars]. 这个方法将会列出所有变量, 和 [var_export](http://php.net/var_export) 或者 [print_r](http://php.net/print_r)函数类似, 但是是以html的格式
   
    //打印变量 $foo 和 $bar  

    echo Debug::vars($foo, $bar);

Kohana也提供了一个方法显示一个特殊文件的源代码，通过使用[Debug::source].

    // 显示此行的源代码
    echo Debug::source(__FILE__, __LINE__);

如果在显示应用程序文件信息时不想暴露安装目录，就可以使用这个方法 [Debug::path]:

    // Displays "APPPATH/cache" rather than the real path
    echo Debug::path(APPPATH.'cache');

如果出现了问题，可以检查 Kohana 日志以及 Web 服务器的日志

还可以使用调试工具如 Xdebug [Xdebug](http://www.xdebug.org/)