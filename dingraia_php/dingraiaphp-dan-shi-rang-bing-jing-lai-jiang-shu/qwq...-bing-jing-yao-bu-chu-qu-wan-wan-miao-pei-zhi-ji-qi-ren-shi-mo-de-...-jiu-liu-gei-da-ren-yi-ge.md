---
description: （为什么安装完了还不给我用！差评！）
---

# QWQ...冰晶要不出去玩玩喵？配置机器人什么的...就留给大人一个人做好啦！

## config/cropid.json

机器人收到第一条消息后，会返回<mark style="color:yellow;">chatbotCorpId</mark>参数，这个是组织id，我们需要如下填写（多组织可以往下写新键）：

```json
{
	"chatbotCorpId": {
		"AppKey": "你的小程序appkey",
		"AppSecret": "你的小程序appsecret",
		"coolAppCode": "酷应用id（见下方）",
		"encodingAesKey": "事件回调才需要：加密aes_key",
		"token": "事件回调才需要：签名token",
		"main_group": "核心群id，可以是机器人调试群"
	}
}
```

`酷应用id：`

![](<../../.gitbook/assets/image (64).png>)

## config/group.json

群白名单以及群权限配置

<mark style="color:yellow;">不用的话不用管</mark>，这是个示例

white\_list为白名单模式下可用群组id列表

permission为每个群拥有权限

```json
{
	"white_list": ["114514", "1919810", "233", "原神启动"],
	"permission": {
		"114514": ["涩图", "草", "skin_ark"],
		"1919810": ["涩图", "草", "ai"],
		"233": ["涩图", "草", "ai", "skin_ark"],
		"原神启动": []
	}
}
```
