```

/**
 * 2015年，Jesse James Garrett 提出ajax的概念
 * 这一技术能够向服务器请求额外的数据而不用卸载页面，会带更好的用户体验。
 * Asynchronous JavaScript + XML 的简写
 * Ajax技术的核心是XMLHttpRequest对象，这是由微软首先引入的一个特性
 * 在XMR出现前，Ajax式的通信必须借助一些hack的手段来实现
 * 大多数是使用隐藏的框架或者内嵌的框架
 * IE5是首一款引入XMR对象的浏览器
 */

 // 适用IE7之前的版本
 function createXMR() {
     if (typeof arguments.callee.activeXString != 'string') {
         var versions = ['MSXML2.XMLHttp.6.0', 'MSXML2.XMLHttp.3.0', 'MSXML2.XMLHttp'],
         i,
         len;
         for (i=0,len=versions.length;i < len; i++ ) {
             try {
                 new ActiveXObject(versions[i]);
                 arguments.callee.activeXString = versions[i];
                 break;
             } catch (ex) {
                 // pass
             }
         }
     }
     return new ActiveXObject(arguments.callee.activeXString);
 }

 // IE7+、Firefox、Opera、Chrome、和Safari都支持原生XMLHttpRequest，所以使用 
 // var xhr = new XMLHttpRequest();就好啦

 // 升级下createXHR函数
 function createXMRTwo() {
     if (typeof XMLHttpRequest != 'undefined') {
         return new XMLHttpRequest();
     } else if (typeof ActiveXObject != 'undefined') {
        if (typeof arguments.callee.activeXString != 'string') {
            var versions = ['MSXML2.XMLHttp.6.0', 'MSXML2.XMLHttp.3.0', 'MSXML2.XMLHttp'],
            i,
            len;
            for (i = 0, len = versions.length;  i < len; i++ ) {
                try {
                    new ActiveXObject(versions[i]);
                    arguments.callee.activeXString = versions[i];
                    break;
                } catch (ex) {
                    // pass
                }
            }
        }
        return new ActiveXObject(arguments.callee.activeXString);
     } else {
        throw new Error('No XMR object available!');
     }
 }

 /**
  * XHR 到底怎么用呢？
  * 几个方法调用 open send abort etc.
  * 重要属性：responseText-作为响应主体被返回的文本
  *     responseXML: 如果响应的内容的类型是'text/html'或者'application/xml',那么这个属性会包含响应数据的xml dom 文档
  *     status: 响应的HTTP状态
  *     statusText：HTTP状态说明
  */

  // open 方法详解 
  // 接收三个参数：要发送的请求的类型{get,post,etc..}、请求的url、表示是否异步发送请求的布尔值
  var xhr = createXMRTwo(); // 采用增强版的对象创建方式
  xhr.open('get', 'example.php', false); // 第三个参数 true-表示发送异步请求 false-表示发送同步请求  

  // 解析： 1.url(统一资源定位符，描述一台特定服务器上某资源的特定位置)相当于执行代码的当前页面
  //       2.调用open()并不会真正发送请求，而只是启动了一个请求以备发送

  // send 登场 这个方法才是真正发送请求
  xhr.send(null);

  // 调用请求后，等待服务器响应后，响应的数据会自动填充到XHR对象中
  // 一般来说可以将http状态码为200作为成功的标志，此时responseText内容已准备就绪，可以访问了。

  if ((xhr.status >= 200 && xhr.status < 300) || xhr.state === 304) {
      alert(xhr.responseText);
  } else {
      alert('Request was unsuccessful: ' + xhr.state);
  }

  /**
   * 一般来说，我们发送请求一般都是异步，不可能都是同步请求，所以需要对xhr进行监控
   * 那么xhr对象的readyState属性就登场了！该属性表示了请求的响应过程的当前活动阶段
   * 属性的取值如下：
   *    0: 表示未初始化。尚未调用open方法
   *    1: 启动。已经调用open方法，但尚未调用send方法
   *    2: 发送。已经调用send方法，但未收到响应
   *    3: 接收。已经收到部分响应数据
   *    4: 完成。已经收到全部响应数据，而且已经可以在客户端使用了
   * 每次 readyState 值发生变化，都会触发一次 readystatechange事件，因此可以使用它检测变化后的readyState值
   * 通常，我们只对readyState的值为4的阶段感兴趣，因为返回了我们要的数据。
   * 不过，必须在调用open事件前指定onreadystatechange事件才能保持浏览器兼容(**怎么还有这么恶心的设定**)
   */

   var xhr = createXMRTwo();
   xhr.onreadystatechange = function () {
       if (xhr.readyState == 4) {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.state === 304) {
                alert(xhr.responseText);
            } else {
                alert('Request was unsuccessful: ' + xhr.state);
            }
       }
   };
   xhr.open('get', 'example.php', true); // 设定为异步请求
   xhr.send(null);

   // xhr 取消异步请求的方法  在没有收到响应是如果要取消请求
   xhr.abort();

   /**
    * Http头部信息
    * 
    * 默认情况下，在发送xhr时，还会发送以下头信息
    * Accept: 浏览器能够处理的内容类型
    * Accept-Charset: 浏览器能够处理的字符集
    * Accept-Encoding: 浏览器能够处理的压缩编码
    * Accept-Language: 浏览器当前设置语言
    * Cennection: 浏览器与服务器之间的类型
    * Cookie: 当前页面设置的任何cookie
    * Host: 发出请求页面所在的域
    * Referer: 发出请求的页面的URI
    * User-Agent: 浏览器的用户代理字符串
    * 不同的浏览器会发送不一样的头部信息
    * 使用setRequsetHeader() 去自定义请求的头部信息
    */
   var xhr = createXMRTwo();
   xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.state === 304) {
                alert(xhr.responseText);
            } else {
                alert('Request was unsuccessful: ' + xhr.state);
            }
        }
    };
    xhr.open('get', 'example.php', true); // 设定为异步请求
    /** 在这步设定 **/
    xhr.setRequestHeader('MyHeader', 'myValue');
    xhr.send(null);

    /**
     * 服务端响应后获取header信息
     * getResponseHeader('key') && getResponseHeader() 
     * 获取出来的值是多行的文本  
     */

     var myHeader = xhr.getResponseHeader('myHeader');
     var allHeader = xhr.getAllResponseHeaders();

     /**
      * GET POST 请求
      * XMLHttpRequest2.0
      * FormData
      * 超时调用 timeout 只是适用于IE8+
      */
     var data = new FormData();
     data.append('name', 'Nick Jason');

     xhr.send(data);
     xhr.timeout = 1000;

     /**
      * 进度事件
      * Progress Events 进度事件
      * API：
      *     loadstart - 在接收响应数据的第一个字段时触发
      *     progress - 在接收期间持续不断地触发
      *     error - 在请求发生时错误时触发
      *     abort - 在因为调用abort() 方法而终止链接时触发
      *     load - 在接收完整响应数据时触发
      *     loadend - 在通信完成时 或者 触发error、abort、load时触发
      */

    /*** 重点讲下progress事件 ***/
    /**
     * @param { event } 
     * 
     * event 对象包含了几个重要属性
     *    target: xhr对象
     *    lengthComputable: 一个表示进度信息是否可用的布尔值
     *    position: 表示已经接收的字符数
     *    totalSize: 表示根据Content-Length响应头部确定的预期字符数
     *    有了这些信息就可以创建一个进度指示器
     */
    xhr.onprogress = function () {
        var divStatus = document.getElementById('status');
        if (event.lengthComputable) {
            divStatus.innerHTML = "Received " + event.position + " of " + event.totalSize + ' bytes'; 
        }
    }

    /** 为了正常调用，进度事件必须要在 open() 方法之前调用 */

    /** 跨域资源访问 **/

    // CORS - Cross-Origin-Resource-Sharing 跨域资源共享
    // 它背后的基本思想是：就是使用了自定义的HTTP头部信息让浏览器和服务器沟通，从而决定请求或响应是否成功还是失败。
    // 服务端认为某一个请求可以接收，就在 Access-Control-Allow-Origin 头部返回相同信息（如果是公共资源，可以返回'*'）
    // 或者指定某个头部信息的值，来实现跨域资源共享

    /**
     * 图片 Ping 
     * JSONP
     * Comet
     * Web Sockets
     * 总结
     */





```