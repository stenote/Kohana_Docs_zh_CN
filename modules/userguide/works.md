# 用户指南怎样工作

用户指南在文档中使用[Markdown](markdown)。包括用户指南页面和API浏览器里面的代码注释都是采用markdown语法。

##用户指南页面

用户指南内容包含在相应的模块中，在 `guide/<module>` 。比如,Kohana的文档在 `system/guide/kohana`  ，ORM的文档在 `modules/orm/guide/orm` , 数据库在`modules/database/guide/database` ,其它的以此类推。

每个模块都有一个索引页面 在`guide/<module>/index.md`。

每个模块的目录在 `guide/<module>/menu.md` 。

其它的全部页面在 `guide/<module>` 。它们可以按照目录的方式组织管理，命名自由。



更多关于为自己的模块添加用户指南页面的内容，请参考[添加自己的模块](adding)

### 图像

在用户指南中使用的任何的图片都应该放在`media/guide/<module>/`。比如，如果一个页面包含图片 `![Image Title](hello-world.jpg)` ，那么这张图片的位置应该是`media/guide/<module>/hello-world.jpg` 。 ORM模块部分的图像应该是在 `modules/orm/media/guide/orm` ， Kohana文档中的图像应该是在`system/media/guide/kohana`。

### API浏览器

API浏览器的内容由实际的源代码产生。类、常量、属性、方法的描述 都由注释中展开获得，按照Markdown格式解析。比如，你在查看[Kohana_Core::init](http://github.com/kohana/core/blob/c443c44922ef13421f4a/classes/kohana/core.php#L5)的注释时可以看到一个markdown目录和表。这些在API浏览器中都能够正确解析和显示`@param`, `@uses`, `@throws`, `@returns`,其它的标签也是同样的。

TODO：在关于怎样评论你类、常量、方法等给出更确定的细节。比如，包含的包和它是怎样与api 模块相关联的。