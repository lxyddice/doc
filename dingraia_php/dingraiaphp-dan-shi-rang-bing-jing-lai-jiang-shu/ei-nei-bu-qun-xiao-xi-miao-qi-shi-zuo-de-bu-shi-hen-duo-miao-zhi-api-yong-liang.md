# 诶？内部群消息喵？其实做的不是很多喵\~（指API用量）

#### 内部群消息-文本

```php
if ($globalmessage == "冰晶？你怎么慌慌张张的？") {
    sampleText('没...没什么啦姐姐...',$webhook,5, [$staffid]);
}
```

```php
if ($globalmessage == "冰晶好像有事瞒着我呢？") {
    sampleText('没有喵！冰晶...冰晶一直很安稳喵？',$webhook);
}
```

```
sampleText(消息内容,webhook,撤回时间（不撤回不传或传0）, 发私聊消息时userid（数组，不传则群聊消息）);
```

#### 内部群消息-图片

```php
if ($globalmessage == "（终于跑回房间了，还好姐姐没多在意...）（拿出涩图和跳蛋）") {
    sampleImageMsg("https://pic.lxyddice.top/i/2023/07/31/owybi7.png",$webhook);
}
```

```
sampleImageMsg(图片url,webhook);
```

#### 内部群消息-音频

<mark style="color:red;">音频文件一定要是.ogg且小于2MB</mark>

```php
if ($globalmessage == "喵？居然有这么多奇奇怪怪的...录音喵？") {
    sampleAudio("asset/冰晶娇喘.ogg",$webhook,-1);
}
```

```
sampleAudio(文件地址（相对于index.php）,webhook,音频时长（使用getuid3自动获取传-1，否则传毫秒）)
```

#### 内部群消息-文件

<mark style="color:red;">文件一定要是合法后缀名且小于20MB，详情看</mark>[<mark style="color:red;">https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066</mark>](https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066)

```php
if ($globalmessage == "哇呜！？为什么写真集在床头啊喵！") {
    sampleFile("asset/冰晶の写真-XA10.zip",$webhook,"PCL2.zip","zip");
}
```

内部群消息-视频

<mark style="color:red;">未完成！</mark>

<mark style="color:red;">文件一定要是.mp4且小于20MB，详情看</mark>[<mark style="color:red;">https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066</mark>](https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066)

```php
if ($globalmessage == "/file PCL2") {
    sampleVideo("asset/114514.mp4",$webhook);
}
```

