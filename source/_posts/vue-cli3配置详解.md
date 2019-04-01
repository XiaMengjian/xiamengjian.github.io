---
title: vue cli3配置详解
date: 2019-03-29 15:17:18
tags:
---

当运行yarn dev时  实际运行的时vue-cli-service 
vue-cli-service执行lib/commands/build/index.js
// 这边只举例build情况
执行的方法：L:70 
``` js
       await build(Object.assign({}, args, {
          modernBuild: true,
          clean: false
        }), api, options)
```
build方法的定义就在L:91行

## vue-cli-service 服务
###  serve 命令
会启动一个开发服务器 (基于 webpack-dev-server) 并附带开箱即用的模块热重载 (Hot-Module-Replacement)

```
用法：vue-cli-service serve [options] [entry]

选项：

  --open    在服务器启动时打开浏览器
  --copy    在服务器启动时将 URL 复制到剪切版
  --mode    指定环境模式 (默认值：development)
  --host    指定 host (默认值：0.0.0.0)
  --port    指定 port (默认值：8080)
  --https   使用 https (默认值：false)
```
### build
会在 dist/ 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好的缓存而做的自动的 vendor chunk splitting。它的 chunk manifest 会内联在 HTML 里。


# vue cli所有的默认配置
@vue/cli-service options


# parallel 类似happypack
源码：@vue/cli-plugin-babel index.js L:19
值：boolean
默认值: hasMultipleCores  多核机器上默认开启
```
function hasMultipleCores () {
  try {
    return require('os').cpus().length > 1
  } catch (e) {
    return false
  }
}
```

`是否为 Babel 或 TypeScript 使用 thread-loader。
该选项在系统的 CPU 有多于一个内核时自动启用，仅作用于生产构建
`

默认配置：



# env
带`VUE_APP_` 或被插件DefinePlugin 写入到process.env中，这样src目录下的就能访问到
如果不带 `VUE_APP_` 的话 只能在webpack的配置中才能访问到



# pwa


# e2e cypress 测试安装




```
optimization: {
    splitChunks: { 
      chunks: "initial",         // 代码块类型 必须三选一： "initial"（初始化） | "all"(默认就是all) | "async"（动态加载） 
      minSize: 0,                // 最小尺寸，默认0
      minChunks: 1,              // 最小 chunk ，默认1
      maxAsyncRequests: 1,       // 最大异步请求数， 默认1
      maxInitialRequests: 1,     // 最大初始化请求书，默认1
      name: () => {},            // 名称，此选项课接收 function
      cacheGroups: {                // 缓存组会继承splitChunks的配置，但是test、priorty和reuseExistingChunk只能用于配置缓存组。
        priority: "0",              // 缓存组优先级 false | object |
        vendor: {                   // key 为entry中定义的 入口名称
          chunks: "initial",        // 必须三选一： "initial"(初始化) | "all" | "async"(默认就是异步)
          test: /react|lodash/,     // 正则规则验证，如果符合就提取 chunk
          name: "vendor",           // 要缓存的 分隔出来的 chunk 名称
          minSize: 0,
          minChunks: 1,
          enforce: true,
          reuseExistingChunk: true   // 可设置是否重用已用chunk 不再创建新的chunk
        }
      }
    }
  }
 
```


HashedModuleIdsPlugin
NamedChunksPlugin

对脚本文件应用 [chunkhash] 对 extractTextPlugin 应用的的文件应用 [contenthash]；
使用 CommonsChunkPlugin 合理抽出公共库 vendor（包含社区工具库这些 如 lodash）, 如果必要也可以抽取业务公共库 common(公共部分的业务逻辑)，以及 webpack的 runtime；
在开发环境下使用 NamedModulesPlugin 来固化 module id，在生产环境下使用 HashedModuleIdsPlugin 来固化 module id
使用 NamedChunksPlugin 来固化 runtime 内以及在使用动态加载时分离出的 chunk 的 chunk id。
建议阅读一下全文，因为不看你很难明白为什么要如上这么做。




uglifyjs-webpack-plugin //去除console UglifyJs不支持ES6的压缩，如果代码压缩前没有编译到ES5，

```
ERROR in xx/xxxxx.js from UglifyJs
Unexpected token xxxx
```

换成terser-webpack-plugin

https://github.com/webpack/webpack/issues/5858

From 2018-09-14, uglifyjs-webpack-plugin switches back to uglify-js instead of uglify-es. If you need uglify ES6 code please use terser-webpack-plugin.

compression-webpack-plugin //gzip压缩



为sass提供全局样式，以及全局变量
  可以通过在main.js中Vue.prototype.$src = process.env.VUE_APP_SRC;挂载环境变量中的配置信息，然后在js中使用$src访问。

  css中可以使用注入sass变量访问环境变量中的配置信息
  
  
```js
module.exports = {
    css: {
        modules: false,
        extract: IS_PROD,
        sourceMap: false,
        loaderOptions: {
          sass: {
            // 向全局sass样式传入共享的全局变量
            data: `@import "~assets/scss/variables.scss";$src: "${process.env.VUE_APP_SRC}";`
          }
        }
    }
}
```
在scss中引用

```js
.home {
    background: url($src + '/images/500.png');
}
```


lodash 与全局函数


```js
   configureWebpack: {
        plugins: [
            new webpack.ProvidePlugin({
                '_': path.resolve(__dirname, './src/tools/lodash.js')
            }),
            new webpack.ProvidePlugin({
                '$': path.resolve(__dirname, './src/tools/_merge.js')
            })
        ]
    },
```



```js 
// 下面是引入nodejs的路径模块
var path = require('path')
// 下面是utils工具配置文件，主要用来处理css类文件的loader
var utils = require('./utils')
// 下面引入webpack，来使用webpack内置插件
var webpack = require('webpack')
// 下面是config目录下的index.js配置文件，主要用来定义了生产和开发环境的相关基础配置
var config = require('../config')
// 下面是webpack的merger插件，主要用来处理配置对象合并的，可以将一个大的配置对象拆分成几个小的，合并，相同的项将覆盖
var merge = require('webpack-merge')
// 下面是webpack.base.conf.js配置文件，我其他博客文章已经解释过了，用来处理不同类型文件的loader
var baseWebpackConfig = require('./webpack.base.conf')
// copy-webpack-plugin使用来复制文件或者文件夹到指定的目录的
var CopyWebpackPlugin = require('copy-webpack-plugin')
// html-webpack-plugin是生成html文件，可以设置模板，之前的文章将过了
var HtmlWebpackPlugin = require('html-webpack-plugin')
// extract-text-webpack-plugin这个插件是用来将bundle中的css等文件产出单独的bundle文件的，之前也详细讲过
var ExtractTextPlugin = require('extract-text-webpack-plugin')
// optimize-css-assets-webpack-plugin插件的作用是压缩css代码的，还能去掉extract-text-webpack-plugin插件抽离文件产生的重复代码，因为同一个css可能在多个模块中出现所以会导致重复代码，换句话说这两个插件是两兄弟
// 详情见(1)
var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')

// 如果当前环境变量NODE_ENV的值是testing，则导入 test.env.js配置文，设置env为"testing"
// 如果当前环境变量NODE_ENV的值不是testing，则设置env为"production"
var env = process.env.NODE_ENV === 'testing'
  ? require('../config/test.env')
  : config.build.env

// 把当前的配置对象和基础的配置对象合并
var webpackConfig = merge(baseWebpackConfig, {
  module: {
      // 下面就是把utils配置好的处理各种css类型的配置拿过来，和dev设置一样，就是这里多了个extract: true，此项是自定义项，设置为true表示，生成独立的文件
    rules: utils.styleLoaders({
      sourceMap: config.build.productionSourceMap,
      extract: true
    })
  },
  // devtool开发工具，用来生成个sourcemap方便调试
  // 按理说这里不用生成sourcemap多次一举，这里生成了source-map类型的map文件，只用于生产环境
  devtool: config.build.productionSourceMap ? '#source-map' : false,
  output: {
      // 打包后的文件放在dist目录里面
    path: config.build.assetsRoot,
    // 文件名称使用 static/js/[name].[chunkhash].js, 其中name就是main,chunkhash就是模块的hash值，用于浏览器缓存的
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    // chunkFilename是非入口模块文件，也就是说filename文件中引用了chunckFilename
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
  plugins: [
    // http://vuejs.github.io/vue-loader/en/workflow/production.html
    // 下面是利用DefinePlugin插件，定义process.env环境变量为env
    new webpack.DefinePlugin({
      'process.env': env
    }),
    new webpack.optimize.UglifyJsPlugin({
        // UglifyJsPlugin插件是专门用来压缩js文件的
      compress: {
        warnings: false // 禁止压缩时候的警告信息，给用户一种vue高大上没有错误的感觉
      },
      // 压缩后生成map文件
      sourceMap: true
    }),
    // extract css into its own file
    new ExtractTextPlugin({
        // 生成独立的css文件，下面是生成独立css文件的名称
      filename: utils.assetsPath('css/[name].[contenthash].css')
    }),
    // Compress extracted CSS. We are using this plugin so that possible
    // duplicated CSS from different components can be deduped.
    new OptimizeCSSPlugin({
        // 压缩css文件
      cssProcessorOptions: {
        safe: true
      }
    }),
    // generate dist index.html with correct asset hash for caching.
    // you can customize output by editing /index.html
    // see https://github.com/ampedandwired/html-webpack-plugin
    // 生成html页面
    new HtmlWebpackPlugin({
        //非测试环境生成index.html
      filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.build.index,
        // 模板是index.html加不加无所谓
      template: 'index.html',
      // 将js文件放到body标签的结尾
      inject: true,

      minify: {
          // 压缩产出后的html页面
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
      // necessary to consistently work with multiple chunks via CommonsChunkPlugin
      // 分类要插到html页面的模块
      chunksSortMode: 'dependency'
    }),


    // split vendor js into its own file
    // 下面的插件是将打包后的文件中的第三方库文件抽取出来，便于浏览器缓存，提高程序的运行速度
    new webpack.optimize.CommonsChunkPlugin({
        // common 模块的名称
      name: 'vendor',
      minChunks: function (module, count) {
        // any required modules inside node_modules are extracted to vendor
        // 将所有依赖于node_modules下面文件打包到vendor中
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    // extract webpack runtime and module manifest to its own file in order to
    // prevent vendor hash from being updated whenever app bundle is updated
    // 把webpack的runtime代码和module manifest代码提取到manifest文件中，防止修改了代码但是没有修改第三方库文件导致第三方库文件也打包的问题
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      chunks: ['vendor']
    }),
    // copy custom static assets
    // 下面是复制文件的插件，我认为在这里并不是起到复制文件的作用，而是过滤掉打包过程中产生的以.开头的文件
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
  ]
})

if (config.build.productionGzip) {
    // 开启Gzi压缩打包后的文件，老铁们知道这个为什么还能压缩吗？？，就跟你打包压缩包一样，把这个压缩包给浏览器，浏览器自动解压的
    // 你要知道，vue-cli默认将这个神奇的功能禁用掉的，理由是Surge 和 Netlify 静态主机默认帮你把上传的文件gzip了
  var CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp( // 这里是把js和css文件压缩
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}

if (config.build.bundleAnalyzerReport) {
    // 打包编译后的文件打印出详细的文件信息，vue-cli默认把这个禁用了，个人觉得还是有点用的，可以自行配置
  var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

module.exports = webpackConfig
```


# Bug idea 过滤了 .map文件！！！

* require('yargs').argv 读取脚本 --xx = 'ccc'  
* 




```js
    // rules for <style lang="module">
      const vueModulesRule = baseRule.oneOf('vue-modules').resourceQuery(/module/)
      applyLoaders(vueModulesRule, true)

      // rules for <style>
      const vueNormalRule = baseRule.oneOf('vue').resourceQuery(/\?vue/)
      applyLoaders(vueNormalRule, false)

      // rules for *.module.* files
      const extModulesRule = baseRule.oneOf('normal-modules').test(/\.module\.\w+$/)
      applyLoaders(extModulesRule, true)

      // rules for normal CSS imports
      const normalRule = baseRule.oneOf('normal')
      applyLoaders(normalRule, modules)
```


```js
全局环境下使用第三方库（provide-plugin or imports-loader）
new webpack.ProvidePlugin({
        _: 'lodash'
}),
如果，你想指定lodash的某个工具函数可以全局使用，如：_.concat，
new webpack.ProvidePlugin({
        // _: 'lodash',
        _concat: ['lodash', 'concat']
}),

```