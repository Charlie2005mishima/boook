## 开发环境与工具

### 一、开发环境概述

开发环境是一组用于开发软件的工具集合。其核心是**文本编辑功能**，并辅以语法高亮、类型检查、代码格式化和自动补全等特性。

集成开发环境（IDE）如 **VS Code** 将所有这些功能整合到单个应用程序中。

基于终端的开发工作流则组合了多种工具，例如：

- **tmux**（终端复用器）
- **Vim**（文本编辑器）
- **Zsh**（Shell）
- 语言特定的命令行工具，如 **Ruff**（Python linter 和格式化工具）和 **Mypy**（Python 类型检查器）

**IDE 与终端工作流的对比**：

- 图形化 IDE 更易学习，且通常拥有更好的开箱即用 AI 集成（如 AI 自动补全）
- 终端工作流更轻量，在没有 GUI 或无法安装软件的环境中可能是唯一选择

建议对两者都有基本的了解，并精通其中至少一个。如果没有偏好的 IDE，推荐从 **VS Code** 开始。

---

### 二、文本编辑与 Vim

编程时，大部分时间花在**代码导航、阅读代码片段和编辑代码**上，而非从头到尾阅读文件或编写长段代码。

Vim 是一个针对这种任务分布而优化的文本编辑器。

#### 2.1 Vim 的设计哲学

Vim 有一个漂亮的设计理念：**其界面本身就是一门编程语言**，专为导航和编辑文本而设计。

- 按键（带有助记名称）是**命令**，这些命令是**可组合的**
- Vim 避免使用鼠标，因为鼠标太慢
- Vim 甚至避免使用方向键，因为需要过多的手指移动
- 最终效果：一个感觉像**脑机接口**的编辑器，能跟上你的思考速度

#### 2.2 其他软件中的 Vim 支持

许多涉及文本编辑的程序都支持 **“Vim 模式”**，无论是作为内置功能还是通过插件：

- VS Code 有 **VSCodeVim** 插件
- Zsh 内置支持 Vim 仿真
- Claude Code 也内置支持 Vim 编辑器模式

#### 2.3 模态编辑（Modal Editing）

Vim 是一个**模态编辑器**，针对不同任务有不同的操作模式：

| 模式                                 | 用途                     |
| ------------------------------------ | ------------------------ |
| **Normal（普通模式）**         | 在文件中移动和进行编辑   |
| **Insert（插入模式）**         | 插入文本                 |
| **Replace（替换模式）**        | 替换文本                 |
| **Visual（可视模式）**         | 选择文本块（普通/行/块） |
| **Command-line（命令行模式）** | 运行命令                 |

不同模式下按键有不同的含义。例如，字母 `x` 在 Insert 模式下插入字符“x”，在 Normal 模式下删除光标下的字符，在 Visual 模式下删除选中的内容。

**模式切换**：

- 按 `<ESC>` 从任何模式返回 Normal 模式
- 从 Normal 模式：
  - `i` → Insert 模式
  - `R` → Replace 模式
  - `v` → Visual 模式
  - `V` → Visual Line 模式
  - `<Ctrl-v>` → Visual Block 模式
  - `:` → Command-line 模式

默认配置下，Vim 在左下角显示当前模式。初始/默认模式是 Normal 模式。

> 💡 **提示**：`<ESC>` 键使用频繁，建议将 **Caps Lock** 映射为 Escape，或设置其他替代映射。

#### 2.4 基础：插入文本

从 Normal 模式按 `i` 进入 Insert 模式。此时 Vim 的行为与普通文本编辑器无异，直到按 `<ESC>` 返回 Normal 模式。

这是开始使用 Vim 编辑文件所需的所有基础知识（尽管如果一直待在 Insert 模式，效率并不高）。

#### 2.5 Vim 的界面是一门编程语言

Vim 的界面是一门编程语言：按键是命令，命令可以组合。

##### 2.5.1 移动（Movement）

大部分时间应待在 Normal 模式，使用移动命令导航文件。

Vim 中的移动也称为 **“名词”**（nouns），因为它们指向文本块。

| 类别               | 命令                           | 说明                       |
| ------------------ | ------------------------------ | -------------------------- |
| **基本移动** | `h` `j` `k` `l`        | 左、下、上、右             |
| **单词**     | `w`                          | 下一个单词                 |
|                    | `b`                          | 单词开头                   |
|                    | `e`                          | 单词结尾                   |
| **行**       | `0`                          | 行首                       |
|                    | `^`                          | 行首第一个非空白字符       |
|                    | `$`                          | 行尾                       |
| **屏幕**     | `H`                          | 屏幕顶部                   |
|                    | `M`                          | 屏幕中间                   |
|                    | `L`                          | 屏幕底部                   |
| **滚动**     | `<Ctrl-u>`                   | 向上滚动                   |
|                    | `<Ctrl-d>`                   | 向下滚动                   |
| **文件**     | `gg`                         | 文件开头                   |
|                    | `G`                          | 文件结尾                   |
| **行号**     | `:{number}` 或 `{number}G` | 跳转到第 {number} 行       |
| **匹配**     | `%`                          | 跳转到匹配的括号/花括号    |
| **查找**     | `f{char}` / `t{char}`      | 在当前行向前查找/到 {char} |
|                    | `F{char}` / `T{char}`      | 在当前行向后查找/到 {char} |
|                    | `,` / `;`                  | 在匹配结果中导航           |
| **搜索**     | `/{regex}`                   | 搜索正则表达式             |
|                    | `n` / `N`                  | 在搜索结果中导航           |

##### 2.5.2 选择（Selection）

可视模式选择：

- `v` → Visual 模式
- `V` → Visual Line 模式
- `<Ctrl-v>` → Visual Block 模式

可以使用移动键来扩展选择。

##### 2.5.3 编辑（Edits）

所有以前用鼠标完成的操作，现在都用键盘通过**与移动命令组合的编辑命令**来完成。Vim 的编辑命令也称为 **“动词”**（verbs），因为动词作用于名词。

| 命令          | 说明                                            |
| ------------- | ----------------------------------------------- |
| `i`         | 进入 Insert 模式                                |
| `o` / `O` | 在下方/上方插入新行                             |
| `d{motion}` | 删除 {motion}                                   |
|               | `dw` = 删除单词                               |
|               | `d$` = 删除到行尾                             |
|               | `d0` = 删除到行首                             |
| `c{motion}` | 修改 {motion}（相当于`d{motion}` 后跟 `i`） |
|               | `cw` = 修改单词                               |
| `x`         | 删除字符（相当于`dl`）                        |
| `s`         | 替换字符（相当于`cl`）                        |
| Visual +`d` | 删除选中内容                                    |
| Visual +`c` | 修改选中内容                                    |
| `u`         | 撤销                                            |
| `<Ctrl-r>`  | 重做                                            |
| `y`         | 复制/“猛拉”（yank）                           |
| `p`         | 粘贴                                            |
| `~`         | 翻转字符大小写                                  |
| `J`         | 合并行                                          |

##### 2.5.4 计数（Counts）

可以用**数字**将名词和动词组合，执行多次操作：

- `3w` → 向前移动 3 个单词
- `5j` → 向下移动 5 行
- `7dw` → 删除 7 个单词

##### 2.5.5 修饰符（Modifiers）

修饰符可以改变名词的含义：

- `i`（inner/内部）→ 匹配括号/引号**内部**的内容
- `a`（around/周围）→ 匹配括号/引号**包括**符号本身

示例：

- `ci(` → 修改当前圆括号对内部的内容
- `ci[` → 修改当前方括号对内部的内容
- `da'` → 删除单引号字符串（包括引号本身）

##### 2.5.6 综合示例：修复 Fizz Buzz

以下是一个有问题的 [fizz buzz](https://en.wikipedia.org/wiki/Fizz_buzz) 实现：

```python
def fizz_buzz(limit):
    for i in range(limit):
        if i % 3 == 0:
            print("fizz", end="")
        if i % 5 == 0:
            print("fizz", end="")
        if i % 3 and i % 5:
            print(i, end="")
        print()

def main():
    fizz_buzz(20)
```

从 Normal 模式开始，用以下命令序列修复问题：

| 问题                            | 命令                                      | 说明                 |
| ------------------------------- | ----------------------------------------- | -------------------- |
| **`main()` 从未被调用** | `G`                                     | 跳转到文件末尾       |
|                                 | `o`                                     | 在下方打开新行       |
|                                 | 输入`if __name__ == "__main__": main()` | 在 Insert 模式下输入 |
|                                 | `<ESC>`                                 | 返回 Normal 模式     |
| **从 0 开始而非 1**       | `/range`                                | 搜索 “range”       |
|                                 | `ww`                                    | 向前移动两个单词     |
|                                 | `i`                                     | 进入 Insert 模式     |
|                                 | 添加`1, `                               | 在`range` 后插入   |
|                                 | `<ESC>`                                 | 返回 Normal 模式     |
|                                 | `e`                                     | 跳转到下一个单词末尾 |
|                                 | `a`                                     | 开始追加文本         |
|                                 | 添加`+ 1`                               |                      |
|                                 | `<ESC>`                                 | 返回 Normal 模式     |
| **5 的倍数打印 “fizz”** | `:6`                                    | 跳转到第 6 行        |
|                                 | `ci"`                                   | 修改双引号内部的内容 |
|                                 | 改为`"buzz"`                            |                      |
|                                 | `<ESC>`                                 | 返回 Normal 模式     |

#### 2.6 学习 Vim

学习 Vim 的最佳方式是掌握基础知识（以上内容），然后在所有软件中启用 Vim 模式并开始在实际中使用。

**避免**使用鼠标或方向键。在某些编辑器中，甚至可以**禁用方向键**来强制养成好习惯。

**额外资源**：

- [上一期课程的 Vim 讲座](https://missing.csail.mit.edu/2020/editors/)
- `vimtutor`（Vim 自带教程）—— 安装 Vim 后可在终端运行
- [Vim Adventures](https://vim-adventures.com/) —— 游戏化学习 Vim
- [Vim Tips Wiki](https://vim.fandom.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2019/)
- [VimGolf](https://www.vimgolf.com/) —— 用最少的按键完成任务
- [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
- [Vim Screencasts](http://vimcasts.org/)
- 《Practical Vim》一书

---

### 三、代码智能与语言服务器

IDE 通常通过扩展连接到实现 **语言服务器协议（LSP）** 的语言服务器，提供语言特定的支持。

**示例**：

- VS Code 的 Python 扩展依赖 **Pylance**
- VS Code 的 Go 扩展依赖第一方的 **gopls**

通过安装扩展和语言服务器，可以在 IDE 中启用许多语言特定功能：

| 功能                 | 说明                                                       |
| -------------------- | ---------------------------------------------------------- |
| **代码补全**   | 更好的自动补全，如输入`object.` 后显示对象的字段和方法   |
| **内联文档**   | 悬停和自动补全时显示文档                                   |
| **跳转到定义** | 从使用处跳转到定义处                                       |
| **查找引用**   | 查找某字段或类型的所有引用位置                             |
| **导入辅助**   | 整理导入、删除未使用的导入、标记缺失的导入                 |
| **代码质量**   | 自动缩进、格式化，以及类型检查器和 linter 在键入时发现错误 |

> 📌 代码质量相关内容将在 [代码质量讲座](https://missing.csail.mit.edu/2026/code-quality/) 中深入讲解。

#### 3.1 配置语言服务器

对于某些语言，只需安装扩展和语言服务器即可。对于其他语言，需要告诉 IDE 你的环境才能获得最大收益。

例如，将 VS Code 指向你的 Python 环境，语言服务器就能看到已安装的包。

> 📌 环境相关内容将在 [打包和交付代码讲座](https://missing.csail.mit.edu/2026/shipping-code/) 中深入讲解。

根据语言不同，可能有一些可配置的设置。例如，在 VS Code 的 Python 支持中，可以为不使用 Python 可选类型注解的项目禁用静态类型检查。

---

### 四、AI 驱动的开发

自 2021 年中期 **GitHub Copilot** 推出以来，LLM 在软件工程中被广泛采用。

目前主要有三种形式：

#### 4.1 自动补全（Autocomplete）

AI 驱动的自动补全与传统 IDE 自动补全的形式相同，在光标位置建议补全。

有时它作为被动功能“开箱即用”。此外，AI 自动补全通常通过**代码注释**来提示（prompt）。

**示例 1**：编写脚本下载讲座笔记并提取所有链接。

```python
import requests

def download_contents(url: str) -> str:
```

模型会自动补全函数体：

```python
    response = requests.get(url)
    return response.text
```

**示例 2**：用注释引导补全。

```python
def extract(content: str) -> list[str]:
```

模型给出不理想的补全：

```python
    lines = contents.splitlines()
    return [line for line in lines if line.strip()]
```

通过**代码注释**引导：

```python
def extract(content: str) -> list[str]:
    # extract all Markdown links from the content
```

这次模型给出更好的补全：

```python
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)
```

> ⚠️ **注意**：这种 AI 工具的一个缺点是**只能在光标处提供补全**。上例中将 `import re` 放在函数内部而非模块级别，不是好实践。
>
> 实践中应使用更具描述性的函数名（如 `extract_links`）并编写文档字符串（docstring），模型应能据此生成更好的补全。

补全整个脚本：

```python
print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

#### 4.2 内联聊天（Inline Chat）

内联聊天允许**选择一行或一个代码块**，然后直接提示 AI 模型提出修改建议。

这种交互模式下，模型可以**修改现有代码**（这与自动补全不同，自动补全只能在光标之后补全代码）。

沿用上面的例子，假设决定不使用第三方 `requests` 库。选择相关三行代码，调用内联聊天并说：

> “use built-in libraries instead”

模型建议：

```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')
```

#### 4.3 编码智能体（Coding Agents）

编码智能体将在 [Agentic Coding](https://missing.csail.mit.edu/2026/agentic-coding/) 讲座中深入讲解。

#### 4.4 推荐软件

一些流行的 AI IDE：

- **VS Code** + GitHub Copilot 扩展
- **Cursor**

GitHub Copilot 目前对**学生、教师和流行开源项目的维护者免费**。

> 📌 这是一个快速发展的领域，许多领先产品功能大致相当。

---

### 五、扩展与其他 IDE 功能

IDE 是强大的工具，扩展使其更加强大。

鼓励自行探索。以下是一些常用扩展的列表：

- [Vim Awesome](https://vimawesome.com/) —— Vim 插件列表
- [VS Code 扩展（按流行度排序）](<https://marketplace.visualstudio.com/search?target=VSCode&category=All%20categories&sortBy=Installs>)

#### 5.1 开发容器（Development Containers）

主流 IDE 支持开发容器（如 VS Code 支持）。开发容器允许使用容器来运行开发工具，有助于**可移植性或隔离**。

> 📌 容器相关内容将在 [打包和交付代码讲座](https://missing.csail.mit.edu/2026/shipping-code/) 中深入讲解。

#### 5.2 远程开发（Remote Development）

使用 SSH 在远程机器上进行开发（如 VS Code 的 Remote SSH 插件）。

这在需要在云端高性能 GPU 机器上开发和运行代码时非常方便。

#### 5.3 协作编辑（Collaborative Editing）

像 Google Docs 一样编辑同一文件（如 VS Code 的 Live Share 插件）。

---

### 六、练习

1. **启用 Vim 模式**：在所有支持 Vim 模式的软件（编辑器和 Shell）中启用，并在接下来一个月内用 Vim 模式完成所有文本编辑。每当觉得效率低下或觉得“应该有更好的方法”时，去搜索，很可能确实有更好的方法。
2. **完成 VimGolf 挑战**：完成 [VimGolf](https://www.vimgolf.com/) 上的一个挑战。
3. **配置语言服务器**：为正在进行的项目配置 IDE 扩展和语言服务器。确保所有预期功能（如对库依赖的跳转到定义）正常工作。如果没有可用代码，可以使用 GitHub 上的开源项目（如 [cobra](https://github.com/spf13/cobra)）。
4. **安装有用的扩展**：浏览 IDE 扩展列表，安装一个看起来有用的扩展。
