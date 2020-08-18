---
title: "Html常用标签"
date: 2020-06-13T14:03:41+08:00
draft: true
tags: ["HTML"]
series: ["HTML"]
categories: ["HTML"]
---

## a 标签
a标签的两个属性：
1. href (hyper ref 超引用或者超链接)
+ 可以选择的取值
    + 网址(跳转到别人的页面上)
        1. https://google.com
        2. http://google.com
        3. //google.com (继承当前的协议)
    + 路径(在自己http服务进行跳转)
        + /a/b/c 以及 a/b/c
        1. 开启http服务之后，("/")根目录就不再是硬盘的根目录
        2. ("/")根目录的位置在于你在哪里开启了http服务 
        3. 不是("/")根目录开头的就是相对路径
        + index.html 以及 ./index.html
        + 都是到当前目录去找"index.html"
    + 伪协议
        + javscript:代码;
        ```html
        <!-- 点击a标签弹出窗口输出"1" -->
        <a href="javascript:alert(1);">JavaScript伪协议</a>
        <!-- a标签什么都不做，也不想要页面刷新的常用做法 -->
        <a href="javascript:;">查看</a>
        ```
    + 锚点
        ```html
            <div id="xxx">锚点</div>
            <!-- a标签跳转到id为"xxx"的地方 -->
            <a href="#xxx">查看</a>
        ```
    + 其他:
    + mailto:邮箱;  发邮件
    + tel:手机号   打电话
2. target 
+ 在哪个窗口打开这个超链接
+ 内置名字
    1. _blank 在新的空白标签页打开链接
    2. _self 在当前标签(或iframe层)打开链接
    3. _top 用于iframe 在最外层打开链接
    4. _parent 用于iframe 在上一层打开链接

+ 程序员命名
    1. window的name
    2. ifram的name
    3. 例如`target="xxx"` 在name叫xxx的窗口(如果没有,则新建一个name叫xxx的窗口)打开链接

## iframe 标签
+ 内嵌窗口，很少使用了

## table 标签
表格
+ 相关的标签如下:
    - table
    - thead
    - tbody
    - tfoot
    - tr
    - td
    - th
+ 它们各自的关系如下: 
```html
<table>
    <!-- table里的常用三个标签 -->
    <thead></thead>
        <!-- table row -->
        <tr>
            <!-- 表头 -->
            <th></th>
            ...
        </tr>
        <tr>
            <!-- table data -->
            <td></td>
            ...
        </tr>
        ...
    <tbody></tbody>
    <tfoot></tfoot>
</table>
```

- 相关的样式
    - table-layout
      - 值
        - auto 个性化定制 如:字数多，比字数小的宽
        - fixed 等宽
    - border-collapse
      - 值
        - collapse 合并边框
    - border-spacing
      - 值设为0 也是合并边框
    ```css
    table {
        table-layout:auto;
        // 合并边框 
        border-collapse: collapse;
        border-spacing: 0;
    }
    ```

## img 标签
+ 默认用法
```html
<img src="图片地址" alt="图片描述信息" />
```

+ 作用
  + 发出get请求,展示一张图片

+ 属性
  + alt
  + src
  + height
    + 只写高度，宽度会自适应 `height=400`
  + width
    + 只写宽度，高度会自适应 `width=400`
  + 注意:永远不要让图片变形 
    + 同时随意写height,width 容易造成图片变形
    + 所以写的时候，可以只选width去写.

+ 事件
  + onload
  + onerror
```javascript
// xxx 为img标签的id
xxx.onload = function() {
    console.log("图片加载成功");
}
// 加载失败使用替换的404图片
xxx.onerror = function() {
    console.log("图片加载失败");
    xxx.src = "/404.png";
}
```
+ 响应式
  + max-width:100%
  + 适用任意设备
  ```css
  img {
      max-width:100%;
  }
  ```

## form 标签
+ 默认用法:
```html
<!-- /xxx 请求页面 -->
<form action="/xxx" method = "POST" 
autocomplete="on"
>
...
    <input type = "text"/>
    <input type = "submit" />
</form> 
```
+ 作用
  + 发get 或 post 请求, 然后刷新页面
+ 属性
  + action
    + 值 请求的路径
  + autocomplete
    + 值
      + on
      + off
  + method
    + 值
      + get
      + post
  + target
    + 与a标签的target一样
+ 事件
  + onsubmit (提交)
+ form标签下 input 与 button 的区别
```html
<!-- button里面可以做很多嵌套 -->
<input type="submit" value="提交" />
<button type="submit" ><strong>提交</strong></button>
```
  + 只写button 默认该button的type就是"submit"

## input 标签
+ 作用
  + 让用户输入内容
+ 属性
  + type:
    + 值
      + button
      + checkbox
      + email
      + file
      + hidden
      + number
      + password
      + radio
      + search
      + submit
      + tel
      + text
    + 代码例子
    ```html
    <input type="text" />
    <hr />
    <input type="color" />
    <hr />
    <input type="password" />
    <hr />
    <input type="radio" name="gender" /> 男
    <input type="radio" name="gender" /> 女
    <hr />
    <input type="checkbox" name="hobby"/>唱
    <input type="checkbox" name="hobby"/>跳
    <input type="checkbox" name="hobby"/>RAP
    <hr />
    <input type="file" />
    <input type="file" multiple/>
    <hr />
    <input type="hidden"/>
    <hr />
    <textarea></textarea>
    <hr />
    <select>
        <option value="1">星期一</option>
        <option value="
        2">星期二</option>
    </select>
    ...
    ```
    + 其他属性:
      + name/autofocus/checked/disabled/maxlength/pattern/value/placeholder
+ 事件
  + onchange/onfocus/onblur

## 注意事项
+ 一般不监听input的click事件
+ form里面的input要有name
+ form里要放一个type=submit才能触发submit事件



