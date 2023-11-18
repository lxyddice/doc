# 配置机器人

## config/cropid.json

机器人收到第一条消息后，会返回<mark style="color:yellow;">chatbotCorpId</mark>参数，这个是组织id，我们需要如下填写（多组织可以往下写新键）：

`{"`<mark style="color:yellow;">chatbotCorpId</mark>`":{"AppKey":"你的小程序appkey","AppSecret":"你的小程序appsecret","coolAppCode":"酷应用id（见下方）","encodingAesKey":"事件回调才需要：加密aes_key","token":"事件回调才需要：签名token","main_group":"核心群id，可以是机器人调试群"}}`

`酷应用id：`

![](<../../.gitbook/assets/image (64).png>)

## config/group.json

群白名单以及群权限配置

<mark style="color:yellow;">不用的话不用管</mark>，这是个示例

`{"white_list":["114514","1919810","233","原神启动"],"permission":{"114514":["涩图","草","skin_ark"],"1919810":["涩图","草","ai"],"233":["涩图","草","ai","skin_ark"],"原神启动":[]}}`

### 接下来是一些插件开发以及自带方法

[fa-song-shi-yong-webhook-de-pu-tong-xiao-xi.md](fa-song-shi-yong-webhook-de-pu-tong-xiao-xi.md "mention")
