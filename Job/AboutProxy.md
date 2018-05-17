# 关于 Proxy 与 Object.defineProperty 的对比

## 首先说一下 Object.defineProperty 的问题

Vue 的实践中，双向绑定是有漏洞的，就是对数组的监控。这并不是 Vue 的问题，而是 Object.defineProperty 本身的缺陷导致的。 
### For Example
```
var obj = {arr: []};
var arr1 = obj.arr;
Object.defineProperty(obj, "arr", {
  set: function(newValue){console.log("set");},
  get: function(){console.log("get"); return arr1},
  configurable: true,  enumerable: true
});
obj.arr[0] = 1; 
// get 
// 1 
// 这里并没有走 set 拦截，这里就说明了 Object.defineProperty 对数组的缺陷问题
obj.arr = [1, 2];
// set
// 只有改变整个对象的引用才会走进拦截
```

## Proxy 是未来
Proxy 是 es6 的标准对象，它不仅可以对数组内容进行监控，而且还提供了更多的拦截方式，如：apply、ownKeys、deleteProperty、has等等。  
Proxy 返回的是一个新对象，我们可以只操作新的对象达到目的，而Object.defineProperty只能遍历对象属性直接修改。  
### For Example
```
  const arr = [];
  const newArr = new Proxy(arr, {
    get: function(target,key,receiver){
      console.log("get")
      return Reflect.get(target,key,receiver);
    },
    set: function(target, key, value, receiver){
      console.log("set");
      return Reflect.set(target, key, value, receiver);
    }
  });
  newArr[0] = 1; // "set"
```

## Proxy 现阶段的问题
Proxy 作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利。  
当然，Proxy的劣势就是兼容性问题，而且无法用polyfill磨平，因此Vue的作者才声明需要等到下个大版本(3.0)才能用Proxy重写。
