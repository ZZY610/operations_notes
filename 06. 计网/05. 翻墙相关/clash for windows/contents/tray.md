# Enhanced Tray

## 版本要求

0.15.2 版本更新后，支持 Enhanced Tray。

## 系统区别

由于 Windows 任务栏只能显示一个图标，无法显示更多其他信息，为了保证和 macOS 有类似的体验，在 Windows 下 Enhanced Tray 将会以悬浮窗的形式显示。

默认情况下，Enhanced Tray 显示的内容包括 4 部分：图标，流量速度，规则模式及自定义内容。

## 自定义内容

自定义内容允许是简单**字符串**或**脚本返回值**。

当使用脚本（Script）时，需要在模块导出一个`run`方法：

```js
module.exports.run = () => {
  return "hello world"; // 如果想以双行显示两个字符串，此处可以返回一个长度为2的数组
};
```

当然，脚本支持使用 npm 引入其他第三方模块。
