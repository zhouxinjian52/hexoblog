---
title 一段代码让你直接看懂Promise
date 2017-01-08 162916
tags
- Promise
- 异步

categories  Promise
---

### Promise到底怎么用？？？

```javascript
创建一个Promise实例，获取数据。并把数据传递给处理函数resolve和reject。需要注意的是Promise在声明的时候就执行了。
var getUserInfo=new Promise(function(resolve,reject){
    $.ajax({
        typeget,
        url,
        successfunction(data){
            if(data.Status==1){
                resolve(data.ResultJson)在异步操作成功时调用
            }else{
                reject(data.ErrMsg);在异步操作失败时调用
            }
        }
    });
})
另一个ajax Promise对象，
var getDataList=new Promise(function(resolve,reject){
    $.ajax({
        typeget,
        urlindex.aspx,
        successfunction(data){
            if(data.Status==1){
                resolve(data.ResultJson)在异步操作成功时调用
            }else{
                reject(data.ErrMsg);在异步操作失败时调用
            }
        }
    });
})
Promise的方法then,catch方法
getUserInfo.then(function(ResultJson){
    通过拿到的数据渲染页面
}).catch(function(ErrMsg){
    获取数据失败时的处理逻辑
})
Promise的all方法，等数组中的所有promise对象都完成执行
Promise.all([getUserInfo,getDataList]).then(function([ResultJson1,ResultJson2]){
    这里写等这两个ajax都成功返回数据才执行的业务逻辑
})
```

> 不用我再过多解释了吧！！

