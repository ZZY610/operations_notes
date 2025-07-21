# jre和jdk
JRE vs JDK：从“能跑”到“能开发”的完整家谱  
——一张图、一张表、一段历史，彻底讲清

────────────────────────
1. 一句话区分
• JRE（Java Runtime Environment）  
  = **Java 运行环境**  
  只负责“让 .class / .jar 跑起来”。

• JDK（Java Development Kit）  
  = **Java 开发工具包**  
  = JRE + 编译器 + 调试器 + 一堆开发小工具。

────────────────────────
2. 成员清单（对照表）

| 组件 | 在 JRE | 在 JDK | 作用 |
|---|---|---|---|
| java 命令 | ✅ | ✅ | 启动 JVM |
| javac 命令 | ❌ | ✅ | 把 .java 编译成 .class |
| javap / jdeps / jlink | ❌ | ✅ | 反编译、依赖分析、模块化打包 |
| jdb | ❌ | ✅ | 命令行调试器 |
| jconsole / jvisualvm | ❌ | ✅ | 监控、性能分析 |
| src.zip（源码） | ❌ | ✅ | 官方类库源码，IDE 跳转用 |
| javadoc | ❌ | ✅ | 生成 API 文档 |
| jmod 工具 | ❌ | ✅ | Java 9+ 模块系统打包 |
| JVM 动态链接库（libjvm.so / jvm.dll） | ✅ | ✅ | 真正跑字节码的虚拟机 |
| 基础类库（rt.jar → 模块） | ✅ | ✅ | java.lang, java.util … |

────────────────────────
3. 目录结构（以 Linux OpenJDK 17 为例）

JDK 根目录  
├── bin/  
│   ├── java (JVM 启动器)  
│   ├── javac (编译器)  
│   └── jar, jlink, jdeps …  
├── conf/ (配置文件)  
├── include/ (JNI 头文件)  
├── jmods/ (Java 9+ 的模块文件)  
├── lib/ (JDK 专用库)  
└── **jre/** ← 早期 JDK 8 才有，JDK 11+ 已合并  
    ├── bin/ (java 等)  
    └── lib/ (核心类库)

> 从 JDK 9 开始，**JDK 本身即包含运行时**，不再单独提供独立 JRE 安装包。

────────────────────────
4. 版本演进时间线
• 1996 JDK 1.0：java、javac、appletviewer 三件套  
• 1998 JDK 1.2：引入 Collections，JRE 概念首次出现  
• 2004 JDK 5：泛型、注解，JRE 与 JDK 结构定型  
• 2014 JDK 8：Lambda、JRE 与 JDK 8u 广泛部署  
• 2017 JDK 9：模块系统 (JPMS)，JRE 变“运行时镜像”  
• 2021 JDK 17 LTS：长期支持，JRE 不再独立发布  
• 2023 JDK 21 LTS：最新 LTS，JRE 彻底融入 JDK

────────────────────────
5. 安装时到底选哪个？
| 场景 | 推荐包 |
|---|---|
| 只想跑 .jar / Tomcat / Elasticsearch | **JRE**（或直接用 JDK 亦可） |
| 写代码、编译、调试 | **JDK** |
| 容器镜像（Docker） | `openjdk:17-jdk-slim` 或 `17-jre`（极简运行时） |

────────────────────────
6. 小实验：区分 JDK 与 JRE
```bash
# 1) 仅装 JRE 的机器
which javac   # 找不到
java -version # 能跑 HelloWorld.jar

# 2) 安装 JDK 后
javac Hello.java  # 成功生成 Hello.class
java Hello        # 运行
```

────────────────────────
7. 常见坑
• “装了 JDK 却找不到 javac”  
  → PATH 没指到 `<jdk>/bin`。  
• “IDE 报错 Project SDK 不可用”  
  → 误把 JRE 路径当成 JDK。  
• Java 9+ 之后 `rt.jar` 消失  
  → 类库变成 `jmods/*.jmod`，用 `jlink` 裁剪运行时。

────────────────────────
8. 一句话总结
• **JRE = 跑 Java 的最小子集**  
• **JDK = JRE + 开发全家桶**  
• **JDK 11+ 之后，官方不再单独发布 JRE**，直接拿 JDK 即可。