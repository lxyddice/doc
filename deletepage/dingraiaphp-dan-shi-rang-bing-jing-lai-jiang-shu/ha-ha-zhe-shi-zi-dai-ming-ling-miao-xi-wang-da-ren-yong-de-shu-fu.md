# 哈哈\~这是自带命令喵，希望大人用得舒服\~

### 为了大人安装好后可以方便使用，冰晶特别准备了以下指令，喵？

冰晶小提示：默认第一位使用机器人的用户uid为10001，具有最高权限<mark style="color:red;">**\*.\***</mark>

### plugin/demo.php

```php
<?php
if ($globalmessage == "/hi"){
    send_message('hi!', $webhook, $staffid);
}

if ($globalmessage == "/t1"){
    $result = stringf($globalmessage);
    send_message($result, $webhook);
}

if (strpos($globalmessage, "/test1") === 0) {
    $result = stringf($globalmessage);
    send_message($result, $webhook, $staffid);
}

if (strpos($globalmessage, "/test2") === 0) {
    $res = requests("POST", "https://api.lxyddice.top/api/gk", $data);
    send_message($res['body'], $webhook);
}
```

### index.php

```php
/bot_start
```

是时候开始工作啦\~ 需要权限bot.start

```php
/bot_close
```

机器人也要睡觉啦\~  需要权限bot.close

```php
/groupid
```

获取群id喵

```php
/plugin enable <插件名>
```

插件，启动！  需要权限bot.plugin

```php
/plugin disable <插件名>
```

诶？插件在修嘛...那就先不开好啦  需要权限bot.plugin

```php
/plugin list
```

看看大人用了哪些插件喵...  需要权限bot.plugin

/lp check 检查权限   需要权限lp.check

/lp permission \<uid> <权限节点> \<add/remove> 添加或删除权限  需要权限lp.change

/lp group permission <组名> <权限节点> \<add/remove> 添加或删除用户组权限  需要权限lp.change

/lp group user <组名> \<add/remove> 添加或删除用户于用户组  需要权限lp.change

### 接下来，你需要配置一些机器人小程序信息



[pei-zhi-ji-qi-ren.md](../../dingraiaphp/dingraiaphp/pei-zhi-ji-qi-ren.md "mention")
