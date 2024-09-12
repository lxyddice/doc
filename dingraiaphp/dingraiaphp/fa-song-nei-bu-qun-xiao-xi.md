# 发送内部群消息

不要问为什么要传webhook（）  这是回报url，当触发错误时通过webhook发普通消息，可以填开发群的webhook或者干脆null，不过建议$webhook

#### 内部群消息-文本

```php
if ($globalmessage == "/spt") {
    sampleText('ok',$webhook,5, [$staffid]); //发送ok，5秒后撤回，私聊发给$staffid
}
if ($globalmessage == "/spt2") {
    sampleText('okk',$webhook); //发送ok，不撤回，发送到群
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

sampleImageMsg("https://pic.lxyddice.top/i/2023/07/31/owybi7.png", $webhook, 0, [$staffid]);
//发送图片 不撤回 私聊发给$staffid
```

```
sampleImageMsg(图片url,webhook,撤回时间,私聊参数);
```

#### 内部群消息-音频

<mark style="color:red;">音频文件一定要是.ogg且小于2MB</mark>

```php
if ($globalmessage == "/丁真 烟") {
    sampleAudio("asset/烟Distance.ogg",$webhook,-1);
}

sampleAudio("asset/igs.ogg",$webhook,-1, 0, [$staffid]);
//发送igs.ogg -1：自动生成时间 0：不撤回 [$staffid]：私聊发
```

```
sampleAudio(文件地址（相对于index.php）,webhook,音频时长（使用getuid3自动获取传-1，否则传毫秒）,撤回时间,私聊参数)
```

#### 内部群消息-文件

<mark style="color:red;">文件一定要是合法后缀名且小于20MB，详情看</mark>[<mark style="color:red;">https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066</mark>](https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066)

```php
if ($globalmessage == "/file PCL2") {
    sampleFile("asset/PCL2.zip",$webhook,"PCL2.zip","zip");
}

sampleFile("asset/PCL2.zip",$webhook,"PCL2.zip","zip", 0, [$staffid]);
//发送PCL2给群u 文件名 文件扩展 不撤回 私聊参数
```

内部群消息-视频

<mark style="color:red;">文件一定要是.mp4且小于20MB，详情看</mark>[<mark style="color:red;">https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066</mark>](https://open.dingtalk.com/document/orgapp/upload-media-files#h2-xg2-l8o-066)

```php
if ($conversationType == 2) { //是群消息
    $r = sampleVideo("data/download/wyy/{$id}.mp4", $webhook, "data/download/wyy/{$id}.jpg");
    //视频位置 封面位置
} else {
    $r = sampleVideo("data/download/wyy/{$id}.mp4", $webhook, "data/download/wyy/{$id}.jpg", "mp4", 0, [$staffid]);
    //视频位置 封面位置 强制mp4 撤回时间 私聊参数
}
```
