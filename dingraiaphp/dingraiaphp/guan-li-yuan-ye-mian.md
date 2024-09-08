# 管理员页面

在版本v240426.1-Alpha后，管理员页面开始测试

## 简介

如果在 config/bot.json 中 indexDefault 为 1 的话，直接访问框架首页，您应该能看见此页面

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

点击框中人的图标即可进入管理员页面

若没有开启此选项，您也可以手动添加查询参数 action=admin 进入

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

用户名和密码在 在 config/bot.json 中的 htmlAdmin 内

```json
"htmlAdmin":{
    "appId":"xxx", //钉钉登录时使用的appId
    "username":"lxyddice", //用户名
    "password":"5f4w6f4w8415", //密码
    "code":"fewf748we", //该参数无用
    "dingtalkOauth2Allow":[
        "xy8881145141919810" //允许使用钉钉登录的用户id
    ]
}
```

如果没有设置2FA的话登录无需填写验证码

## 钉钉登录

无需输入用户名密码，直接点按钮即可

## 2FA

由于还没写完前端页面，2FA需要手动开启。请确保您已在安装API插件 module/DingraiaPHP/app/api/htmlAdmin.php

您可以在此处获取2FA相关的API

{% embed url="https://api.lxyddice.top/dingbot/php/?action=p&page=apiDoc#/htmlAdmin%E5%86%85%E7%BD%AE%E6%8F%92%E4%BB%B6API%2F%E9%9C%80%E7%99%BB%E5%BD%95Session%EF%BC%88%E5%8F%AA%E9%80%82%E5%90%88html%E5%86%85%E8%AF%B7%E6%B1%82%EF%BC%89/get__type_htmlAdmin_check2FA_Account" %}
