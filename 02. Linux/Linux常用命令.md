# Linux常用命令
## which 
输出shell命令的完整路径。
## pkill
根据进程名或其他条件（PID、进程组ID）来定位进程。

## iotop
监控磁盘IO使用状态。

## file
`file` 命令用于确定文件类型，它通过检查文件的内容和特征来确定文件的类型。这个命令通常用于确定文件是否为文本文件、二进制文件，或者识别文件的编码格式等。以下是 `file` 命令的详细解释：

**命令语法：**
```bash
file [选项] 文件名
```

**常用选项：**
- `-b`：不显示文件名，仅显示文件类型信息。
- `-i`：以 MIME 类型输出文件类型信息。
- `-z`：尝试压缩文件类型信息。
- `-k`：不区分大小写。
- `-L`：对符号链接进行操作，而不是它们所指向的文件。

**示例用法：**

1. 查看文件类型：
   ```bash
   file example.txt
   ```

   这将输出文件类型信息，例如 "example.txt: ASCII text" 表示文件为纯文本文件。

2. 使用 `-i` 选项以 MIME 类型输出文件信息：
   ```bash
   file -i example.pdf
   ```

   输出结果类似于 "example.pdf: application/pdf; charset=binary"。

3. 批量处理多个文件：
   ```bash
   file -b *
   ```

   这将批量处理当前目录下的所有文件，并仅显示文件类型信息。

4. 查看压缩文件的信息：
   ```bash
   file -z compressed.gz
   ```

   这将显示压缩文件的类型信息。

5. 不区分大小写查看文件类型：
   ```bash
   file -k Example.TXT
   ```

   这将不区分大小写地检查文件类型。

## netstat

## ss