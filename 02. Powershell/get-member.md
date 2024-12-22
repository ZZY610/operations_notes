# get-member
`Get-Member` 是 PowerShell 中用来查看对象成员（属性、方法、事件等）的一个重要命令。它允许你查看传入对象的属性和方法，以及更多详细信息。`Get-Member` 通过管道接受对象，或者直接通过 `-InputObject` 参数指定对象。它具有多个参数，用于自定义输出或限制结果范围。

### `Get-Member` 的常用参数：

1. **`-Name`**
   - **作用**：仅显示对象的成员名称，而不是完整的定义（默认会显示名称、类型和定义）。
   - **用法**：
     ```powershell
     Get-Process | Get-Member -Name
     ```
   - **示例输出**：
     ```
     Handles
     Id
     ProcessName
     ```

2. **`-MemberType`**
   - **作用**：只显示指定类型的成员。例如，只显示属性、方法、脚本方法等。
   - **可用值**：
     - `AliasProperty`、`CodeProperty`、`Property`、`Method`、`NoteProperty`、`ScriptMethod`、`Event`、`Field` 等等。
   - **用法**：
     ```powershell
     Get-Process | Get-Member -MemberType Property
     ```
   - **示例输出**：
     ```
     Handles     Property    System.Int32 Handles {get;}
     Id          Property    System.Int32 Id {get;}
     ProcessName Property    System.String ProcessName {get;}
     ```

3. **`-Force`**
   - **作用**：显示所有隐藏的成员，包括从基类继承的成员、非公共成员。
   - **用法**：
     ```powershell
     Get-Process | Get-Member -Force
     ```
   - **示例输出**：
     ```
     BasePriority       Property System.Int32 BasePriority {get;}
     EnableRaisingEvents Property System.Boolean EnableRaisingEvents {get;set;}
     HasExited          Property System.Boolean HasExited {get;}
     ...
     ```

4. **`-InputObject`**
   - **作用**：直接指定一个对象而不是从管道传递对象时使用。
   - **用法**：
     ```powershell
     $process = Get-Process -Id 1
     Get-Member -InputObject $process
     ```
   - **示例输出**：
     和通过管道传递一样，列出该对象的成员。

5. **`-Static`**
   - **作用**：显示对象的静态成员。静态成员是属于类型而不是实例的成员。
   - **用法**：
     ```powershell
     [Math] | Get-Member -Static
     ```
   - **示例输出**：
     ```
     Abs        Method     static System.Double Abs(double value)
     Acos       Method     static System.Double Acos(double d)
     Cos        Method     static System.Double Cos(double d)
     ...
     ```

6. **`-View`**
   - **作用**：指定显示的视图。默认视图为 `Extended`，会显示对象的脚本属性、脚本方法等扩展成员。
   - **可用值**：
     - `Base`：显示基础成员，即不包括扩展成员。
     - `Extended`：显示扩展成员，如脚本属性和方法。
   - **用法**：
     ```powershell
     Get-Process | Get-Member -View Base
     ```
   - **示例输出**：
     ```
     Handles     Property   System.Int32 Handles {get;}
     Id          Property   System.Int32 Id {get;}
     ProcessName Property   System.String ProcessName {get;}
     ```

7. **`-TypeDefinition`**
   - **作用**：显示完整的类型定义，包括继承关系、接口、属性、方法等详细信息。这个参数不直接用于 `Get-Member`，而是通过 `Add-Member` 添加的自定义对象使用。

### 示例用法和解释：

1. **显示对象的所有属性（`-MemberType Property`）**：
   ```powershell
   Get-Process | Get-Member -MemberType Property
   ```
   这个命令只显示进程对象中的所有属性，而不会显示方法或事件。

2. **查看静态成员（`-Static`）**：
   ```powershell
   [Math] | Get-Member -Static
   ```
   查看 `System.Math` 类型的静态方法，如 `Abs()`、`Sin()` 等。

3. **查看继承和隐藏的成员（`-Force`）**：
   ```powershell
   Get-Process | Get-Member -Force
   ```
   使用 `-Force` 参数可以查看到隐藏的或从基类继承下来的成员，例如内部字段、保护属性等。

4. **过滤特定的成员类型（`-MemberType Method`）**：
   ```powershell
   Get-Process | Get-Member -MemberType Method
   ```
   只显示进程对象的所有方法，而不显示属性或事件。

### `Get-Member` 命令的完整参数表：

| 参数 | 作用 |
| --- | --- |
| `-Name` | 只显示成员名称 |
| `-MemberType` | 筛选成员类型（属性、方法、事件等） |
| `-Force` | 显示隐藏和继承的成员 |
| `-InputObject` | 直接指定一个对象 |
| `-Static` | 显示静态成员 |
| `-View` | 选择查看基本成员（`Base`）或扩展成员（`Extended`） |

### 总结：
`Get-Member` 是 PowerShell 中非常强大的命令，用来深入了解对象的结构。无论是查看对象的属性、方法，还是查看静态成员，或者是探索隐藏成员，`Get-Member` 都是你了解 PowerShell 对象系统的关键工具。