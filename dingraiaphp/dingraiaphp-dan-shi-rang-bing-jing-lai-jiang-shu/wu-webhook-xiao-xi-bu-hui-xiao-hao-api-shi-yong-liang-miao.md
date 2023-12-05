# 唔？webhook消息？不会消耗API使用量喵！

### 插件目录是plugin喵，把大大的插件丢进去吧\~诶？

#### 冰晶小提示：本页带下划线为非必填

#### 发送普通消息

```php
if ($globalmessage == "我想涩涩"){
    send_message('拒绝涩涩是每个人的责任喵！', $webhook, $staffid);
}
```

<pre><code>send_message(消息内容, 回调地址, <a data-footnote-ref href="#user-content-fn-1">at人userid</a>);
</code></pre>

#### 发送markdown

```php
if ($globalmessage == "我好想涩涩啊") {
    send_markdown("# 不许涩涩！\n## 不然就剪掉大人的罪恶之源喵~", $webhook, "不可以涩涩！");
}
```

<pre><code>send_markdown(消息内容, 回调地址, 标题, <a data-footnote-ref href="#user-content-fn-2">at人userid</a>);
</code></pre>

#### 发送link消息

```php
if ($globalmessage == "但是我就想涩涩嘛") {
    send_link("唔？不可以不可以绝对不可以喵！", $webhook, 0, "可恶！居然无视冰晶的话喵...", "https://img.onlinedown.net/download/202008/161910-5f48bdfe3930b.jpg", "https://ys.mihoyo.com");
}
```

```
send_link(消息内容, 回调地址, 0（强制填0）, 标题, 显示图片url, 点击后跳转链接);
```

#### 发送actioncard（多行按钮）

```php
/if ($globalmessage == "冰晶来艾草") {
    $Atitle = ["QWQ","但是冰晶只属于主人喵...","不可以这样喵..."];
    $actionURL = ["https://ys.mihoyo.com", "https://sr.mihoyo.com", "https://mihoyo.com"];
    send_actionCardB("![1](https://th.bing.com/th/id/OIP.YLp3-S0sVSOMX-o6PLE9bgHaEK?pid=ImgDet&rs=1) \n\n #### 这是一条测试 \n\n 这是一条测试", $webhook, 0, "测试", true);
}
```

必须定义Atitle和actionURL

![](<../../.gitbook/assets/image (66).png>)（图文无关，只做示例）

方法就这样，不想写了（

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

#### 发送actioncard（单按钮）

```php
if ($globalmessage == "其实冰晶也有点想要吧？") {
    send_actionCardA('唔...但是...冰晶想...
    
    不行不行！理智告诉冰晶，这样是不对的喵！', $webhook, 0, "喵呜...", "查看更多", "https://ak.hypergryph.com/");
}
```

![](<../../.gitbook/assets/image (68).png>)（图文无关，只做示例）

方法就这样，不想写了（

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

#### 发送feedcard

```php
if ($globalmessage == "那我要来抓冰晶了~") {
    $Atitle = ["诶诶？","冰晶...跑！","QWQ不要喵！冰晶先跑掉了喵~"];
    $actionURL = ["https://ys.mihoyo.com", "https://sr.mihoyo.com", "https://mihoyo.com"];
    $ApicUrl = ["https://upload-bbs.miyoushe.com/upload/2021/02/23/190961740/f1cf2102f0b80d656683049eec90d421_500237588484630240.jpg", "https://ts1.cn.mm.bing.net/th/id/R-C.3f6257b108fabbdf8f9b33ab85852f1f?rik=TcwJqLka5niUPw&riu=http%3a%2f%2fimg001.dailiantong.com%2fNews%2f20210104%2fzty_20210104103418258.jpg&ehk=o%2f2ZnLPB1KG9XkmRlP1MzYGqxDwQ8h%2fSp4YMtaxCTos%3d&risl=&pid=ImgRaw&r=0", "https://ts1.cn.mm.bing.net/th/id/R-C.c95c6e93fc16d5cbe66f496b1773492d?rik=MiRhheUiuSXgXA&riu=http%3a%2f%2fi0.hdslb.com%2fbfs%2farticle%2f51ca10b8df006709b5c91ad0c712050540cdf8e8.jpg&ehk=MABlilSyOIe23cyung1gApN6pa6QgG5RcmVz33Rx7rs%3d&risl=&pid=ImgRaw&r=0"];
    send_feedcard("![1](https://th.bing.com/th/id/OIP.YLp3-S0sVSOMX-o6PLE9bgHaEK?pid=ImgDet&rs=1) \n\n #### 这是一条测试 \n\n 这是一条测试", $webhook, 0, "测试", true);
}
```

必须定义Atitle和actionURL和ApicUrl

![](<../../.gitbook/assets/image (70).png>)（图文无关，只做示例）

最后的true是指当客户端为pc版时是否在钉钉内直接打开（默认不是，可以不传该参数）

（冰晶坚定地向冰枝魔法研究区跑去）



[^1]: 非必填

[^2]: 非必填
