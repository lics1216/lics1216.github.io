title: wkhtmltox实现 md转 pdf
date: 2019/01/08
categories:
- web
- php
tags:
- wkhtmltox
comments: true
---

doc 是用来展示开发人员写的项目文档的服务，现在要添加一个md2pdf 的下载功能，下面说一下自己是怎样用wkhtmltox实现 md转 pdf

#### doc 实现过程
下面是开发文档展示的过程，只需看wkhtmltox 实现md 转pdf 可以忽略
1. 文档写在仓库 docData，每次commit 时利用git-hook 触发http 请求给服务mcdoc api
2. doc 服务会pull 文档到服务器指定目录，再读取目录和目录下的md/sql 文件，用于前端发挥展示。在doc 是这样实现的，解析md 文件提取标题，和内容返回前端
3. doc 接收到docData 请求会自动pull docData，更新文档

#### 实现 wkhtmltox
考虑到转换的效果、性能，选择在后端完成
* [选择利用js 的实现可以参考这里] (https://segmentfault.com/a/1190000016324962)
* 在服务器安装服务，再通过php 扩展实现，不借助其他服务转pdf 的效果往往不是很好

后端的一些尝试
* [tc-lib-pdf不用其他安装服务，只借助一个php 扩展](https://github.com/tecnickcom/tc-lib-pdf)，但是对md 代码块中文字体显示不出来
* 安装pandoc，这个软件很强大各种格式都可以转。但要安装相应的latex 引擎，过程繁琐（甚至还会碰到C库不支持），而且只是用来转一个md 真是大材小用。
* wkhtmltox php，是一个html2pdf 的工具。看起来好像不可以，稍微改变一下思路是可以的！

普遍问题是支持css和中文，在服务器上安装软件费时

#### 实现思路
参考浏览器的思路，先把md 转成html 标签，再人为赋予它md css，这样md 文件就变成html 文件了，再利用wxhtmltox 转成pdf。 
1. [md格式 转成html 标签可以参考这个]( https://github.com/michelf/php-markdown)
2. 需要给上面的html 标签加上css 样式，[md csss 戳这里](   https://github.com/zhangjikai/markdown-css)
3. [wkthtmltox 的php 扩展] (https://github.com/mikehaertl/phpwkhtmltopdf)

wkhtmltopdf 本身是没有问题的，可能有的扩展不好用，使用扩展遇到问题，可以在issue 中查找解决方法。记得设置css 页面大小A4 纸，否则一旦超出宽度 pdf 显示不全哦！

遇到的问题总结
1. 技术选择不太擅长，一旦试了，不行又费时。不能准确把握选用这项技术时会出现的问题
2. linux 安装软件遇到很多问题，安装php 扩展要总结一下。
    * 包括资源获取，源码包source code，已编译的二进制文件包
    * 安装时出问题
3. php require/use 区别，对文件的操作
4. wkhtmltopdf 中文乱码处理（编码问题一定可以解决）

理解浏览器显示md 文件是把md 转成 html 标签，再加上样式。引擎也是起到同样得作用，明白这一点就可以避开安装引擎这个坑！

#### wxhtmltox 安装
```
# 1. 获取安装包
wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz

# 2. 解压到 /usr/local/wkhtmltox/
tar -xf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz -C /usr/local

# 3. 添加path /usr/local/wkhtmltox/bin
echo 'export PATH=/usr/local/wkhtmltox/bin:$PATH' >> /etc/profile
source /etc/profile
```

#### php 实现
```

if (!is_dir($dirPDF)) {
    mkdir($dirPDF, 0755, true);
}
// 生成HTML str， 人为加md css
$res = MdDocHelper::getInstance()->getFileContent($app, $md);
$content = MarkdownExtra::defaultTransform($res['data']);
$content = str_replace('<pre>', '<pre class="prettyprint linenums">', $content);
$content = str_replace('<a href=', '<a target="_blank" href=', $content);
$meta = "<meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\">";
$mdCSS = config('mdcss');
$html = $meta.$mdCSS.$content;

// 开始转 pdf
$pdf = new Pdf($html);
$pdf->setOptions([
'binary' => config('mcdoc.wkhtmltopdfPath'),
]);
if (!$pdf->saveAs($pathPDF)) {
    $error = $pdf->getError();
    return [
    'res' => 1,
    'msg' => $error
    ];
}

```
欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！