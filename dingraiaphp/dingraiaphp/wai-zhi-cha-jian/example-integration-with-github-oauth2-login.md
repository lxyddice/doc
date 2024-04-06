# Example: Integration with Github OAuth2 login

### Create files

* module/DingraiaPHP/data/githubOAuth2.json
* module/DingraiaPHP/data/githubOAuth2Login.json
* module/DingraiaPHP/config/githubOAuth2.json

### Code

#### Update code file

* module/DingraiaPHP/main.php (Add code in function "DingraiaPHPLoadMoudlePluginMain")

```php
    if (isset($_GET['githubOAuth2Login']) || isset($_GET["githubOAuth2"])) {
        require_once("module/DingraiaPHP/plugin/githubOAuth2.php");
        $c = DingraiaPHPgithubOAuth2ModulePluginMain();
        if ($c) {
            $c["chat_mode"] = "lxyddice";
            $r[] = $c;
            return $r;
        }
        return false;
    }
```

* module/DingraiaPHP/app/requireRunPlugin.php (Add code in "if ($\_GET\['action'] == "api") {")

```php
require_once(__DIR__."/api/githubOAuth2.php");
```

#### New files

* module/DingraiaPHP/plugin/githubOauth2.php (New file)

```php
<?php
function DingraiaPHPgithubOAuth2ModulePluginMain() {
    if (isset($_GET["githubOAuth2Login"])) {
        /*阶段A*/
        DingraiaPHPgithubOAuth2ModulePluginLogin();
    } else {
        /*阶段B*/
        DingraiaPHPgithubOAuth2ModulePluginCheck();
    }
}

//Because GFW,if server in China must use proxy to request Github.So if you don't use
//this function with "DingraiaPHPCurl" you can delete it,and change request urls
function DingraiaPHPgithubOAuth2ModulePluginProxyRequests($u, $b, $h) {
    $curl_rand = uniqid();
    $res = requests("POST", "curl_url", [
        "curl_url" => $u,
        "curl_type" => "POST",
        "curl_body" => $b,
        "curl_header" => $h,
        "curl_key" => md5(time().$curl_rand."curl_key"),
        "curl_timestamp" => time(),
        "curl_rand" => $curl_rand
    ], ["Content-Type" => "application/json"], 10);
    return $res["body"];
}

function DingraiaPHPgithubOAuth2ModulePluginLogin() {
    $state = $_GET['state'];
    $client_id = $_GET['client_id'];
    $f = __DIR__."/../config/githubOAuth2.json";
    $c = read_file_to_array($f);
    if (!isset($c[$client_id])) {
        DingraiaPHPResponseExit(400, "clientId not exists");
    } else {
        $conf = $c[$client_id];
    }
    $redirect_uri = urldecode($_GET['redirect_uri']);
    $f = read_file_to_array(__DIR__."/../data/githubOAuth2.json");
    $key = uuid();
    $f[$key] = ["state"=>$state, "redirect_uri"=>$redirect_uri, "client_id"=>$client_id, "use"=>0];
    $redirect_uri = $conf['redirect_uri'];
    write_to_file_json(__DIR__."/../data/githubOAuth2.json", $f);
    $url = "https://github.com/login/oauth/authorize?client_id={$client_id}&redirect_uri={$redirect_uri}&scope=user&state={$key}";
    header("Location: $url");
    exit();
}
function DingraiaPHPgithubOAuth2ModulePluginCheck() {
    if (isset($_GET['code']) && isset($_GET['state'])) {
        $f = read_file_to_array(__DIR__."/../data/githubOAuth2.json");
        $authCode = $_GET['code'];
        $state = $_GET['state'];
        if (isset($f[$state]) && isset($f[$state]['use']) && $f[$state]['use'] == 0) {
            $client_id = $f[$state]['client_id'];
            $c = read_file_to_array(__DIR__."/../config/githubOAuth2.json");
            if (isset($c[$client_id])) {
                $appsec = $c[$client_id]["AppSecret"];
                $headers = ["Content-Type" => "application/json","Accept"=>"application/json"];
                $data = ["client_secret" => $appsec,"client_id" => $client_id,"code" => $authCode,"redirect_uri" => $c[$client_id]["redirect_uri"]];
                $res = DingraiaPHPgithubOAuth2ModulePluginProxyRequests("https://github.com/login/oauth/access_token",$data, $headers);
                $res = json_decode($res, true);
                if (isset($res['access_token'])) {
                    $token = $res['access_token'];
                    $headers["Authorization"] = "Bearer {$token}";
                    $headers["User-Agent"] = "lxyddice";
                    $res = DingraiaPHPgithubOAuth2ModulePluginProxyRequests("https://api.github.com/user",[], $headers);
                    $res = json_decode($res, true);
                    if (isset($res['login'])) {
                        $ck = DingraiaPHP_serviceBanMain(["userId"=>$res["unionId"]], "githubOAuth2Login");
                        if ($ck["code"] == 0) {
                            $l = read_file_to_array(__DIR__."/../data/githubOAuth2Login.json");
                            $l[$state] = $res;
                            $l[$state]["authCode"] = $token;
                            $f[$state]['use'] = 1;
                            write_to_file_json(__DIR__."/../data/githubOAuth2.json",$f);
                            write_to_file_json(__DIR__."/../data/githubOAuth2Login.json",$l);
                            $url = $f[$state]['redirect_uri'];
                            $parsedUrl = parse_url($url);
                            if (isset($parsedUrl['query'])) {
                                $url .= "&DingraiaPHPState={$state}&state=" . urlencode($f[$state]['state']);
                            } else {
                                $url .= "?DingraiaPHPState={$state}&state=" . urlencode($f[$state]['state']);
                            }
                            header("Location: $url");
                        } else {
                            DingraiaPHPResponseExit($ck["code"], $ck['msg']);
                        }
                    }
                } else {
                    DingraiaPHPResponseExit(500, "Failed to check your login from Gtihub");
                    tool_log(3, ["state"=>$state,"message"=>$res]);
                }
            } else {
                DingraiaPHPResponseExit(403, "ClientId does not exist");
            }
        } else {
            DingraiaPHPResponseExit(403, "The callback parameter does not exist");
        }
    }
}
```

* module/DingraiaPHP/app/api/githubOAuth2.php (New file):

```php
<?php

if ($bot_run_as) {
    if ($_GET['type'] == "githubOAuth2Get") {
        if (isset($_GET['state']) && isset($_GET['DingraiaPHPState']) && isset($_GET['timeStamp']) && isset($_GET['sign'])) {
            $DingraiaPHPState = $_GET['DingraiaPHPState'];
            $state = $_GET['state'];
            $ts = $_GET['timeStamp'];
            $sign = $_GET['sign'];
            $f = read_file_to_array(__DIR__."/../../data/githubOAuth2Login.json");
            $o = read_file_to_array(__DIR__."/../../data/githubOAuth2.json");
            $tsign = hash('sha256', $DingraiaPHPState.$state.$ts.$bot_run_as['config']['dingraiaAuthKey']);
            if ($sign == $tsign) {
                if (isset($f[$DingraiaPHPState]) && $o[$DingraiaPHPState]['state'] == $state) {
                    $apiResponse['code'] = 0;
                    $apiResponse['result'] = $f[$DingraiaPHPState];
                } else {
                    $g = read_file_to_array(__DIR__."/../data/tempGithubOAuth2.json");
                    if (isset($g[$DingraiaPHPState])) {
                        $apiResponse['code'] = 0;
                        $apiResponse['result'] = $g[$DingraiaPHPState];
                    } else {
                        $apiResponse['success'] = false;
                        $apiResponse['code'] = -8;
                    }
                }
            } else {
                $apiResponse['success'] = false;
                $apiResponse['code'] = -5;
            }
        } else {
            $apiResponse['success'] = false;
            $apiResponse['code'] = -2;
        }
    }
}
```

### How to use

Complete the profile

* module/DingraiaPHP/config/githubOAuth2.json

```json
{"client_id":{"redirect_uri":"redirect_uri","AppSecret":"app_secret"}}
```

like dingtalk oauth2 login,you can construct url to DingraiaPHP

```
https://xxx.com/index.php?githubOAuth2
&client_id=client_id
&state=prevent CSRF
&redirect_uri=when login success will location to url
```

After login, the user is redirected to

```
https://xxx.com/index.php?code=xxx&state=xxxx
```

At this point, the framework will check whether the uuid has been used, if it has been used, it will report an error, otherwise it will request the user information from the Dingpin server. Then jump to You set the callback url

```
https://yyy.com/?DingraiaPHPState=Unique uuid
&state= original state
```

Then, you need to request the framework's API to get the token and user information. The address is

```
https://xxx.com/?action=api&type=oauth2Get
&DingraiaPHPState= Unique uuid above
&state= original state
&timeStamp= full second time stamp
&sign= signature
```

Where sign is hash('sha256', $DingraiaPHPState.$state.$ts.$conf\['dingraiaAuthKey']);&#x20;

$conf\['dingraiaAuthKey'] is the dingraiaAuthKey parameter of config/bot.json The timestamp validation timeout is dingraiaAuthTimeout
