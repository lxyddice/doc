---
description: 这玩意多少有点抽象，并且已实现功能很少，但是能用。下面一步步教如何使用qwq
---

# 可交互卡片消息

## 登录你的开发者平台

### 在顶栏选择开放能力-卡片平台-新建模板

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

填写相关信息后进入搭建平台

钉钉官方的卡片开发文档[https://open.dingtalk.com/document/orgapp/overview-card-template](https://open.dingtalk.com/document/orgapp/overview-card-template)

### 下面以我的机器人的菜单为示例（菜勿喷，谢谢你QAQ）：

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

开头是markdown内容，如下

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

第二行为点击按钮后文字会改变，mode为false表示当后面传入的变量为false时，才会显示

<figure><img src="../../.gitbook/assets/image (3).png" alt="" width="277"><figcaption></figcaption></figure>

下面说四个竖排按钮，每个按钮不同的功能

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### 回传请求

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

也就是当群u点击卡片的“用户帮助”时，钉钉服务器会向我们服务器post一段回传请求，包含了一段json为"content":{"bot\_help\_v1":"userhelp"}，我们就能知道群u点了什么东西

#### 跳转链接

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

我们想要群u点击按钮后隐藏按钮，则相对于上面的第二行为点击按钮后文字会改变

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

也就是群u点击后我们的服务器通过api请求变量mode变为false，则实现隐藏

其他没什么好讲的了...下面是框架实现

