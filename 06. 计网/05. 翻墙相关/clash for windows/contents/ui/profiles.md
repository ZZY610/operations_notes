# Profiles 配置

![](~@imgs/ui-profiles1.png)

::: tip
深颜色表示当前配置文件已被载入成功

远程配置文件指通过 URL 导入的配置文件
:::

每个配置文件都包括如下信息：

- 名称
- 类型（远程显示域名，本地显示 local）
- 上次更新时间
- 操作栏

操作栏分别可以进行如下操作：

- Edit: 使用内置编辑器进行文本编辑
- Edit externally: 使用外部编辑器进行文本编辑
- Update: 刷新配置文件(仅限远程配置文件)
- Show in folder: 在系统文件夹显示配置文件
- Diff: 创建diff文件进行编辑以合并到原配置文件
- Proxies: 编辑链接策略
- Rules: 编辑链接规则
- Copy: 复制配置文件
- QRCode: 生成当前配置文件QR码(仅限远程配置文件)
- Settings: 编辑当前配置文件相关
- Delete: 删除当前配置文件