# DingraiaPHP开发者

## 开发者上一步得到的上一步得到的获取token

### 使用钉钉OAuth2登录

#### 快速登录页面

{% embed url="https://api.lxyddice.top/dingbot/php/?action=p&page=xyApp" %}

## 获取登录请求代码

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppGetState`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppGetState)

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "state": "5c5a3ff9-52bd-c49b-b05f-1bca6273fcd0",
        "timeStamp": 1714488342
    },
    "request_id": "1714488342_ae67ae84-b6ad-a09a-a6cb-336f1d1fc32f"
}
```
{% endtab %}

{% tab title="403" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714491128_b77f858a-1fc0-d9a8-4f62-977f813fec4a",
    "tips": "IP xx.xx.xx.xxx 已存在登录请求，请使用 type=xyAppClearIpState 取消"
}
```
{% endtab %}
{% endtabs %}



##

## 清除登录请求代码

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppClearIpState`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppClearIpState)

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "state": "5c5a3ff9-52bd-c49b-b05f-1bca6273fcd0",
        "createAt": 1714488342,
        "timeStampe": 1714491351
    },
    "request_id": "1714491351_a327e328-3b48-ee06-800f-f64142fdd00a"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "success": false,
    "code": -6,
    "message": "（翻找）没...没找到诶？TAT",
    "result": null,
    "request_id": "1714491462_8b9c557f-c299-3194-0d97-acec865c4450",
    "tips": "IP xx.xx.xx.xxx 不存在登录请求"
}
```
{% endtab %}
{% endtabs %}

## 获取钉钉登录的跳转地址

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppLoginWithDingtalkOAuth2`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppLoginWithDingtalkOAuth2\&state=5c5a3ff9-52bd-c49b-b05f-1bca6273fcd0)

**Params**

| Name    | Type   | Description |
| ------- | ------ | ----------- |
| `state` | string | 得到的登录请求代码   |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "state": "a95bb8c2-b084-869c-d76f-52ba5da21309",
        "createAt": 1723393731,
        "timeStampe": 1723393732,
        "urlId": "ANVpQjLH"
    },
    "request_id": "1723393732_46e5bcdd-908c-e23c-298f-04bbf8e25a2d",
    "apiPath": "xyAppLoginWithDingtalkOAuth2"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714491585_710721ef-04ee-7d06-d6be-dc3cdb47e1f0",
    "tips": "不存在登录请求 State:d1a906af-2ee0-f51d-6ada-6d1e11de1e44"
}
```
{% endtab %}
{% endtabs %}

## 获取钉钉登录的跳转地址

<mark style="color:blue;">`GET`</mark> [https://api.lxyddice.top/dingbot/php/?action=api\&type=getShortUrlInfo](https://api.lxyddice.top/dingbot/php/?action=api\&type=getShortUrlInfo)

**Params**

| Name | Type   | Description  |
| ---- | ------ | ------------ |
| `id` | string | urlId  短链接代码 |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "url": "http://api.lxyddice.top/dingbot/php/?client_id=xxxxx"
    },
    "request_id": "1723393746_5a1e2650-b635-7272-5983-d60f2292c8ae",
    "apiPath": "getShortUrlInfo"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "success": false,
    "code": -6,
    "message": "（翻找）没...没找到诶？TAT",
    "result": null,
    "request_id": "1723393846_b1fd2cb2-034b-5b09-aee6-fc5c36d6fe6f",
    "apiPath": "getShortUrlInfo"
}
```
{% endtab %}
{% endtabs %}

完成登录

{% tabs %}
{% tab title="登录成功得到token" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "token": "abcdefghijklmnopqrstuvwxyz",
        "expiration": 1714497275,
        "unionId": "xxx"
    },
    "request_id": "1714490072_fe3fc718-fd61-6564-dfc2-000d309a0a59"
}
```
{% endtab %}

{% tab title="登录成功但无权限" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714491720_5776f845-6c59-546d-02c6-5666a051c5fc",
    "tips": "很遗憾，您似乎不具备钉钉登录获取开发者token的权限~ unionId:xxx"
}
```
{% endtab %}
{% endtabs %}



## 验证token可用性

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppCheckToken`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppCheckToken\&token=wrHm8JjzwaPX5XN6r2g7Lqtkke5sUhrCVHMXL8BiCo90qm9BH6hnniTPQagrzWzX)

**Params**

| Name  | Type   | Description |
| ----- | ------ | ----------- |
| token | String | 上面得到的token  |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "unionId": "xxx",
        "expiration": 1714497275
    },
    "request_id": "1714490762_6d9a20b5-cc20-77e9-5117-7968b68ba80e"
}
```
{% endtab %}

{% tab title="403" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714491964_a8a0d13f-f4bd-0e2e-0f99-663dc6fe2977"
}
```
{% endtab %}
{% endtabs %}



## 创建APP以便获取id和密钥

<mark style="color:blue;">`GET`</mark> [<mark style="color:blue;">https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppGetId</mark>](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppGetId\&token=IZHIMJoxx1Bq8oayDg5SEOPU0BAOPeBfuYQpTvsoaYX1sJU5SHfvHgTnDu8lxsjp)

**Body**

| Name    | Type   | Description |
| ------- | ------ | ----------- |
| `token` | string | AccessToken |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": [
        {
            "appId": "xxx",
            "appSecret": "xxxxxxxx"
        }
    ],
    "request_id": "1714633692_76398e1c-5f8e-7c35-9b00-c0ef41df64dd",
    "apiPath": "xyAppGetId"
}
```
{% endtab %}

{% tab title="403" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714714263_8c358e28-3c93-4acf-32aa-70d561ae242f",
    "apiPath": "xyAppGetId"
}
```
{% endtab %}
{% endtabs %}



## 使用APPID和APPSECRET获取TOKEN

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppGetAccessToken`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppGetAccessToken)

**Params**

| Name        | Type   | Description |
| ----------- | ------ | ----------- |
| `appId`     | string | appId       |
| `appSecret` | string | appSecret   |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "code": 0,
    "message": "API调用成功了喵~",
    "result": {
        "token": "xxxxxxxxxxxxxxxxxxx",
        "expiration": 1714645804,
        "unionId": "xxxxx"
    },
    "request_id": "1714638604_123f7afb-409d-55d9-4059-27a70dfc408f",
    "apiPath": "xyAppGetAccessToken"
}
```
{% endtab %}

{% tab title="403" %}
```json
{
    "success": false,
    "code": -5,
    "message": "冰晶拒绝大人的请求(//̀Д/́/)",
    "result": null,
    "request_id": "1714714489_02bef3e2-f000-32bd-d8b0-c91481ee0c71",
    "apiPath": "xyAppGetAccessToken"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "success": false,
    "code": -6,
    "message": "（翻找）没...没找到诶？TAT",
    "result": null,
    "request_id": "1714714496_ec9e6def-f66d-fdef-2349-78665396b2d7",
    "apiPath": "xyAppGetAccessToken"
}
```
{% endtab %}
{% endtabs %}
