### 一.Ajax简介 优劣势 应用场景及技术

---

- Ajax简介:

  - Ajax是`Asynchronous Javascript And XML`(异步的`javascript`和`XML`)的简写
  - 并不是一种单一的技术,而是利用一系列交互式网页应用相关的技术形成的结合体
  - `Ajax`是一种快速创建动态网页的技术.通过在后台与服务器进行的少量数据交换,`Ajax`可以使网页实现局部更新.这意味着可以在不刷新网页的情况下,对网页的某部分进行更新

- 优点:

  - 页面无刷新,用户体验好
  - 异步通信,更加快响应能力
  - 减少繁杂的请求,减轻服务器负担
  - 基于标准化并被广泛支持的技术,不需要下载小插件或小程序

- 缺点:

  - `ajax`干掉了`back`按钮,对浏览器的后退机制造成了破坏
  - 存在一定的安全问题
  - 对搜索引擎的支持比较弱
  - 破坏了程序的异常机制
  - 无法用`URL`直接访问

- 应用场景:

  1. 数据验证
  2. 按需取数据
  3. 自动更新页面

- `ajax`包含以下五个部分

  - 使用css和xhtml来表示
  - 使用`DOM`模型来交互和动态显示
  - 使用`XMLHttpRequest`来和服务器进行一步通信
  - 使用`javascript`来绑定和调用

  在上面几中技术中，除了`XmlHttpRequest`对象以外，其它所有的技术都是基于`web`标准并且已经得到了广泛使用的，`XMLHttpRequest`虽然目前还没有被`W3C`所采纳，但是它已经是一个事实的标准，因为目前几乎所有的主流浏览器都支持它![img](http://upload-images.jianshu.io/upload_images/1480597-4c6beed36bb246c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  ![XMLHttpRequest对象的方法](http://upload-images.jianshu.io/upload_images/1480597-1092e842e08d9012.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 二.创建ajax的步骤

---

> `Ajax`的原理是通过`XMLHttpRequest`对象来向服务器发送异步请求,从服务器获得数据,然后通过`javascript`来操作`DOM`来更新页面.

#### 1.创建`XMLHttpRequest`对象

---

`Ajax`的核心是`XMLHttpRequest`对象，它是`Ajax`实现的关键，发送异步请求、接受响应以及执行回调都是通过它来完成

所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。

- 创建`XMLHttpRequest`对象的语法

  ``` javascript
  var xhr=new XMLHttpRequest()
  ```

- 老版本ie使用的`ActiveX`对象:

  ```javascript
  var xhr = new  ActiveXObject('Microsoft.XMLHTTP')
  ```

  为了应对所有的现代浏览器，包括 `IE5` 和 `IE6`，请检查浏览器是否支持 `XMLHttpRequest `对象。如果支持，则创建` XMLHttpRequest `对象。如果不支持，则创建` ActiveXObject `：

- 兼容各个浏览器的创建`Ajax`的工具函数

  ````javascript
  function createRequest (){
  	try {
  		xhr = new XMLHttpRequest();
  	}catch (tryMS){
  		try {
  			xhr = new ActiveXObject("Msxm12.XMLHTTP");
  		} catch (otherMS) {
  			try {
  				xhr = new ActiveXObject("Microsoft.XMLHTTP");
  			}catch (failed) {
  				xhr = null;
  			}
  		}
  	}
  	return xhr;
  }
  ````

#### 2.准备请求

---

- 初始化该`XMLHttpRequest`对象,接收三个参数

  ```javascript
  xhr.open(method,url,async)
  ```

- 第一个参数表示请求类型可以是`POST`或者`GET`

- `GET`请求:

  ```javascript
  xhr.open("GET",demo.php?name=test&age=18,true)
  ```

- `POST`请求:

  ```javascript
  xhr.open("POST",demo.php,true)
  ```

- 第二个参数请求发送目标的URL

- 第三个参数表示同步还是异步模式发出 ,默认为true

  - `false`:同步模式发送请求会暂停所有代码的执行,直到服务器响应为止
  - `true`:异步模式发送请求的同时页面可以继续加载,执行其他代码

#### 3.发送请求

---

```javascript
xhr.send()
```

- `GET`请求:

  一般情况下,`Ajax`提交的参数多是些简单的字符串,可以直接使用`GET`方法将提交的参数写到`open`方法的`url`参数中,此时`send`方法的参数为`null`或为空

  ``` javascript
  xhr.open('GET',demo.php?user_name=hou&password=123,true)
  xhr.send(null)
  ```

- `POST`请求

  如果需要像表单那样的`POST`数据,需要先使用`setRequestHeader()`来添加请求头.然后在`send()`方法里规定发送的数据

  ```javascript
  xhr.open('POST',demo.php,true);
  xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded;charset=UTF-8")
  xhr.send("user_name=hou&password=123")
  ```

#### 4.处理响应

---

```javascript
xhr.onreadystatechange=function(){
  if(xhr.readyState==4&&xhr.status==200){
    console.log(xhr.responseText)
  }
}
```

- `onreadystatechange` :当处理过程发生变化的时候执行下面的函数
- `readyState`:`ajax`处理过程
  - 0: 请求未初始化(还没调用open())
  - 1:请求已经建立,还没有发送(还没调用send())
  - 2:请求已经发送,正在处理中(通常现在可以在响应中获取内容头)
  - 3:请求正在处理中,通常响应中已有部分数据可用了,但是服务器还没完成响应的生成
  - 4:响应已完成 可以获取并使用服务器的响应了
- `status`属性:
  - 1**：请求收到，继续处理
  - 2**：操作成功收到，分析、接受
  - 3**：完成此请求必须进一步处理
  - 4**：请求包含一个错误语法或不能完成
  - 5**：服务器执行一个完全有效请求失败
- `responseText`:获取字符串形式的响应数据
- `responseXML`:获取`XMl`形式的响应数据
- 对象转换`json`格式使用`JSON.stringify`
- `json`格式转换对象使用`JSON.parse()`
- 返回值一般为`json`字符串,可以用`JSON.parse(xhr.responseText)`转化为`JSON`对象

#### 5.封装例子

---

- 将`AJAX`请求封装成ajax()方法,参数为一个配置对象params

  ```javascript
  function ajax(params){
    params=params||{};
    params.data=params.data||{};
    //判断ajax请求还是jsonp请求
    var json=params.jsonp?jsonp(params):json(params);
    //ajax请求
    function json(params){
      //请求方式默认是get toUpperCase方法把字符串转化为大写
      params.type=(params.type||'GET').toUpperCase()
      //避免有特殊字符 必须格式化传输数据
      params.data=formatParmas(params.data)
      var xhr=null
      // 实例化XMLHttpRequest对象   
      if(window.XMLHttpRequest) {   
        xhr = new XMLHttpRequest();   
      } else {   
        // IE6及其以下版本   
        xhr = new ActiveXObjcet('Microsoft.XMLHTTP');   
      }; 
    }
  }
  ```

#### jsonp

---

- 要访问web服务器的数据处理XMLHttpRequest还有一种方法是JSONP

- 什么是jsonp?

  - JSONP(JSON with padding)是一个非官方协议,他允许在服务器端集成Script tags返回至客户端,通过javascript callback的形式实现跨域访问

- JSONP有什么用

  - 由于同源策略的限制,XMLHttpRequest只允许请求当前源(域名、端口、协议)的资源,为了实现跨域请求,跨域通过script标签实现跨域请求,然后在服务端输出JSON数据并执行回调函数,从而解决跨域的数据请求

- 如何使用jsonp

  - 在客户端声明回调函数之后,客户端通过script标签向服务器跨域请求数据,然后服务端返回响应的数据并动态执行回调函数

- 使用XMLHttpRequest时,我们得到一个字符串;要用JSON.parse把字符串转化为对象,使用jsonp时script标志会解析并执行返回的代码我们处理数据时已经是一个javascript对象了

- 实例

  ```javascript
  <meta content="text/html; charset=utf-8" http-equiv="Content-Type" />  
  <script type="text/javascript">  
      function jsonpCallback(result) {  
          alert(result.a);  
          alert(result.b);  
          alert(result.c);  
          for(var i in result) {  
              alert(i+":"+result[i]);//循环输出a:1,b:2,etc.  
          }  
      }  
  </script>  
  <script type="text/javascript" src="http://crossdomain.com/services.php?callback=jsonpCallback"></script>  
  <!--callback参数指示生成JavaScript代码时要使用的函数jsonpcallback-->
  ```

- 注意浏览器的缓存问题

  - 在末尾增加一个随机数可避免频繁请求同一个链接出现的缓存问题
  - `<script type="text/javascript" src="http://crossdomain.com/services.php?callback=jsonpCallback&random=(new Date()).getTime()"></script>  

- 封装例子

  ````javascript
  function ajax(params){
    params=params||{};
    params.data=params.data||{}
    var json=params.jsonp?jsonp(params):json(params)
    //ajax请求
    function json(params){
      params.type=(params.type||'GET').toUpperCase();
      params.data=formatParams(params.data)
      var xhr=null
      //实例化XMLHttpRequest对象
      	if(window.XMLHttpRequest) {
  			xhr = new XMLHttpRequest();
  		} else {
  			// IE6及其以下版本
  			xhr = new ActiveXObjcet('Microsoft.XMLHTTP');
  		};
      //监听事件
      xhr.onreadystatechange=function(){
        if(xhr.readyState==4){
          var status=xhr.status;
          if(status>=200&&status<300){
            var response=""
            var type=xhr.getResponseHeader('content-type')
            if(type.indexof('xml')!==-1&&xhr.responseXML){
              response=xhr.responseXML//document对象响应
            }else if(type==='application/json'){
              response=JSON.parse(xhr.responseText);//json响应
            }else{
              response=xhr.responseText
            };
            params.success&&params.success(response)
          }else{
            params.error&&params.error(status)
          }
        }
      };
      //链接和传输数据
      if(params.type=='GET'){
        xhr.open(params.type,params.url+'?'+params.data,true)
        xhr.send(null)
      }else{//post请求
        xhr.open(params.type,params.url,true)
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded; charset=UTF-8');
        xhr.send(params.data);
      }
    }
    //jsonp请求
    function jsonp(params){
      //创建script标签并加入页面中
      var callbackName=params.jsonp;
      var head=document.getElementsByTagName('head')[0];
      //设置传递给后台的参数名
      params.data['callback']=callbackName;
      var data=formatParams(params.data)
      var script=document.createElement('script');
      head.appendChild(script);
      
      //创建jsonp回调函数
      Window[callbackName]=function(json){
        head.removeChild(script)
        clearTimeout(script.timeer);
        window[callbackName]=null;
        params.success&&params.success(json);
      };
      //发送请求
      script.src=params.url+'?'+data
      //超时处理
      if(params.time) {
  			script.timer = setTimeout(function() {
  				window[callbackName] = null;
  				head.removeChild(script);
  				params.error && params.error({
  					message: '超时'
  				});
  			}, time);
  		}
    };
    //格式化参数
  	function formatParams(data) {
  		var arr = [];
  		for(var name in data) {
  			arr.push(encodeURIComponent(name) + '=' + encodeURIComponent(data[name]));
  		};
  		// 添加一个随机数，防止缓存
  		arr.push('v=' + random());
  		return arr.join('&');
  	}
  	// 获取随机数
  	function random() {
  		return Math.floor(Math.random() * 10000 + 500);
  	}
  }
  ````

- 调用

  ```javascript
  ajax({
  				url: 'get.php',
  				type: 'GET',
  				data: {'intro': 'get请求'},
  				success:function(res){
  					res = JSON.parse(res);
  					document.getElementById('a').innerHTML = res.intro;
  					console.log(res);
  				}
  			});
  		ajax({
  				url: 'post.php',
  				type: 'POST',
  				data: {'intro': 'post请求'},
  				success:function(res){
  					res = JSON.parse(res);
  					document.getElementById('b').innerHTML = res.intro;
  					console.log(res);
  				}
  			});
  		ajax({
  				url: 'http://music.qq.com/musicbox/shop/v3/data/hit/hit_all.js',
  				jsonp: 'jsonpCallback',
  				data: {'callback': 'jsonpCallback'},
  				success:function(res){
  					JsonCallback(json);
  				}
  			});
  ```

  ​