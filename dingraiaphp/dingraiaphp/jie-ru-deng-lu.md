# 接入登录

## 看前须知

这是个挺有用的玩意

## 特性

支持多企业/App下程序的回调，只需要你的配置文件没问题，则框架会匹配正确的appKey

## 配置登录

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption><p>开发配置-分享设置-右边有个接入登录</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

写 框架url/index.php

点添加

## 使用

在你要的地方重定向到登录url，比如

```url
https://xxx.com/index.php?client_id=小程序的ClientID也就使appKey
&state=防止重放攻击的随机值，后续需要验证
&redirect_uri=登录后重定向到的url，需urlencode
```

如果配置得当，在记录后将会跳转到钉钉ouath2登录界面

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

用户登录完成后会跳转到

```url
https://xxx.com/index.php?authCode=钉钉返回的token&state=唯一uuid
```

在此处，框架会检查该uuid是否使用过，使用过的话会报出错误，否则会向钉钉服务器请求获取用户信息。随后跳转到

```url
你设定的回调url?DingraiaPHPState=唯一uuid&state=原始state
```

随后，你需要请求框架的API获取token和用户信息。地址为

```
https://xxx.com/api/get.php?type=oauth2Get
&DingraiaPHPState=上面的唯一uuid
&state=原始state
&timeStamp=整秒时间戳
&sign=签名
```

其中，sign是 hash('sha256', $DingraiaPHPState.$state.$ts.$conf\['dingraiaAuthKey']);

$conf\['dingraiaAuthKey'] 为config/bot.json的 dingraiaAuthKey 参数

时间戳验证超时为 dingraiaAuthTimeout
