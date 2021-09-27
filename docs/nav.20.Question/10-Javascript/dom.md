1. children和childnotes区别
Node.children：返回指定节点的所有element子节点,即返回节点元素
Node.childNodes：返回指定节点的所有子节点，包括节点元素和文本元素

eg:
``` html
<ul id="juice">
	<li>Coffee</li>
	<li>Coke</li>
	<li>Orange Juice</li>
</ul>
```
```js
document.getElementById('juice').children; //长度为3
document.getElementById('juice').childNodes; //长度为7，ul中的每个换行都作为节点计算进去了
```
2. [h]获取元素的大小，获取元素的位置 offsetleft offsettop, clientHeight,clientWidth
参考：[用Javascript获取页面元素的位置](http://www.ruanyifeng.com/blog/2009/09/find_element_s_position_using_javascript.html)

1. [h]原生JS操作DOM的方法有哪些？
获取节点的方法getElementById、getElementsByClassName、getElementsByTagName、getElementsByName、querySelector、querySelectorAll
对元素属性进行操作的 getAttribute、setAttribute、removeAttribute方法
对节点进行增删改的appendChild、insertBefore、replaceChild、removeChild、createElement等

3. script 标签中 async 跟 defer 的区别
没有defer或async属性，浏览器会立即加载并执行相应的脚本。也就是说在渲染script标签之后的文档之前，不等待后续加载的文档元素，读到就开始加载和执行，此举会阻塞后续文档的加载；
- async:
<script async src="example.js"></script>
有了async属性，表示后续文档的加载和渲染与js脚本的加载和执行是并行进行的，即异步执行；

- defer
<script defer src="example.js"></script>
有了defer属性，加载后续文档的过程和js脚本的加载(此时仅加载不执行)是并行进行的(异步)，js脚本的执行需要等到文档所有元素解析完成之后，DOMContentLoaded事件触发执行之前

4. 请指出document.ready和onload的区别
document.ready方法在DOM树加载完成后就会执行，
而window.onload是在页面资源（比如图片和媒体资源，它们的加载速度远慢于DOM的加载速度）

5. 如何获取radio值
```js
var b1= document.getElementsByName('a1');
for (var i = 0; i < b1.length; i++) {
   if (b1[i].checked == true) {//如果选中，下面的alert就会弹出选中的值
      alert(b1[i].value);
   }
```


8. DOM事件中target和currentTarget的区别
target是事件触发的真实元素,currentTarget是事件绑定的元素.

事件处理函数中的this指向是中为currentTarget

currentTarget和target，有时候是同一个元素，有时候不是同一个元素 （因为事件冒泡）

当事件是子元素触发时，currentTarget为绑定事件的元素，target为子元素
当事件是元素自身触发时，currentTarget和target为同一个元素。

10. 如何判断是Button
 在点击事件里判断 e.target.nodeName.toUpperCase == "BUTTON"

11. 页面上有一个input，还有一个p标签，改变input后p标签就跟着变化，如何处理