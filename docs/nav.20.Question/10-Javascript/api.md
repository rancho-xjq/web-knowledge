2. [h]typeof操作符返回值有哪些，对undefined、null、NaN使用这个操作符分别返回什么
typeof的返回值有undefined、boolean、string、number、object、function、symbol。
对undefined使用返回undefined
null使用返回object
NaN使用返回number


3. [h]类型判断的方法有哪些？
typeof、instanceof 、 Object.prototype.toString()

4. [h]setTimeout和setInterval的区别，包含内存方面的分析？
setTimeout表示间隔一段时间之后执行一次调用，而setInterval则是每间隔一段时间循环调用，直至clearInterval结束。
内存方面，setTimeout只需要进入一次队列，不会造成内存溢出，setInterval因为不计算代码执行时间，有可能同时执行多次代码，
导致内存溢出。

参考：

[JS 中settimeout和setinterval函数的区别](https://my.oschina.net/u/3636678/blog/1499852)

[setTimeout() 和 setInterval() 本质区别在哪里？](https://segmentfault.com/q/1010000005989491)

5. [h]如何阻止事件冒泡和默认事件？
标准的DOM对象中可以使用事件对象的stopPropagation()方法来阻止事件冒泡，但在IE8以下中IE的事件对象通过设置事件对象的cancelBubble属性为true来阻止冒泡；
默认事件的话通过事件对象的preventDefault()方法来阻止，而IE通过设置事件对象的returnValue属性为false来阻止默认事件。


8. [h]原生js字符串方法有哪些？
- 获取类方法: 
    1. charAt方法用来获取指定位置的字符
    2. 获取指定位置字符的unicode编码的charCodeAt方法，
    3. 与之相反的fromCharCode方法，通过传入的unicode返回字符串。
- 查找类方法有indexof()、lastIndexOf()、search()、match()方法。
- 截取类的方法有substring、slice、substr三个方法，
substring和slice都是指定截取的首尾索引值，不同的是传递负值的时候
substring会当做0来处理，而slice传入负值的规则是-1指最后一个字符，substr方法则是第一个参数是开始截取的字符串，第二个是截取的字符数量，
和slice类似，传入负值也是从尾部算起的
- 其他的还有replace、split、toLowerCase、toUpperCase方法。


11. 正则表达式四个api

12. 手写reduce
```js
// MDN: Array.prototype.reduce
Array.prototype.myReduce = function (callback, initialValue) {
  if (this === null) {
    throw new TypeError('Array.prototype.reduce called on null or undefined')
  }
  if (typeof callback !== 'function') {
    throw new TypeError(`${callback} is not a function`)
  }
  const array = Object(this)
  const len = array.length >>> 0
  let index = 0
  let result
  // 处理初始值
  if (arguments.length > 1) {
    result = initialValue
  } else {
    // example: [,,,,5]
    while(index < len && !(index in array)) {
      index++
    }
    if (index >= len) {
      throw new TypeError('Reduce of empty array with no initial value')
    }
    value = array[index++]
  }
  while (index < len) {
    if (index in array) {
      result = callback(result, array[index], index, array)
    }
    index++
  }
  return result
}
const array = [1, , 2, 3, , , 5]
const myResult = array.myReduce((acc, cur) => acc + cur, 0)
const originResult = array.reduce((acc, cur) => acc + cur, 0)
console.log(myResult)     // 11
console.log(originResult) // 11
```
12. [h]手写bind
```js
Function.prototype.myBind = function(context) {
  if(typeof this !== 'function') {
    throw TypeError('error');
  }
  const self = this;
  const args = [...arguments].slice(1);
  return function F() {
    if(this instanceof F) {
      return new self(...args, ...arguments);
    }
    return self.apply(context, args.concat(...arguments));
  }
}
function foo() {
  console.log(this.age);
}
var obj = {
  age: 121
}
var newFunc = foo.myBind(obj);
newFunc(); // 输出121
```
参考： [手写call、apply和bind方法](http://wangtunan.gitee.io/blog/interview/#手写call、apply和bind方法)

13. reqeustAnimationFrame有用过没，是如何使用的？就是递归调用呗。她是属于微任务还是宏任务？

14. async/await和promise性能差异


14. 手写extend
```js
function inherit (child, parent) {
  // 1.继承父类原型上的属性
  child.prototype = Object.create(parent.prototype)
  // 2.修复子类的构造函数
  child.prototype.constructor = child
  // 3.存储父类
  child.super = parent
  // 4.继承静态属性
  if (Object.setPrototypeOf) {
    Object.setPrototypeOf(child, parent)
  } else if (child.__proto__) {
    child.__proto__ = parent
  } else {
    for (const key in parent) {
      if (parent.hasOwnProperty(k) && !(k in child)) {
        child[key] = parent[key]
      }
    }
  }
}
// 父类
function Parent (name) {
  this.name = name
  this.parentColors = ['red']
}
Parent.prototype.sayName = function () {
  console.log(this.name)
}
Parent.create = function (name) {
  return new Parent(name)
}
// 子类
function Child (name) {
  this.name = name
  this.childColors = ['green']
}
// 继承
inherit(Child, Parent)

// test
const child1 = new Child('child1')
console.log(child1 instanceof Child)  // true
console.log(child1 instanceof Parent) // true
console.log(child1.name)              // child1
console.log(child1.childColors)       // ['green']
console.log(child1.parentColors)      // undefined

const child2 = Child.create('child2')
console.log(child2 instanceof Child)  // false
console.log(child2 instanceof Parent) // true
console.log(child2.name)              // child2
console.log(child2.childColors)       // undefined
console.log(child2.parentColors)      // ['red']
```
参考：[手写类extends关键词方法](http://wangtunan.gitee.io/blog/interview/#手写类extends关键词方法)

15. 数组的方法有哪些与区别

16. [h]call和apply的区别和应用场景

17. Set 和 Map 数据结构

18. WeakMap 和 Map 的区别?



20. 简单实现async/await中的async函数
async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里

function spawn(genF) {
    return new Promise(function(resolve, reject) {
        const gen = genF();
        function step(nextF) {
            let next;
            try {
                next = nextF();
            } catch (e) {
                return reject(e);
            }
            if (next.done) {
                return resolve(next.value);
            }
            Promise.resolve(next.value).then(
                function(v) {
                    step(function() {
                        return gen.next(v);
                    });
                },
                function(e) {
                    step(function() {
                        return gen.throw(e);
                    });
                }
            );
        }
        step(function() {
            return gen.next(undefined);
        });
    });
}

21. [h]es6 api升级内容
- 举一些ES6对String字符串类型做的常用升级优化?
- 举一些ES6对Array数组类型做的常用升级优化
- 举一些ES6对Number数字类型做的常用升级优化
- 举一些ES6对Object类型做的常用升级优化?(重要)
- 举一些ES6对Function函数类型做的常用升级优化?

22. Proxy是什么，有什么作用？
[Proxy](https://es6.ruanyifeng.com/#docs/proxy)

23. Iterator是什么，有什么作用？

24. Object.is() 与原来的比较操作符 ===、== 的区别？
Object.is就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。
```js
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
```
不同之处只有两个：一是+0不等于-0，二是NaN等于自身。
```js
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```
ES5 可以通过下面的代码，部署Object.is。
```js
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
```
25. 介绍JS有哪些内置对象？


27. [h]你一般情况下怎么判断数据类型。typeof去判断数据类型时返回值有哪些。typeof和instanceof的区别？
- 直接使用bject.property.toString.call
基本数据类型都能返回相应的类型。

Object.prototype.toString.call(999) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(Symbol()) // "[object Symbol]"
Object.prototype.toString.call(42n) // "[object BigInt]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(true) // "[object Boolean]
复杂数据类型也能返回相应的类型
Object.prototype.toString.call({a:1}) // "[object Object]"
Object.prototype.toString.call([1,2]) // "[object Array]"
Object.prototype.toString.call(new Date) // "[object Date]"
Object.prototype.toString.call(function(){}) // "[object Function]"

- typeof和instanceof的区别
在 JavaScript 中，判断一个变量的类型尝尝会用 typeof 运算符，在使用 typeof 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 “object”。
instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造 
函数的 prototype 属性。
语法：object instanceof constructor
参数：object（要检测的对象.）constructor（某个构造函数）
描述：instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。

参考：[JS数据类型之问——检测篇](http://47.98.159.95/my_blog/blogs/javascript/js-base/002.html#_2-instanceof能否判断基本数据类型)

31. for…in迭代和for…of有什么区别？

32. 说一下你对generator的了解？

33. [h]import和require的区别

34. [h]symbol

35. ==的隐式转化

36. requestAnimationFrame和 setTime、setInterval的区别，requestAnimationFrame 可以做什么

37. 如何判断object是数组类型？
alert(typeof 1);                // 返回字符串"number" 
alert(typeof "1");              // 返回字符串"string" 
alert(typeof true);             // 返回字符串"boolean" 
alert(typeof {});               // 返回字符串"object" 
alert(typeof []);               // 返回字符串"object " 
alert(typeof function(){});     // 返回字符串"function" 
alert(typeof null);             // 返回字符串"object" 
alert(typeof undefined);        // 返回字符串"undefined"
其中，typeof {}和typeof []的结果都是object，那么问题来了，我怎么通过typeof去判断一个对象是不是数组类型呢？

对象是对象，数组也是对象，js中万物皆对象，很显然，通过简单的typeof运算符是不能够达到目的，我们得换个方法。

1、从原型入手，Array.prototype.isPrototypeOf(obj);

利用isPrototypeOf()方法，判定Array是不是在obj的原型链中，如果是，则返回true,否则false。

2.Array.isArray()方法。

Array.isArray([1, 2, 3]);  // true
Array.isArray({foo: 123}); // false
Array.isArray('foobar');   // false
Array.isArray(undefined);  // false　

38. 已知如下对象，请基于es6的proxy方法设计一个属性拦截读取操作的例子，要求实现去访问目标对象example中不存在的属性时，抛出错误：Property “$(property)” does not exist
const man={
    name:'jscoder',
    age:22
}
 //补全代码
const proxy = new Proxy(...)
proxy.name //"jscoder"
proxy.age //22
proxy.location //Property "$(property)" does not exist
考点 es6 javascript的Proxy 实例的方法 ,get() get方法用于拦截某个属性的读取操作。

var man = {
    name:'jscoder',
    age:22
};
var proxy = new Proxy(man, {
    get: function(target, property) {
        if(property in target) {
            return target[property];
        } else {
            throw new ReferenceError(`Property ${property} does not exist.`);
        }
    }
});
console.log(proxy.name)
console.log(proxy.age)
console.log(proxy.location)
Proxy 实例的方法的其他方法参考这个链接，很详细 https://blog.csdn.net/qq_30100043/article/details/53443017

39. 写出下面会输出的值
if([]==false){console.log(1)};
if({}==false){console.log(2)};
if([]){console.log(3)}
if([1]==[1]){console.log(4)}

// 只输出1,3
这个是隐式转换，if([])直接调用blooean()方法，==号的转换套路要知道

40. tostring和valueof有什么区别

42. [h]ES6中的Map和Set

43. 取数组的最大值（ES5、ES6）

44. [h]some、every、find、filter、map、forEach有什么区别

45. 如何找0-5的随机数，95-99呢

46. setInterval需要注意的点

47. 介绍defineProperty方法，什么时候需要用到

48. typeof为什么对null错误的显示
这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象然而 null 表示为全零，所以将它错误的判断为 object 。

49. 详细说下instanceof
instanceof它主要是用于检测某个构造函数的原型对象在不在某个对象的原型链上。
算了，直接手写实现吧：
function myInstanceof (left, right) {
  let proto = Object.getPrototypeOf(left);
  while (true) {
    if (proto === null) return false;
    if (proto === right.prototype) return true;
    proto = Object.getPrototypeOf(proto)
  }
}

50. 介绍js有哪些内置对象

Object 是 JavaScript 中所有对象的父对象
数据封装类对象： Object 、 Array 、 Boolean 、 Number 和 String
其他对象： Function 、 Arguments 、 Math 、 Date 、 RegExp 、 Error

51. JS 原生拖拽节点

给需要拖拽的节点绑定 mousedown , mousemove , mouseup 事件
mousedown 事件触发后，开始拖拽
mousemove 时，需要通过 event.clientX 和 clientY 获取拖拽位置，并实时更新位置
mouseup 时，拖拽结束
需要注意浏览器边界值，设置拖拽范围
