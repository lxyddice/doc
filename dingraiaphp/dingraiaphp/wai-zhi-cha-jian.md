---
description: v240112.1-Alpha新增
---

# 外置插件

## 这是什么？

外置插件，顾名思义就是可以与钉钉无关的消息交互，接收来自其他地方的请求并使用DingraiaPHP进行处理

## 使用

这是我写的一个与MaaArknights交互的外置插件，文档为[https://maa.plus/docs/%E5%8D%8F%E8%AE%AE%E6%96%87%E6%A1%A3/%E8%BF%9C%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE.html](https://maa.plus/docs/%E5%8D%8F%E8%AE%AE%E6%96%87%E6%A1%A3/%E8%BF%9C%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE.html)

打开 module/DingraiaPHP/main.php

在 DingraiaPHPLoadMoudlePluginMain 函数内新增以下代码：

```php
if (isset($_GET["MAAArknightsGetTask"]) || isset($_GET['MAAArknightsReportStatus'])) {
    require_once("module/DingraiaPHP/plugin/MAAArknights.php");//引入外置插件主文件
    $c = DingraiaPHPMaaArknightsMain($body, $conf);//外置插件主文件函数
    if ($c) {
        $c["chat_mode"] = "MAAArknights";//设置chat_mode
        $r[] = $c;
        return $r;//返回给框架
    }
    return false;
}
```

打开 module/DingraiaPHP/plugin/MAAArknights.php

```php
<?php

function DingraiaPHPMaaArknightsMain($body, $conf) {
    global $bot_run_as;
    
    header('Content-Type:application/json; charset=utf-8');
    $MAAAllowUserList = ["xxx"];//改成MAA里的用户标识符
    $MAAAllowDeviceList = ["xxx"];//MAA里的设备标识符
    
    $bot_run_as['echoLoadPlugins'] = false;
    if (isset($_GET['MAAArknightsGetTask'])) {
        if ($_SERVER['REQUEST_METHOD'] === "POST") {
            if (in_array($body['user'], $MAAAllowUserList) && in_array($body['device'], $MAAAllowDeviceList)) {
                $bot_run_as['chat_mode'] = "MAAArknights";
                return $body;
            } else {
                DingraiaPHPResponseExit(403, "User or device not allowed", null, true, true);
            }
        } else {
            DingraiaPHPResponseExit(405, "Method Not Allowed.Need post request", null, true, true);
        }
    }
    
    if (isset($_GET['MAAArknightsReportStatus'])) {
        if ($_SERVER['REQUEST_METHOD'] === "POST") {
            if (in_array($body['user'], $MAAAllowUserList) && in_array($body['device'], $MAAAllowDeviceList)) {
                $bot_run_as['chat_mode'] = "MAAArknightsReport";
                return $body;
            } else {
                DingraiaPHPResponseExit(403, "User or device not allowed", null, true, true);
            }
        } else {
            DingraiaPHPResponseExit(405, "Method Not Allowed.Need post request", null, true, true);
        }
    }
    return false;
}
```

这就是该外置插件的主体

打开MAA，填入地址

获取为 https://xxx.com/?MAAArknightsGetTask

汇报为 https://xxx.com/?MAAArknightsReportStatus

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

这时，可以在 plugin 文件夹（消息插件）继续开发了

比如 plugin/Maa.php

<pre class="language-php"><code class="lang-php">if (strpos($globalmessage, "/maa ") === 0) {//如果用户指令以/maa 开头
    if (permission_check("bot.owner", $guserarr["uid"])) {
    //如果用户具有 "bot.owner" 权限，接下来的代码块将被执行
        $task = stringf($globalmessage)['params'][1];
        $ff = read_file_to_array("data/DingraiaPHP/MAAArknights/editTask.json");
        $user = "用户标识符";
        $ff[$user][] = [$task, $webhook];
        write_to_file_json("data/DingraiaPHP/MAAArknights/editTask.json",$ff);
        //获取从指令中提取的任务参数，并将任务相关的信息存储到一个JSON文件中
    } else {
        send_message('Access denied', $webhook, $staffid);//如果用户没有足够的权限，发送消息通知权限不足。
    }
}
<strong>if ($bot_run_as['chat_mode'] == "MAAArknights") {
</strong><strong>//检查机器人当前运行的聊天模式是否为 "MAAArknights"
</strong>    $ff = read_file_to_array("data/DingraiaPHP/MAAArknights/editTask.json");
    //读取之前存储的任务信息，然后生成一个包含任务ID和类型的数组，并将该数组写入日志文件。
    if (count($ff[$DingraiaPHPGet['user']]) > 0) {
        foreach ($ff[$DingraiaPHPGet['user']] as $item) {
            $type = $item[0];
            $webhook = $item[1];
            $id = uuid();
            $newArray[] = ["id" => $id, "type" => $type];
            write_to_file_json("data/DingraiaPHP/MAAArknights/log/{$id}.json",["id" => $id, "type" => $type, "webbook" => $webhook, "time" => time()]);
        }
        echo(json_encode(["tasks"=>$newArray]));
        $ff[$DingraiaPHPGet['user']] = [];
        write_to_file_json("data/DingraiaPHP/MAAArknights/editTask.json",$ff);
    } else {
        echo(json_encode(["tasks"=>[]]));
    }
    //将任务数组以JSON格式返回给MAA，并清空任务列表
}
if ($bot_run_as['chat_mode'] == "MAAArknightsReport") {
    //检查机器人当前运行的聊天模式是否为 "MAAArknightsReport"
    $maaLogFile = "data/DingraiaPHP/MAAArknights/log/{$DingraiaPHPGet['task']}.json";
    if (file_exists($maaLogFile)) {
        $logFileData = read_file_to_array($maaLogFile);
        $webhook = $logFileData['webbook'];
        if ($logFileData['type'] == 'CaptureImageNow' || $logFileData['type'] == "CaptureImage") {
            $logFileData['reportStatus'] = $DingraiaPHPGet;
            $img =  $DingraiaPHPGet['payload'];
            file_put_contents("data/DingraiaPHP/MAAArknights/log/images/{$DingraiaPHPGet['task']}.jpeg", base64_decode($img));
            $res = "https://xxx.com/data/DingraiaPHP/MAAArknights/log/images/{$DingraiaPHPGet['task']}.jpeg";
            send_markdown("![0]($res)", $webhook);
        } else {
            send_message($DingraiaPHPGet['task']."-".$DingraiaPHPGet['status'], $webhook);
        }
    }
    //根据任务类型执行不同的操作，包括保存图像或发送消息。
}
</code></pre>

\<small>GPT写的，改了点（懒）\</small>

### 效果

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>炫耀（bushi</p></figcaption></figure>

