# 发送使用webhook的普通消息

### 插件全部丢在plugin下，会自动加载启用的插件

#### 发送普通消息

```php
if ($globalmessage == "/hi"){
    send_message('hi!', $webhook, $staffid);
}
```

<pre><code>send_message(消息内容, 回调地址, <a data-footnote-ref href="#user-content-fn-1">接收人userid</a>);
</code></pre>

[^1]: 
