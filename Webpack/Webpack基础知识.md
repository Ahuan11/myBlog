## Webpack简介

### 1.webpack是什么？

​		Webpack是一种前端资源构建工具，一个静态模块打包器（module bundler）。

​		在Webpack看来，前端的所有资源文件（js/json/css/img/less/...）都会作为模块处理

​		它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源(bundle).

### 2.webpack的五个核心概念

- **Entry**
  - 入口（Entry）指示Webpack以哪个文件为入口起点开始打包，分析构建依赖图。
- **Output**
  - 输出（Output）指示  Webpack  打包后的资源bundles输出到哪里去，以及如何命名

- **Loader**
  - Loader 让Webpack能够去处理哪些非Javascript文件（Webpack 自身只理解JavaScript）
- **Plugins**
  - 插件（Plugins）可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。
- **Mode**
  - 模式（Mode）指示Webpack使用相应模式的配置
  - ![image-20221028135105970](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/image-20221028135105970.png)