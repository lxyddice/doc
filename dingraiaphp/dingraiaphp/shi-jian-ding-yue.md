# 事件订阅

## 看前须知

钉钉的事件订阅快要开付费了，请减少不必要的事件

## 特性

支持多企业下程序的回调，只需要你的配置文件没问题，则框架会自动寻找能解密的appKey确定是哪个app发的

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

<pre class="language-php"><code class="lang-php">/*
$bot_run_as['dingraiaMasterType'] == message 为主从普通消息
$bot_run_as['dingraiaMasterType'] == callback 为主从事件回调
$bot_run_as['chat_mode'] == mcb 为正常事件回调
$bot_run_as['chat_mode'] == cb 为主从事件回调
*/
<strong>if ($bot_run_as['chat_mode'] == "cb") {
</strong>    send_message($bot_run_as['callbackContent'],webhook);
}
</code></pre>

修改一个装了机器人的群的群名，webhook写机器人固定webhook

如果可以，你会收到机器人的信息

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

## 其他参数

* $bot\_run\_as\['callbackContent'] 事件内容
* $bot\_run\_as\['appInfo'] 触发事件的App

## 其他功能

### 加密

```php
require_once("module/DingraiaPHP/getcallback.php");
en_DingraiaDingtalkCallback(加密字符串,$nonce, 签名token, aes_key, appKey);

//使用示例
en_DingraiaDingtalkCallback('success',$nonce, $token, $encodingAesKey, $suiteKey);
/*
输出（注意，是json，事失败返回false）：
{"msg_signature":"加签","encrypt":"success的加密内容","timeStamp":"时间戳","nonce":"nonce"}
*/
```

### 解密

```php
require_once("module/DingraiaPHP/getcallback.php");
$result = de_DingraiaDingtalkCallback(msg_sign, 时间戳, nonce, 加密内容, 签名token, aes_key, appKey);

//注意，若成功，返回的是json，失败会返回false
```

## 后记

* 该加解密代码改编于[https://github.com/hlf513/dingtalk-crypto](https://github.com/hlf513/dingtalk-crypto)，十分感谢原作者！
