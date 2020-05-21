# fed-e-task-01-01

# 崔锐 | Part 1 | 模块一

## 简答题

### 第一题：请说出下列最终的执行结果，并解释为什么？

```jsx
var a = [];

for (var i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i);
  };
}
a[6]();
```

执行结果：10

原因：因为 i 是var声明的，成为了全局变量成员。所以当代码执行完后，i 就是10。所以数组 a中的每个element的打印结果都应该是 10。

### 第二题：请说出下列最终的执行结果，并解释为什么？

```jsx
var tmp = 123;

if (true) {
  console.log(tmp);
  let tmp;
}
```

执行结果：ReferenceError: tmp is not defined

原因：虽然tmp在全局被声明和赋值，但是在 if 的block块作用域里，用let 声明了一个 tmp，所以在这个 if 块作用域里的，tmp指的是这个let块内变量。而 let 声明语句写在输出语句后，当执行到输出语句，虽然块内发生了hoisting，但是 let 声明 的变量 tmp 处于死区，输出语句无法找到。所以 用let ，const声明 的变量要写在使用前。

### 第三题：结合 ES6 新语法，用最简单的方式找出数组中的最小值？

```jsx
var arr = [12, 34, 32, 89, 4];

Math.min(...arr)
```

### 第四题：请详细说明 var, let, const 三种声明变量的方式之间的具体差别？

ES2015前，没有block块作用域概念，只是global 和 local (function)变量，声明变量都用 var，所以var 的作用域是函数本地 或者全局。

ES2015后，加入了blcok作用域 概念，let 和const 就是快作用域变量。 

let 和const 一旦声明，不能 被再次声明。

另外，const 声明即赋值，否则之后不能再赋值。const 如果是赋的值是primitive类型，值也不能被改变；其他的类型，内部属性可以改变。

### 第五题：请说出下列最终的执行结果，并解释为什么？

```jsx
var a = 10;
var obj = {
  a: 20,
  fn () {
    setTimeout(() => {
      console.log(this.a)
    })
  }
}

obj.fn();
```

执行结果：20

原因：setTimeout 内部 的函数一个 arrow function，this 指向最近的函数的this指向，就是obj，所以能读到a的值是20。箭头函数的this 的指向是上下文里对象的this指向。

### 第六题：简述 Symbol 类型的用途

Symbol 是ES2015新增的数据类型。最主要的作用就是为对象添加独一无二的属性名。Symbol 不能被枚举，不能隐式转换，不能数学运算，不能字符串拼接等。其中的取得 同一 Symbol.for("描述")，可以得到同一符号。

```jsx
const s1 = Symbol();
// 可作为对象属性名
const obj = {
	[s1]: 'test'
}
const s2 = Symbol.for("123");
const s3 = Symbol.for("123");
console.log(s2 === s3);   // true
```

### 第七题：说说什么是浅拷贝，什么是深拷贝？

浅拷贝 ：（赋值操作）将原对象或数组的引用（指针）直接赋给新对象或数组，比如 对象 A，和对象B，B要拷贝A，那么A和B都指向同一个引用也就是A的地址，如果A改变，B也改变，如果B改变，A也改变。

深拷贝：将原对象的各个属性的 值逐个复制，而且将属性所包含的对象也一次采用深拷贝的方法递归复制到 新对象上 。这里拷贝的是值，复制的是对象本身。如果数据结构复杂，层级比较深，一些深拷贝方法只会做到不完全深拷贝，比如Array的slice,concat,from方法，和Object的 assign方法。

### 第八题 ：谈谈你是如何理解JS异步编程的，Event Loop 是做什么的，什么是宏任务，什么是微任务？

JS异步：JavaScript本身是同步的，当加入一些API后，可以实现异步。比如 在浏览器环境中 ，当有ajax，dom请求，setTimeout等webAPI，这些任务进入了webAPI等待，一旦完成操作，将被加入callback  queue回调队列。这时候，event loop 会持续查看栈与回调队列，一旦栈执行完毕，如果回调队列有任务，那么event loop就会触发调用。

Event Loop：持续查看 stack 和callback queue中是否有 未执行的任务。

宏任务：回调队列中的任务。执行过程中可以临时加上一些额外需求，这样可以选择作为一个新的宏任务进到队列中排队。目前绝大多数异步调用都是作为宏任务执行。

微任务：直接再当前任务结束后立即执行。Promise & MutationObserver, 和 node里process.nextTick

### 第九题：将下面异步代码使用 Promise 改进？

```jsx
setTimeout(function() {
  var a = "hello";
  setTimeout(() => {
    var b = "lagou";
    setTimeout(() => {
      var c = "I love U";
      console.log(a + b + c);
    }, 10);
  }, 10);
}, 10)
```

解答：

```jsx
Promise.resolve()
  .then(() => {
    var a = "hello";
    return a;
  })
  .then(val => {
    var b = " lagou";
    return val + b
  })
  .then(val => {
    var c = " I love U";
    console.log(val + c)
  })
```

### 第十题：请简述 TypeScript 与  JavaScript 之间的关系？

TypeScript 是 JavaScript 的超集或扩展集，含有JavaScript的所有元素，可以载入JavaScript代码，扩展了JavaScript的语法。

TypeScript 是前端领域中的第二语言。小项目选JavaScript本身，大项目建议 TypeScript。

### 第十一题：请谈谈你所认为的 TypeScript 优缺点？

TypeScript，像babel，可以编译到最低ES3的代码，兼容性高好。

功能更为强大，生态 也更健全、更完善。微软自加的开发工具特别友好。

缺点一：语言本身多 了很多概念。接口，泛型，枚举，which 提高学习成本。TypeScript 属于【渐进式】

缺点二：项目初期，TypeScript 会增加一些成本。（很多声明需要单独编写）