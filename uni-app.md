# uni-app

## 安装运行注意事项

1. 安装微信开发者工具（默认写入环境变量路径）
2. 安装HBuilder X编辑器，并创建一个uni-app项目
3. 在HBuilder中选择运行 -> 运行到小程序模拟器 -> 第一个选项
4. 报错，提示手动打开工具 -> 设置 -> 安全设置，将服务端口开启 **这里的操作步骤并不是HBuilder中的操作步骤，是在微信开发者工具中，关闭项目进入首页，再点击右上角的设置 -> 安全 -> 服务端口按钮 -> 开启**

## HbuilderX快捷键

* 快速定位某个函数的定义位置 Alt+左键
* 



## uni-app开发基础知识

### uni-app的数据缓存方案
uni-app的本地数据缓存使用uni的提供的公共api，在项目中可以直接使用uni.api进行调用。**常用于缓存用户的登录信息、token和自定义key等**

### [uni-app的App.vue](https://uniapp.dcloud.net.cn/collocation/App.html#%E5%BA%94%E7%94%A8%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
uni-app的App.vue组件没有<template></template>,主要负责引入全局样式、全局变量和全局生命周期。此外，uni-app支持基于Vue的单页面应用开发，但App.vue本身并不作为一个具体的页面组件来处理用户请求或显示内容。它更多地扮演一个配置和初始化的角色，确保应用能够正确启动并处理全局事件。

App.vue入口组件
1. App.vue是uni-app的入口组件，所有页面都是在App.vue下进行切换
2. App.vue本身不是页面，这里不能编写视图元素，也就是没有<template>元素
App.vue的作用：
1. 应用的生命周期
2. 编写全局样式
3. 定义全局数据 globalData

