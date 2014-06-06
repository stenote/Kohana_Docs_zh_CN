# OAuth 配置

OAuth 的所有配置都在 `config/oauth.php` 中， 配置文件是按照提供者名字组织。

## 示例配置文件

    return array(
        /**
         * Twitter application registration: https://twitter.com/apps
         */
        'twitter' => array(
            'key' => 'your consumer key',
            'secret' => 'your consumer secret'
        ),
        /**
         * Google application registration: https://www.google.com/accounts/ManageDomains
         */
        'google' => array(
            'key' => 'your domain name',
            'secret' => 'your consumer secret'
        ),
    );

[!!] 每种授权提供者都 **必须** 定义用户key 和 secret。