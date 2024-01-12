# 事件订阅

## 脚本位置

* secret.php

## 注意事项

1. 需要composer扩展Hlf\DingTalkCrypto\Crypto
2. 一次只能接收一个组织的消息回调
3. 可能存在bug，比如凌晨四点...突然...出现...『鬼啊！』几条钉钉发来的答辩数据！听说是失败后会重发，但是为什么重发了一个月了都不取消...
4. 事件类型在官方文档没给，自己一个一个触发实验

## 使用

1. 定义变量$token、$encodingAesKey、$suiteKey
2. token为![](<../../../.gitbook/assets/image (2).png>)，encodingAesKey为![](<../../../.gitbook/assets/image (1) (1).png>)，suiteKey为AppKey，请求网址为你的Dingraia\_php地址/secret.php
3. 测试![](<../../../.gitbook/assets/image (2) (1).png>)
4. 如果成功了就拉个人进内部群，应该能在log4.json看到相关数据，下面是进群欢迎的代码

```php
if ($edata['EventType'] == 'chat_add_member') {//类型为chat_add_member
        $chatbotCorpId = $edata['CorpId'];//得到corpid
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $robotCode = $cropidkey['AppKey'];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);//钉钉api万物之源
        $urname = userinfo($edata['UserId'][0],$token)['result']['name'];//获取用户名字
        sampleText("欢迎{$urname}加入本群~",null);//内部群消息
    }
```
