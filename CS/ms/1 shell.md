```bash
# date 打印当前时间
missing:~$ date 
Fri 10 Jan 2020 11:49:31 AM EST

# echa 打印出传入的参数
# Shell 按空格分割命令。
missing:~$ echo hello 
hello

# 带空格
"My Snake" My\ Snake

# man 显示命令的手册页
missing:~$ man date
# 多数命令也支持 --help

# cd 切换目录
missing:~$ cd /bin
missing:/bin$ cd /
missing:/$ cd ~
missing:~$
# 绝对路径：以 `/` 开头
# 相对路径：不以 `/` 开头

# pwd 当前目录
missing:~$ pwd
/home/missing

# 父目录：.. 子目录：.
missing:~$ cd /
missing:/$ cd bin/../bin/../bin/././../bin/..
missing:/$

# ls 列出文件
missing:~$ ls /bin

# 基础操作
missing:~$ cat file # 打开
missing:~$ bat file # 打开，支持语法高亮
missing:~$ sort file # 按顺序打开
missing:~$ uniq file # 消除连续重复行
missing:~$ head file # 打印前几行
missing:~$ tail file # 打印后几行

# grep 搜索文本
missing:~$ grep pattern file
missing:~$ grep -i pattern file # 忽略大小写
missing:~$ grep "^D" file # 找以D开头
missing:~$ grep "[59]$" file # 找以5或9结尾
missing:~$ grep -r "homework" ./notes/ # 在文件夹中找
missing:~$ grep -v math students.txt # 找出不包含的

# sed 流编辑器
missing:~$ sed -i 's/pattern/replacement/g' file
# 把pattern换成replacement
# /g：全部替换；不加就只替换第一个

# find 查找文件
missing:~$ find ~/Downloads -type f -name "*.zip" -mtime +30
missing:~$ find ~ -type f -size +100M -exec ls -lh {} \;
missing:~$ find . -name "*.py" -exec grep -l "TODO" {} \;
# 查找 Downloads 目录中超过 30 天的 ZIP 文件
# 查找 home 目录中大于 100MB 的文件并列出
# 查找包含 "TODO" 的 py 文件

# awk 文件解析
missing:~$ awk '{print $2}' file
# 打印每行的第二列（以空白分隔）
missing:~$ awk -F, '{print $2}' file
# 指定逗号作为分隔符，打印第二列
```



## 五、组合工具：管道（Pipes）与重定向

### 1. 管道 `|`

管道将一个程序的标准输出连接到另一个程序的标准输入。

完整示例（从远程服务器分析 SSH 日志）：

```bash
missing:~$ ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' \
| sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' \
| sort | uniq -c \
| sort -nk1,1 | tail -n10 \
| awk '{print $2}' | paste -sd,
postgres,mysql,oracle,dell,ubuntu,inspur,test,admin,user,root
```

这个命令链：

1. 从远程服务器获取 SSH 日志
2. 用 `grep` 筛选断开连接的消息
3. 用 `sed` 提取用户名
4. 用 `sort | uniq -c` 统计出现次数
5. 按次数排序取前 10
6. 用 `awk` 提取用户名，`paste` 合并为逗号分隔列表

### 2. 重定向

- `> file`：将标准输出写入文件（覆盖）
- `>> file`：将标准输出追加到文件

### 3. tee 命令

```bash
verbose cmd | tee verbose.log | grep CRITICAL
```

`tee` 将标准输入同时输出到标准输出和文件。

---

## 六、Shell 编程语言（Bash）

Shell 也是一种完整的编程语言，支持变量、条件、循环和函数。

### 1. Shebang（释伴）

脚本第一行：

```bash
#!/bin/bash
```

当文件被执行时，系统会使用 `/bin/bash` 来运行脚本内容。也可以用于 Python 脚本：`#!/usr/bin/python`。

### 2. set - 严格模式

```bash
set -euo pipefail
```

- `-e`：命令失败时脚本立即退出
- `-u`：使用未定义变量时报错（而非空字符串）
- `-o pipefail`：管道中任何命令失败都导致脚本退出

### 3. 推荐工具

- 使用 `shellcheck` 检查 Shell 脚本错误
- LLM 可用于编写、调试 Shell 脚本，或在脚本超过 100 行时翻译为 Python
