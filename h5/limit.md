
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>淘宝无限适配</title>
    <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script>
    <script>
      /*
	    * @Author: Chosen
		* @Date: 2019-08-19 19:00:08
        * @Last Modified by: Chosen
        * @Last Modified time: 2019-08-19 20:49:27
        * @Description: 1. 引入阿里 cdn 脚本后，其会自动设置 meta，如果 meta 已存在，就被脚本修改
        * @i.e. 说明：使用淘宝方案时不需手动设置 meta
        * @Description: 2. 设置 meta 中的 scale = 1 / dpr
        * @Description: 3. 设置 html: font-size(rem) = (design-width / 10)px
        * @Description: 4. iPhone: dpr >= 3 那就是 3，dpr >= 2 那就是 2，否则 dpr 为 1
        * @Description: 5. android 手机 dpr 都是 1，不用考虑 window.devicePixelRatio
        * @Desxription: 6. ===== 脚本做了以上 1 ~ 5 点事情 =====
        * @Note: 7. 除了利用 rem 作为单位的元素之外，其他单位都会被缩放为原来的 scale 倍，所以此处解决了 1px 的问题
        * @Note: 8. 由上一条，像文本字体，一般不用 rem 单位（视情况而定）。那么想要达到预期的字号，就要放大 dpr 倍
        * @Note: 9. 比如：期望 <p> 标签的字体为 14px，那样式应该写成：(14 * dpr)px。或者用 sass less 混合宏处理
        * @Calculate: 10. 计算示意
                          html                        design: 750px(注: 不是iOS模式下的 pt)

                          1rem                 ->                75px
                  ------------------------            -------------------------
                      $size(布局尺寸)                     $design(设计稿元素尺寸)

          @Calculate-formula: $size = ($design / 75)rem;
        */
      setTimeout(() => {
        const deviceInfo = document.querySelector('.info');
        deviceInfo.innerHTML = `device-width: ${document.documentElement.clientWidth}, 
        dpr: ${window.devicePixelRatio}`;
      }, 1000);
    </script>
    <style>
      html, body, h1, h2, h3 {
        margin: 0;
        padding: 0;
      }
      h3 {
        text-align: center;
        padding: .5rem;
        border: 1px solid #559dc1;
      }
      .a, .b {
        width: 9rem;
        height: 3rem;
        line-height: 3rem;
        font-size: 32px; /* dpr = 2 时，缩放后，显示效果是 16px */
		text-align: center;
        box-sizing: border-box;
        border: 1px solid #666;
        margin: 1rem auto;
        border-radius: 10px;
      }
      .b {
        border: 2px solid red;
      }
      .info {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <h3>淘宝无限适配：JS</h3>

    <div class="a">1px 全边框</div>
    <div class="b">2px 全边框</div>
    <p class="info"></p>
  </body>
</html>

```