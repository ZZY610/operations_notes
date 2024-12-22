# vscode使用pwsh最新版
在 Visual Studio Code (VSCode) 中，虽然默认可能会使用 PowerShell 5.1，但你可以手动配置它以使用你安装的 PowerShell 7.2 版本。以下是步骤：

### 1. **安装最新版PowerShell 7.2**
如果你已经从 GitHub 上下载并安装了 PowerShell 7.2，请确保它能够在命令行中正常运行。你可以在终端（或命令提示符）中运行以下命令来检查 PowerShell 7.2 是否安装正确：

```bash
pwsh --version
```

如果正确安装，应该会显示你安装的版本号，比如 `7.2.x`。

### 2. **打开 VSCode 并安装 PowerShell 扩展**
确保你已经安装了 PowerShell 扩展，可以从 VSCode 的扩展市场中找到并安装。扩展名为 **PowerShell**，发布者为 Microsoft。

### 3. **配置 VSCode 以使用 PowerShell 7.2**

1. 打开 VSCode。
2. 按下 `Ctrl + Shift + P`，输入并选择 **Preferences: Open Settings (JSON)**。

在你看到的选项中，选择的项取决于你希望配置 PowerShell 7.2 的作用范围：

1. **Preferences: Open User Settings (JSON)**：
   - 这是最常见的选择。配置只对当前用户生效，不论你打开哪个工作区（Workspace），VSCode 都会使用此设置。这适合大多数场景。
   
2. **Preferences: Open Workspace Settings (JSON)**：
   - 如果你只希望在当前的工作区（特定项目文件夹）中使用 PowerShell 7.2，那么选择这个。这个配置仅对你当前打开的项目有效，其他工作区不会使用此配置。

3. **Preferences: Open Default Settings (JSON)**：
   - 这是 VSCode 的默认设置，不能直接修改，更多是用于参考。不要选择这个。

### 推荐选择：

**选择 "Preferences: Open User Settings (JSON)"**。这样，你的 PowerShell 7.2 设置将在整个 VSCode 环境中生效，而不局限于某一个项目。

4. 在配置文件的 JSON 中，添加或修改以下配置：

```json
{
    "powershell.powerShellDefaultVersion": "PowerShell 7",
    "powershell.powerShellAdditionalExePaths": {
        "PowerShell 7": "C:\\path\\to\\pwsh.exe"
    }
}
```

你看到的数据可能有这样的：
```json
{
    "workbench.editor.enablePreview": false,
    "workbench.colorTheme": "PowerShell ISE"
}
```

修改为：
```json
{
    "powershell.powerShellDefaultVersion": "PowerShell 7",
    "powershell.powerShellAdditionalExePaths": {"PowerShell 7":"D:\\PowerShell-7.4.5-win-x64\\pwsh.exe"}
    }
```


- `C:\\path\\to\\pwsh.exe` 是你 PowerShell 7.2 的安装路径。例如，如果你安装在默认位置，可以是 `C:\\Program Files\\PowerShell\\7\\pwsh.exe`。

4. 保存配置文件后，VSCode 会使用 PowerShell 7.2 作为默认终端。

### 4. **验证 PowerShell 版本**
- 打开 VSCode 中的终端，运行 `pwsh` 或 `Get-Host`，你应该看到当前使用的 PowerShell 版本是 7.2。

### 5. **切换 PowerShell 版本**
VSCode 提供了方便的 PowerShell 版本切换功能。你可以按下 `Ctrl + Shift + P` 并选择 **PowerShell: Show Session Menu**，然后在菜单中选择你想要使用的 PowerShell 版本（如 PowerShell 7.2）。

这样，VSCode 就会自动切换到你指定的 PowerShell 7.2 版本了。

---

通过这些步骤，你应该能够让 VSCode 使用你自己下载的 PowerShell 7.2 版本。如果在切换过程中遇到问题，确保你在配置文件中填写的路径是正确的 PowerShell 7.2 的安装路径。