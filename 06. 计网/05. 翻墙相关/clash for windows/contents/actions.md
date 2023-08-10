# Actions

## 版本要求

0.18.8 版本更新后，支持使用 JavaScript 定义 proxy 及 profile 右键菜单中 `Run script` 选项行为

## 定义行为

1. 进入 Settings 界面
2. 滚动至 Actions 栏
3. 点击 Scripts 右边 Edit 打开编辑器，填入：
   ```yaml
   scripts:
     proxy:
       code: |
         module.exports.run = (payload, { dialog }) => {
           dialog({
             message: `proxy name: ${payload.name}`
           })
         }
     profile: {}
   ```
4. 点击编辑器右下角保存按钮
5. 进入 Proxies 界面，在任意 proxy 中右键选择 Run script 即会弹出对话框显示该 proxy 的 name 属性

::: tip
除了使用 code 指定运行代码，也可以用 file 属性指定执行脚本路径
:::

### 参数说明

CFW 调用用户定义的`run`方法时，会传入 2 个参数，分别是**proxy 或 profile 属性**，**工具类对象/方法**

#### proxy 或 profile 属性

payload 是一个对象，表示右键点击的对象

#### 工具类对象/方法

包括：

- axios：网络请求框架，[GitHub](https://github.com/axios/axios)
- yaml：yaml 框架，[GitHub](https://github.com/eemeli/yaml)
- notify：发出系统通知方法，签名为`function notify(title:string, message:string, silent:bool)`
- console：日志输出至文件，方便调试，在 Settings 界面中 Actions 设置下方 Console Output 打开
- homeDir：Home Directory 目录
- dialog: 对话框，可以获取用户选择，[Docs](https://www.electronjs.org/zh/docs/latest/api/dialog#dialogshowmessageboxbrowserwindow-options)

:::tip
除了以上工具类，在使用`.js`文件时，也可以通过`npm`引入第三方模块
:::
