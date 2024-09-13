# 自定义函数和小工具

#### 修改用户金钱

```php
if (updateMoney($guserarr['uid'], -$exchangeAmount/10)) {
    $rconCommand = "eco give $name $exchangeAmount";
    $response = $rcon->sendCommand($rconCommand);
    $cleanResponse = preg_replace('/§[0-9a-fk-or]/', '', $response);
    send_message($cleanResponse, $webhook, $staffid);
} else {
    send_message('更新失败', $webhook, $staffid);
}
```

```php
updateMoney($uid, $changeAmount);
```

#### 切割字符串

```php
$time = stringf($globalmessag)

stringf("/114 514 1919 810")['len'];//3
stringf("/114 514 1919 810 233")['len'];//4
stringf("/114 514 1919 810 233")['params'][0];///114
stringf("/114 514 1919 810 233")['params'][2];//1919

//你也可以传第二个参进去自定义分割
$mes = stringf($globalmessage,".");
stringf("/114.514 1919.810 233")['params'][2];//514 1919
```

#### 检查用户权限

```php
if ($globalmessage == "/get_accessToken") {
    if (permission_check("get_accessToken", $guserarr["uid"])) {
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
        send_message($token,$webhook,$staffid);
    } else {
        send_message('Access denied', $webhook, $staffid);
    }
}
```

```
permission_check(权限名, uid);
```

如果拥有  bot.\*  权限则拥有bot.下所有权限，该函数兼容权限组，优先级为用户允许>组允许>组拒绝

#### 字节转换

formatBytes(字节大小, 保留位数（不传则两位）);

```php
$usedMemory = formatBytes(114514);
```

#### 钉钉用户信息

```php
$res = userinfo($userid, $token);
```

```php
userinfo(userid, accessToken);
```

获取钉钉用户unionid到userid

```php
$res = getbyunionid(unionid, accessToken);
```

#### 群加人

```php
add_member(accessToken, 群id, userid（数组）);
```

#### 翻译

```php
translate(需要翻译的文本, accessToken, 从什么语言, 到什么语言);
```

#### 群禁言用户

```php
mute_user(禁言时间（毫秒）, 群id, userid, accessToken);
```

#### 群取消禁言用户

```php
unmute_user(群id, userid, accessToken);
```

从unionid查询userid

```php
getbyunionid($unionid, $token)
```



获取accessToken

```php
$cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
$token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
```

```php
get_accessToken(AppKey,AppSecret);
```

token会缓存一段时间避免大量调用浪费api量

#### 修改群u的群名

```php
if (strpos($globalmessage, "/nick ") === 0) {
    if (permission_check("change_user_name",$guserarr['uid'])) {
        $nick = stringf($globalmessage)['params'][1];
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
        $res = change_user_name($conversationId, $atUsers[0]['staffId'], $nick, $token);
        send_message($res, $webhook, $staffid);
    } else {
        send_message('Access denied', $webhook, $staffid);
    }
}
```

```php
change_user_name(群id, userid, 改成什么名字, accessToken);
```

#### op和deop（群给予管理员和取消管理员）

```php
if ($globalmessage == "/op ") {
    if (permission_check("op", $guserarr["uid"])) {
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
        $res = op($conversationId,[$atUsers[0]['staffId']],$token);
        send_message($res, $webhook, $staffid);
    } else {
        send_message('Access denied', $webhook, $staffid);
    }
}

if ($globalmessage == "/deop ") {
    if (permission_check("deop", $guserarr["uid"])) {
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
        $res = deop($conversationId,[$atUsers[0]['staffId']],$token);
        send_message($res, $webhook, $staffid);
    } else {
        send_message('Access denied', $webhook, $staffid);
    }
}
```

```php
op(群id,userid（数组）,accessToken);
deop(群id,userid（数组）,accessToken);
```

#### 创建模板群

```php
create_group(accessToken, 群头像id, 群模板id, 群名, 群主userid, 管理员userid,群成员userid（数组）);
```

注意：群管理员和群成员暂不可用，请用其他方法修改管理员和拉人

#### 群信息

```php
group_info(accessToken, 群id);
```

#### 群拉人（新版用这个）

```php
group_add_member($token,$groupid,$uids);
```

```php
group_add_member(accessToken, 群id, 要拉的人userid（数组）);
```

#### 群成员信息

```php
group_member_get(accessToken, 群id);
```

#### 神权（踢人）

```php
if ($globalmessage == "/kick ") {
    if (permission_check("kick", $guserarr["uid"])) {
        $cropidkey = read_file_to_array("config/cropid.json")[$chatbotCorpId];
        $token = get_accessToken($cropidkey['AppKey'],$cropidkey['AppSecret']);
        $ku = $atUsers[0]['staffId'];
        $res = kick($token,$ku,$conversationId);
        send_message($res, $webhook, $staffid);
    } else {
        send_message('Access denied', $webhook, $staffid);
    }
}
```

```php
kick(accessToken,userid,群id);
```

#### 仿py的requests库

```php
$res = requests("GET","https://api.lxyddice.top/api/gk")['body'];//返回响应体
$res = requests("POST", "https://api.lxyddice.top/api/gk", $data)['code'];//返回响应码
```

```php
requests(请求方法, url, 请求体, 请求头, 超时时间);
```

小提示：默认的data不会转为json，要i的话请添加在header内添加`["Content-Type" => "application/json"]`

#### requests下载

```php
$res = requests_download_file("GET","data/download（文件夹）","https://xxx.com")['saved_file'];//返回下载位置
```

#### 格式化ymd时间

```php
parseCustomTimeFormat('30i15s');//1815
```

y：年  m：月  d：日  h：小时  i：分钟  s：秒

#### 格式化时间到ymd

```php
formatTimeFromSeconds('100');//1i40s
```

y：年  m：月  d：日  h：小时  i：分钟  s：秒

#### 数据库以![](<../../.gitbook/assets/image (15).png>)查询用户数据

<mark style="color:yellow;">警告！该方法为用户注册api，也就是传入userid不在库中会自动注册！所以不要随意调用传数据！</mark>

<mark style="color:yellow;">wuid为Dingraia\_py的uid，不需要动他</mark>

```php
userid2uid(userid);
```

返回数据为

```php
$arr = array(
    'userid' => $userid,
    'uid' => $uid,
    'wuid' => $wuid,
    'staffid' => $staffid,
    'name' => $name,
    'money' => $money,
    'ban' => $ban
);
```

#### 数据库以uid（10001）查询用户信息

```php
uid2userinfo(uid);
```

返回数据为

```php
$arr = array(
    'userid' => $userid,
    'uid' => $uid,
    'wuid' => $wuid,
    'staffid' => $staffid,
    'name' => $name,
    'money' => $money,
    'ban' => $ban
);
```

#### 数据库以staffid查询用户信息

```php
staffid2userinfo(staffid);
```

返回数据为

```php
$arr = array(
    'userid' => $userid,
    'uid' => $uid,
    'wuid' => $wuid,
    'staffid' => $staffid,
    'name' => $name,
    'money' => $money,
    'ban' => $ban
);
```

#### 向文件写入数据

当内容为php字典时会转换为json

```php
write_to_file_json(文件位置, 内容);
```

读取json转为php字典

```php
read_file_to_array(文件位置);
```

#### 日志系统

```php
$logid = tool_log(1, 'bot run');//返回日志id
```

```php
tool_log(等级, 内容);
```

等级为

* 0：Debug
* 1：Info
* 2：Warn
* 3：Error
* 4：Fatal

#### 获取ogg文件时长毫秒

需要getid3扩展

```php
getOGGDurationInMilliseconds(文件位置);
```

#### 检查群权限

```php
check_group_permission(群id, 权限名);
```

#### uuid

```php
uuid();
```

#### 撤回群消息

```php
groupMessages_recall_v2($token,$robotCode, 群id, 撤回时间, 内部消息id);
```

#### 撤回单聊消息

```php
function userMessages_recall($token, 无用随意传,$robotCode, 撤回时间, 内部消息id)
```

#### 下载内容

```php
$res = requests_download_file($thumbnail, "data/download/pixiv", "GET")['saved_file'];
```

```php
requests_download_file(下载链接, 保存目录, 请求方式（可选，默认GET）, 请求体（可选）, 请求头（可选）, 超时时间（可选）) {
```

返回save\_data则为相对路径，如  data/download/pixiv/114514.png

#### 轮询并输出字典（无用）

```php
iterateDictionary(array);
```

#### 自定义错误

### v231210.1-Alpha 新增

```php
DingraiaPHPResponseExit($errCode, $message = "Unkown Error", $m = null,$stop = true, $json = false)
```

errCode为http错误码，如 403

message为退出标题，如 Forbidden

m为退出消息，如 AuthKey is error

stop为是否终止执行

json为是否json输出，但如果get参数传入format=json时会使用json输出

最终效果：![](<../../.gitbook/assets/image (4).png>)![](<../../.gitbook/assets/image (1) (1) (1) (1) (1).png>)

### v240426.1-Alpha 新增

#### PDO相关-运行sql代码

```php
DingraiaPHP_pdoRunQuery($pdo, $sql, $params = array())
```

#### 创建临时登录

```php
DingraiaPHP_createTempDingtalkLogin($token, $uid);
```

uid是10001那个

成功的话会返回uuid，请查看 接入登录 的相关教程获取登录数据

此功能方便在机器人内 联动 其他使用 同一个 appId 的 钉钉OAuth2 的 网站快登录

#### 外置插件-脚本运行末尾时运行函数

```php
DingraiaPHPAddEndModulePlugin($file, $fn_name);
```

file 是外置插件位置，fn\_name 是要运行的函数名

比如

```php
DingraiaPHPAddEndModulePlugin("module/DingraiaPHP/plugin/iirose.php", "DingraiaPHPIiroseEnd");
```

#### 新增普通输出

```php
DingraiaPHPAddNormalResponse($key = null,$t,$newArray = false)
                            //键          值 是否另起项
```

#### logger &#x20;

### v240715-Alpha 新增

这个logger可以把调试信息webhook到指定网址，推荐配合py写的接收器食用

```php
$bot_run_as["logger"]["class"]->日志等级(日志内容)
```

等级：

* trace
* debug
* info
* warning
* error
* critical
* success

### v240911-Alpha 新增与忘记写文档时补上

org\_delete\_user 企业删除用户

```php
org_delete_user($token, $userid);
```

```php
org_delete_user(token, 要删除的用户id);
```

DingraiaPHPCheckWarningWord 拦截webhook返回消息时检查有没有违禁词

```php
DingraiaPHPCheckWarningWord("xxx");
```

```php
DingraiaPHPCheckWarningWord(要检测的违禁词);
```

app\_json\_file\_add\_list 往运行日志里拉依托（）

```php
app_json_file_add_list($fp, $t)
```

```php
app_json_file_add_list(运行日志位置，通常是$bot_run_as["RUN_LOG_FILE"], 要新增的内容，字典)
```

upload\_to\_dingtalk\_v2 上传媒体文件到钉钉

缓存机制为一个月，以文件位置为主键

```php
upload_to_dingtalk_v2($type, $file, $token)
```

```php
upload_to_dingtalk_v2(文件类型，如voice video, 文件位置, $token)
```

normalizeArrayFormat 把神金的字典转换为数组

在生活中，我们可能会遇到

```json
{
	"0": [],
	"1:": "我超！粥！",
	"2": {
		"我喜欢玉足": true
	}
}
```

这样很神金的json

此时可以调用

```php
normalizeArrayFormat($arr)
```

把它变为

```json
[
	[],
	"我超！粥！",
	{
		"我喜欢玉足": true
	}
]
```

useRobotcode2Corpid 从robotCode得到组织ID

```php
useRobotcode2Corpid($robotCode);
```

### v240913-Alpha更新

短链接还原

```php
$bot["tools"]->shortUrlReduction($url);

DingraiaPHPTools::shortUrlReduction($url);

//返回为长链接，失败为false
```

