# comm-vue

> A Vue.js project

## Build Setup

``` bash
# vue + axios + sass + element-ui  


# 安装 vue-cli
npm install -g vue-cli
# 新建项目 进入项目
cd my-project 
# vue 初始化
vue init webpack project-name
#进入 project-name
cd project-name
# install dependencies
npm install
# 安装 element-ui
npm install element-ui --save-dev

#main.js 引入
import ElementUI from 'element-ui' 
import 'element-ui/lib/theme-chalk/index.css'

#安装 axios
npm install axios --save-dev
#main.js 引入
import axios from 'axios'
Vue.prototype.$http = axios

#安装 sass
npm install node-sass --save-dev
npm install sass-loader --save-dev
# build conf.js 
{
    test: /\.sass$/,
    loader: ['style', 'css', 'sass']
}
# 页面 <style lang="scss"></style>

# router 修改 router/index.js new Router()添加  mode history H5路径  不加默认/#/结尾
  mode: 'history', 

# 安装vuex 
npm install vuex --save-dev

src文件夹新建vuex文件夹 在vuex文件夹下新建 store.js
main.js引入
import Vuex from 'vuex'
import store from './vuex/store'

Vue.use(Vuex)

# 多页面

#在 utils.js 添加两个函数
// glob是webpack安装时依赖的一个第三方模块，还模块允许你使用 *等符号, 例如lib/*.js就是获取lib文件夹下的所有js后缀名的文件
var glob = require('glob')
    // 页面模板
var HtmlWebpackPlugin = require('html-webpack-plugin')
// 取得相应的页面路径，因为之前的配置，所以是src文件夹下的pages文件夹
var PAGE_PATH = path.resolve(__dirname, '../src/pages')
    // 用于做相应的merge处理
var merge = require('webpack-merge')


//多入口配置
// 通过glob模块读取pages文件夹下的所有对应文件夹下的js后缀文件，如果该文件存在
// 那么就作为入口处理
exports.entries = function() {
    var entryFiles = glob.sync(PAGE_PATH + '/*/*.js')
    var map = {}
    entryFiles.forEach((filePath) => {
        var filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        map[filename] = filePath
    })
    return map
}

//多页面输出配置
// 与上面的多页面入口配置相同，读取pages文件夹下的对应的html后缀文件，然后放入数组中
exports.htmlPlugin = function() {
    let entryHtml = glob.sync(PAGE_PATH + '/*/*.html')
    let arr = []
    entryHtml.forEach((filePath) => {
        let filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        let conf = {
            // 模板来源
            template: filePath,
            // 文件名称
            filename: filename + '.html',
            // 页面模板需要加对应的js脚本，如果不加这行则每个页面都会引入所有的js脚本
            chunks: ['manifest', 'vendor', filename],
            inject: true
        }
        if (process.env.NODE_ENV === 'production') {
            conf = merge(conf, {
                minify: {
                    removeComments: true,
                    collapseWhitespace: true,
                    removeAttributeQuotes: true
                },
                chunksSortMode: 'dependency'
            })
        }
        arr.push(new HtmlWebpackPlugin(conf))
    })
    return arr
}

# 修改 webpack.base.conf.js

 entry: utils.entries(),

# 修改 webpack.dev.conf.js
    new webpack.NoEmitOnErrorsPlugin(),
    // https://github.com/ampedandwired/html-webpack-plugin
    // new HtmlWebpackPlugin({
    //   filename: 'index.html',
    //   template: 'index.html',
    //   inject: true
    // }),
    new FriendlyErrorsPlugin()
  ].concat(utils.htmlPlugin())

# 修改 webpack.prod.conf.js

 // new HtmlWebpackPlugin({
    //   filename: config.build.index,
    //   template: 'index.html',
    //   inject: true,
    //   minify: {
    //     removeComments: true,
    //     collapseWhitespace: true,
    //     removeAttributeQuotes: true
    //     // more options:
    //     // https://github.com/kangax/html-minifier#options-quick-reference
    //   },
    //   // necessary to consistently work with multiple chunks via CommonsChunkPlugin
    //   chunksSortMode: 'dependency'
    // }),

        ])
  ].concat(utils.htmlPlugin())

# serve with hot reload at localhost:8080 
npm run dev

# build for production with minification  打包到 dist
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
