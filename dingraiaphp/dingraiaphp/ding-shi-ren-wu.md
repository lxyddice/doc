# 定时任务

## 这是什么？

这是一个有趣的功能，可以让脚本持续运行并重复执行某些任务，如发布群公告、定时获取数据等等

## 使用

* 打开data/bot/cron/plugin文件夹，这个文件夹专门放置定时任务的插件，定时任务启动时框架会自动加载里面的php文件
* 在index.php或plugin的插件内使用`DingraiaPHPCron();`开启定时任务
* 若要停止，可以使用`DingraiaPHPCronStop();`

### 创建一个定时任务

打开示例 data/bot/cron/plugin/demo.php ，创建一个函数，如发送消息。建议添加前缀防止重复

```php
<?php
function cronFn_sendA() {
    requests("GET", "https://api.lxyddice.top/v2/");//示例：访问url
}
?>
```

打开 data/bot/cron/register.php 在`DingraiaPHPRunCron`中注册该任务

```php
<?php
function DingraiaPHPRunCron() {
    $currentSeconds = date('s');
    if ($currentSeconds % 300 == 0) {//当秒数可以被300整除时执行，也就五分钟一次
        cronFn_sendA();//你也可以自己设置启动时间
    }
}
?>
```

### 更新任务

任务加载后，会载入内存重复运行，因此要修改任务需要先终止再重开



### 重启任务

该方法为v231229.1-Alpha新增

```php
if (isset($_GET['cronRestart'])) {
    DingraiaPHPCronRestart();
}
```

## 定时重启

#### w？

开发者注意到bug，当php持续一段时间后会杀进程，因此让其一直持续的办法可能如此了



## 注意

### 方法

`DingraiaPHPCron();//运行定时任务`

`DingraiaPHPCronStop();//停止定时任务`

`DingraiaPHPCronRestart();//重启定时任务`

### 防止重复运行

定时任务只会开一个，后续调用时会终止并输出`Cron is running`

### 开启时卡死

这是正常的，直接关掉浏览器即可，并不推荐使用指令激活定时任务，推荐设置查询参数来开启/关闭。后续可能在api新增。

## 冰晶吐槽

* 为什么不用cron喵？
* 笨冰晶，自带不是更方便吗
* 对诶？为了主人...大人们更方便才对的，喵！
