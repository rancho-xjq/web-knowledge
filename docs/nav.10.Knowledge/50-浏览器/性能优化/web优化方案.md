web优化策略

1. 提高FCP（First Contenful Paint），也就是屏幕上绘制单个像素。
字体通常是需要一段时间才能加载的大文件。一些浏览器隐藏文本直到字体加载，导致不可见文本的闪光(FOIT),可以通过以下font-display: swap;设置去避免FOIT，设置之后，在字体文件还没下载完之前，网页会先使用系统自带的字体，在字体下载完之后再去使用相应的字体。
例如：
@font-face {
  font-family: 'Pacifico';
  font-style: normal;
  font-weight: 400;
  src: local('Pacifico Regular'), local('Pacifico-Regular'), url(https://fonts.gstatic.com/s/pacifico/v12/FwZY7-Qmy14u9lezJ-6H6MmBp0u-.woff2) format('woff2');
  font-display: swap;
}

另外可以使用以下标签，预下载字体文件
 <link rel="preload"> 


2. 减少样式计算的范围和复杂性
减少选择器的复杂性如：.box:nth-last-child(-n+1) .title 这种，以及减少选择器范围如：直接写标签名 a span之类的。尽可能使用BEM结构的类名代替

3. 使用flex布局，而不是display:table

4. 避免强制同步布局，即在读取样式之前去修改样式
例如：
bad
```js
function logBoxHeight() {

  box.classList.add('super-big');

  // Gets the height of the box in pixels
  // and logs it out.
  console.log(box.offsetHeight); 
}
```

good
```js
function logBoxHeight() {
  // Gets the height of the box in pixels
  // and logs it out.
  console.log(box.offsetHeight);

  box.classList.add('super-big');
}
```

5. 避免布局颠簸
bad
```js
function resizeAllParagraphsToMatchBlockWidth() {

  // Puts the browser into a read-write-read-write cycle.
  for (var i = 0; i < paragraphs.length; i++) {
    paragraphs[i].style.width = box.offsetWidth + 'px';
  }
}
```
good
```js
var width = box.offsetWidth;
function resizeAllParagraphsToMatchBlockWidth() {
  for (var i = 0; i < paragraphs.length; i++) {
    // Now write.
    paragraphs[i].style.width = width + 'px';
  }
}
```

6.  使用transform进行元素的平移 旋转 放大 缩小 倾斜 matrix，减少布局重排（回流），同时可以对要动画的元素使用will-change或促进移动元素translateZ，这样渲染的时候，元素渲染能单独分层。
例如：
.moving-element {
  will-change: transform;
}
.moving-element {
  transform: translateZ(0);
}
但是要避免过度使用，以免创建太多的层，因为每一层都需要内存和管理
* {
  will-change: transform;
  transform: translateZ(0);
}

7. DOM 优化原理与基本实践
  DOM 为什么这么慢
    JS 是很快的，在 JS 中修改 DOM 对象也是很快的。在JS的世界里，一切是简单的、迅速的。但 DOM 操作并非 JS 一个人的独舞，而是两个模块之间的协作。
    当我们用 JS 去操作 DOM 时，本质上是 JS 引擎和渲染引擎之间进行了“跨界交流”。这个“跨界交流”的实现并不简单，它依赖了桥接接口作为“桥梁”
    过“桥”要收费——这个开销本身就是不可忽略的。我们每操作一次 DOM（不管是为了修改还是仅仅为了访问其值），都要过一次“桥”。过“桥”的次数一多，就会产生比较明显的性能问题。因此“减少 DOM 操作”的建议，并非空穴来风。

  过桥很慢，到了桥对岸，我们的更改操作带来的结果也很慢。
  很多时候，我们对 DOM 的操作都不会局限于访问，而是为了修改它。当我们对 DOM 的修改会引发它外观（样式）上的改变时，就会触发回流或重绘。本质上还是因为我们对 DOM 的修改触发了渲染树（Render Tree）的变化所导致

  回流：当我们对 DOM 的修改引发了 DOM 几何尺寸的变化（比如修改元素的 width、height、padding、margin、left、top、border 等等）时，浏览器需要重新计算元素的几何属性（其他元素的几何属性和位置也会因此受到影响），然后再将计算的结果绘制出来。这个过程就是回流（也叫重排）。当你要用到像这样的属性：offsetTop、offsetLeft、 offsetWidth、offsetHeight、scrollTop、scrollLeft、scrollWidth、scrollHeight、clientTop、clientLeft、clientWidth、clientHeight 时，你就要注意了！这些值有一个共性，就是需要通过即时计算得到。因此浏览器为了获取这些值，也会进行回流。除此之外，当我们调用了 getComputedStyle 方法，或者 IE 里的 currentStyle 时，也会触发回流。原理是一样的，都为求一个“即时性”和“准确性”。

  重绘：当我们对 DOM 的修改导致了样式的变化、却并未影响其几何属性（比如修改了颜色或背景色）时，浏览器不需重新计算元素的几何属性、直接为该元素绘制新的样式（跳过了上图所示的回流环节）。这个过程叫做重绘。

  DOM操作优化
    1.减少dom操作
      bad
      ```js
      
      for(var count=0;count<10000;count++){ 
        document.getElementById('container').innerHTML+='<span>我是一个小测试</span>'
      }
      ```
      good
      ```js
        减少过路费
        // 只获取一次container
        let container = document.getElementById('container')
        for(let count=0;count<10000;count++){ 
          container.innerHTML += '<span>我是一个小测试</span>'
        } 
        减少不必要的dom更改
        let container = document.getElementById('container')
        let content = ''
        for(let count=0;count<10000;count++){ 
          // 先对内容进行操作
          content += '<span>我是一个小测试</span>'
        } 
        // 内容处理好了,最后再触发DOM的更改
        container.innerHTML = content
      ```
    2.在DOM Fragment 中去操作dom
    DocumentFragment 接口表示一个没有父级文件的最小文档对象。它被当做一个轻量版的 Document 使用，用于存储已排好版的或尚未打理好格式的XML片段。因为 DocumentFragment 不是真实 DOM 树的一部分，它的变化不会引起 DOM 树的重新渲染的操作（reflow），且不会导致性能等问题。
打开Chrome DevTools，按Esc键打开Rendering面板，选择Show paint rectangles，如果是比较新的谷歌浏览器则选中Pain flashing，根据屏幕闪烁绿色查看绘制情况。

    ```js
      let container = document.getElementById('container')
      // 创建一个DOM Fragment对象作为容器
      let content = document.createDocumentFragment()
      for(let count=0;count<10000;count++){
        // span此时可以通过DOM API去创建
        let oSpan = document.createElement("span")
        oSpan.innerHTML = '我是一个小测试'
        // 像操作真实DOM一样操作DOM Fragment对象
        content.appendChild(oSpan)
      }
      // 内容处理好了,最后再触发真实DOM的更改
      container.appendChild(content)
    ```
  我们运行这段代码，可以得到与前面两种写法相同的运行结果。
  可以看出，DOM Fragment 对象允许我们像操作真实 DOM 一样去调用各种各样的 DOM API，我们的代码质量因此得到了保证。并且它的身份也非常纯粹：当我们试图将其 append 进真实 DOM 时，它会在乖乖交出自身缓存的所有后代节点后全身而退，完美地完成一个容器的使命，而不会出现在真实的 DOM 结构中。这种结构化、干净利落的特性，使得 DOM Fragment 作为经典的性能优化手段大受欢迎，这一点在 jQuery、Vue 等优秀前端框架的源码中均有体现。

  3.规避回流与重绘 

    a. 缓存敏感属性
      bad
      ```js
         // 获取el元素
        const el = document.getElementById('el')
        // 这里循环判定比较简单，实际中或许会拓展出比较复杂的判定需求
        for(let i=0;i<10;i++) {
            el.style.top  = el.offsetTop  + 10 + "px";
            el.style.left = el.offsetLeft + 10 + "px";
        }
      ```

      good
      ```js
        // 缓存offsetLeft与offsetTop的值
        const el = document.getElementById('el') 
        let offLeft = el.offsetLeft, offTop = el.offsetTop

        // 在JS层面进行计算
        for(let i=0;i<10;i++) {
          offLeft += 10
          offTop  += 10
        }

        // 一次性将计算结果应用到DOM上
        el.style.left = offLeft + "px" 
        el.style.top = offTop  + "px"
      ```

    b.避免逐条改变样式，使用类名去合并样式
      bad
      ```js
        const container = document.getElementById('container')
        container.style.width = '100px'
        container.style.height = '200px'
        container.style.border = '10px solid red'
        container.style.color = 'red'
      ```

      good
      ```css
        .basic_style {
          width: 100px;
          height: 200px;
          border: 10px solid red;
          color: red;
        }
      ```
      ```js
        const container = document.getElementById('container')
        container.classList.add('basic_style')
      ```
    
    c.将 DOM “离线”
    回流和重绘，都是在“该元素位于页面上”的前提下会发生的。一旦我们给元素设置 display: none，将其从页面上“拿掉”，那么我们的后续操作，将无法触发回流与重绘——这个将元素“拿掉”的操作，就叫做 DOM 离线化。
     bad
      ```js
        // 获取el元素
        const container = document.getElementById('container')
        container.style.width = '100px'
        container.style.height = '200px'
        container.style.border = '10px solid red'
        container.style.color = 'red'
        ...（省略了许多类似的后续操作）
      ```

      good
      ```js
        let container = document.getElementById('container')
        container.style.display = 'none'
        container.style.width = '100px'
        container.style.height = '200px'
        container.style.border = '10px solid red'
        container.style.color = 'red'
        ...（省略了许多类似的后续操作）
        container.style.display = 'block'
      ```
    拿掉一个元素再把它放回去，这不也会触发一次昂贵的回流吗？这话不假，但我们把它拿下来了，后续不管我操作这个元素多少次，每一步的操作成本都会非常低。当我们只需要进行很少的 DOM 操作时，DOM 离线化的优越性确实不太明显。一旦操作频繁起来，这“拿掉”和“放回”的开销都将会是非常值得的。

8. 提取首屏关键的css
浏览器必须先下载并解析CSS文件，然后才能显示页面，这使CSS成为了阻止渲染的资源。如果CSS文件很大，或者网络条件很差，则对CSS文件的请求可能会大大增加渲染网页的时间。所以可以使用内联css，把css直接写在html的style标签上，但是内联大量CSS，则会延迟其余HTML文档的传输。如果一切都优先，那么什么都不是。内联也有一些缺点，因为它阻止了浏览器将CSS缓存以在以后的页面加载中重复使用。因此比较好的方案是把首屏渲染的关键css内联，对于剩余部分的 CSS，我们可以使用异步的方式进行加载以及CDN缓存CSS。

选择以下工具其中一个，工程化的去提取关键css
https://github.com/addyosmani/critical
https://github.com/anthonygore/html-critical-webpack-plugin
https://github.com/filamentgroup/criticalCSS
https://github.com/pocketjoso/penthouse
webpack插件与使用
https://github.com/anthonygore/html-critical-webpack-plugin
https://vuejsdevelopers.com/2017/07/24/critical-css-webpack/

使用以下代码去异步的方式进行加载非关键css
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="styles.css"></noscript>

onload="this.onload=null;this.rel='stylesheet'" 资源加载完成之后清空onload函数，防止重复调用并重写rel

9. 查看当前页面总共使用的内存
在url输入chrome://flags 
Experimental Web Platform features 选择Enabled开启试验性api
执行代码查看内存
async function run() {
  const result = await performance.measureMemory();
  console.log(result);
}
run();

10. 按需动态加载js
原来：
```js
import moduleA from "library";

form.addEventListener("submit", e => {
  e.preventDefault();
  someFunction();
});

const someFunction = () => {
  // uses moduleA
}
```

按需动态加载：
```js
form.addEventListener("submit", e => {
  e.preventDefault();
  import('library.moduleA')
    .then(module => module.default) // using the default export
    .then(someFunction())
    .catch(handleError());
});

const someFunction = () => {
    // uses moduleA
}
```

使用aync或者defer来异步加载js文件
  <script async src="index.js"></script>
  async 模式下，JS 不会阻塞浏览器做任何其它的事情。它的加载是异步的，当它加载结束，JS 脚本会立即执行

  <script defer src="index.js"></script>
  defer 模式下，JS 的加载是异步的，执行是被推迟的。等整个文档解析完成、DOMContentLoaded 事件即将被触发时，被标记了 defer 的 JS 文件才会开始依次执行。
  从应用的角度来说，一般当我们的脚本与 DOM 元素和其它脚本之间的依赖关系不强时，我们会选用 async；当脚本依赖于 DOM 元素和其它脚本的执行结果时，我们会选用 defer。
11. 删除第三方库中未使用的代码
参考插件https://github.com/GoogleChromeLabs/webpack-libs-optimizations

12. 网页图片进行懒加载
很多时候，网页打开的时候，并不需要展示视口之外的图片，因此我们可以做图片懒加载优化，只有图片出现在视口之后才加载显示图片。推荐使用库[lazysizes](https://github.com/aFarkas/lazysizes)

13. 图片优化
  图片格式的选择
    JPEG/JPG：
      特点：有损压缩、体积小、加载快、不支持透明
      应用场景：在我们日常开发中，JPG 图片经常作为大的背景图、轮播图或 Banner 图出现
      缺陷：有损压缩在上文所展示的轮播图上确实很难露出马脚，但当它处理矢量图形和 Logo 等线条感较强、颜色对比强烈的图像时，人为压缩导致的图片模糊会相当明显。
    PNG-8 与 PNG-24：
      特点：无损压缩、质量高、体积大、支持透明
      应用场景：复杂的、色彩层次丰富的图片、小的 Logo、颜色简单且对比强烈的图片或背景等
    SVG
        特点：文本文件、体积小、图片可无限放大而不失真、兼容性好、1 张 SVG 足以适配 n 种分辨率
        缺陷：SVG 的局限性主要有两个方面，一方面是它的渲染成本比较高，这点对性能来说是很不利的。另一方面，SVG 存在着其它图片格式所没有的学习成本（它是可编程的）。
        使用方式与应用场景：
          将 SVG 写入 HTML：
          ```html
          <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <title></title>
            </head>
            <body>
                <svg xmlns="http://www.w3.org/2000/svg"   width="200" height="200">
                    <circle cx="50" cy="50" r="50" />
                </svg>
            </body>
          </html>
          ```
          将 SVG 写入独立文件后引入 HTML:
          ``` html
            <img src="文件名.svg" alt="">
          ```
    Base64
      特点：文本文件、依赖编码、小图标解决方案
      缺陷与使用场景：
        Base64 编码后，图片大小会膨胀为原文件的 4/3（这是由 Base64 的编码原理决定的）。如果我们把大图也编码到 HTML 或 CSS 文件中，后者的体积会明显增加，即便我们减少了 HTTP 请求，也无法弥补这庞大的体积带来的性能开销，得不偿失。
        在传输非常小的图片的时候，Base64 带来的文件体积膨胀、以及浏览器解析 Base64 的时间开销，与它节省掉的 HTTP 请求开销相比，可以忽略不计，这时候才能真正体现出它在性能方面的优势。

        因此，Base64 并非万全之策，我们往往在一张图片满足以下条件时会对它应用 Base64 编码：
          图片的实际尺寸很小（大家可以观察一下掘金页面的 Base64 图，几乎没有超过 2kb 的）
          图片无法以雪碧图的形式与其它小图结合（合成雪碧图仍是主要的减少 HTTP 请求的途径，Base64 是雪碧图的补充）
          图片的更新频率非常低（不需我们重复编码和修改文件内容，维护成本较低）

    WebP
      特点： Google 专为 Web 开发的一种旨在加快图片加载速度的图片格式，它支持有损压缩和无损压缩。支持jpg png gif的特点
      缺点：浏览器兼容性
      如何判断是否支持webp？
        浏览器端
        服务器端
  1. 加载合适的图片尺寸，在网页中，加载合适的图片尺寸能减少网络流量的消耗。比如一个100px * 100px的头像缩略图，src指向的图片大小是999*999，明显就浪费了图片下载流量。
  优化方法如下：
  1. 使用img的srcset/sizes属性
  img 元素的 srcset 属性用于浏览器根据宽、高和像素密度来加载相应的图片资源。
  例如：
  <img src="small.jpg " srcset="big.jpg 1440w, middle.jpg 800w, small.jpg 1x" />
  表示浏览器宽度达到 800px 则加载 middle.jpg ，达到 1400px 则加载 big.jpg。注意：像素密度描述只对固定宽度图片有效

  img 元素的 size 属性给浏览器提供一个预估的图片显示宽度。
  <img src="images/gun.png" alt="img元素srcset属性浅析"
          srcset="images/bg_star.jpg 1200w, images/share.jpg 800w, images/gun.png 320w"
          sizes="(max-width: 320px) 300w, 1200w"/>
  表示浏览器视口为 320px 时图片宽度为 300px，其他情况为 1200px。
  值得注意的是，在实际浏览器中 img的srcset/sizes属性可能不太好，可以使用上述的懒加载库[lazysizes](https://github.com/aFarkas/lazysizes)来实现这两个标签对应的功能
  2. 图片处理，可以使用[sharp](https://github.com/lovell/sharp)调整图片大小。

  3. 图片压缩，可以使用[imagemin](https://github.com/imagemin/imagemin)

  4. 图片的综合处理(ImageMagick)[https://www.imagemagick.org/script/index.php]
  使用ImageMagick的®创建，编辑，撰写，或转换位图图像。它可以读取和写入各种格式（超过200种）的图像，包括PNG，JPEG，GIF，HEIC，TIFF，DPX，EXR，WebP，Postscript，PDF和SVG。ImageMagick可以调整图像大小，翻转，镜像，旋转，变形，剪切和变换图像，调整图像颜色，应用各种特殊效果或绘制文本，线条，多边形，椭圆和贝塞尔曲线。

  5. 使用图片CDN

  6. 使用webp格式图片
    可以使用(imagemin-webp)[https://github.com/imagemin/imagemin-webp]插件可以把图片转换为webp格式图片
    再使用兼容写法
    <picture>
      <source type="image/webp" srcset="flower.webp">
      <source type="image/jpeg" srcset="flower.jpg">
      <img src="flower.jpg" alt="">
    </picture>

  7. 对于图标图片是用icontfont或者svg格式
  [Replace complex icons with SVG](https://developers.google.cn/web/fundamentals/design-and-ux/responsive/images#replace_complex_icons_with_svg)

16. 使用video代替gif动画
可以使用(ffmpeg)[https://www.ffmpeg.org/]把gif转成video，对于视频还可以进一步转成webpm格式。
使用webpm格式的时候，可以使用以下兼容写法
<video autoplay loop muted playsinline>
  <source src="my-animation.webm" type="video/webm">
  <source src="my-animation.mp4" type="video/mp4">
</video>

17. 使用GZIP压缩资源（HTML、JS、CSS、Image等）

18. 预解析与预加载资源
  参考资料 [异步加载(defer、async、module)和预加载(preload、prefetch、dns-prefetch、preconnect 、prerender)](https://www.jianshu.com/p/14b4cbce5e27)
  对于关键资源，可以使用预加载

19. 避免页面多次重定向
  比如进入A页面之后，服务器重定向到B页面，然后B页面重定向到C页面

20. 对于服务器静态资源，使用CDN和浏览器缓存
 使用CDN增加资源地区访问速度
 使用浏览器缓存策略：强缓存或者协商缓存，进行资源的缓存

21. js、css标签在的位置
  把样式放在顶部<head>
    默认情况下，CSS 是阻塞的资源。浏览器在构建 CSSOM 的过程中，不会渲染任何已处理的内容。即便 DOM 已经解析完毕了，只要 CSSOM 不 OK，那么渲染这个事情就不 OK（这主要是为了避免没有 CSS 的 HTML 页面丑陋地“裸奔”在用户眼前）。
    我们知道，只有当我们开始解析 HTML 后、解析到 link 标签或者 style 标签时，CSS 才登场，CSSOM 的构建才开始。很多时候，DOM 不得不等待 CSSOM。因此我们可以这样总结：
      CSS 是阻塞渲染的资源。需要将它尽早、尽快地下载到客户端，以便缩短首次渲染的时间。

  把js脚本放到底部</body>
    JS 引擎是独立于渲染引擎存在的。我们的 JS 代码在文档的何处插入，就在何处执行。当 HTML 解析器遇到一个 script 标签时，它会暂停渲染过程，将控制权交给 JS 引擎。JS 引擎对内联的 JS 代码会直接执行，对外部 JS 文件还要先获取到脚本、再进行执行。等 JS 引擎运行完毕，浏览器又会把控制权还给渲染引擎，继续 CSSOM 和 DOM 的构建

22. 使用工具压缩css js
对于css的注释和js使用工具压缩
