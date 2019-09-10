
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
	<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no">
    <title>自定义方案</title>
    <script>
	    /*
	    * @Author: Chosen
		* @Date: 2019-08-16 01:39:03
        * @Last Modified by: Chosen
        * @Last Modified time: 2019-08-16 01:49:27
        * @Description: 1. 此方法模拟淘宝弹性布局方案-flexible
        * @Description: 2. 区别于淘宝，此方案必先预先设置好：<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no">
        * @Description: 3. 才能正常获取到 doucment.documentElement.clienWidth === 375 等；
        * @Description: 4. 然后通过 js 对其进行改变；
        * @Description: 5. 所有通过 rem 设置的值，不会被缩小，布局上正确显示计算后的值；
        * @Description: 6. 而没有使用 rem 为单位设置的值，布局上渲染的是被缩小 2 倍的效果，控制台中显示设置的值，而不是缩小后的值；
        * @Description: 7. 设计稿上的尺寸，使用 px 作为单位来对 rem 进行换算；
        * @Calculate: 8. 计算示意
                          html                        design: 750px(注: 不是iOS模式下的 pt)

                          1rem                 ->                75px
                  ------------------------            -------------------------
                      $size(布局尺寸)                     $design(设计稿元素尺寸)

          @Calculate-formula: $size = ($design / 75)rem;
        */
      document.addEventListener('DOMContentLoaded', () => {
      	(function(win, document) {
			var dpr, rem, scale;
			var docEl = document.documentElement;
			var metaEl = document.querySelector('meta[name="viewport"]');

			var dpr = window.devicePixelRatio;
			scale = 1 / dpr;
			dpr = win.devicePixelRatio || 1;
			rem = docEl.clientWidth * dpr / 10;
			const cw = docEl.clientWidth;
					console.log('cw, dpr, rem: ', cw, dpr, rem);
			// 设置viewport，进行缩放，达到高清效果
			metaEl.setAttribute('content', `width=${dpr * docEl.clientWidth},
				initial-scale=${scale},
				maximum-scale=${scale},
				minimum-scale=${scale},
				user-scalable=no`
			);
			document.documentElement.style.fontSize = `${rem}px`;
			document.body.style.fontSize = `${dpr * 12}px`;
			// 设置data-dpr属性，留作的css hack之用
			docEl.setAttribute('data-dpr', dpr);
			// 给js调用的，某一dpr下rem和px之间的转换函数
			window.rem2px = function(v) {
				v = parseFloat(v);
				return v * rem;
			};
			window.px2rem = function(v) {
				v = parseFloat(v);
				return v / rem;
			};
			window.dpr = dpr;
			window.rem = rem;
		}(window, document));
      });
	</script>
	<script>
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
		h1, h2, h3, h4, h5, h6 {
		    font-size: 100%;
		    font-weight: 500;
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
			text-align: center;
			font-size: 32px; /* dpr = 2 时，缩放后，显示效果是 16px */
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
    <h3>自定义方案：JS</h3>

    <div class="a">1px 全边框</div>
		<div class="b">2px 全边框</div>
		<p class="info"></p
  </body>
</html>

```