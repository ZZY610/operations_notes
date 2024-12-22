# excel测试文件

```
# 导入ImportExcel模块
Import-Module ImportExcel

# 设置输出的 Excel 文件路径
$outputFile = "C:\path\to\your\simple_numbers.xlsx"

# 创建一个简单的数字数组（每一行都是一个数字）
$data = @()

# 循环生成10行数字数据
for ($i = 1; $i -le 10; $i++) {
    $row = [pscustomobject]@{
        "数字" = $i
    }
    $data += $row
}

# 导出到 Excel 文件
$data | Export-Excel -Path $outputFile -WorksheetName "数字表格" -AutoSize

# 提示操作完成
Write-Host "Excel文件已生成，路径：$outputFile"

```

```powershell
# 设置输出的 Excel 文件路径
$outputFile = "D:\testdata2.xlsx"

# 导入ImportExcel模块
Import-Module ImportExcel

# 创建一个函数，用来生成多种数据类型的测试数据
function Generate-TestData {
    # 创建一个空的数组用于存储数据
    $data = @()

    # 添加列名（第一行）
    $columns = @{
        "日期" = Get-Date -Format "yyyy-MM-dd"
        "数字" = Get-Random -Minimum 100 -Maximum 999
        "字符串" = "TestString0"
        "布尔值" = $true
        "货币" = 1234.56
        "百分比" = "50%"
        "超链接" = "https://example.com"
    }
    
    # 添加列名到数据集
    $data += [pscustomobject]$columns

    # 生成一些测试数据
    for ($i = 1; $i -le 10; $i++) {
        # 生成每一行的数据
        $row = @{
            "日期" = (Get-Date).AddDays($i)                   # 日期类型
            "数字" = Get-Random -Minimum 100 -Maximum 999      # 随机数字
            "字符串" = "TestString" + $i                       # 字符串
            "布尔值" = ($i % 2 -eq 0)                         # 布尔值
            "货币" = (Get-Random -Minimum 1000 -Maximum 9999)  # 货币值
            "百分比" = ("{0:P2}" -f ((Get-Random -Minimum 1 -Maximum 100)/100))  # 百分比
            "超链接" = "https://example.com"                   # 超链接
        }
        
        # 添加行到数据集
        $data += [pscustomobject]$row
    }

    # 返回生成的数据
    return $data
}

# 生成多个sheet页的数据
$sheet1Data = Generate-TestData

# 导出第一个sheet页的数据
$sheet1Data | Export-Excel -Path $outputFile -WorksheetName "Sheet1" -AutoSize -TableName "Data1"

```
