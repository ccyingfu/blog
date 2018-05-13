# 关于 Object.defineProperty

###

Object.defineProperty 是 ES5 最重要的特性之一。目前在项目中的应用，主要是用于数据拦截。

> ### 用法一：设置私有变量。
> 
> ###
> 
> For Example：
> 
>     var person = {};
>     Object.defineProperty(person, "name", {
>         value: "lee"
>     });
>     person.name = "wang";
>     console.log(person.name); // lee
> 


> ### 用法二：属性拦截。
> 
> ###
> 
> 曾经在一个项目里用来获取地址栏参数 
> 
> 比如：获取 https://a.com?key=value&name=lee location.search 中每个键对应的值。
> 
> For Example：
> 
>     function getSearchObj(){
>         var searchValue = document.location.search.substring(1);
>         if(!searchValue) return [];
>         var arr = searchValue.split("&");
>         var obj = {};
>         arr.forEach(item => {
>             var kvarr = item.split("=");
>             obj[kvarr[0]] = kvarr[1];
>         });
>         return obj;
>     }
>
>    var URL = {};
>    Object.defineProperty(URL, "search", {
>        value: getSearchObj()
>    });
>    console.log(URL.search.key);  // value
>    console.log(URL.search.name); // lee
>    console.log(URL.search.nothing); // undefined

