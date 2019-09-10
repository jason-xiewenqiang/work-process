## 多页面打包 
##### glob 包 第一套
 + 动态获取entry和设置html-wewbpack-plugin数量
 + 在启动webpack时 利用 glob.sync 同步方式   
    1. entry: glob.sync(path.join(__dirname, './src/*/index.js'))
     ```
     module.exports = {
         entry: {
             index: './src/index/index.js',
             search: './src/search/index.js'
         }
     }
     
     ```
 + glob 怎么用？ ----> 穿梭入口 【npm包地址】  [https://www.npmjs.com/package/glob](https://www.npmjs.com/package/glob)
    ```
        glob("**/*.js", function(err, files) {
            // err 错误
            // files 列表
        })
        
    ```
 + 文件结构要求
    ```
       ---- root
            |-- assets
            ....
            |-- src
                |-- index
                     |---- index.js
                     |---- index.html
                
                |-- search
                     |---- index.js
                     |---- index.html
            ...        
            |-- utils
    ```
 + 在webpack里怎么用
   ```
    // webpack.prod.config.js
   
    const setMap = () => {
        const entry = {};
        const htmlWebpackPlugins = [];
    
        const entryFiles = glob.sync(path.join(__dirname, './src/*/index.js'))
        
        // [/user/code/my-project/src/index/index.js, /user/code/my-project/src/index/index.js]
        
        Object.keys(entryFiles).map((index) => {
            const file = entryFiles[index];
            const match = file.match(/src\(.*)\/index\.js/);
            const pageName = match && match[1];
            entry[pageName] = file;
    
            htmlWebpackPlugins.push(
                new HtmlWebpackPlugin({
                    template: path.join(__dirname, `src/${pageName}/index.html`),
                    filename:`${pageName}.html`,
                    chunks: [pageName],  // 使用哪一个模块（js css要对应）
                    inject:true,
                    minify: {
                        html5: true,
                        collapseWhitespace: true,
                        preserveLineBreaks: false,
                        minifyCSS: true,
                        minifyJS: true,
                        removeComments: false
                    }
                })
            )
        })
        return {
            entry,
            htmlWebpackPlugins
        }
    }

    const {entry, htmlWebpackPlugins} = setMap() // 调起函数读取文件配置
    
    
    module.exports = {
        entry: entry,
        plugins: [].concat(htmlWebpackPlugins)  // 加入进去
    }
   
   ```
##### 使用node动态读取文件 fs模块
+ 利用fs模块进行读取
```
    module.exports = {
        // 读取文件内容
        fileReplace(filePath, obj) {
            const readme = fs.readFileSync(filePath, 'utf8')
            fs.outputFileSync(
                filePath,
                readme.replace(/\$(.*?)\$/g, (it, $1) => {
                    return obj[$1]
                })
            )
        },
    }
    
    
    获取到的内容是 [ 
      '/Users/xiewenqiang/code/admin-commons/src/AdminCommons',
      '/Users/xiewenqiang/code/admin-commons/src/DCCommons',
      '/Users/xiewenqiang/code/admin-commons/src/TuBrand',
      '/Users/xiewenqiang/code/admin-commons/src/TuCascader',
    ]
    
    然后进行组合 entry 以及 htmlWebpackPlugins 两个组内容
    
    
    const mergeObject = {} //配置 entry 入口的文件路径
    const HtmlWebpackPluginArray = [] // 配置多页面的输出   == 多个 HtmlWebpackPlugin
    fileNameArray.forEach((item) => {
        if (!mergeObject.hasOwnProperty(item)) {
            mergeObject[item] = path.join(
                __dirname,
                `../../src/${item}/index.js`
            )
            HtmlWebpackPluginArray.push(
                new HtmlWebpackPlugin({
                    title: item,
                    cache: false,
                    filename: `${item}.html`,
                    inject: 'body',
                    template: templatePath,
                    chunks: [item]
                })
            )
        }
    })

```