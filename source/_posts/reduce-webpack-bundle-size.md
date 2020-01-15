---
title: 优化前端Webpack打包文件体积
desc: 介绍了如何优化前端Webpack打包文件的体积
date: 2020-01-15 15:00:06
tags:
---

本文介绍了如何有效的减少前端Webpack打包文件的体积，优化网站使用体验。

<!--more-->

最近用umi和antd搭了一个Web App，js文件大约2M，体积有点大，打算优化一下。

![优化之前js文件大约2M](https://user-images.githubusercontent.com/12998118/72412676-e25cfe00-37a8-11ea-960c-3dd56f64e203.png)

## 分析

使用[webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)查看各个包占的体积

在.umirc.js配置插件：

```js
const BundleAnalyzerPlugin = require('umi-webpack-bundle-analyzer').BundleAnalyzerPlugin

...
chainWebpack(config) {
  process.env.NODE_ENV === 'production' &&
  process.env.analyze === 'true' &&
  config.merge({
    plugin: {
      bundleAnalyzer: {
        plugin: BundleAnalyzerPlugin,
        args: [{ analyzerPort: 3030 }]
      }
    }
  })
}
...
```

![使用webpack-bundle-analyzer查看各个包占的体积](https://user-images.githubusercontent.com/12998118/72412674-e25cfe00-37a8-11ea-8bdd-3ef3b7a2a13f.png)

## 优化

### 使用Day.js替换moment.js

Antd会使用到moment.js，从上放图可以看出，左下方moment占有了较大的体积。这里我们轻量的Day.js插件来替换moment.js。Antd提供了一个方便替换的插件[antd-dayjs-webpack-plugin](https://github.com/ant-design/antd-dayjs-webpack-plugin)。

![使用Day.js替换moment.js之后](https://user-images.githubusercontent.com/12998118/72412673-e1c46780-37a8-11ea-9dd5-7f863ba2691d.png)

### 减小lodash的体积

使用时，避免引入整个lodash包：

```js
import _ from 'lodash'
// 替换为
import _get from 'lodash/get'
```

同时可以使用[lodash-webpack-plugin](https://github.com/lodash/lodash-webpack-plugin)插件来进一步减小体积。

![减小lodash体积之后](https://user-images.githubusercontent.com/12998118/72412672-e12bd100-37a8-11ea-9c89-1342d552e09c.png)

### 减小Ant Design Icon的体积

从上方的分析图可以看，左侧Antd icon文件占了很大一部分体积，因为Antd新版本包含了多个样式很多的icon的svg。

如果Icon使用的个数有限，可以通过[临时解决方案](https://github.com/HeskeyBaozi/reduce-antd-icons-bundle-demo)来减小打包体积。但如果项目中有配置的icon类型，比如`<Icon type={icon} />`这样的写法就不适用了。具体可以参见[讨论issue](https://github.com/ant-design/ant-design/issues/12011)。这个占时不做修改，期待Antd 4.0有更好的解决方案。

## 结果

优化后，体积2M→1.6M，减小了20%

![优化后体积约1.6M](https://user-images.githubusercontent.com/12998118/72412670-e0933a80-37a8-11ea-80cf-a058b3fdf409.png)
