# URL Scheme

## 下载配置

CFW 支持使用 URL Scheme 快速导入配置文件：

```
clash://install-config?url=<encoded URI>
```

### 响应头

#### content-disposition

如果响应头中存在`content-disposition`字段，则使用`filename`对应的值作为配置文件名（例如)，否则使用 URL 最后一部分作为配置文件名

```
content-disposition: attachment; filename="abc.yaml"
```

#### profile-update-interval

如果响应头中存在`profile-update-interval`字段，则配置文件自动更新间隔设置为对应的值，以小时为单位

```
profile-update-interval: 12
```

#### subscription-userinfo

如果响应头中存在`subscription-userinfo`字段，则其对应的流量及到期信息会显示在 Profiles 模块中，[规范参考此处](https://github.com/crossutility/Quantumult/blob/master/extra-subscription-feature.md)

```
subscription-userinfo: upload=455727941; download=6174315083; total=1073741824000; expire=1671815872
```

::: tip
此外，CFW 支持识别添加在配置文件首行的`subscription-userinfo`，格式以`#`开头，以`;`结尾。示例

```
# upload=455727941; download=6174315083; total=1073741824000; expire=1671815872;
```

因此可以使用预处理脚本定期更新本地配置文件中的订阅信息，[参考案例](https://docs.cfw.lbyczf.com/contents/parser.html#向本地配置文件添加订阅信息)。
:::

#### profile-web-page-url

如果响应头中存在`profile-web-page-url`字段，则在右键点击 profile 菜单中会显示`Open web page`选项，允许用户跳转到对应的门户首页

## 退出软件

```
clash://quit
```
