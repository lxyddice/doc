# 使用前工作与安装

### 虽然有一键安装，但是不确保可以正常工作

#### 推荐环境：

* PHP 7.0-7.4
* MYSQL 5-8
* 这是一台vps而不是虚拟主机，或者你的虚拟主机可以编辑站点配置文件

#### 必须操作：

* 禁止.json、.txt文件的访问，nginx配置如下：

```nginx
location ~* ^/.*.(json|.txt|.yml|.cfg)$
{
    return 404;//改成403或444也不是不行
}
```

#### 推荐操作：

* 手动安装composer.phar  [https://getcomposer.org/download/](https://getcomposer.org/download/)  下载cpmposer.phar到网页根目录
* require以下依赖：[https://api.lxyddice.top/file/james-heinrich.zip](https://api.lxyddice.top/file/james-heinrich.zip) [https://api.lxyddice.top/file/hlf\_513.zip](https://api.lxyddice.top/file/hlf\_513.zip) [https://api.lxyddice.top/file/alibabacloud.zip](https://api.lxyddice.top/file/alibabacloud.zip) （require命令我忘了QAQ）
* 如果是手动安装则放到vendor（composer依赖安装位置），不要忘记autoload.php

然后你可以开始进入安装程序，连接数据库、定义composer位置等

安装成功后即可前往下一个页面

[zi-dai-ming-ling.md](../zi-dai-ming-ling.md "mention")
