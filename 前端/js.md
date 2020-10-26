
**es6简化写法** 
``` javascript
let res = {
    code:200, 
    id:12,
    data:[{name:'Bob'}, {name:'Shuang Dan'}, {name:'HuaHua'}]
}
let {code,id, data} = res;
if (code==200) {
    ....
}

// console.log('hello'.indexOf('he') !== -1)
console.log('hello'.includes('he'))
console.log('hello'.startsWith('he'))
console.log('hello'.repeat(200)) // 重复多少次
```

**数组排序**
```
arr = [20,10, 2, 30, 100, 55]
arr.sort()  // [10, 100, 2, 20, 30, 55] 转为字符串排序
arr.sort((a, b)=>a-b) // [2, 10, 20, 30, 55, 100]  正序
arr.sort((a, b)=>b-a) // [100, 55, 30, 20, 10, 2] 倒序
```

**写js文件的高级格式**
```
;(function(){
    // Write your code
})()
```

**js中var， let， const的区别**
- 通俗讲const只管自己本身的事， let只管自己区域内的事，var什么事都管，可以覆盖前面的声明的变量。

- 块级作用域，var没有块级作用域，let可以定义块级作用域变量
```
{ 
  var i = 9;
} 
console.log(i);  // 9
__________________分隔符____________________
{ 
  let i = 9;     // i变量只在 花括号内有效！！！
} 
console.log(i);  // Uncaught ReferenceError: i is not defined
```
- var定义的变量 在 for循环中，每一个循环都是一个独立的整体，let变量 传到for循环作用域，不会受外界的影响。
```
for (var i = 0; i <10; i++) {  
  setTimeout(function() {  // 同步注册回调函数到 异步的 宏任务队列。
    console.log(i);        // 执行此代码时，同步代码for循环已经执行完成
  }, 0);
}
// 输出结果
10   共10个
// 这里面的知识点： JS的事件循环机制，setTimeout的机制等
```
var改为let 声明
```
// i虽然在全局作用域声明，但是在for循环体局部作用域中使用的时候，变量会被固定，不受外界干扰。
for (let i = 0; i < 10; i++) { 
  setTimeout(function() {
    console.log(i);    //  i 是循环体内局部作用域，不受外界影响。
  }, 0);
}
// 输出结果：
0  1  2  3  4  5  6  7  8 9
```

- let 声明变量后才能调用，否则报错，不能重复定义变量。

- 使用const声明的是常量，在后面出现的代码中不能再修改该常量的值。
