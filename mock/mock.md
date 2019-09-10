### 优化前端对接流程节点 - 前端mock开发
+ web项目开发流程一般如下
+ 0.web框架和技术确认和评审 --- 基础开发框架搭建
+ 1.需求评审：需求内容分析，内容拆解，对接UI和交互
+ 2.前端后端对接接口内容
+ 3.进入需求开发阶段 ------ mock切入
+ 4.联调接口
+ 5.对接测试和跑测试用例
+ 6.提测
+ 7.bug回归
+ 8.uat+上线-需求结束
#### mock 切入时机是在前后端定义和统一对接信息中参与
+ 做mock的一些好处是：
    1. 前后端确认事项 1、项目需要几个接口 2、接口的url格式 3、后端传回的数据格式
    2. 并行开发：在定义了接口数据的基础上，前后端分离开单独开发功能
    3. 整体性：让前端更好处理页面细节，定义不同参数去适应各种场景，比如error，success等
    4. 解藕：前端后端的解藕，各司其职。
+ 做这个定义过程会让工作流程变得清晰，不会说前后端交杂过多，节省沟通成本。
+ 完成这么一个流程的关键在于 接口数据的定义：那开发前会定义出需求的开发接口文档（大致），还要有动态更新的通知机制，让前后端的信息保持通畅。
    1. 比如接口参数变化了，如何通知到前端这边。
    2. 真正联调时，沟通和自测上需要共同协作，保证开发质量。
    3. 接口信息托管   yapi 平台（需要搭建）看看怎么去搭建。后端怎么去完成自己的开发模拟我不管，大概也是用postman进行模拟请求。
##### 前端的数据mock是怎么实行的
+ 现在的打包框架已经是赋能很多的功能，比如webpack，自身就做了mock的一些设置，当然，此片文章要点评的也是如何进行webpack的mock数据组织。
+ 文件结构：（待补充）
+ webpack的config怎么去配置（相关api的介绍和讲解 https://github.com/YMFE/yapi）
+ 具体实例（有没有地址？需要完善）--- 了解是初步，实现是基础，实例是完美。需要做！！




```
const pathContext = ['/admin/*'];
function getProxyMaps() {
  const maps = {};
  pathContext.forEach(item => {
    maps[item] = {
      target: 'http://' + proxyHost,
      secure: false,
      changeOrigin: true,
      bypass(req, res, proxyOptions) {
        if (fs.existsSync(path.join(__dirname, 'mock', 'map-local-config'))) {
          // 实时读取，更改mock配置后不用重启webpack
          let map = fs.readFileSync(path.join(__dirname, 'mock', 'map-local-config'), 'utf-8');
          map = map.replace(/\s*\/\/.*(?:\r|\n|$)/g, '').trim();
          // console.log(map)
          console.log(('http://' + proxyHost + req.url).blue);
          if (map) {
            try {
              map = JSON.parse(map);
            } catch (e) {
              map = '';
              console.error('################## 看这里 ######################'.red);
              console.error('请检查 map-local-config 文件的内容是否为合法json'.red);
              console.error('################## 看这里 ######################'.red);
            }
            for (let x in map) {
              if (req.url.match(/^([\/\w+]+)\??/)[1] === x) {
                console.log('mapped local:'.green, path.join(__dirname, 'mock', map[x]).replace(/\\/g, '/').green);
                res.sendFile(path.join(__dirname, 'mock', map[x]));
                break;
              }
            }
          }
        }
      },
    }
    console.log(maps)
  });
  return maps;
}
```
