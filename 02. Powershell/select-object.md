# select-object
`Select-Object` 是 PowerShell 中一个非常常用的 cmdlet，用于选择对象的特定属性、前几项或后几项结果，甚至创建自定义属性。它在数据筛选和处理过程中扮演了重要角色。下面是对 `Select-Object` 的详细介绍，并通过多个实例展示其作用。

### 1. 基本语法

```powershell
Select-Object [-Property] <string[]> [-ExcludeProperty <string[]>] [-ExpandProperty <string>] [-First <int>] [-Last <int>] [-Skip <int>] [-Unique]
```

#### 主要参数：
- `-Property`：指定要选择的属性。
- `-First`：选择结果集的前 N 项。
- `-Last`：选择结果集的后 N 项。
- `-Skip`：跳过前 N 项结果。
- `-ExpandProperty`：展开并选择某个属性的值，而不是对象本身。
- `-Unique`：只选择唯一的项，去重。

### 2. `Select-Object` 的主要用途

#### 用途 1: 选择特定属性

当我们只对某些属性感兴趣时，可以使用 `-Property` 参数。

**示例:**
```powershell
Get-Process | Select-Object -Property Name, Id, CPU
```
这将从 `Get-Process` 的输出中只选择进程的名称、ID 和 CPU 占用。

#### 用途 2: 获取前 N 项或后 N 项

- `-First` 参数用于选择前 N 项。
- `-Last` 参数用于选择最后 N 项。

**示例 1: 获取前 3 项**
```powershell
Get-Service | Select-Object -First 3
```

**示例 2: 获取最后 2 项**
```powershell
Get-Service | Select-Object -Last 2
```

#### 用途 3: 跳过前 N 项

使用 `-Skip` 参数可以跳过结果集的前 N 项。

**示例:**
```powershell
Get-EventLog -LogName System | Select-Object -Skip 5
```
这将跳过前 5 条系统日志并显示其余的日志。

#### 用途 4: 去重（Unique）

`-Unique` 参数可以用于去除重复项，保留唯一项。

**示例:**
```powershell
Get-EventLog -LogName System | Select-Object -Property Source -Unique
```
此命令会输出唯一的日志源。

#### 用途 5: 展开属性

有时对象的某些属性本身是嵌套对象或集合。使用 `-ExpandProperty` 可以直接展开并显示该属性的值。

**示例:**
```powershell
Get-Process | Select-Object -ExpandProperty StartTime
```
这将直接输出进程的启动时间，而不是包含启动时间的对象。

#### 用途 6: 创建自定义属性

通过 `Select-Object` 还能创建自定义属性，这在需要修改或添加字段时非常有用。

**示例:**
```powershell
Get-Process | Select-Object Name, @{Name="CPU Usage (s)"; Expression={($_.CPU)}}
```
这里创建了一个名为 `CPU Usage (s)` 的自定义列，用来展示 CPU 使用时间。

### 3. 实例集锦

#### 实例 1: 选择进程的特定属性并限制结果数
```powershell
Get-Process | Select-Object -Property Name, Id -First 5
```
输出前 5 个进程的名称和 ID。

#### 实例 2: 获取最后 3 个服务的名称
```powershell
Get-Service | Select-Object -Property Name -Last 3
```
输出最后 3 个服务的名称。

#### 实例 3: 跳过前 10 个日志并输出接下来的 5 个
```powershell
Get-EventLog -LogName Application | Select-Object -Skip 10 -First 5
```
此命令跳过前 10 条应用日志，并显示接下来的 5 条。

#### 实例 4: 获取唯一的日志源
```powershell
Get-EventLog -LogName System | Select-Object -Property Source -Unique
```
输出唯一的日志源，去除重复项。

#### 实例 5: 展开属性值
```powershell
Get-Service | Select-Object -ExpandProperty DisplayName
```
只输出服务的显示名称，直接展开这个属性。

#### 实例 6: 创建自定义属性（添加计算字段）
```powershell
Get-Process | Select-Object Name, @{Name="Memory (MB)"; Expression={($_.WS / 1MB)}}
```
该命令创建了一个名为 `Memory (MB)` 的自定义列，显示进程使用的内存（以 MB 为单位）。

### 4. 结论

`Select-Object` 是一个功能强大的工具，可以帮助你选择、过滤和修改命令输出的内容，灵活定制所需的结果。掌握其多种用法，尤其是创建自定义属性和通过 `-ExpandProperty` 处理复杂对象，将极大地提高数据处理效率。