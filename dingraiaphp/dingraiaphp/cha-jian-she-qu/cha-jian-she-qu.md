---
description: 插件社区暂时只通过API上传代码，这是因为前端没写（不会啊QAQ）
---

# 插件社区

## 使用

### 获取插件代码

<mark style="color:green;">`GET`</mark> [https://api.lxyddice.top/v1/DingraiaPHP/plugins/get](https://api.lxyddice.top/v1/DingraiaPHP/plugins/get?plugin=com.lxyddice.betterHelp)

\<Description of the endpoint>

**Params**

<table><thead><tr><th width="257">参数名</th><th>示例值</th><th>简介</th></tr></thead><tbody><tr><td>plugin</td><td><code>com.xxx.xx</code></td><td>插件包名</td></tr><tr><td>access_token</td><td><code>xxxxxxxxxxxx</code></td><td>插件开发者token</td></tr></tbody></table>

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "code": 0,
    "success": true,
    "message": "ok",
    "result": {
        "code": "",
        "filename": "com.lxyddice.betterHelp.php",
        "info": "修复无法删除的bug",
        "version": "1.1.2",
        "updateTime": "2024-02-07 16:16:39",
        "hash_sha256": "ad45fbd725e08b62021ea5d005ad2d4c67180a6e0f6f4588ed9e2a952bb252c2",
        "hash_md5": "44fb494207076498fb27040ebae9a156",
        "sign": "eafefa0c9fe3d13553dfc9fe425a1096"
    }
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "code": 404,
    "success": false,
    "message": "plugin not found"
}
```
{% endtab %}

{% tab title="403" %}
```json
{
    "code": 403,
    "success": false,
    "message": "plugin error"
}
```
{% endtab %}
{% endtabs %}

## 注意

* POST数据量不要超过100kb
* 请勿上传包含病毒、木马、危害计算机或服务器安全的代码，确保您上传的内容不要违反国家法律法规
* 请遵守包名原则，类似于JAVA的包名，如com.lxyddice.me
* 请不要冒充他人上传，包名和代码中的署名可以写您的代号
* 违反国家法律法规、DingraiaPHP社区规则的代码，开发组有权封禁您的上传账号并清空相关的代码
* 为了您的账号安全，请不要泄露您的上传账号的密码或OAuth2账号登录权
