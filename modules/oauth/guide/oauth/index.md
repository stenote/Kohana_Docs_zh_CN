# 关于开放授权(OAuth)

[OAuth](http://oauth.net/) 是一个开放协议，它允许以一种简单和标准的方式进行安全的API授权认证，适用于桌面和web应用。这个模块提供OAuth v1.0 协议标准的干净PHP实现 ，并支持 PLAINTEXT 和 HMAC-SHA1 签名。

## 支持的开放授权

下列开放授权是默认支持的:

* [Twitter](http://twitter.com/) 使用 [OAuth_Provider_Twitter]
* [Google](http://www.google.com/) 使用 [OAuth_Provider_Google]

其它的开放授权支持可以通过扩展[OAuth_Provider]创建。