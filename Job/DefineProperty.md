# 关于 Object.defineProperty

###

Object.defineProperty 是 ES5 最重要的特性之一。目前在项目中的应用，主要是用于数据拦截。

> ### 用法一：设置"私有"属性。  
> 虽然不是真的私有，但是可以提空一个场景，就是不能修改，防止被引用者修改。
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
>         if(!searchValue) return {};
>         var arr = searchValue.split("&");
>         var obj = {};
>         arr.forEach(item => {
>             var kvarr = item.split("=");
>             obj[kvarr[0]] = kvarr[1];
>         });
>         return obj;
>     }
>
>     var URL = {};
>     Object.defineProperty(URL, "search", {
>         value: getSearchObj()
>     });
>     console.log(URL.search.key);  // value
>     console.log(URL.search.name); // lee
>     console.log(URL.search.nothing); // undefined


> ### 用法三：模拟“类”。
> 
> ###
> 
> 参考 [fclass](https://github.com/ccyingfu/fclass)


> ### 用法四：MVVM 框架 Vue 中双向绑定的底层逻辑。
> 
> ###
> 
> 最简单的“数据-视图”双向绑定
> 
> For Example:
>
>     <!-- html -->
>     <input id="name" />
>   
>     <!-- js -->
>     var person = {};
>     var input = document.getElementById("name");
>     Object.defineProperty(person, "name", {
>         get: function(){
>             return input.value
>         },
>         set: function(newValue){
>             input.value = newValue;
>         }
>     });
>     input.addEventListener("keyup", function(e){
>         person.name = e.target.value;
>     }, false);
> 


