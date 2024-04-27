---
description: 越写越觉得，这哪是PHP啊，哪有这么抽象（
---

# 用户页面

在版本v240426.1-Alpha后，用户页面开始测试

## 介绍

类似于 flask 等框架的 templeats 页面功能

使用前请确保您已安装用户页面外置插件

## 使用

在 module/DingraiaPHP/app/templeats/ 中是页面文件夹

打开 page.json

```json
{
    "index":"index.php",
    "cloudflareChallenge":"cloudflareChallenge.php",
    "apiDoc":"apiDoc.php",
    "default":"404.php"
}
```

其中，default为page参数不存在时的页面

默认已经存在了一个 index 的页面了

接下来，如果在 config/bot.json 中 indexDefault 为 1 的话，直接访问框架首页，您应该能看见此页面

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

您会跳转到 [/?action=p\&page=index](https://api.lxyddice.top/dingbot/php/?action=p\&page=index)

action=p不管，page参数则代表上面的页面代号

## 开发

index.php

```php
<?php
if (file_exists("fn.php")) {
    require_once("fn.php");
} elseif (file_exists("module/DingraiaPHP/app/admin/fn.php")) {
    require_once("module/DingraiaPHP/app/admin/fn.php");
} else {
    exit("Can't find toolkit,are you install DingraiaPHP?");
}
?>
index.php
```

上面几行是验证请求来源的，无特殊要求必须存在，下面则是主要代码

然后在 page.json 添加相关配置即可
