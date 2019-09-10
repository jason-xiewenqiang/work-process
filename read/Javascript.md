# 读《高性能Javascript》
### 第一章
###### 内容总结
+ 管理浏览器中的javascript代码是一件棘手的问题，因为代码执行过程会阻塞浏览器其他进程，比如用户界面绘制，遇见script标签，页面必须停下来等待代码下载并执行，然后才进行其他部分执行。所以总结几个方案来减少它对性能的影响
1. </body>闭合之前，将所有的script标签放到页面底部，这样能确保在脚本执行前，页面已经完成了渲染
2. 合并脚本，页面中的javascirpt标签越少，加载的速度就越快，响应速度也就越快。无论外链文件还是内嵌脚本都是如此
3. 有多种无阻塞下载脚本的方式：
   1. 使用<script>标签的defer属性 
   2. 使用动态创建的script脚本下载并执行代码
   3. 使用XHR对象下载javascript代码并注入页面中
    
### 第二章
###### 内容总结（理解原型与原型链，作用域与作用域链，闭包等概念有助于阅读理解这些操作）
+ 在javascript中，数据存储的位置会对代码整体的性能产生极大的影响。数据储存共有4种方式：字面量、变量、数组项、对象成员，他们都有各自的特点：
1. 访问字面量和局部变量的速度最快，而访问数组元素和对象成员相对比较慢
2. 由于局部变量存在于作用域的起始位置，因此访问局部变量会比访问跨作用域变量要快。变量在作用域链的位置越深，访问所需的时间就越长。由于全局变量总是处于作用域链的最末端，因此访问也是最慢的。
3. 避免使用with语句，因为它会改变执行作用域链，同样try-catch中的catch也会有同样的影响，因此使用要小心
4. 嵌套的对象成员会明显影响性能，因此要少使用。
5. 属性和方法在原型链中的位置越深，访问起来它的速度也越慢
6. 通常来说，你可以把常用的对象成员、数组元素、跨域变量保存在局部变量上来改善js性能，因为方法局部变量总是最快的
+ 对应的操作一般有哪些？
 1. 如果能使用字面量和局部对象，尽量使用
 2. 将一些常用的全局变量或数组元素保存为局部变量，进行使用
 3. with与eval不用，try-catch合理使用
 4. 管理你的闭包，在能保证使用合理的情况下使用
 5. 嵌套成员的使用合理化，比如window.location.href比location.href慢，使用后者
 6. 缓存对象属性，某个属性使用多次，那就进行缓存处理。
### 第三章
##### 内容总结
+ 访问和操作DOM是现代web应用的重要组成部分。但每次穿越连接ECMAScript和DOM两个岛屿之间的桥梁，都会被收过路费，为了减少DOM编程带来的性能损失，有以下几点：
 1. 最小化DOM访问次数，尽可能在js端处理。
 2. 如果需要多次访问某一个DOM节点，请使用局部变量储存它的引用。
 3. 小心处理HTML集合，因为他们实时连系着底层文档。把集合的长度缓存到一个变量中，并在迭代中使用它。如果需要经常操作集合，建议把它拷贝到一个数组上
 4. 如果可能的话，以最快的速度API访问，比如querySelecttorAll() 和 firstElementChild。
 5. 要留意重绘和重排，批量修改样式时，“离线”操作DOM树，使用缓存，并减少访问布局信息的次数。
 6. 动画中使用绝对定位。
 7. 使用事件委托来减少事件处理器的数量。
 8. 重绘和重排（面试点）
    1. 什么是重绘和重排：
       
    ```
      重排与重绘概念：
      当DOM的变化影响了元素的几何属性（宽高），比如改变边框宽度或者给段落增加文字，导致行数增加，
      浏览器需要重新计算元素的几何属性，同样其他元素的几何属性和位置都受到影响。
      浏览器会使渲染树上受到影响的部分失效，并重新构造渲染树，这个过程称为重排（reflow）。
      重排完成后，浏览器会重新绘制收到影响的部分到屏幕中，这个过程叫重绘（repaint）。
    ```
    2. 下面几个情况会出现重排
    ```
        A、添加或删除可见的DOM元素
        B、元素位置改变
        C、元素尺寸改变（包括内边距、外边距、边框宽度、厚度、高度、宽度等）
        D、内容发生变化
        E、页面渲染器初始化
        F、浏览器窗口尺寸发生变化
        *** 有些改变会触发整个页面重排：比如滚动条出现时 ***
    ```
    
### 第四章
##### 内容总结
+ Javascript和其他编程语言一样，代码的写法和算法会影响运行时间。与其他的语言不同的是，Javascript可用资源有限，因此优化技术更为重要
+ for、while和do-while循环性能相当，并没有一种循环类型明显快于或慢于其他类型
+ 避免使用for-in循环除非你需要遍历一个属性数量未知的对象
+ 改善循环的性能的最佳方式是减少每次迭代的运算和减少循环的迭代次数
+ 通常来说，switch总是比if-else快，但并不总是最佳解决方案
+ 在判断条件较多时，查找表比if-else和switch会更快
+ 浏览器的调用栈大小限制了递归算法哎javascript中的应用，栈溢出错误会导致其他代码中断运行
+ 如果你遇到栈溢出错误，可将方案改为迭代算法，或应用Memoization来避免重复计算

### 第五章
##### 内容总结
+ 密集的字符串操作和草率地编写正则表达式可能会产生严重的性能障碍，针对这些问题，有以下相应的措施解决
+ 当连接数量巨大或尺寸巨大的字符串时，数组合并是唯一在IE7以及更早版本中性能合理的方法
+ 如果不需要考虑IE7以及更早的版本性能，数组项合并是最慢的字符串连接方法之一，推荐使用简单的+和+=操作符代替，避免不必要的的中间字符串
+ 回溯既是正则表达式匹配功能的基本组成部分，也是正则表达式的低效来源
+ 基本地方，但是因为某些特殊的字符串匹配动作导致运行缓慢甚至浏览器崩溃，避免这个问题的办法是：使相邻的字元互斥，避免嵌套量词对同一字符串的相同部分多次匹配，通过重复利用预查的原子组去除不必要的回溯
+ 提高正则表达式效率的各种技术手段会有助于正则表达式更快的匹配，并在非匹配位置上花更少时间。
+ 正则表达式并不总是完成工作的最佳工具，尤其是你只搜索字面字符串的时候。
+ 尽管有许多方法可以去除字符串的首尾空白，但使两个简单的正则表达式（一个来去除头部空白，另一个用来去除尾部空白）来处理大量字符串内容提供一个简洁而跨浏览器的方法。从字符串末尾开始循环向前搜索第一个非空白字符，或者将此技术同正则表达式结合起来，会提供一个更好的替代方案，他很少受到字符串长度的影响。

### 第六章
##### 快速相应用户界面
```
    // Splitting Up Tasks
    /**
     * @param { steps, args, callback }
     * steps --> tasks array  args ---> mission needed  callback                           
     */
    function mulitstep(steps, args, callback) {
        var tasks = steps.cocat(); // 克隆数组
        setTimeout(function() {
            var task = tasks.shift();
            task.apply(null, args || []);
            
            // 检查是否还有其他任务
            if (tasks.lenght > 0) {
                setTimeout(argument.callee, 25);
            } else {
                callback();
            }
        }, 25)
    }

```
+ JavaScript和用户界面更新在同一个进程中运行，因此一次只能处理一件事情。这意味着当JavaScript代码运行时，用户界面不能响应输入，反之亦然。高效的管理UI线程就是要确保Javascript不能运行太长时间，以免影响用户体验。
+ 任何javascript任务都不应当执行超过100毫秒，过长的运行时间会导致UI更新出现明显的延迟，从而对用户的体验产生负面影响。
+ javascript运行期间，浏览器响应用户的行为存在差异，无论如何，javascript长时间运行将导致用户体验变得混乱和脱节。
+ 定时器可用来安排代码延迟执行，它使得你可以把长时间运行脚本分解承诺一系列的小任务，
+ web worker是新版浏览器支持的特性，它允许你在UI线程外部窒息感javascript代码，从而避免锁定UI。
+ Web应用越复杂，积极主动地管理UI线程就越重要，即使javascript代码再重要，也不能影响用户体验。
```

    /**
     * web worker 运行环境
     * 组成部分：TODO: ---->
     * 
     */
     
     
    /**
     * web worker 运行环境
     * 组成部分：
     * 
     * 一个 navigation 对象，只包含四个属性：appName,appVersion,userAgent,platform
     * 一个 location 对象（与window.location相同，同样属性都是只读的）
     * 一个 self 对象 指向全局的worker对象
     * 一个 importScript对象，用来加载worker所用到的外部JavaScript文件
     * 所有的ECMAScript对象
     * XMLHttpRequest对象
     * setInterval与setTimeout
     * 一个 close方法
     * 
     */
    
     // 与worker通信
     var worker = new Worker('code.js');
     worker.onmessage = function (event) {
        alert(event.data);
     }
    
     worker.postMessage('Jack');
    
     // code.js 内部
    
     self.onmessage = function(event) {
        self.postMessage('hello ' + event.data + ' !');
     }
    
     // 与worker通信的唯一方式就是 基于这样的消息系统
    
     // 加载外部文件 importScript 的方法加载外部文件
     importScripts('file.js');
     self.onmessage = function(event) {
        self.postMessage('hello ' + event.data + ' !');
     }
    
     // worker 实际应用 解析大容量的JSON
    
     var worker = new Worker('bigJson.js');
    
     worker.onmessage = function(event) {
         var jsonData = event.data;
         evaluateData(jsonData);
     }
    
     worker.postMessage(jsonText);
    
     // 在jsonParser 内部
    
     self.onmessage = function() {
         var jsonText = event.data;
         // 解析json
         var data = JSON.parse(jsonText);
    
         // 回传
         self.postMessage(data);
     }
    
     // 应用场景
     // A: 编码。解码大字符串
     // B: 复杂的数学运算（图像或者视频处理）
     // C: 大数组排序
     


```

    
