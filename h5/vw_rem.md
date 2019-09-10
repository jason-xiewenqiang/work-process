
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
    <title>vw + rem</title>
    <script>
      /*
	    * @Author: Chosen
		* @Date: 2019-08-19 19:00:08
        * @Last Modified by: Chosen
        * @Last Modified time: 2019-08-19 20:49:27
        * @Description: 1. vw 设置 html 根字体：font-size（vw -> rem）
        * @Description: 2. 固定根字体为：rem = 100px
        * @Description: 3. 因页面布局没缩放，所以 dpr 用不上
        * @Description: 4. 为啥不设置成 10px 呢？因为浏览器最小单位是 12px。10px 会被置为 12px，这不是我们想要的！
        * @Description: 5. 如上写死 meta 标签，设备宽度等于视口宽度。不进行缩放，表示 750 设计稿用 375 换算。此处用 750 做 demo 设计稿
        * @Calculate: 6. 单位换算示意（26.666667vw 的由来）

                              视口（px）                           视口（vw）

                        750px / 2 (其他设计稿同理)        ->         100vw
                    -------------------------------            -------------
                              100px(rem)                          $rem(vw)

          @Calculate-formula: $rem = 26.666667vw, i.e. rem = 100px;
          @Description: 7. 最后就可以使用 rem 布局了：$size = ($design / 100)rem; // $design(设计稿元素尺寸)
          @Note: 8. 由于没有缩放，所以不使用 rem 单位的数值是多少布局渲染就是多少，1px 像素直线需要用伪类自行实现
        */
      setTimeout(() => {
        const deviceInfo = document.querySelector('.info');
        deviceInfo.innerHTML = `device-width: ${document.documentElement.clientWidth}, dpr: ${window.devicePixelRatio}`;
      }, 1000);
    </script>
    <style>
      html, body, h1, h2, h3 {
        margin: 0;
        padding: 0;
      }
      html {
        font-size: 26.66666667vw;
      }
      h3 {
        text-align: center;
        padding: .05rem;
        border: 1px solid #559dc1;
      }
      .a, .b {
        width: 3rem;
        height: 1.5rem;
        line-height: 1.5rem;
        text-align: center;
        font-size: 32px;
        box-sizing: border-box;
        border: 1px solid #666;
        margin: .5rem auto;
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
  <body style="font-size: 12px">
    <h3>vw 设置 rem</h3>

    <div class="a">1px 全边框</div>
    <div class="b">2px 全边框</div>
    <p class="info"></p>
  </body>
</html>

```