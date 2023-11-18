# 事件订阅

## 看前须知

很烂，而且最近钉钉有奇奇怪怪的重试机制，很抽象，但是能用

## 配置事件订阅

记下aes\_key和token，请求网址为./index.php

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

然后，在<mark style="color:green;">config/cropid.json</mark>填写相关信息：

```json
{
    "企业cropid": {
	"AppKey": "如果你配置了前面的内容那这应该是有东西的",
	"AppSecret": "如果你配置了前面的内容那这应该是有东西的",
	"coolAppCode": "如果你配置了前面的内容那这应该是有东西的",
        "encodingAesKey": "上面的aes_key",
	"token": "上面的token",
	"main_group": "如果你配置了前面的内容那这应该是有东西的"
    }
}
```

## 使用

如果前面的东西都顺利，那么应该能保存成功了。我们为了测试，可以点击改群名的事件启用![](<../../.gitbook/assets/image (99).png>)

在插件文件夹写入以下代码：

```php
if ($bot_run_as['chat_mode'] == "cb") {
    send_message($bot_run_as['callbackContent'],webhook);
}
```

修改一个装了机器人的群的群名，webhook写机器人固定webhook

如果可以，你会收到机器人的信息

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

## 其他参数

* $bot\_run\_as\['callbackContent'] 事件内容
* $bot\_run\_as\['appInfo'] 触发事件的App
