
## 版本控制与 Git

### 一、版本控制概述

版本控制系统（VCS）是用于跟踪源代码（或其他文件和文件夹集合）变更的工具。它们帮助维护变更历史，并促进协作。

从逻辑上讲，VCS 将某个顶级目录下的文件和文件夹的变更，跟踪为一系列快照（snapshot），每个快照封装了该目录下所有文件/文件夹的完整状态。VCS 还维护元数据，如谁创建了每个快照、与每个快照关联的消息等。

**版本控制的用途**：

- 查看项目的旧快照
- 记录某些变更的原因
- 在并行分支上开发
- 查看他人的变更
- 解决并发开发中的冲突
- 回答如下问题：
  - 谁写了这个模块？
  - 这个文件的这一行是什么时候编辑的？被谁编辑的？为什么编辑？
  - 在过去 1000 个版本中，某个单元测试是什么时候/为什么失败的？

虽然存在其他 VCS，但 **Git 是版本控制的事实标准**。

> 由于 Git 的接口是一个“泄漏的抽象”（leaky abstraction），自上而下学习 Git（从接口/命令行开始）可能会导致很多困惑。可以死记硬背几个命令，把它们当作咒语——但 Git 的底层设计和思想是优美的。因此，本课程采用**自下而上**的方式：先理解数据模型，再学习命令行接口。

---

### 二、Git 的数据模型

Git 的精妙之处在于其经过深思熟虑的数据模型，它支持版本控制的所有优良特性：维护历史、支持分支、促进协作。

#### 2.1 快照（Snapshots）

Git 将某个顶级目录下的文件和文件夹集合的历史，建模为一系列快照。

在 Git 术语中：

- **Blob**（文件）：只是一堆字节
- **Tree**（目录）：将名称映射到 blob 或 tree（因此目录可以包含其他目录）
- **Snapshot**（快照）：被跟踪的顶级 tree

**示例**：一个 tree 结构如下：

```
<root> (tree)
├── foo (tree)
│   └── bar.txt (blob, contents = "hello world")
└── baz.txt (blob, contents = "git is wonderful")
```

顶级 tree 包含两个元素：一个 tree “foo”（本身包含一个 blob “bar.txt”），和一个 blob “baz.txt”。

#### 2.2 历史建模：关联快照

Git 中的历史是一个**有向无环图（DAG）** 的快照。

每个快照（在 Git 中称为 **commit**）引用一组“父”（parents）快照——即它之前的一个或多个快照。之所以是一组父而不是单个父（线性历史的情况），是因为一个快照可能从多个父继承，例如由于合并（merging）两个并行的开发分支。

**可视化**：

```
o <-- o <-- o <-- o
        ^
         \
          --- o <-- o
```

ASCII 艺术中，`o` 对应各个 commit（快照），箭头指向每个 commit 的父 commit。第三个 commit 之后，历史分叉为两个独立的分支。

未来这些分支可能被合并，产生一个新的快照：

```
o <-- o <-- o <-- o <---- o
        ^            /
         \          v
          --- o <-- o
```

粗体显示的是新创建的**合并 commit**。

**Commits 在 Git 中是不可变的**。这并不意味着错误不能被纠正——只是对 commit 历史的“编辑”实际上是创建全新的 commit，然后更新引用（references）指向新的 commit。

#### 2.3 数据模型（伪代码）

```pseudocode
// 一个文件是一堆字节
type blob = array<byte>

// 一个目录包含命名的文件和目录
type tree = map<string, blob | tree>

// 一个 commit 有父、元数据和顶级 tree
type commit = struct {
    parents: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

这是一个简洁、简单的历史模型。

#### 2.4 对象与内容寻址（Objects and Content-Addressing）

**Object**（对象）是 blob、tree 或 commit：

```pseudocode
type object = blob | tree | commit
```

在 Git 的数据存储中，所有对象都通过其 **SHA-1 哈希** 进行内容寻址：

```pseudocode
objects = map<string, object>

def store(object):
    id = sha1(object)
    objects[id] = object

def load(id):
    return objects[id]
```

Blob、tree 和 commit 都以这种方式统一：它们都是对象。当它们引用其他对象时，并不在磁盘表示中包含它们，而是通过哈希值引用它们。

**示例**：使用 `git cat-file -p` 查看 tree 对象：

```
100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85 baz.txt
040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87 foo
```

查看 baz.txt 对应的 blob：

```
git is wonderful
```

#### 2.5 引用（References）

现在，所有快照都可以通过其 SHA-1 哈希值来标识。但人类不擅长记住 40 位十六进制字符串。

Git 的解决方案是**人类可读的名称**，称为 **references**（引用）。引用是指向 commit 的指针。与不可变的对象不同，引用是**可变的**（可以更新以指向新的 commit）。

例如，`master` 引用通常指向主开发分支中的最新 commit。

```pseudocode
references = map<string, string>

def update_reference(name, id):
    references[name] = id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id in references:
        return load(references[name_or_id])
    else:
        return load(name_or_id)
```

这样，Git 就可以使用 “master” 这样的人类可读名称来引用历史中的特定快照。

一个重要的细节是：我们经常需要知道“我们当前在历史中的什么位置”，以便当我们创建新快照时，知道它相对于什么（如何设置 commit 的 `parents` 字段）。在 Git 中，这个“当前位置”是一个特殊的引用，叫做 **HEAD**。

#### 2.6 仓库（Repositories）

粗略地说，**Git 仓库**就是数据对象（objects）和引用（references）。

在磁盘上，Git 存储的所有东西就是对象和引用——这就是 Git 数据模型的全部。

所有 Git 命令都映射为对 commit DAG 的某种操作：添加对象和添加/更新引用。每当你输入任何命令时，想一想这个命令对底层图数据结构做了什么操作。反之，如果你想对 commit DAG 做某种变更（例如“丢弃未提交的更改，让 master 引用指向 commit 5d83f9e”），很可能有一个命令可以做到（例如 `git checkout master; git reset --hard 5d83f9e`）。

---

### 三、暂存区（Staging Area）

暂存区是与数据模型正交的另一个概念，它是创建 commit 的接口的一部分。

你可能想象快照的实现方式是：有一个“创建快照”命令，基于工作目录的当前状态创建新快照。有些版本控制工具就是这样工作的，但 Git 不是。

Git 希望有**干净的快照**，但基于当前状态创建快照并不总是理想的。例如：

- 你实现了两个独立的功能，想创建两个独立的 commit，第一个引入功能 A，第二个引入功能 B
- 你有调试用的 print 语句散布在代码中，同时还有一个 bug 修复；你想提交 bug 修复而丢弃所有 print 语句

Git 通过 **staging area**（暂存区）机制来应对这些场景，允许你指定哪些修改应该包含在下一次快照中。

---

### 四、Git 命令行接口

> 以下命令的详细解释请参考 [Pro Git](https://git-scm.com/book/en/v2)。

#### 4.1 基础命令

| 命令                                 | 说明                                             |
| ------------------------------------ | ------------------------------------------------ |
| `git help <command>`               | 获取 git 命令的帮助                              |
| `git init`                         | 创建一个新的 git 仓库，数据存储在`.git` 目录中 |
| `git status`                       | 告诉你当前的状态                                 |
| `git add <files>`                  | 将文件添加到暂存区                               |
| `git commit`                       | 创建一个新的 commit                              |
| `git log`                          | 显示扁平化的历史日志                             |
| `git log --all --graph --decorate` | 将历史可视化为 DAG                               |
| `git diff <filename>`              | 显示你对暂存区所做的修改                         |
| `git diff <revision> <filename>`   | 显示不同快照之间文件的差异                       |
| `git checkout <revision>`          | 更新 HEAD（如果签出的是分支，则更新当前分支）    |

> **提交消息**：写好的 commit 消息很重要！参见 [A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) 和 [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)。

#### 4.2 分支与合并（Branching and Merging）

| 命令                         | 说明                       |
| ---------------------------- | -------------------------- |
| `git branch`               | 显示分支                   |
| `git branch <name>`        | 创建分支                   |
| `git switch <branch>`      | 切换到分支                 |
| `git checkout -b <branch>` | 创建分支并切换到它         |
| `git merge <branch>`       | 合并到当前分支             |
| `git mergetool`            | 使用工具帮助解决合并冲突   |
| `git rebase <base>`        | 将一组补丁变基到新的基底上 |

#### 4.3 远程（Remotes）

| 命令                                               | 说明                             |
| -------------------------------------------------- | -------------------------------- |
| `git remote`                                     | 列出远程仓库                     |
| `git remote add <name> <url>`                    | 添加远程仓库                     |
| `git push <remote> <local>:<remote>`             | 向远程发送对象，并更新远程引用   |
| `git branch --set-upstream-to=<remote>/<branch>` | 设置本地分支与远程分支的对应关系 |
| `git fetch <remote>`                             | 从远程获取对象/引用              |
| `git pull <remote>`                              | 等同于`git fetch; git merge`   |
| `git clone <url>`                                | 从远程下载仓库                   |

#### 4.4 撤销（Undo）

| 命令                   | 说明                    |
| ---------------------- | ----------------------- |
| `git commit --amend` | 编辑 commit 的内容/消息 |
| `git reset <file>`   | 从暂存区移除文件        |
| `git restore <file>` | 丢弃工作目录中的修改    |

---

### 五、高级 Git

| 命令/特性               | 说明                                                      |
| ----------------------- | --------------------------------------------------------- |
| `git config`          | Git 是[高度可定制的](https://git-scm.com/docs/git-config)  |
| `git clone --depth=1` | 浅克隆，不带完整版本历史                                  |
| `git add -p`          | 交互式暂存                                                |
| `git rebase -i`       | 交互式变基                                                |
| `git blame`           | 显示谁最后编辑了哪一行                                    |
| `git stash`           | 临时移除工作目录的修改                                    |
| `git bisect`          | 二分搜索历史（例如查找回归）                              |
| `git revert`          | 创建一个新的 commit 来撤销之前 commit 的效果              |
| `git worktree`        | 同时签出多个分支                                          |
| `.gitignore`          | [指定](https://git-scm.com/docs/gitignore)故意不跟踪的文件 |

---

### 六、杂项

- **GUI 客户端**：Git 有许多 [GUI 客户端](https://git-scm.com/downloads/guis)，但本课程个人不使用，而是使用命令行接口
- **Shell 集成**：在 shell 提示符中显示 Git 状态非常方便（[zsh](https://github.com/olivierverdier/zsh-git-prompt)、[bash](https://github.com/magicmonty/bash-git-prompt)），通常包含在 [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) 等框架中
- **编辑器集成**：类似地，编辑器与 Git 的集成也很方便。[fugitive.vim](https://github.com/tpope/vim-fugitive) 是 Vim 的标准插件
- **工作流**：本课程教授了数据模型和一些基本命令，但没有告诉你在大项目上应该遵循什么实践——有许多[不同的](https://nvie.com/posts/a-successful-git-branching-model/)[方法](https://www.endoflineblog.com/gitflow-considered-harmful)[可供选择](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- **GitHub**：Git 不是 GitHub。GitHub 有一种特定的代码贡献方式，称为 **pull request**
- **其他 Git 托管服务**：GitHub 并不是唯一的，还有 [GitLab](https://about.gitlab.com/) 和 [BitBucket](https://bitbucket.org/) 等

---

### 七、资源

- **[Pro Git](https://git-scm.com/book/en/v2)**：强烈推荐阅读。在理解数据模型的基础上，读完第 1-5 章应该就能熟练使用 Git
- **[Oh Shit, Git!?!](https://ohshitgit.com/)**：从常见 Git 错误中恢复的简短指南
- **[Git for Computer Scientists](https://eagain.net/articles/git-for-computer-scientists/)**：Git 数据模型的简短解释
- **[Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/)**：Git 实现细节的详细解释
- **[How to explain git in simple words](https://smusamashah.github.io/blog/2017/10/14/explain-git-in-simple-words)**
- **[Learn Git Branching](https://learngitbranching.js.org/)**：浏览器中的游戏，教你 Git

---

### 八、练习

1. **新手入门**：如果没有 Git 经验，阅读 [Pro Git](https://git-scm.com/book/en/v2) 前几章或完成 [Learn Git Branching](https://learngitbranching.js.org/) 教程。在学习过程中，将 Git 命令与数据模型联系起来
2. **探索课堂网站仓库**：克隆 [class website 仓库](https://github.com/missing-semester/missing-semester)：

   - 将版本历史可视化为图形
   - 谁最后修改了 `README.md`？（提示：使用带参数的 `git log`）
   - 最后一次修改 `_config.yml` 中 `collections:` 行的 commit 消息是什么？（提示：使用 `git blame` 和 `git show`）
3. **从历史中删除文件**：一个常见错误是提交了不应由 Git 管理的大文件或敏感信息。尝试将文件添加到仓库，做几次 commit，然后从历史中删除该文件（不仅仅是最近的 commit）。参考 [从仓库中移除敏感数据](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)
4. **练习 `git stash`**：克隆一个 GitHub 仓库，修改其中一个现有文件。运行 `git stash` 会发生什么？运行 `git log --all --oneline` 会看到什么？运行 `git stash pop` 撤销 `git stash`。在什么场景下这会有用？
5. **配置 Git 别名**：Git 提供配置文件 `~/.gitconfig`。创建一个别名，使得运行 `git graph` 时能得到 `git log --all --graph --decorate --oneline` 的输出。可以通过直接编辑 `~/.gitconfig` 或使用 `git config` 命令来添加别名
6. **全局 .gitignore**：设置全局忽略文件 `~/.gitignore_global`（先运行 `git config --global core.excludesfile ~/.gitignore_global`，然后手动创建该文件）。忽略 OS 或编辑器特定的临时文件，如 `.DS_Store`
7. **提交 Pull Request**：Fork [class website 仓库](https://github.com/missing-semester/missing-semester)，找到一个拼写错误或其他可以改进的地方，在 GitHub 上提交 pull request
8. **解决合并冲突**：模拟协作场景来练习解决合并冲突：

   - 用 `git init` 创建新仓库，创建一个名为 `recipe.txt` 的文件，写几行内容（例如一个简单的菜谱），然后 commit
   - 创建两个分支：`git branch salty` 和 `git branch sweet`
   - 在 `salty` 分支中修改一行（例如将 “1 cup sugar” 改为 “1 cup salt”），然后 commit
   - 在 `sweet` 分支中修改同一行（例如将 “1 cup sugar” 改为 “2 cups sugar”），然后 commit
   - 切换回 `master`，尝试 `git merge salty`，然后 `git merge sweet`
   - 查看 `recipe.txt` 的内容——`<<<<<<<`、`=======` 和 `>>>>>>>` 标记是什么意思？
   - 通过编辑文件保留你想要的内容、移除冲突标记、用 `git add` 和 `git commit`（或 `git merge --continue`）完成合并来解决冲突。或者尝试使用 `git mergetool`
