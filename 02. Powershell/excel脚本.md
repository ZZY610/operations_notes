# excel脚本

写一个使用import excel模块的脚本，实现以下功能：
1、删除文件中某一列所有单元格前四个字符
2、将文件某一列或者某一行填充同样的数据，比如全部填充字符串jack（这里只是举例）
3、使用正则表达式过滤某一列数据（表达式可以由我自己写），过滤结果行可以导出。

可以使用 PowerShell 的 `ImportExcel` 模块来处理 Excel 文件中的数据，完成这三项功能。下面是一个完整的 PowerShell 脚本示例，详细解释了如何实现以下操作：

1. 删除某一列所有单元格前四个字符。
2. 将某一列或某一行填充同样的数据。
3. 使用正则表达式过滤某一列数据，并将过滤结果导出。

### 脚本示例

```powershell
# 首先确保已经安装了 ImportExcel 模块
# Install-Module -Name ImportExcel

# 设置文件路径和表格参数
$excelFilePath = "C:\path\to\your\file.xlsx"
$sheetName = "Sheet1"
$columnToModify = "B"  # 需要删除前4个字符的列 (例如B列)
$columnToFill = "C"    # 要填充数据的列 (例如C列)
$rowToFill = 5         # 如果要填充某一行，例如第5行
$fillData = "jack"     # 要填充的数据
$columnToFilter = "D"  # 要进行正则表达式过滤的列 (例如D列)
$regexPattern = "^[0-9]{4}"  # 示例正则表达式：过滤以4位数字开头的内容
$outputFilteredFile = "C:\path\to\filtered_output.xlsx"

# 导入 Excel 文件
$excel = Import-Excel -Path $excelFilePath -WorksheetName $sheetName

# 功能1: 删除某一列所有单元格的前四个字符
foreach ($row in $excel) {
    if ($row.$columnToModify -ne $null) {
        $row.$columnToModify = $row.$columnToModify.Substring(4)
    }
}

# 功能2: 填充某一列或某一行
# 填充某列的每个单元格
foreach ($row in $excel) {
    $row.$columnToFill = $fillData
}

# 或者填充某一行的所有单元格
$excel[$rowToFill] | ForEach-Object {
    $_.PSObject.Properties | ForEach-Object {
        $_.Value = $fillData
    }
}

# 功能3: 使用正则表达式过滤某一列数据
$filteredData = $excel | Where-Object {
    $_.$columnToFilter -match $regexPattern
}

# 导出过滤结果到新文件
$filteredData | Export-Excel -Path $outputFilteredFile -WorksheetName "FilteredResults"

# 将修改后的数据导出回 Excel
$excel | Export-Excel -Path $excelFilePath -WorksheetName $sheetName -Force

Write-Host "操作完成！"
```

### 脚本详细解释：

1. **删除某一列所有单元格前四个字符**：
   - 我们使用 `Substring(4)` 方法，删除列中每个单元格的前四个字符。
   
2. **填充某一列或某一行数据**：
   - 列填充：使用一个循环，将某列中的每个单元格填充为指定的字符串（例如 `jack`）。
   - 行填充：遍历指定行的所有单元格，并填充相同的数据。

3. **使用正则表达式过滤某一列数据**：
   - 使用 `Where-Object` 和 `-match` 运算符，应用正则表达式过滤指定列的内容。
   - 过滤后的结果通过 `Export-Excel` 命令导出为新的 Excel 文件。

### 脚本的实际运行

1. **路径配置**：修改 `$excelFilePath` 为实际的 Excel 文件路径。
2. **列、行配置**：根据你的需求，修改需要操作的列或行号。
3. **正则表达式**：你可以根据自己的需求更改 `$regexPattern` 来适应不同的过滤条件。


----


使用 PowerShell 和 ImportExcel 模块来读取指定的 Excel 文件，并计算某列数据的最大值、最小值、平均值和方差。

你可以使用 PowerShell 和 `ImportExcel` 模块来读取指定的 Excel 文件，并计算某列数据的最大值、最小值、平均值和方差。以下是实现该功能的 PowerShell 脚本，详细注释说明如何完成这一任务：

### 脚本功能：
1. **读取指定的 Excel 文件**。
2. **获取用户指定的列**。
3. **计算该列的最大值、最小值、平均值和方差**。

### PowerShell 脚本

```powershell
# 确保 ImportExcel 模块已经安装
# 安装命令： Install-Module -Name ImportExcel -Force -Scope CurrentUser

# 定义读取和计算Excel列数据的函数
function Calculate-ColumnStats {
    param (
        [string]$filePath,            # Excel 文件路径
        [string]$sheetName,           # 要读取的工作表名
        [string]$columnName           # 要计算的列名
    )

    # 导入Excel文件，并指定工作表
    $data = Import-Excel -Path $filePath -WorksheetName $sheetName

    # 检查列名是否存在
    if (-not ($data | Get-Member -MemberType Properties | Where-Object Name -eq $columnName)) {
        Write-Host "指定的列 '$columnName' 不存在于 Excel 文件中。" -ForegroundColor Red
        return
    }

    # 提取指定列的所有数据（假设是数值类型）
    $columnData = $data.$columnName | Where-Object { $_ -match '^\d+(\.\d+)?$' } | ForEach-Object { [double]$_ }

    if (-not $columnData) {
        Write-Host "指定列 '$columnName' 没有可用的数值数据。" -ForegroundColor Red
        return
    }

    # 计算统计信息
    $maxValue = $columnData | Measure-Object -Maximum | Select-Object -ExpandProperty Maximum
    $minValue = $columnData | Measure-Object -Minimum | Select-Object -ExpandProperty Minimum
    $averageValue = $columnData | Measure-Object -Average | Select-Object -ExpandProperty Average
    $count = $columnData.Count

    # 计算方差
    $mean = $averageValue
    $variance = ($columnData | ForEach-Object { ($_ - $mean) * ($_ - $mean) } | Measure-Object -Sum).Sum / $count

    # 输出结果
    Write-Host "统计信息:"
    Write-Host "最大值: $maxValue"
    Write-Host "最小值: $minValue"
    Write-Host "平均值: $averageValue"
    Write-Host "方差: $variance"
}

# 示例调用脚本
# 文件路径、工作表名称和列名需要根据实际情况填写
$filePath = "C:\path\to\your\file.xlsx"
$sheetName = "Sheet1"  # 假设工作表名为 Sheet1
$columnName = "数值列"  # 假设列名为 数值列

# 调用函数
Calculate-ColumnStats -filePath $filePath -sheetName $sheetName -columnName $columnName
```

### 详细说明：
1. **`Import-Excel`**：从指定的 Excel 文件中导入数据，返回一个包含数据的对象。
2. **`$data.$columnName`**：从导入的数据中提取用户指定的列内容。
3. **`Where-Object { $_ -match '^\d+(\.\d+)?$' }`**：对列数据进行过滤，确保它是数值类型，适用于纯数字计算。
4. **统计计算**：
    - `Measure-Object -Maximum/-Minimum/-Average`：分别计算最大值、最小值和平均值。
    - **方差**：方差是通过对所有数值与均值的差的平方求和后除以数据总数来计算的。

### 示例输入：
假设你有一个 Excel 文件 `testdata.xlsx`，其中 `Sheet1` 里有一列名为 "数值列"，你可以通过指定该列来计算它的最大值、最小值、平均值和方差。

### 示例输出：
```plaintext
统计信息:
最大值: 100
最小值: 1
平均值: 50.5
方差: 833.25
``` 

### 依赖：
确保已经安装了 `ImportExcel` 模块，可以通过以下命令安装：
```powershell
Install-Module -Name ImportExcel -Force -Scope CurrentUser
``` 

这个脚本适用于处理数值型的 Excel 列，并可以根据用户指定的列名灵活计算。