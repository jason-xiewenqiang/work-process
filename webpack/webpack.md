### entry
+ 依赖图的入口是entry
+ 对于非代码的图片、字体依赖都会不断加入到依赖树中
```
    
    单入口： entry是一个字符串
    module.exports = {
        entry: './path/to/my/entry/file.js'
    }
    多入口：entry是一个对象
    module.exports = {
        entry: {
            app: './src/app.js',
            adminApp: './src/adminApp.js'
        }
    }
    
```

### Output
+ output 用来告诉webpack如何将编译后的文件输入到磁盘
```

    单入口的配置
    module.export = {
        entry: './path/to/my/entry/file.js',
        output: {
            filename: 'bundle.js',
            path: __dirname + '/dist'
        }
    }
    
    多入口配置 --- 占位符的方式
    module.exports = {
        entry: {
            app: './src/app.js',
            search: './src/search.js'
        },
        output: {
            filename: '[name].js',
            path: __dirname + '/dist'
        }
    }

```

### loaders
+ webpack 开箱即用的只有支持js和JSON两种文件类型，通过Loaders去支持其他文件类型并且把他们转化成有效的模块，并且可以添加到依赖图中
+ 本身是一个函数，接受源文件作为参数，返回转化结果
```

    常用的Loaders有哪一些？
    bebel-loader        转换ES6、7等js新特性语法
    css-loader          支持css文件的加载和解析
    less-loader         将less为你教案转化成css
    ts-loader           将TS转换成js
    file-loader         进行图片、字体等打包
    raw-loader          将文件以字符串的形式导入
    thread-loader       多进程打包js和css
    
    
    Loaders的用法
    const path = require('path');
    module.exports = {
        output: {
            filename: 'bundle.js'
        },
        module: {
            rules: [
                {test: /\.txt$/, useL 'raw-loader'}    test ---- 指定 匹配规则
                                                       use  ---- 制定使用的loader
            ]
        }
    }

```

### Plugins
+ 插件用户bundle文件的优化，资源管理和环境编辑注入
+ 作用于整个构建过程

```

    常见的Plubins有哪一些？
    CommonsChunkPlugin          将chunks相同的模块代码提取成公开的js
    CleanWebpackPlugin          清理构建目录
    ExtractTextWebpackPlugin    将css从bundle文件里提取出一个独立的css文件
    CopyWebpackPlugin           将文件或者文件夹拷贝到输出目录
    HtmlWebpackPlugin           创建html文件去承载输出的bundle
    UglifyjsWebpackPlugin       压缩js
    ZipWebpackPlugin            将打包出来的资源生成一个zip包
    
    用法
    const path = require('path');
    module.exports = {
        output: {
            filename: 'bundle.js'
        },
        plugins: [
            new HtmlWebpackPlugin({
                template: './src/index.js'   ---  放入 plugins数组中
            })
        ]
    }

```


### mode 
+ Mode 用来指定当前的构建环境是： production、development、none
+ 设置mode可以使用webpack内置函数，默认值是production
```

    Mode 内置函数功能
    
    development             设置process.env.NODE_ENV的值为development
                            开启NamedChunksPlugin 和 NamedModulesPlugin
                            
    production              设置process.env.NODE_ENV 的值为production
                            开启FlagDependencyUsagePlugin, FlagIncludedChunksPlugin
                            ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, 
                            OccurrenceOrderPlugin, SideEffectsFlagPlugin和TerserPlugin
    
    node                    不开启任何优化配置
    

```



### 资源解析ES6

+ 使用Babel-loader   babel的配置文件是： .babelrc

```

  const path = require('path');
  
  module.exports = {
      entry: './src/index.js',
      output: {
        filename: 'bundle.js'  
        path: path.resolve(__dirname, 'dist')
      },
      module: {
          rules: [
            {
                test: /\.js$/,
                use: 'babel-loader'
            }
          ]
      }
  }
  
  
  .babel

```


## webpack4-22打包npm库并发布

##### 打包一个js库，简单克隆的js工具库 里面只有一个方法 deepClone 拷贝对象

###### 支持压缩版和非压缩版 支持CMD CJS ECM 引入

```
graph TD

    A[A] -->B(going to home)
    B --> C{going to company}
    C --> |one | D[loop]
    C --> |two | F[loop]
    C --> |three | E[loop]
    
```


