title: window.print() 的打印功能
date: 2018/12/25
categories:
- web
- js
tags:
- window.print
comments: true
---

#### 打印部分页面
某些软件应用需要实现打印的功能，比如企业内部系统的报销单，申请合同，财务对账等。可以调用js window.print() 方法调用浏览器的打印功能，再选择打印机完成打印。说说打印部分页面的常用实现方式：
* 暂时替换现页面的 document.body（如果有弹窗就不太合适）
```
let printDataHtml = document.getElementById('printData').innerHTML;
let pageHtml = document.body.innerHTML;
document.body.innerHTML = printDataHtml;

window.print();

document.body.innerHTML = pageHtml;
// 还得刷新页面
window.location.reload()
```
* css @media print，控制要打印和不需要打印的部分
* 打开一个新页面来完成打印

这里利用第三种方式来实现功能需求
```css
<!--打印分页-->
.A4 {          
    page-break-before: auto;          
    page-break-after: always;  
}
</style>
```
```html
<div id='printData'>
  <!--第一页-->
  <div class='A4'>
    xxxxx
  </div>
  <!--第二页-->
  <div class="A4>
    xxxxx
  </div>
</div>
```
```js
<script>
  printDate() {
    let printDataHtml = document.getElementById('printData').innerHTML;
    let oPop = window.open('', 'oPop');
    let str = '<!DOCTYPE html>';
    str += '<html>';
    str += '<head>';
    str += '<meta charset="utf-8">';
    str += '<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">';
    str += '<style>';
    str += '.A4 {page-break-before: auto;page-break-after: always;}';
    str += '</style>';
    str += '</head>';
    str += '<body>';
    str += printDataHtml;
    str += '</body>';
    str += '</html>';

    oPop.document.write(str);
    oPop.print();
    oPop.close();
  }
</script>
```
#### table布局写页面
对于那些报销、合同类样式，利用table 布局能很快实现。利用好td colspan 属性控制一行占几列
```
table {
    border-collapse: collapse;
    border: 2px solid rgb(100, 100, 100);
    letter-spacing: 1px;
    font-family: sans-serif;
    font-size: .7rem;
    width: 850px;
    margin-bottom: 20px;
}

td {
    border: 1px solid rgb(100, 100, 100);
    text-align: center;
    padding: 10px 10px;
    display: table-cell;
}

<table class=''>
    <tr>
        <td colspan="3">xxx</td>
    </tr>
    <tr>
        <td>xxxx</td>
        <td>xxxx</td>
        <td>xxxx</td>
    </tr>
<table>
```

![duck1](/images/20181225/print1.png)

#### js 操作对象
1. js 操作对象和数组，组合出vue :for 的数据对象
```
# 1. 类似json 数组
[
  {a: 'xx', b: 'xx'},
  {a: 'xx', b: 'xx'}
]

# 2. 使用对象
{
    xx:{a:'xx', b:'xx'},
    xx:{a:'xx', b:'xx'}
} 
```
2. 结合需求使用的是对象
```
# 定义对象
let printData = {};

# 对象是否存在key, 该方法对数组也通用
printData.hasOwnProperty(key)

# 添加元素，数组也通用
printData[xx] = {
    a: xx,
    b: xx
}

# 遍历数组
xx.forEach((key,value, array) =>{
    
});

# 遍历对象
Object.keys(printData).forEach((key) => {
  // xxx
});
```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！
