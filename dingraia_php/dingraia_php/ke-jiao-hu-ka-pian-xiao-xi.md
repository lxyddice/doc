---
description: 这玩意多少有点抽象，并且已实现功能很少，但是能用。下面一步步教如何使用qwq
---

# 可交互卡片消息

## 发送卡片

### 登录你的开发者平台

### 在顶栏选择开放能力-卡片平台-新建模板

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

填写相关信息后进入搭建平台

钉钉官方的卡片开发文档[https://open.dingtalk.com/document/orgapp/overview-card-template](https://open.dingtalk.com/document/orgapp/overview-card-template)

### 下面以我的机器人的菜单为示例（菜勿喷，谢谢你QAQ）：

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

开头是markdown内容，如下

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

第二行为点击按钮后文字会改变，mode为false表示当后面传入的变量为false时，才会显示

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="277"><figcaption></figcaption></figure>

下面说四个竖排按钮，每个按钮不同的功能

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### 回传请求

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

也就是当群u点击卡片的“用户帮助”时，钉钉服务器会向我们回调路由（后面有讲）post一段回传请求，包含了一段json为"content":{"bot\_help\_v1":"userhelp"}，我们就能知道群u点了什么东西

#### 跳转链接

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

我们想要群u点击按钮后隐藏按钮，则相对于上面的第二行为点击按钮后文字会改变

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

也就是群u点击后我们的服务器通过api请求变量mode变为false，则实现隐藏

#### 相关变量

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

做完了点右上发布，返回卡片列表就能看到卡片id喵

#### 其他没什么好讲的了...下面是框架实现

直到v231018.1-alpha，只实现了群内发卡片接收回调并改变公有内容，私有数据没有写完

```php
if ($globalmessage == "/help_new") {
    $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
    $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
    $cid = uuid()."-".uuid();
        $res = send_interactiveCards($token,'11175fc5-4e31-4f73-9173-ce66eea596a7.schema',$conversationId,$robotCode,1,["cardParamMap"=>["mode"=>"true","1"=>"114514","不管如何塞点东西，不然报错"=>"不然狠狠厚儒你"]],"",$cid,"xxxx");
}
```

```php
$cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
$token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
//token是钉钉api万物之源，别忘了哟~
$cid = uuid()."-".uuid(); // 超级（寄）长的uuid，保证你的卡片114514年都不会重复uuid，当然，你可以用自己的
send_interactiveCards($token,卡片id,群id,robotcode,变量替换（下面讲）,（没写完，传空值）,$cid（可选，不写就写null框架自动配置, 回调路由（可选，下面讲）); //这段才是关
```

#### 变量替换

```php
["cardParamMap"=>["mode"=>"true","1"=>"114514","不管如何塞点东西，不然报错"=>"不然狠狠厚儒你"]]
```

cardParamMap是指文本类型变量替换，多媒体类型自己看钉钉文档

变量mode为true，对应上面显示菜单按钮

1为114514代表把第二段md内容改为114514，但是由于mode不为false所以不会显示

#### 回调路由

注册回调路由：请自行完成

{% embed url="https://open.dingtalk.com/document/orgapp/registration-card-interaction-callback-address-1" %}

## 更新卡片

我们接收到了回调来的数据，可以用更新卡片来对群u作出回应。如图所示：

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

点击“用户帮助”

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

为了区分普通消息与卡片回调，框架内代码做了切割。

正常来说，前面的  if ($globalmessage == "/114514")  就是普通消息，区分方法为

```php
if ($bot_run_as['chat_mode'] == 'card_callback') {
    $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
    $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
    $content = $bot_run_as['content']['cardPrivateData']['params']; //提取用户选择，防止变量冲突的话可以改改
    //下面是处理方法
    if ($content['同意'] == 1) {
        $res = update_interactiveCards($token,$bot_run_as['outTrackId'], ["cardParamMap"=>["title"=>"# 原神，启动！","text"=>"你说的对，但是《原神》是由米哈游自主研发的一款全新开放世界冒险游戏。游戏发生在一个被称作「提瓦特」的幻想世界，在这里，被神选中的人将被授予「神之眼」，导引元素之力。你将扮演一位名为「旅行者」的神秘角色，在自由的旅行中邂逅性格各异、能力独特的同伴们，和他们一起击败强敌，找回失散的亲人——同时，逐步发掘「原神」的真相。"]]);
    } else {
        $res = update_interactiveCards($token,$bot_run_as['outTrackId'], ["cardParamMap"=>["title"=>"# 原神怎么你了","text"=>"差不多得了😅屁大点事都要拐上原神，原神一没招你惹你，二没干伤天害理的事情，到底怎么你了让你一直无脑抹黑，米哈游每天费尽心思的文化输出弘扬中国文化，你这种喷子只会在网上敲键盘诋毁良心公司，中国游戏的未来就是被你这种人毁掉的😅"]]);
    }
}
```

```php
update_interactiveCards($token,卡片唯一id，即上文$cid, 卡片更新后变量值);
```

卡片唯一id可以用  $bot\_run\_as\['outTrackId']  得到，除非你想更新别的卡片

cardParamMap与上面发送的语法相同

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption><p>点击“启动原神”，服务器收到了$content['同意'] == 1</p></figcaption></figure>

```php
if ($content['bot_help_v1'] == 'userhelp') {
    update_interactiveCards($token,$bot_run_as['outTrackId'], ["cardParamMap"=>["1"=>"/me  个人信息\n/uid <uid>  根据uid查询信息\n/found <word>  根据关键词查询名字\n/api  获取apikey（私聊使用）"]]);
} 
```

👆原理相同

## 卡片回调模式下取得参数

```php
$bot_run_as['userId'] = $c['userId'];$bot_run_as['userId'] = $c[',$bot_run_as['userId'] = $c['userId'];'];
```
