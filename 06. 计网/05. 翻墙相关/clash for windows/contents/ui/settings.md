# 设置 Settings

[toc]

## Settings

### Reset All Settings

重置 CFW 的全部设置，恢复到默认值

### Force Quit

强制退出 CFW

### Quit

正常退出 CFW

## Security

### Core Secret

设置 Home Directory/config.yaml 中 secret 参数为随机 uuid4，访问核心 API 被网页直接调用

### Allow unsafe URLs

设置在请求时不验证认证的 URL

## General

### Settings Editor

设置 Settings 界面使用的编辑器，如需要自定义命令，则需要命令支持中断（类似`subl --wai`）

### Notifications

设置是否允许弹出系统级通知

### Silent Start

设置启动后是否显示主界面

### Random Controller Port

设置是否使用随机端口作为核心控制器端口（Home Directory/config.ymal 中 external-controller 中 port 部分）

### Random Mixed Port

设置是否使用随机端口作为 General 中 Port 参数（Home Directory/config.ymal 中 mixed-port 参数）

### Lightweight Mode

[轻量模式](../lightweight.md)

### Run Time Format

设置主界面左下角运行时间格式

### GUI Log Folder

设置 GUI 日志文件夹

### Show New Version Icon

设置是否显示新版本提示标志

### Automatic Upgrade (macOS)

设置是否自动更新，设置后有新版本将在后台更新，更新完成后下次启动即是最新版本

## Appearance

### Theme

设置主题颜色

### Follow System Theme

设置是否根据系统选择 Light / Dark 主题

### Font Family

设置主界面字体，如字体不存在可能会导致界面显示不全

### Use System Emoji

设置是否使用系统 Emoji 字体，关则表示使用 CFW 内置 Emoji 字体

### Default Icon Path (Windows)

设置默认状态下任务栏图标

### System Proxy On Icon Path (Windows)

设置 System Proxy 开启下任务栏图标

### Hide Tray Icon

设置是否隐藏任务栏图标

### Enhanced Tray

设置是否启用自定义任务栏（macOS）/ 悬浮窗（Windows）

将右侧图标拖动至左侧表示显示该元素

[具体参考](../tray.md)

### Text

设置 Enhanced Tray 的 Script 部分显示为固定文本

### Script

设置 Enhanced Tray 的 Script 部分显示为 Script 指向 JavaScript 脚本运行返回的内容

### Script Interval

设置 Enhanced Tray 的 Script 的刷新频率

### Transparent （Windows）

设置 Enhanced Tray 是否使用透明界面

## System Proxy

### Type （Windows）

设置系统代理类型

### PAC Content （Windows）

设置系统代理类型为 PAC 时，对应 PAC 脚本内容。[规范参考此处](https://developer.mozilla.org/en-US/docs/Web/HTTP/Proxy_servers_and_tunneling/Proxy_Auto-Configuration_PAC_file)

### Bypass Domain/IPNet

设置系统代理绕过域名或 IP 集合

### Specify Protocol （Windows）

设置是否指定系统代理协议（仅建议在使用 Python 编程遇到网络问题时打开）

### Static Host

设置系统代理 Host 部分为静态内容，不设置则 host 默认为 127.0.0.1

## Mixin

[具体参考](../mixin.md)

### Type

设置 Mixin 类型

### YAML

设置当 Mixin Type 为 YAML 时对应内容

### JavaScript

设置当 Mixin Type 为 JavaScript 时对应内容

## Proxies

### Proxy Item Width

设置 Proxies 界面中每个节点显示宽度

### Mini List Width

设置 Proxies 界面中右侧导航栏展开宽度

### Order By

设置 Proxies 界面中节点排序依据

可选：

- Default：默认排序
- Latency：延迟排序
- Alphabet：字母表排序

### Latency Test URL

设置 Proxies 界面中点击延迟测试按钮时使用的 URL

### Latency Test Timeout

设置 Proxies 界面中点击延迟测试按钮时超时时间

### Show Filter

设置 Proxies 界面是否显示关键字过滤图标

### Hide Unselectable Group

设置 Proxies 界面中是否隐藏不可选策略组

## Connections

### Break When Proxy Change

设置在 Proxies 界面中切换节点时打断 connection 的策略

可选：

- None：不打断
- Chain：连接中包含节点时打断
- All：打断所有连接

### Break When Profile Change

设置切换 profile 时是否打断所有连接

### Break When Mode Change

设置切换 mode 时是否打断所有连接

### Display Chain Type

设置 Connections 界面中显示的 chain 类型

可选：

- Proxy：节点名
- Group：策略组名
- Both：节点名以及策略组名

## Providers

### Use CFW Editor

设置是否使用内置编辑器打开 provider 文件

## Outbound

### Interface Name

设置出口网络网卡（如非必要请勿设置，后续版本将去除此功能）

## Child Processes

[具体参考](../childprocess.md)

### Processes

设置子进程

## Profiles

### Parsers

设置预处理器内容，[具体参考](../parser.md)

### Console Output

设置预处理器调用 `console.log`时输出日志文件目录

### Folder Path

设置 profiles 文件夹路径，默认 Home Directory / profiles

### Request Headers

设置更新配置文件时请求头内容，如需设置单个配置文件请求头内容，请在 Profiles 界面中操作

### Select After Updated

设置更新配置文件后是否选中该 profile

## Logs

### Preload

设置 Logs 界面预加载日志行数，设置为 0 则不预加载

## Actions

[具体参考](../actions.md)

### Scripts

设置执行 Action 时调用代码

### Console Output

设置 Actions 代码执行 `console.log`时输出文件路径

## Shortcuts

### System Proxy

设置 System Proxy 开关快捷键

### TUN Mode

设置 TUN Mode 开关快捷键

### Mixin

设置 Mixin 开关快捷键

### Global Mode

设置将 mode 切换为 Global 快捷键

### Direct Mode

设置将 mode 切换为 Direct 快捷键

### Rule Mode

设置将 mode 切换为 Rule 快捷键

### Script Mode

设置将 mode 切换为 Script 快捷键

### Show/Hide Dashboard

设置显示/隐藏主界面快捷键

### Run Tray Script

设置执行 Enhanced Tray 的 Script 代码快捷键

## Cache

### Fake IP Cache

`Clear`清除 Fake IP 缓存

## Experimental Features（macOS）

### DHCP Server

[具体参考](../dhcp.md)
