# 部署minecraft服务器
## 1、系统基本信息

- 操作系统：Ubuntu 22.04
- CPU：2核2gb
- 存储空间：40gb
- MC版本：1.16.5

## 2、部署流程

好的！根据你的服务器配置（**Ubuntu 22.04 | 2核2GB内存 | 40GB存储**），以下是针对低配环境的 **极详细部署步骤**，从环境配置到优化调整，确保两人联机流畅运行。

---

### **2.1 服务器环境准备**
#### **1. 连接服务器**
   ```bash
   ssh root@<你的公网IP>  # 替换为你的华为云IP
   ```

#### **2. 系统更新**
   ```bash
   apt update && apt upgrade -y  # 更新系统
   apt install wget nano screen -y  # 安装基础工具
   ```

#### **3. 安装Java环境**
   - Minecraft 1.16.5 建议用 **Java 8**（兼容旧版且内存占用低）：
   ```bash
   apt install openjdk-8-jdk -y  # 安装Java 8
   java -version  # 验证输出应为 "1.8.0_xxx"

    ```
    openjdk version "1.8.0_432"
    OpenJDK Runtime Environment (build 1.8.0_432-8u432-ga~us1-0ubuntu2~22.04-ga)
    OpenJDK 64-Bit Server VM (build 25.432-bga, mixed mode)
    ```
   ```

---

### **2.2 部署Minecraft服务端**
#### **1. 创建专用目录**
   ```bash
   mkdir -p /opt/minecraft && cd /opt/minecraft  # 创建并进入目录
   ```

#### **2. 下载服务端文件**
   - 选择 **1.16.5版本**
   ```bash
   wget https://launcher.mojang.com/v1/objects/1b557e7b033b583cd9f66746b7a9ab1ec1673ced/server.jar
   ```
   > 注：如需其他版本，去[Minecraft官网](https://www.minecraft.net/zh-hans/download/server)获取对应链接。

#### **3. 首次启动生成配置**
   ```bash
   java -Xmx1024M -Xms512M -jar server.jar nogui  # 分配1GB最大内存，512MB初始内存
   ```
   - **首次启动会报错**（需同意EULA协议）：
     ```bash
     vim eula.txt  # 编辑文件
     ```
     修改 `eula=false` → `eula=true`，保存退出。

---

### **2.3 配置服务器参数**
#### **1. 修改 `server.properties`**
   ```bash
   nano server.properties
   ```
   ```properties
   # 关键配置（适配低配服务器）
   query.port=25565     # 服务器连接开放端口
   max-players=2        # 限制2人
   view-distance=6      # 视距降低为6（默认10，减少内存占用）
   online-mode=false    # 关闭正版验证（允许离线账号联机）
   spawn-protection=0   # 取消出生点保护
   difficulty=easy      # 简单难度
   gamemode=survival    # 生存模式
   enable-command-block=false  # 关闭命令方块（减少资源占用）
   ```

#### **2. 禁用自动保存（可选但推荐）**
   - 编辑 `spigot.yml`（如果使用Spigot服务端）或通过插件控制，但此处简化操作：
   ```bash
   echo "save-all" >> start.sh  # 后续通过手动保存（减少I/O压力）
   ```

---

### **2.4 启动与后台运行**
#### **1. 使用Screen管理会话**
   ```bash
   screen -S minecraft  # 创建名为"minecraft"的会话
   java -Xmx1024M -Xms512M -jar server.jar nogui  # 启动服务器
   ```
   - **退出Screen后台运行**：按 `Ctrl+A` 后按 `D`。

#### **2. 验证服务是否运行**
   ```bash
   screen -list  # 查看是否存在 "minecraft" 会话
   netstat -tuln | grep 25565  # 检查端口监听状态
   ```

---

### **2.5 华为云安全组配置**
#### **1. 开放25565端口**
   - 登录 **华为云控制台** → **弹性云服务器** → 选择你的实例 → **安全组** → **配置规则** → **添加规则**：
     - **协议端口**：`TCP:25565`
     - **源地址**：`0.0.0.0/0`（允许所有IP访问，或指定小伙伴的IP提高安全性）

---

### **2.6 客户端连接方式**
#### **1. 获取服务器地址**
   - 格式：`公网IP:25565`（例如：`123.123.123.123:25565`）

#### **2. 玩家加入步骤**
   - 启动Minecraft客户端 → **多人游戏** → **添加服务器** → 输入上述地址。

---

## 3、日常维护

### **3.1 针对2GB内存的优化技巧**
#### **1. JVM参数优化**
   ```bash
   # 启动命令调整为（分配更精确的内存）：
   java -Xmx1400M -Xms256M -XX:+UseConcMarkSweepGC -XX:+CMSIncrementalMode -jar server.jar nogui
   ```
   - `-Xmx1400M`：最大内存限制为1.4GB（为系统留出600MB）
   - `-XX:+UseConcMarkSweepGC`：使用低延迟垃圾回收器

#### **2. 关闭无用生物生成**
   ```bash
   # 编辑 server.properties
   spawn-monsters=false   # 关闭怪物生成
   spawn-animals=false    # 关闭动物生成
   spawn-npcs=false       # 关闭村民生成
   ```

#### **3. 定期清理实体**
   - 在游戏中输入命令：
   ```bash
   /kill @e[type=item]  # 清理地面掉落物
   /kill @e[type=arrow] # 清理射出的箭矢
   ```

---

### **3.2 维护与管理**
#### **1. 手动备份世界**
   ```bash
   cd /opt/minecraft
   tar -czvf world-backup-$(date +%Y%m%d).tar.gz world/  # 打包压缩存档
   ```

#### **2. 关闭服务器**
   - 进入Screen会话：
   ```bash
   screen -r minecraft  # 进入会话
   /stop                # 输入关闭命令
   ```

#### **3. 自动化启动脚本**
   ```bash
   nano /opt/minecraft/start.sh
   ```
   输入以下内容：
   ```bash
   #!/bin/bash
   cd /opt/minecraft
   java -Xmx1400M -Xms256M -XX:+UseConcMarkSweepGC -jar server.jar nogui
   ```
   - 赋予执行权限：
   ```bash
   chmod +x start.sh
   ```

---

### **3.3 常见问题排查**
#### **1. 连接超时**
   ```bash
   ufw allow 25565/tcp  # 确保本地防火墙开放端口
   systemctl stop firewalld  # 临时关闭防火墙（仅测试用）
   ```

#### **2. 内存不足崩溃**
   - 查看日志：
   ```bash
   tail -n 100 logs/latest.log | grep "OutOfMemory"
   ```
   - 解决方案：减少视距（`view-distance=4`）或升级服务器配置。

#### **3. 卡顿优化**
   - 编辑 `bukkit.yml`（如果使用CraftBukkit）：
   ```yaml
   ticks-per:
     animal-spawns: 800  # 降低动物生成频率
     monster-spawns: 800 # 降低怪物生成频率
   ```

---

### **3.4 扩展建议（按需选择）**
#### **1. 安装轻量级面板**
   - 使用 [AMP](https://cubecoders.com/amp) 或 [PufferPanel](https://www.pufferpanel.com/) 实现网页管理。

#### **2. 配置动态域名**
   - 华为云提供 **DNS解析服务**，将域名绑定到公网IP，实现域名访问（如 `mc.yourdomain.com`）。

---

通过以上优化，即使是 **2核2GB** 的低配服务器，也能流畅支持两人联机。如果后续需要升级版本或添加模组，建议先测试内存占用并适当调整JVM参数。