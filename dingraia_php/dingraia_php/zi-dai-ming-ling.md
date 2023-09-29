# 自带命令

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

运行机器人  需要权限bot.start

```php
/bot_close
```

停止机器人  需要权限bot.close

```php
/groupid
```

获取群id

```php
/plugin enable <插件名>
```

启用插件  需要权限bot.plugin

```php
/plugin disable <插件名>
```

禁用插件  需要权限bot.plugin

```php
/plugin list
```

插件列表  需要权限bot.plugin

/lp check 检查权限   需要权限lp.check

/lp permission \<uid> <权限节点> \<add/remove> 添加或删除权限  需要权限lp.change

/lp group permission <组名> <权限节点> \<add/remove> 添加或删除用户组权限  需要权限lp.change

/lp group user <组名> \<add/remove> 添加或删除用户于用户组  需要权限lp.change

### 接下来，你需要配置一些机器人小程序信息

注：默认第一位使用机器人的用户uid为10001，具有最高权限<mark style="color:red;">**\*.\***</mark>

[pei-zhi-ji-qi-ren.md](pei-zhi-ji-qi-ren.md "mention")
