---
title: 新的Fetch API
date: 2017-01-07 16:29:16
tags:
- Fetch
- 异步
- 数据

categories: 数据请求
---

### 新的Fetch API

#### 什么是Fetch?

> google翻译为取，顾名思义，就是获取某个东西用的；

#### Fetch有何用？

> 在程序当中获取，当然就是获取数据，获取图片，获取某个资源呗！

#### 不是已经有ajax了吗？为什么还要用Fetch？

> ajax是对JavaScript中XMLHttpRequest(XHR)的封装。

> JavaScript 通过XMLHttpRequest(XHR)来执行异步请求，这个方式已经存在了很长一段时间。虽说它很有用，但它不是最佳API。它在设计上不符合职责分离原则，将输入、输出和用事件来跟踪的状态混杂在一个对象里。而且，基于事件的模型与最近JavaScript流行的Promise以及基于生成器的异步编程模型不太搭。所以...

> 新的 Fetch API打算修正上面提到的那些缺陷。 它向JS中引入和HTTP协议中同样的原语（即Fetch）。具体而言，它引入一个实用的函数fetch()用来简洁捕捉从网络上检索一个资源的意图。

|优点|
|:---|
|改善离线体验|
|保持可扩展性|

#### 故在这种情况下，可以在项目中使用Fetch来替代ajax了（仅供参考）：

#### 简单的fetching示例

```javascript
fetch("/data.json").then(function(res) {
  // res instanceof Response == true.
  if (res.ok) {
    res.json().then(function(data) {
      console.log(data.entries);
    });
  } else {
    console.log("Looks like the response wasn't perfect, got status", res.status);
  }
}, function(e) {
  console.log("Fetch failed!", e);
});
```

#### 如果是提交一个POST请求，代码如下：

```javascript
fetch("/data.json", {
  method: "POST",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  body: "firstName=Nikhil&favColor=blue&password=easytoguess"
}).then(function(res) {
  if (res.ok) {
    alert("Perfect! Your settings are saved.");
  } else if (res.status == 401) {
    alert("Oops! You are not authorized.");
  }
}, function(e) {
  alert("Error submitting form!");
});
```

##### fetch()函数的参数和传给Request()构造函数的参数保持完全一致，所以你可以直接传任意复杂的request请求给fetch()。

#### 可以通过传body参数来设置Request的body：

```javascript
var form = new FormData(document.getElementById('data'));
fetch("/login", {
  method: "POST",
  body: form
})
```

## 实战

```javascript
    /**
     * @name GetAjax ajax请求插件+
     * @param url 请求接口
     * @param data 请求参数
     * @param type get或者post
     * @param async 同步或异步加载 true为异步 false为同步
     * @param callback 异步加载回调函数
     */
    
    export default function GetAjax(url, data, type, async, callback) {
        var url = url || '',        // 请求的url
            type = type || 'GET',   // 请求方式
            data = data || {},      // 请求参数
            async = async || true,  // 同步还是异步请求
            json = void 0;          // 返回的数据

        type = type.toUpperCase();  // 把字符串转换成大写
        if (self.fetch) {           // 判断浏览器是否支持fetch，若不支持则使用ajax
        
            // 判断是GET还是POST请求
            let Result = type == "GET" ? fetch("${url}?${urlParam(${data})}") : fetch(url, {
                method: 'POST',
                body: (() => {
                    var json = new FormData(); // 先创建一个空的 FormData，不知道什么是FormData的自行百度
                    for (let k in data) {
                        json.append(k, data[k]);
                    }
                    // 添加完数据后返回一个json
                    return json;
                })()
            });

            // 是时候获取数据了
            Result.then(function(response) {  // 先拿到整个fetch返回数据，取出你需要的json（json就是后台返给你的数据）
                return response.json()
            }).then(function(success) { // 如果请求成功，直接赋值回调
                //  请求成功后赋值
                json = success || [];
                //  请求成功后异步回调
                if (typeof(callback) === 'function') {
                    callback(json, true);
                }
            }).catch(function(ex) {  // 请求失败了，返回失败信息
                json = ex || [];
                //  请求成功后异步回调
                if (typeof(callback) === 'function') {
                    callback(json, false);
                }
            })
        } else {    // 以下为大家熟悉的ajax请求就不多说了
            $.ajax({
                //  请求配置
                url: url,
                type: type,
                data: data,
                async: async

            }).done(function(data) {
                //  请求成功后赋值
                json = data || [];
                //  请求成功后异步回调
                if (typeof(callback) === 'function') {
                    callback(json, true);
                }
                return false;

            }).fail(function(data) {
                json = data || [];
                //  请求成功后异步回调
                if (typeof(callback) === 'function') {
                    callback(json, false);
                }
                return false;
            });
        }
        //  同步返回值
        return json;
    }
```
![](http://ww3.sinaimg.cn/large/0060lm7Tgw1fbi5yzwzrhj30ds0470st.jpg)
![](http://ww3.sinaimg.cn/large/0060lm7Tgw1fbi5z06u6gj30d4050glw.jpg)


> 希望对大家有点帮助
