# CSRF
CSRF(Cross-site request forgery)，即跨站请求伪造，指的是黑客诱导用户点击链接，打开黑客的网站，然后黑客利用用户**目前登录的状态**发起跨站请求。

当你点击了黑客的一个链接，打开了黑客的页面后，黑客有三种方式去实施CSRF攻击。

### 1.自动发起GET请求
黑客网页里可能有这样一段代码：
```html
<img src="https://xxx.com/info?user=hhh&count=100"></img>
```
进入这个页面后自动发送get请求，值得注意的是，这个请求会自动带上关于xxx.com的cookie信息（假设你已经在xxx.com中登录过）。

假如服务端没有相应的验证机制，它可能认为这是一个正常的用户发起的请求，因为携带了相应的cookie，然后进行相应的各种操作，转账汇款等恶意操作。

### 2.自动发起POST请求
黑客可能自己填了一个表单，写了一段自动提交的脚本。
```html
<form id='hacker-form' action="https://xxx.com/info" method="POST">
  <input type="hidden" name="user" value="hhh" />
  <input type="hidden" name="count" value="100" />
</form>
<script>document.getElementById('hacker-form').submit();</script>
```
同样也会携带相应的用户信息，让服务器误以为是一个正常的用户在操作。当用户打开改站点时，这个表单就会自动执行提交。当表单被提交后，服务器就会执行转账操作。
### 3.诱导用户点击链接
在黑客网站上，可能会放上一个链接，诱导你去点击。
```html
<a href="https://xxx/info?user=hhh&count=100" taget="_blank">点击进入修仙世界</a>
```
点击后，自动发送 get 请求，接下来和`自动发 GET 请求`部分同理。

这就是`CSRF`攻击的原理。和`XSS`攻击不同的是，`CSRF`攻击并不需要将恶意脚本注入用户当前的`html`文档中，而是跳转到新的页面，利用服务器的`验证漏洞`和`用户当前登录状态`来模拟用户操作。

## 防范措施

### 1.利用Cookie的SameSite属性
`CSRF`攻击的最重要一环就是自动发送目标站点的`Cookie`，然后利用这一份`Cookie` 模拟用户的身份。
而在`Cookie`的字段中有这么一个关键字段，可以对请求中的`Cookie`的携带做一些限制，这个字段就是`SameSite`。

`SameSite`可以设置三个值，`Strict`、`Lax`和`None`。

a. 在`Strict`模式下，浏览器完全禁止第三方请求携带的`Cookie`。比如`xxx.com`网站只能在`xxx.com`域名当中请求才能携带`Cookie`，在其他网站都不行。
b. 在`Lax`模式下，就宽松一点，但是只能在`get方法提交表单`或者`a标签发送get请求`的情况下可以携带`Cookie`，其他情况均不能。
c. 在`None`模式下，也就是默认模式，请求会自动携带上`Cookie`。

### 2.验证来源站点
这就需要用到请求头中的两个字段`Referer`和`Origin`。

其中，`Origin`只包含域名信息，而`Referer`包含了具体的URL路径。

当然，这两者都是可以伪造的，通过 Ajax 中自定义请求头即可，安全性略差。

### 3.CSRF Token
浏览器向服务器发送请求时，服务器会生成一个字段，将其植入到返回的页面中。

然后浏览器如果要发送请求，就必须带上这个字符串，然后服务器验证是否合法，如果不合法则不予相应，这个字符串就是`CSRF token`，通常第三方站点无法拿到这个token，因此也就会被服务器拒绝。

## 总结

CSRF，跨站请求伪造，指的是黑客诱导用户点击链接，打开黑客的网站，然后黑客利用用户目前登录的状态发起跨站请求。

`CSRF`攻击一般会有三种形式：
1. **自动发送get请求**
2. **自动发送post请求**
3. **诱导点击发送get请求**

防范措施：
- **利用Cookie的`SameSite`属性**
- **验证来源站点**
- **CSRF token**


