# Mixin

## 版本要求

0.9.5 版本更新后，支持向所有配置文件中注入公共属性设置

## 配置文件

例如：在配置文件中统一添加`dns`字段，操作如下：

1. 进入 Settings 页面
2. 滚动至 Profile Mixin 栏
3. 点击 YAML 右边 Edit 小字打开编辑界面
4. 在修改编辑界面内容为：
   ```yaml
   mixin: # 注意下面缩进
     dns:
       enable: true
       listen: :53
       nameserver:
         - 8.8.8.8
   ```
5. 点击编辑器右下角保存

在启动或切换配置时，上面内容将会替换到原有配置文件中进行覆盖

::: tip
配置文件内容不会被修改，混合行为只会发生在内存中

可以通过任务栏图标菜单开关这个行为
:::

## JavaScript

0.10.1 版本更新后，支持使用 JavaScript 进行 mixin 配置，需要传入一个字符串（yaml 多行字符串使用`｜`）

具体步骤如上，只需要将代码填入 JavaScript 栏中，并将 Type 设置成 JavaScript 即可：

```js
module.exports.parse = async (
  { content, name, url },
  { axios, yaml, notify }
) => {
  const extra = {
    dns: {
      enable: true,
      listen: ":53",
      nameserver: ["8.8.8.8"],
    },
  };
  return { ...content, ...extra };
};
```

### 参数说明

CFW 调用用户定义的`parse`方法时，会传入 2 个参数，分别是**配置文件元数据**以及**工具类对象/方法**

#### 配置文件元数据

包括：

1. content：yaml 格式化后的 JavaScript 对象
2. name：配置文件文件名
3. url：配置文件下载地址

#### 工具类对象/方法

包括：

1. axios：网络请求框架，[GitHub](https://github.com/axios/axios)
2. yaml：yaml 框架，[GitHub](https://github.com/eemeli/yaml)
3. notify：发出系统通知方法，签名为`function notify(title:string, message:string, silent:bool)`

### 返回说明

`parse`方法需要返回一个对象，CFW 会将返回的对象序列化后直接载入
