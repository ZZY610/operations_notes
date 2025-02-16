# Ubuntu安装powershell
在Ubuntu 20.04上安装PowerShell 7的详细步骤如下：

---

### **步骤1：更新系统**
```bash
sudo apt update && sudo apt upgrade -y
```

---

### **步骤2：安装依赖项**
确保系统已安装必要的工具（如`wget`和`curl`）：
```bash
sudo apt install -y wget curl apt-transport-https
```

---

### **步骤3：添加Microsoft GPG密钥**
下载微软的GPG密钥以验证软件包安全性：
```bash
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

---

### **步骤4：添加PowerShell软件源**
将PowerShell的APT仓库添加到系统：
```bash
sudo curl -sSL https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/microsoft.list
```

---

### **步骤5：更新软件包列表**
```bash
sudo apt update
```

---

### **步骤6：安装PowerShell 7**
```bash
sudo apt install -y powershell
```

---

### **步骤7：验证安装**
启动PowerShell并检查版本：
```bash
pwsh
# 在PowerShell中运行以下命令：
$PSVersionTable.PSVersion
```
输出应类似：
```
Major  Minor  Patch  PreReleaseLabel BuildLabel
-----  -----  -----  --------------- ----------
7      3      4
```

---

### **步骤8：基本使用**
- **启动PowerShell**：
  ```bash
  pwsh
  ```
- **退出PowerShell**：
  ```powershell
  exit
  ```

---

### **可选操作：设置PowerShell为默认Shell**
```bash
# 1. 将PowerShell添加到合法Shell列表
echo /usr/bin/pwsh | sudo tee -a /etc/shells

# 2. 修改用户默认Shell
sudo chsh -s /usr/bin/pwsh $USER
```

---

### **卸载PowerShell**
```bash
sudo apt remove -y powershell
sudo apt autoremove -y
```

---

### **常见问题解决**
1. **依赖错误**  
   如果安装失败，尝试修复依赖关系：
   ```bash
   sudo apt --fix-broken install
   ```

2. **GPG密钥过期**  
   手动更新密钥：
   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

3. **网络问题**  
   确保服务器能访问微软的软件源（可能需要代理）。

---

### **PowerShell在Linux中的典型用途**
- **跨平台脚本**：编写可在Windows/Linux通用的自动化脚本。
- **系统管理**：通过SSH远程管理Linux服务器。
- **云服务集成**：管理AWS/Azure资源（需安装对应模块）。
