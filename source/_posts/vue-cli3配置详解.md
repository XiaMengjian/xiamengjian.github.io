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