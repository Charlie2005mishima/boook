## 一、命令行接口 (CLI) 基础

### 1. 参数 (Arguments)

```bash
ls -l folder/
```

执行 `/bin/ls`，参数为 `['-l', 'folder/']`。

在 Shell 脚本中访问参数：

- `$1`：第一个参数
- `$2`：第二个参数
- `$@`：所有参数组成的列表
- `$#`：参数个数
- `$0`：程序名称

**Flag 惯例**：以 `-`（短选项）或 `--`（长选项）开头。

- `ls -l` 与 `ls --all`
- 短选项可合并：`ls -l -a` 等价于 `ls -la`
- 常见 flag：`--help`、`--verbose`、`--version`

**变长参数**：命令可接受同类型的变长参数。

```bash
mkdir src # 新建文件夹
mkdir docs
# 等价于
mkdir src docs
mkdir -p a/b/c # 越级建文件夹
```

## 二、通配符 (Globbing)

Glob 是 Shell 在执行程序前展开的特殊模式。

### 1. 通配符 `*`

```bash
rm *.py # 删除
```

### 2. 通配符 `?`

匹配恰好一个任意字符。

### 3. 花括号 `{}`

```bash
touch folder/{a,b,c}.py # 创建
convert image.{png,jpg} # 转化
cp /path/to/project/{setup,build,deploy}.sh /newpath # 拷贝
mv *{.py,.sh} folder # 移动
```

**高级 glob**：zsh 等 Shell 支持 `**` 表示递归路径。

```bash
rm **/*.py  # 递归删除所有 .py 文件
```

## 三、流 (Streams)

管道 `|` 连接程序时，所有程序同时运行，Shell 将数据流从前一个程序传到后一个。

### 1. 管道中的并发

```bash
$ (sleep 15 && cat numbers.txt) | grep -P '^\d$' | sort | uniq &
# 管道敲下后就全部开始启动
[1] 12345
$ ps | grep -P '(sleep|cat|grep|sort|uniq)'
32930 pts/1 00:00:00 sleep
32931 pts/1 00:00:00 grep
32932 pts/1 00:00:00 sort
32933 pts/1 00:00:00 uniq
32948 pts/1 00:00:00 grep
```

除了 `cat` 等待 `sleep` 完成外，其他进程立即启动。

### 2. 标准流

- **stdin**（标准输入）：每个程序都有。
- `-` 作为文件名表示“从 stdin 读取”
- **stdout**（标准输出）：用于管道传输。
- **stderr**（标准错误）：用于警告和错误信息，**不会**被管道传递。

```bash
echo "hello" | grep "hello"
echo "hello" | grep "hello" -  # 等价$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ ls /nonexistent | grep "pattern"
ls: cannot access '/nonexistent': No such file or directory
# 错误信息仍显示，因为 stderr 未被管道传递
```

### 3. 重定向语法

```bash
# 重定向 stdout 到文件（覆盖）
echo "hello" > output.txt

# 重定向 stdout 到文件（追加）
echo "world" >> output.txt

# 重定向 stderr 到文件
ls foobar 2> errors.txt

# 同时重定向 stdout 和 stderr 到同一文件
ls foobar &> all_output.txt

# 从文件重定向 stdin
grep "pattern" < input.txt

# 丢弃输出（重定向到 /dev/null）
cmd > /dev/null 2>&1
```

### 4. fzf - 模糊查找器

从 stdin 读取行，提供交互式界面进行过滤和选择。

```bash
$ ls | fzf
$ cat ~/.bash_history | fzf
```

---

## 四、环境变量 (Environment Variables)

### 1. 变量赋值与访问

```bash
foo=bar          # 赋值（注意 = 两边不能有空格）
echo "$foo"      # 打印 bar
echo '$foo'      # 打印 $foo（单引号是字面量）
```

### 2. 命令替换 (Command Substitution)

```bash
files=$(ls)
echo "$files" | grep README
echo "$files" | grep ".py"
```

`ls` 的 stdout 存入 `$files` 变量。

### 3. 进程替换 (Process Substitution)

`<(CMD)` 执行 CMD，将输出放入临时文件，并用文件名替换 `<()`。

```bash
diff <(ls src) <(ls docs)
# 显示 src 和 docs 目录中文件的差异
```

### 4. 环境变量传递

```bash
printenv  # 查看当前所有环境变量

# 为单个子命令设置环境变量
TZ=Asia/Tokyo date  # 打印东京时间
echo $TZ            # 为空，因为只对子命令生效

# 使用 export 永久修改当前环境
export DEBUG=1      # 所有子进程继承 DEBUG=1
bash -c 'echo $DEBUG'  # 打印 1

unset DEBUG         # 删除变量
```

环境变量惯例用**全大写**（如 `HOME`、`PATH`、`DEBUG`）。

---

## 五、返回码 (Return Codes)

惯例：**返回码 0 表示成功，非零表示出错**。

### 1. 访问返回码

```bash
$?  # 上一条命令的返回码
```

### 2. `&&` 和 `||` 运算符

基于返回码进行短路运算。

```bash
# grep 成功时才执行 echo
grep -q "pattern" file.txt && echo "Pattern found"

# grep 失败时才执行 echo
grep -q "pattern" file.txt || echo "Pattern not found"

# true 总是成功
true && echo "This will always print"

# false 总是失败
false || echo "This will always print"
```

### 3. if 和 while 语句

```bash
# if 使用条件的返回码（0=true，非0=false）
if grep -q "pattern" file.txt; then
    echo "Found"
fi

# while 在命令返回 0 时继续循环
while read line; do
    echo "$line"
done < file.txt
```

---

## 六、信号 (Signals)

### 1. SIGINT（Ctrl-C）

Shell 发送 SIGINT 信号中断进程。

```bash
$ sleep 100
^C
$
```

### 2. Python 捕获 SIGINT 示例

```python
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

运行效果：

```bash
$ python sigint.py
24^C I got a SIGINT, but I am not stopping
26^C I got a SIGINT, but I am not stopping
30^\[1] 39913 quit python sigint.py
```

用 `Ctrl-\` 发送 **SIGQUIT** 强制退出。

### 3. 常用信号

- **SIGTERM**：请求进程优雅退出，用 `kill -TERM <pid>` 发送
- **SIGSTOP**：暂停进程
- **SIGTSTP**（Ctrl-Z）：终端暂停信号

### 4. 作业控制

```bash
$ sleep 1000
^Z
[1] + 18653 suspended sleep 1000

$ nohup sleep 2000 &
[2] 18745 appending output to nohup.out

$ jobs
[1] + suspended sleep 1000
[2] - running nohup sleep 2000

$ kill -SIGHUP %1   # 发送 SIGHUP 给作业 1
[1] + 18653 hangup sleep 1000

$ kill -SIGHUP %2   # nohup 保护免受 SIGHUP

$ jobs
[2] + running nohup sleep 2000

$ kill %2           # 终止作业 2
[2] + 18745 terminated nohup sleep 2000
```

- `fg`：将暂停的作业放到前台
- `bg`：将暂停的作业放到后台
- `jobs`：列出当前终端会话的未完成作业
- `&` 后缀：在后台运行命令
- `$!`：最后一个后台作业的 PID
- **SIGKILL**：无法被捕获，立即终止进程

### 5. trap - 信号捕获

```bash
#!/usr/bin/env bash
cleanup() {
    echo "Cleaning up temporary files..."
    rm -f /tmp/mytemp.*
}
trap cleanup EXIT        # 脚本退出时运行
trap cleanup SIGINT SIGTERM  # Ctrl-C 或 kill 时运行
```

---

## 七、远程机器 (SSH)

### 1. 基本连接

```bash
ssh alice@server.mit.edu
```

### 2. 非交互式命令执行

```bash
# ls 在远程运行，wc 在本地运行
ssh alice@server ls | wc -l

# ls 和 wc 都在远程运行
ssh alice@server 'ls | wc -l'
```

### 3. SSH 密钥认证

```bash
# 生成 ED25519 密钥对
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519

# 复制公钥到服务器
cat .ssh/id_ed25519.pub | ssh alice@remote 'cat >> ~/.ssh/authorized_keys'

# 或使用 ssh-copy-id（更简单）
ssh-copy-id -i .ssh/id_ed25519 alice@remote
```

私钥（`~/.ssh/id_rsa` 或 `~/.ssh/id_ed25519`）相当于密码，绝不能分享。

### 4. 文件传输

```bash
# scp - 传统工具
scp path/to/local_file remote_host:path/to/remote_file

# rsync - 改进版，检测相同文件避免重复复制，支持断点续传
rsync path/to/local_file remote_host:path/to/remote_file
```

### 5. SSH 客户端配置 (`~/.ssh/config`)

```bash
Host vm
    User alice
    HostName 172.16.174.141
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host *.mit.edu
    User alice
```

推荐使用 **Mosh** 作为 SSH 替代，能处理断线、网络切换和高延迟链路。

---

## 八、终端复用器 (tmux)

### 1. 会话 (Sessions)

```bash
tmux                    # 启动新会话
tmux new -s NAME        # 指定名称启动
tmux ls                 # 列出当前会话
tmux a                  # 附加到最后会话
tmux a -t NAME          # 附加到指定会话
```

tmux 快捷键都以 **Ctrl+b** 为前缀（按下并释放后再按下一个键）：

- `Ctrl+b d`：脱离当前会话

### 2. 窗口 (Windows) - 类似编辑器标签页

- `Ctrl+b c`：创建新窗口
- `Ctrl+b N`：切换到第 N 个窗口（数字编号）
- `Ctrl+b p`：切换到上一个窗口
- `Ctrl+b n`：切换到下一个窗口
- `Ctrl+b ,`：重命名当前窗口
- `Ctrl+b w`：列出所有窗口

### 3. 面板 (Panes) - 类似 Vim 分屏

- `Ctrl+b "`：水平分割当前面板
- `Ctrl+b %`：垂直分割当前面板
- `Ctrl+b <方向键>`：移动到指定方向的面板
- `Ctrl+b z`：切换当前面板的缩放
- `Ctrl+b [`：进入滚动模式，按空格开始选择，回车复制
- `Ctrl+b <空格>`：循环切换面板布局

---

## 九、Shell 自定义

### 1. Dotfiles（点文件）

以 `.` 开头的配置文件，默认被 `ls` 隐藏。

常见 dotfiles：

- `~/.bashrc`、`~/.bash_profile` - Bash 配置
- `~/.gitconfig` - Git 配置
- `~/.vimrc`、`~/.vim/` - Vim 配置
- `~/.ssh/config` - SSH 配置
- `~/.tmux.conf` - tmux 配置

### 2. 修改 PATH

```bash
export PATH="$PATH:path/to/append"
```

### 3. 包管理器

- macOS：Homebrew
- Ubuntu/Debian：apt
- Fedora：dnf
- Arch：pacman

```bash
# 安装 ripgrep（更好的 grep）和 fd（更好的 find）
brew install ripgrep
brew install fd
```

安装后可用 `rg` 替代 `grep`，`fd` 替代 `find`。

### 4. `curl | bash` 警告

```bash
# 危险：直接执行未审查的代码
curl -fsSL https://example.com/install.sh | bash

# 安全做法：先下载，审查，再执行
curl -fsSL https://example.com/install.sh -o install.sh
less install.sh  # 审查脚本
bash install.sh
```

### 5. 命令查找

- `command-not-found.com`：搜索命令在各包管理器中的安装方法
- `tldr`：简化版 man 手册，展示常见用法示例

```bash
$ tldr fd
An alternative to find. Aims to be faster and easier to use than find.
Recursively find files matching a pattern in the current directory:
    fd "pattern"
Find files that begin with "foo":
    fd "^foo"
Find files with a specific extension:
    fd --extension txt
```

### 6. 别名 (Aliases)

```bash
alias alias_name="command_to_alias arg1 arg2"

# 常见别名
alias ll="ls -lh"
alias gs="git status"
alias gc="git commit"
alias sl=ls           # 纠正拼写错误
alias mv="mv -i"      # 覆盖前提示
alias mkdir="mkdir -p" # 自动创建父目录
alias df="df -h"      # 人类可读格式

# 别名组合
alias la="ls -A"
alias lla="la -l"

# 忽略别名（使用原始命令）
\ls

# 删除别名
unalias la

# 查看别名定义
alias ll  # 打印 ll='ls -lh'
```

**别名限制**：不能在命令中间插入参数，复杂场景应使用 Shell 函数。

### 7. 历史搜索

- `Ctrl+R`：反向历史搜索
- 配合 fzf 集成后，`Ctrl+R` 变为模糊搜索整个历史

### 8. Dotfiles 组织建议

- 放在独立文件夹中，用 Git 版本控制
- 用脚本创建符号链接
- 优点：易安装、可移植、可同步、有变更历史

### 9. Shell 插件与框架

**框架**：

- prezto
- oh-my-zsh

**插件**：

- `zsh-syntax-highlighting`：输入时高亮有效/无效命令
- `zsh-autosuggestions`：根据历史自动建议命令
- `zsh-completions`：额外的补全定义
- `zsh-history-substring-search`：类似 fish 的历史搜索
- `powerlevel10k`：快速、可定制的提示符主题

> 不需要安装庞大的框架（如 oh-my-zsh）来获得这些功能，单独安装插件通常更快、控制力更强。大型框架会显著拖慢 Shell 启动速度。

Shell 如 **fish** 默认包含了许多上述功能。
