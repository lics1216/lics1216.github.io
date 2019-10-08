title: web 安全
date: 2019/01/29
categories:

- web
- web 安全
tags:
- web 安全
comments: true
---

web 安全一直是个话题，常见的包括防范crsf攻击、sql注入，对存储在服务器的敏感数据如用户信息进行加密处理；防止程序攻网络资源，使用验证码进行识别；对于垃圾信息，敏感信息进行过滤在存储等。
#### csrf 防范
前段时间在laravel框架下写了一个上传excel的功能，请求被认为是csrf攻击被拦截了。于是就花时间了解什么是csrf(跨站请求伪造)，对crsf不太清楚的同学可以[戳这里](https://www.jianshu.com/p/e825e67fcf28)。

laravel框架对前端的每一次请求都会返回一个cookie，key为XSRF-TOKEN，接下来的请求要在头部携带前一次返回的cookie 才能通过csrf 验证。
```html
# 前端实现
<el-upload
ref='upload'
action="/xx/addData"
:headers="headers"
:limit="1"
:show-file-list="false"
:on-progress="xx"
:on-success="xx"
:on-error="xx"
:accept="uploadAccept">
    <el-tooltip class="item" effect="dark" content="只能上传csv/xlsx且不超过2G" placement="top">
        <el-button size="small" type="primary">上传</el-button>
    </el-tooltip>
</el-upload>
```

这个上传excel文件应用element ui组件，被认为是csrf攻击是未在请求头部携带相应的cookie，由于其他请求是通过axios发起的，内部会自动实现，所以能通过安全认证。可以这样解决
```js
headers: {'X-XSRF-TOKEN': this.getCookie('XSRF-TOKEN')}

# 获取本地的cookie
getCookie(name) {
    let cookiesList = document.cookie.split(';');
    for (let cookie of cookiesList) {
        let arr = cookie.split('=');
        if (arr[0] === name) {
            return decodeURIComponent(arr[1]);
        }
    }
    return '';
}
```

**因为开发者不能获取其他域名下的cookie，这样是可以防范csrf的** 

#### api 认证
很多时候我们发送请求都是b2s，就是客户端给服务端发送请求，但有时也需要在后端请求另外一个服即s2s。比如利用php guzzlehttp/guzzle http客户端，这必定涉及api 验证。这里我用的方式是
1. 双方先约定加密key 值，请求方携带参数、对参数加密的sign
2. 接收方对参数重新加密一次，再对比sign，检查参数是否被篡改

在php 中是这样实现的，
```php
# 利用 hash_hmac sha256 算法，返回 params 加密后 的sign 字符串
private function makeSign($params = [])
{
    ksort($params);
    $str = '';
    foreach ($params as $k => $v) {
        if(!is_null($v)) {
        	$str .= '#' . $k . '|' . $v;
        }
    }
    // appkey 是加密时用的key
    return hash_hmac("sha256", $str, $this->appkey);
}
```

小程序开发请求微信服务端api，也是要先获取请求的调用凭证 accessToken，获取这个凭证需要提供该小程序的appid、appAppSecret，凭证也是有时限性的。微信也对用户的信息进行加密后返回，比如运动步数，开发者必须请求加密密钥才能把信息解密。



欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！