# 附录1：钉钉开放平台创建机器人

## 创建小程序与机器人

登录[钉钉开放平台](https://open-dev.dingtalk.com/)小程序对应的组织

点击应用开发-创建应用

类型为小程序

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

在基础信息-应用信息记下AppKey和AppSecret，后面要用

<figure><img src="../../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

点击应用功能-机器人与消息推送，点击复制RobotCode，记住它！

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

模式选择HTTP，接收地址为框架index.php，如https://api.lxyddice.top/框架/index.php

<figure><img src="../../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

点击发布，即可在群内添加机器人

<figure><img src="../../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

## 酷应用

部分功能依赖酷应用，你可以这么创建：

![](<../../../.gitbook/assets/image (91).png>)![](<../../../.gitbook/assets/image (90).png>)随便选个类型，推荐<mark style="color:blue;">**扩展到群会话**</mark>

<figure><img src="../../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

点击 预览发布-发布到自建酷应用列表\
![](<../../../.gitbook/assets/image (94).png>)点击编辑即可看到**酷应用编码**

## 事件与回调

部分功能依赖事件与回调

<figure><img src="../../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

请求网址填上面那个，记住加密aes\_key和签名token\


## 权限

部分功能非常依赖权限

<figure><img src="../../../.gitbook/assets/image (97).png" alt=""><figcaption><p>其他权限乱七八糟全开了就得了，个人权限、通讯录、场景群、机器人、群管理、卡片、储存是必开</p></figcaption></figure>

## 登录与分享

未完成

## 部署与发布

<mark style="color:red;">这有个大坑！如果不发布会导致只有你自己才能看到机器人，其他人无法添加！</mark>

{% embed url="https://open.dingtalk.com/document/isvapp/publish-and-install-isvapp" %}

自己看罢（
