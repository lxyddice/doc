# 发送内部群消息

#### 内部群消息-文本

```php
if ($globalmessage == "/spt") {
    sampleText('ok',$webhook,5, [$staffid]);
}
```

```php
if ($globalmessage == "/spt2") {
    sampleText('okk',$webhook);
}
```

```
sampleText(消息内容,webhook,撤回时间（不撤回不传或传0）, 发私聊消息时userid（数组，不传则群聊消息）);
```

#### 内部群消息-图片

```php
if ($globalmessage == "/grt1") {
    sampleImageMsg("https://pic.lxyddice.top/i/2023/07/31/owybi7.png",$webhook);
}
```

```
sampleImageMsg(图片url,webhook);
```

#### 内部群消息-音频

<mark style="color:red;">音频文件一定要是.ogg且小于2MB</mark>

```php
if ($globalmessage == "/丁真 烟") {
    sampleAudio("asset/烟Distance.ogg",$webhook,-1);
}
```

```
sampleAudio(文件地址（相对于index.php）,webhook,音频时长（使用getuid3自动获取传-1，否则传毫秒）)
```

#### 内部群消息-文件

<mark style="color:red;">文件一定要是合法后缀名且小于20MB，详情看</mark>[<mark style="color:red;">https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066</mark>](https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066)

```php
if ($globalmessage == "/file PCL2") {
    sampleFile("asset/PCL2.zip",$webhook,"PCL2.zip","zip");
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

