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

#### 创建群

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
$res = requests("POST", "https://api.lxyddice.top/api/gk", $data)['code'];//返回响应头
```

```php
requests(请求方法, url, 请求体, 请求头, 超时时间);
```

小提示：默认的data不会转为json，要i的话请添加在header内添加`["Content-Type" => "application/json"]`

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

#### 数据库以![](<../../.gitbook/assets/image (1) (1) (1).png>)查询用户数据

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

* 1：Debug
* 2：Info
* 3：Warn
* 4：Error
* 5：Fatal

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

撤回消息

```php
groupMessages_recall_v2($token,$robotCode, 群id, 撤回时间, 内部消息id);
```

下载内容

```php
$res = requests_download_file($thumbnail, "data/download/pixiv", "GET")['saved_file'];
```

```php
requests_download_file(下载链接, 保存目录, 请求方式（可选，默认GET）, 请求体（可选）, 请求头（可选）, 超时时间（可选）) {
```

返回save\_data则为相对路径，如  data/download/pixiv/114514.png
