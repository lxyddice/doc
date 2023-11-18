# 发送使用webhook的普通消息

### 插件全部丢在plugin下，会自动加载启用的插件

#### 阅读前提示：带下划线为非必填

#### 发送普通消息

```php
if ($globalmessage == "/hi"){
    send_message('hi!', $webhook, $staffid);
}
```

<pre><code>send_message(消息内容, 回调地址, <a data-footnote-ref href="#user-content-fn-1">at人userid</a>);
</code></pre>

#### 发送markdown

```php
if ($globalmessage == "/test3") {
    send_markdown("# 不许涩涩！", $webhook, "涩图！");
}
```

<pre><code>send_markdown(消息内容, 回调地址, 标题, <a data-footnote-ref href="#user-content-fn-2">at人userid</a>);
</code></pre>

#### 发送link消息

```php
if ($globalmessage == "/lktest") {
    send_link("原神，启动！", $webhook, 0, "原神", "https://img.onlinedown.net/download/202008/161910-5f48bdfe3930b.jpg", "https://ys.mihoyo.com");
}
```

```
send_link(消息内容, 回调地址, 0（强制填0）, 标题, 显示图片url, 点击后跳转链接);
```

#### 发送actioncard（多行按钮）

```php
/if ($globalmessage == "/acatest") {
    $Atitle = ["明日方舟","怎么","你了"];
    $actionURL = ["https://ys.mihoyo.com", "https://sr.mihoyo.com", "https://mihoyo.com"];
    send_actionCardB("![1](https://th.bing.com/th/id/OIP.YLp3-S0sVSOMX-o6PLE9bgHaEK?pid=ImgDet&rs=1) \n\n #### 这是一条测试 \n\n 这是一条测试", $webhook, 0, "测试", true);
}
```

必须定义Atitle和actionURL

![](<../../.gitbook/assets/image (66).png>)

方法就这样，不想写了（

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

#### 发送actioncard（单按钮）

```php
if ($globalmessage == "/acatest2") {
    send_actionCardA('![1](https://i0.hdslb.com/bfs/article/dd6e714815db2cbd855160393c2d3212a9c578bc.png)
    
    ### 快来玩明日方舟
    
    来试试明日方舟吧！', $webhook, 0, "测试标题", "查看更多", "https://ak.hypergryph.com/");
}
```

![](<../../.gitbook/assets/image (68).png>)

方法就这样，不想写了（

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

#### 发送feedcard

```php
if ($globalmessage == "/feedcardtest") {
    $Atitle = ["原神","怎么","你了"];
    $actionURL = ["https://ys.mihoyo.com", "https://sr.mihoyo.com", "https://mihoyo.com"];
    $ApicUrl = ["https://upload-bbs.miyoushe.com/upload/2021/02/23/190961740/f1cf2102f0b80d656683049eec90d421_500237588484630240.jpg", "https://ts1.cn.mm.bing.net/th/id/R-C.3f6257b108fabbdf8f9b33ab85852f1f?rik=TcwJqLka5niUPw&riu=http%3a%2f%2fimg001.dailiantong.com%2fNews%2f20210104%2fzty_20210104103418258.jpg&ehk=o%2f2ZnLPB1KG9XkmRlP1MzYGqxDwQ8h%2fSp4YMtaxCTos%3d&risl=&pid=ImgRaw&r=0", "https://ts1.cn.mm.bing.net/th/id/R-C.c95c6e93fc16d5cbe66f496b1773492d?rik=MiRhheUiuSXgXA&riu=http%3a%2f%2fi0.hdslb.com%2fbfs%2farticle%2f51ca10b8df006709b5c91ad0c712050540cdf8e8.jpg&ehk=MABlilSyOIe23cyung1gApN6pa6QgG5RcmVz33Rx7rs%3d&risl=&pid=ImgRaw&r=0"];
    send_feedcard("![1](https://th.bing.com/th/id/OIP.YLp3-S0sVSOMX-o6PLE9bgHaEK?pid=ImgDet&rs=1) \n\n #### 这是一条测试 \n\n 这是一条测试", $webhook, 0, "测试", true);
}
```

必须定义Atitle和actionURL和ApicUrl

![](<../../.gitbook/assets/image (70).png>)

方法就这样，不想写了（

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

### 接下来是内部群

[^1]: 非必填

[^2]: 非必填
