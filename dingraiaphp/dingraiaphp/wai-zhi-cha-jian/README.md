---
description: v240112.1-Alpha新增
---

# 外置插件

## 这是什么？

外置插件，顾名思义就是可以与钉钉无关的消息交互，接收来自其他地方的请求并使用DingraiaPHP进行处理

## 使用

### v240904.Alpha-修改外置插件导入方式

以下我写的一个Github Webhook 插件，接收Github的Webhook并使用钉钉机器人发到群里

module/DingraiaPHP/plugin/githubWebhook.php

```php
<?php
function DingraiaPHPGithubWebhookMain($b, $c) {
    global $bot_run_as;
    global $hideLoadPluginInfo_B;
    
    $bot_run_as["config"]["hideAllEcho"] = 1;
    
    $githubSecret = read_file_to_array("data/com.lxyddice.githubWebhook/config.json")["secret"];
    $bot_run_as["chat_mode"] = "gbwh";
    $bot_run_as["callbackContent"] = $b;
    $bot_run_as["callbackContent"]["header"] = getallheaders();

    DingraiaPHPAddEndModulePlugin("module/DingraiaPHP/plugin/githubWebhook.php", "DingraiaPHPGithubWebhookEnd");
    
    $signature = $_SERVER['HTTP_X_HUB_SIGNATURE_256'] ?? '';
    $body = file_get_contents('php://input');
    $calculatedSignature = 'sha256=' . hash_hmac('sha256', $body, $githubSecret);
    
    if (!hash_equals($signature, $calculatedSignature)) {
        $bot_run_as["verify"] = false;
        $bot_run_as["response"] = ["code" => 403, "message" => "Forbidden"];
        return $b;
    }
    $bot_run_as["response"] = ["code" => 0, "message" => "OK"];
    $bot_run_as["verify"] = true;
    return $b;
}

function DingraiaPHPGithubWebhookEnd() {
    global $bot_run_as;
    $back = $bot_run_as["response"];
    $back["request_id"] =  $bot_run_as["RUN_ID"];
    write_to_file_json("data/bot/app/response.json", ["type"=>"json", "content"=>$back]);
}
```

plugin/com.lxyddice.githubWebhook.php

```php
<?php
if (isset($bot_run_as)) {
    $comLxyddicegithubWebhookFile = [
        "data/com.lxyddice.githubWebhook/config.json"=>["sendWebhooks"=>[],"secret"=>""],
        "data/com.lxyddice.githubWebhook/data.json"=>[],
        "data/com.lxyddice.githubWebhook/last.json"=>[],
        "data/com.lxyddice.githubWebhook/log.json"=>[],
    ];
    foreach ($comLxyddicegithubWebhookFile as $k => $v) {
        if (!file_exists($k)) {
            mkdir(dirname($k), 0777, true);
            write_to_file_json($k, $v);
        }
    }
    if (!file_exists("data/bot/helps/com.lxyddice.githubWebhook.json")) {
        file_put_contents("data/bot/helps/com.lxyddice.githubWebhook.json", json_encode([
            "start" => "gbwk", 
            "plugin" => "com.lxyddice.githubWebhook",
            "name" => "GitHub Webhook",
            "info" => "处理来自GitHub Webhook的事件",
            "help" => "本插件暂无指令，请在data/com.lxyddice.githubWebhook/config.json中配置Webhook地址和密钥",
            "author" => "lxyddice", 
            "version" => "1.0.0"
        ]));
    }
    if ($bot_run_as["chat_mode"] == "gbwh" && $bot_run_as["verify"]) {
        $gbwhLog = read_file_to_array("data/com.lxyddice.githubWebhook/log.json");
        $gbwhLog[] = $bot_run_as['callbackContent'];
        write_to_file_json("data/com.lxyddice.githubWebhook/log.json", $gbwhLog);

        $gbwhConfig = read_file_to_array("data/com.lxyddice.githubWebhook/config.json");

        # 解析各个事件
        $eventType = $bot_run_as['callbackContent']["header"]["X-Github-Event"];
        switch($eventType) {
            case "issues":
                $action = $bot_run_as['callbackContent']['action'];
                if ($action == "opened") {
                    $issue = $bot_run_as['callbackContent']['issue'];
                    $repository = $bot_run_as['callbackContent']['repository'];
                    $html_url = $issue['html_url'];
                    $title = $issue['title'];
                    $body = $issue['body'];
                    $user = $issue['user'];
                    $user_login = $user['login'];
                    $user_html_url = $user['html_url'];
                    $updated_at = $issue['updated_at'];
                    $repository['name'] = $repository['full_name'];
                    $msg = "[{$repository['name']}]({$repository['html_url']}) New issue: [{$title}]({$html_url}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                } elseif ($action == "closed") {
                    $issue = $bot_run_as['callbackContent']['issue'];
                    $repository = $bot_run_as['callbackContent']['repository'];
                    $html_url = $issue['html_url'];
                    $title = $issue['title'];
                    $body = $issue['body'];
                    $user = $issue['user'];
                    $user_login = $user['login'];
                    $user_html_url = $user['html_url'];
                    $updated_at = $issue['updated_at'];
                    $repository['name'] = $repository['full_name'];
                    $msg = "[{$repository['name']}]({$repository['html_url']}) Issue closed: [{$title}]({$html_url}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                }
                break;
            case "issue_comment":
                $action = $bot_run_as['callbackContent']['action'];
                $issue = $bot_run_as['callbackContent']['issue'];
                $comment = $bot_run_as['callbackContent']['comment'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $html_url = $comment['html_url'];
                $body = $comment['body'];
                $user = $comment['user'];
                $user_login = $user['login'];
                $user_html_url = $user['html_url'];
                $updated_at = $comment['updated_at'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New comment on issue [{$issue['title']}]({$issue['html_url']}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                break;
            case "push":
                $pusher = $bot_run_as['callbackContent']['pusher'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $commits = $bot_run_as['callbackContent']['commits'];
                $ref = $bot_run_as['callbackContent']['ref'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New push by [{$pusher['name']}]({$bot_run_as['callbackContent']['sender']['html_url']})\n\nRef: {$ref}\n\n";
                foreach ($commits as $commit) {
                    $msg .= "[".substr($commit['id'], 0, 7)."...]({$commit['url']}) {$commit['message']} by [{$commit['author']['name']}]({$commit['author']['email']})\n\n";
                }
                break;
            case "pull_request":
                $action = $bot_run_as['callbackContent']['action'];
                $pull_request = $bot_run_as['callbackContent']['pull_request'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $html_url = $pull_request['html_url'];
                $title = $pull_request['title'];
                $body = $pull_request['body'];
                $user = $pull_request['user'];
                $user_login = $user['login'];
                $user_html_url = $user['html_url'];
                $updated_at = $pull_request['updated_at'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New pull request: [{$title}]({$html_url}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                break;
            case "pull_request_review":
                $action = $bot_run_as['callbackContent']['action'];
                $pull_request = $bot_run_as['callbackContent']['pull_request'];
                $review = $bot_run_as['callbackContent']['review'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $html_url = $review['html_url'];
                $body = $review['body'];
                $user = $review['user'];
                $user_login = $user['login'];
                $user_html_url = $user['html_url'];
                $updated_at = $review['updated_at'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New review on pull request [{$pull_request['title']}]({$pull_request['html_url']}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                break;
            case "pull_request_review_comment":
                $action = $bot_run_as['callbackContent']['action'];
                $pull_request = $bot_run_as['callbackContent']['pull_request'];
                $comment = $bot_run_as['callbackContent']['comment'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $html_url = $comment['html_url'];
                $body = $comment['body'];
                $user = $comment['user'];
                $user_login = $user['login'];
                $user_html_url = $user['html_url'];
                $updated_at = $comment['updated_at'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New comment on pull request [{$pull_request['title']}]({$pull_request['html_url']}) by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                break;
            case "watch":
                $action = $bot_run_as['callbackContent']['action'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $sender = $bot_run_as['callbackContent']['sender'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New star by [{$sender['login']}]({$sender['html_url']})";
                break;
            case "fork":
                $action = $bot_run_as['callbackContent']['action'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $forkee = $bot_run_as['callbackContent']['forkee'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New fork by [{$forkee['owner']['login']}]({$forkee['owner']['html_url']})";
                break;
            case "create":
                $action = $bot_run_as['callbackContent']['action'];
                $ref = $bot_run_as['callbackContent']['ref'];
                $ref_type = $bot_run_as['callbackContent']['ref_type'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New {$ref_type} {$ref}";
                break;
            case "delete":
                $action = $bot_run_as['callbackContent']['action'];
                $ref = $bot_run_as['callbackContent']['ref'];
                $ref_type = $bot_run_as['callbackContent']['ref_type'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) Delete {$ref_type} {$ref}";
                break;
            case "commit_comment":
                $action = $bot_run_as['callbackContent']['action'];
                $comment = $bot_run_as['callbackContent']['comment'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $html_url = $comment['html_url'];
                $body = $comment['body'];
                $user = $comment['user'];
                $user_login = $user['login'];
                $user_html_url = $user['html_url'];
                $updated_at = $comment['updated_at'];
                $repository['name'] = $repository['full_name'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) New comment on commit by [{$user_login}]({$user_html_url})\n\nCreated at: {$updated_at}\n\n{$body}";
                break;
            case "in_progress":
                $action = $bot_run_as['callbackContent']['action'];
                $repository = $bot_run_as['callbackContent']['repository'];
                $msg = "[{$repository['name']}]({$repository['html_url']}) {$action} in progress";
                break;
            }
            if ($msg) {
                foreach ($gbwhConfig["sendWebhooks"] as $webhook) {
                    send_markdown($msg, $webhook, "GitHub Webhook");
                }
            } else {
                
            }
        }
    }
```

把代码丢进指定文件夹，随后直接运行一次框架生成配置文件

打开 data/com.lxyddice.githubWebhook/config.json

```json
{"sendWebhooks":["钉钉机器人的webhook"],"secret":"密钥"}
```

打开 config/module/plugins.json

<pre class="language-json"><code class="lang-json"><strong>[
</strong>    {"getParams":["githubWebhook"], "requireFile": ["module/DingraiaPHP/plugin/githubWebhook.php"], "chatMode":"gbwh", "start":"DingraiaPHPGithubWebhookMain"}
<strong>]
</strong></code></pre>

现在直接写配置文件就好了，不需要改 main.php 的源代码手动引入

其中 getParams 是接受查询参数的键[^1]时触发该外置插件进行处理，requireFile 是引入文件，chatMode是修改框架模式，start是触发外置插件后运行的函数

打开Github找到项目设置-Webhooks，新增Webhook地址为 框架 index.php?githubWebhook ，密钥就是上面配置文件的那个

应该就能收到仓库更改通知了

#### 效果

<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

### 旧版

以下我写的一个与MaaArknights交互的外置插件

MAA远程控制文档为：[https://maa.plus/docs/%E5%8D%8F%E8%AE%AE%E6%96%87%E6%A1%A3/%E8%BF%9C%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE.html](https://maa.plus/docs/%E5%8D%8F%E8%AE%AE%E6%96%87%E6%A1%A3/%E8%BF%9C%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE.html)

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

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

#### 效果

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>炫耀（bushi</p></figcaption></figure>



[^1]: 例如 index.php?githubWebhook
