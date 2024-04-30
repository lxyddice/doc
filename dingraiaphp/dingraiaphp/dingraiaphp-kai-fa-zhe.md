# DingraiaPHP开发者

## 获取token

### 使用钉钉OAuth2登录



## 获取鉴权State

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppGetState`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppGetState)

获取鉴权State

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



## 获取钉钉登录的跳转地址

<mark style="color:blue;">`GET`</mark> [`https://api.lxyddice.top/dingbot/php/?action=api&type=xyAppLoginWithDingtalkOAuth2`](https://api.lxyddice.top/dingbot/php/?action=api\&type=xyAppLoginWithDingtalkOAuth2\&state=5c5a3ff9-52bd-c49b-b05f-1bca6273fcd0)

\<Description of the endpoint>

**ARA\x**

| Name   | Type   | Description      |
| ------ | ------ | ---------------- |
| `name` | string | Name of the user |
| `age`  | number | Age of the user  |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "id": 1,
  "name": "John",
  "age": 30
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}
