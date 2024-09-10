# 定时任务

## 新版 v240910以后

该功能遇到了暂时可以解决的问题，在不使用composer 危险函数 终端exec 的情况下，使用了伪多线程的方式执行，自动访问重启url延长任务，勉勉强强能用

### 这是什么？

这是一个有趣的功能，可以让脚本持续运行并重复执行某些任务，如定时改群名、定时获取数据等等

### 如何使用？

#### 任务文件

打开 data/bot/cron/tasks.json 这是任务配置文件

```json
[
    {
        "type": "every",
        "time": 86400,
        "run": "cronFn_1"
    },
    {
        "type": "once",
        "time": "20240910163100",
        "run": "cronFn_1"
    },
    {
        "type":"daily",
        "time":"163430",
        "run":"cronFn_1"
    }
]
```

type为类型  time为时间参数  run为要执行的函数

参数

<table><thead><tr><th>type参数</th><th>含义</th><th data-hidden></th></tr></thead><tbody><tr><td>every</td><td>每经过time秒执行</td><td></td></tr><tr><td>once</td><td>在 年月日时分秒 执行（24小时制）</td><td></td></tr><tr><td>daily</td><td>在每天 时分秒 执行（24小时制）</td><td></td></tr></tbody></table>

请注意，run的执行的函数不能传参

打开 bot/config.json 找到 cron 配置组



<table><thead><tr><th>参数名</th><th></th><th data-hidden></th></tr></thead><tbody><tr><td>autoRestart</td><td>自动重启时间，建议设置为php/nginx超时时间 - 3到5秒，如果超时时间小于15秒就别用了</td><td></td></tr><tr><td>runParams</td><td>弃用</td><td></td></tr><tr><td>stopParams</td><td>停止查询参数</td><td></td></tr><tr><td>cronUrl</td><td>框架首页地址</td><td></td></tr><tr><td>token</td><td>cron通过url操作时需要传入的token</td><td></td></tr></tbody></table>

访问 框架首页?cron=restart\&tokrn=上面的token 输出至少有一个自动重启任务就算大成功

进入 data/bot/cron/plugins  在里边写php文件，定时任务启动时会自动加载

## 旧版 v240910以前

该功能遇到了暂时无法解决的问题，在不使用composer 危险函数 终端exec 的情况下，部分定时任务导致卡死时会产生后续任务被丢弃的情况，虽然有点思路了但暂时未投入开发，所以自v240426.1-Alpha后可能无法使用，也不推荐使用

### 这是什么？

这是一个有趣的功能，可以让脚本持续运行并重复执行某些任务，如发布群公告、定时获取数据等等

### 使用

* 打开data/bot/cron/plugin文件夹，这个文件夹专门放置定时任务的插件，定时任务启动时框架会自动加载里面的php文件
* 在index.php或plugin的插件内使用`DingraiaPHPCron();`开启定时任务
* 若要停止，可以使用`DingraiaPHPCronStop();`

#### 创建一个定时任务

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

新增于v231231.3-Alpha

#### 为什么会有这玩意？

开发者注意到bug，当php持续一段时间后会杀进程，因此让其一直持续的办法可能如此了

#### 使用

打开config/bot.json，你会看到如下配置

```json
"cron":{
        "autoRestart":323, //自动重启时间
        "runParams":"?cronRun",
        "stopParams":"?cronStop",
        "restartParams":"?cronRestart",
        "restartUrl":"https://xxx.com/?cronRestart"
    }
```

自动重启时间尽量复杂一些，比如每323秒自动重启，而且要比你设置的php超时时间少

restartUrl是每过自动重启时间就访问一次进行重启

注意，定时自动重启不要用 `DingraiaPHPCronRestart 函数`

### 注意

#### 方法

`DingraiaPHPCron();//运行定时任务`

`DingraiaPHPCronStop();//停止定时任务`

`DingraiaPHPCronRestart();//重启定时任务`

#### 防止重复运行

定时任务只会开一个，后续调用时会终止并输出`Cron is running`

#### 开启时卡死

这是正常的，直接关掉浏览器即可，并不推荐使用指令激活定时任务，推荐设置查询参数来开启/关闭。后续可能在api新增。

## 冰晶吐槽

* 为什么不用cron啊crontab什么的喵？
* 笨冰晶，自带不是更方便吗，反正也不是为了精准任务运行的
* 对诶？为了...大人们更方便才对的喵\~
