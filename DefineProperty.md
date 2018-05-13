# 关于 Object.defineProperty

###

Object.defineProperty 是 ES5 最重要的特性之一。目前在项目中的应用，主要是用于数据拦截。

> ### 用法一：设置私有变量 。
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