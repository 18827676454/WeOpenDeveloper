# WeOpenDeveloper
WeOpenDeveloper 为微信开放平台服务开发工具，基于 WeChatDeveloper 可对公众号进行管理。
更多功能可以参考下面的文档。

Documentation
--


> WeChatDeveloper 是基于官方接口封装，在做微信开发前，必需先阅读微信官方文档。
>* 微信官方文档：http://mp.weixin.qq.com/wiki
>* 开放平台文档：https://open.weixin.qq.com
>* 商户支付文档：https://pay.weixin.qq.com/wiki/doc/api/index.html

> 针对 WeChatDeveloper 也有一准备了帮助资料可供参考。
>* WeChatDeveloper 文档：http://www.kancloud.cn/zoujingli/wechat-developer



* 接口实例所需参数
```php

// 配置参数（可以公众号服务平台获取）
$config = [
    'component_appid'          => 'wx4e63e993e222df8d',
    'component_token'          => 'P8QHTIxpBEq88IrxatqhgpBm2OAQROkI',
    'component_appsecret'      => '7cfa1afa87a41e2ea3445cea015c0974',
    'component_encodingaeskey' => 'L5uFIa0U6KLalPyXckyqoVIJYLhsfrg8k9YzybZIHsx',
];

// 注册授权公众号 AccessToken 处理
$config['GetAccessTokenCallback'] = function ($authorizer_appid) use ($config) {
    $open = new \WeOpen\Service($config);
    $authorizer_refresh_token = ''; // 通过$authorizer_appid从数据库去找吧，在授权绑定的时候获取
    $result = $open->refreshAccessToken($authorizer_appid, $authorizer_refresh_token);
    if (empty($result['authorizer_access_token'])) {
        throw new \WeChat\Exceptions\InvalidResponseException($result['errmsg'], '0');
    }
    $data = [
        'authorizer_access_token'  => $result['authorizer_access_token'],
        'authorizer_refresh_token' => $result['authorizer_refresh_token'],
    ];
    // 需要把$data记录到数据库
    return $result['authorizer_access_token'];
};
```

* Ticket 接收处理
```php

try{

    // 实例公众号服务接口
    $server = new \WeOpen\Service($config);
    
    // 获取并更新Ticket推送
    if (!($data = $server->getComonentTicket())) {
        return "Ticket event handling failed.";
    }
    
} catch (Exception $e) {

    // 出错啦，处理下吧
    echo $e->getMessage() . PHP_EOL;

}
```

* 实例指定接口
```php
try{

    // 实例公众号服务接口
    $open = new \WeOpen\Service($config);
    
    // 获取公众号接口操作实例
    $wechat = $open->instance('User', 'wx60a43dd8161666d4');
    
    // 获取公众号粉丝列表
    $list = $wechat->getUserList();
    var_export($list);
    
} catch (Exception $e) {

    // 出错啦，处理下吧
    echo $e->getMessage() . PHP_EOL;

}

