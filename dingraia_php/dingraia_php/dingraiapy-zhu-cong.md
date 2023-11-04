# Dingraia\_py主从

{% embed url="https://open-dev.dingtalk.com/fe/noticeDetail?id=254253" %}

## 为什么要这么做

由于钉钉Webhook\&Stream免费转商业化，导致普通版的at普通消息和时间订阅受到限制，所以搞了个主从模式。原理是作为主人（）的Dingraia\_py为stream模式时，无需解密即可得到事件回调，那么把数据发给Dingraia\_php（从）就能减少重复事件订约、合并机器人为一了。

由于php加解密搞得乱七八糟，官方sdk也跑不动，因此特别推出主从模式（）

## 使用

1. 框架内已经有合法性验证，所以你要在config/bot.json的dingraiaAuthKey写入与主端相同的密钥，否则请求会被丢弃
2. dingraia\_php\_Master-slaveMode为slave说明运行在『从』模式，dingraiaAuthTimeout为主从超时
3. 后面忘了

## 类型

```php
<?php
/*dingraia_master模式*/
/*
$bot_run_as['dingraiaMasterType'] == message 为普通消息
$bot_run_as['dingraiaMasterType'] == callback 为事件回调
*/
if ($bot_run_as['chat_mode'] == "dingraia_master") {
    send_message($bot_run_as['trueDingraiaAuth'].$bot_run_as['dingraiaMasterType'],"原神启动");
}
//$bot_run_as['trueDingraiaAuth']为正确的主从验证
```

## TODO

还没想好

## 冰晶吐槽

为什么不直接用py版本得了喵！

（好像...有点道理？）
