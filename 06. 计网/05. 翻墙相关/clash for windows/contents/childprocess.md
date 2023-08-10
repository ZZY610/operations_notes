# 启动其他应用

版本 0.6.1 更新后，加入启动其他第三方应用的能力。可以在 CFW 启动后自动启动其他软件

## 使用

1. 进入 Settings 页面
2. 点击滚动至 Child Processes 设置
3. 点击 Processes 右边 Edit 按钮，打开的编辑器中添加：
   ```yaml
   processes:
     - command: bark-helper-go.exe # 程序名称
       args:
         - "-t" # 运行参数1
         - "abcdefghijk" # 运行参数2
       options:
         cwd: C:\bark-helper # 子进程工作目录
       log: true # 将在 cwd 目录生成输出日志
   ```
4. 点击编辑器右下角保存

添加完毕后重启 CFW 即可生效

其他参数参考：http://nodejs.cn/api/child_process.html#child_process_child_process_spawn_command_args_options
