## 打包与交付代码

> 让代码按预期工作很难；让同样的代码在另一台机器上运行往往更难。交付代码意味着将你写的代码转换成一种可用的形式，让别人能在没有你电脑精确配置的情况下运行它。

交付代码的形式多种多样，取决于编程语言、系统库、操作系统以及构建目标（软件库、命令行工具、Web 服务等）的选择。但所有场景都有一个共同模式：**我们需要定义交付物是什么（即工件 artifact），以及它对周围环境做出了哪些假设。** 本讲通过 Python 生态系统的示例来讲解这些概念。

---

### 一、依赖与环境（Dependencies & Environments）

在现代软件开发中，抽象层无处不在。程序自然会将其逻辑卸载到其他库或服务中，这就在你的程序和它运行所需的库之间引入了**依赖关系**。

#### 1.1 依赖问题

以 Python 为例，获取网页内容通常需要 `requests` 库：

```python
import requests
response = requests.get("https://missing.csail.mit.edu")
```

但 `requests` 并不随 Python 运行时一起提供，如果未安装就会报错：

```bash
$ python fetch.py
Traceback (most recent call last):
  File "fetch.py", line 1, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'
```

要解决这个问题，需要先用 `pip install requests` 安装该库。

`pip install requests` 的执行过程包括：
1. 在 **PyPI**（Python Package Index）中搜索 `requests`
2. 为当前平台寻找合适的工件
3. 解析依赖——`requests` 本身依赖其他包，安装程序必须找到所有传递依赖的兼容版本
4. 下载工件，解压并复制到文件系统中的正确位置

```bash
$ pip install requests
Collecting requests
  Downloading requests-2.32.3-py3-none-any.whl (64 kB)
Collecting charset-normalizer<4,>=2
  Downloading charset_normalizer-3.4.0-cp311-cp311-manylinux_x86_64.whl (142 kB)
Collecting idna<4,>=2.5
  Downloading idna-3.10-py3-none-any.whl (70 kB)
Collecting urllib3<3,>=1.21.1
  Downloading urllib3-2.2.3-py3-none-any.whl (126 kB)
Collecting certifi>=2017.4.17
  Downloading certifi-2024.8.30-py3-none-any.whl (167 kB)
Installing collected packages: urllib3, idna, charset-normalizer, certifi, requests
Successfully installed certifi-2024.8.30 charset-normalizer-3.4.0 idna-3.10 requests-2.32.3 urllib3-2.2.3
```

可以看到 `requests` 有自己的依赖（如 `certifi`、`charset-normalizer` 等），它们必须在 `requests` 安装之前先安装。

安装完成后，Python 运行时就能找到这个库了：

```bash
$ python -c 'import requests; print(requests.__path__)'
['/usr/local/lib/python3.11/dist-packages/requests']
$ pip list | grep requests
requests 2.32.3
```

#### 1.2 不同语言的依赖管理

不同编程语言有不同的依赖管理工具和惯例：
- **Rust**：工具链是统一的——`cargo` 处理构建、测试、依赖管理和发布
- **Python**：统一发生在规范层面——有标准化的规范定义打包工作方式，允许多个竞争工具（`pip` vs `uv`，`setuptools` vs `hatch` vs `poetry`）
- **LaTeX**：TeX Live 或 MacTeX 等发行版预装了数千个包

#### 1.3 依赖冲突与依赖地狱

引入依赖也引入了**依赖冲突**。当程序需要不兼容版本的同一个依赖时，冲突就会发生。

例如，如果 `tensorflow==2.3.0` 需要 `numpy>=1.16.0,<1.19.0`，而 `pandas==1.2.0` 需要 `numpy>=1.16.5`，那么任何满足 `numpy>=1.16.5,<1.19.0` 的版本都是有效的。但如果项目中另一个包需要 `numpy>=1.19`，就产生了冲突——没有有效版本能满足所有约束。这种情况被称为**依赖地狱**。

#### 1.4 虚拟环境

应对冲突的一种方法是将每个程序的依赖隔离到自己的环境中。在 Python 中，我们创建**虚拟环境（virtual environment）** ：

```bash
$ which python
/usr/bin/python
$ python -m venv venv
$ source venv/bin/activate
$ which python
/home/missingsemester/venv/bin/python
$ which pip
/home/missingsemester/venv/bin/pip
```

虚拟环境是一个独立的语言运行时版本，拥有自己的一套已安装包，将依赖从全局 Python 安装中隔离出来。

> **建议**：为每个项目创建一个虚拟环境是个好习惯。
>
> ⚠️ **警告**：虽然许多现代操作系统预装了 Python 等编程语言运行时，但修改这些安装是不明智的，因为操作系统本身可能依赖它们。应优先使用独立的环境。

虚拟环境还允许你使用不同版本的语言运行时：

```bash
$ uv venv --python 3.12 venv312
Using CPython 3.12.7
Creating virtual environment at: venv312
$ source venv312/bin/activate && python --version
Python 3.12.7
$ uv venv --python 3.11 venv311
Using CPython 3.11.10
Creating virtual environment at: venv311
$ source venv311/bin/activate && python --version
Python 3.11.10
```

这在你需要跨多个 Python 版本测试代码，或者项目需要特定版本时很有帮助。

> **推荐**：强烈建议尽可能使用 `uv pip` 代替 `pip`，它能大幅减少安装时间。
>
> **关于 `uv`**：安装 `uv` 只需 `pip install uv`。使用 `uv` 的接口与 `pip` 相同，但速度显著更快：
> ```bash
> $ uv pip install requests
> Resolved 5 packages in 12ms
> Prepared 5 packages in 0.45ms
> Installed 5 packages in 8ms
> + certifi==2024.8.30
> + charset-normalizer==3.4.0
> + idna==3.10
> + requests==2.32.3
> + urllib3==2.2.3
> ```

---

### 二、工件与打包（Artifacts & Packaging）

在软件开发中，我们区分**源代码**和**工件**。开发者编写和阅读源代码，而工件是从源代码生产的打包、可分发的输出——准备好被安装或部署。

工件可以简单到一份可直接运行的代码文件，也可以复杂到包含应用程序所有必要组件的整个虚拟机。

#### 2.1 为什么需要打包

考虑这个例子，当前目录下有一个 Python 文件 `greet.py`：

```bash
$ cat greet.py
def greet(name):
    return f"Hello, {name}!"
$ python -c "from greet import greet; print(greet('World'))"
Hello, World!
$ cd /tmp
$ python -c "from greet import greet; print(greet('World'))"
ModuleNotFoundError: No module named 'greet'
```

一旦切换到不同目录，导入就失败了。这是因为 Python 只在特定位置搜索模块（当前目录、已安装的包和 `PYTHONPATH` 中的路径）。**打包解决了这个问题——将代码安装到已知位置。** 

#### 2.2 Python 打包：pyproject.toml 与 Wheel

在 Python 中，打包一个库涉及生成一个工件（称为 **wheel**），包安装程序（如 `pip` 或 `uv`）可以用它来安装相关文件。Wheel 包含：
- 代码文件
- 包的元数据（名称、版本、依赖）
- 在环境中放置文件的指令

构建工件需要编写一个**项目文件**（也称为清单），详细说明项目的具体信息、所需依赖、包的版本等。在 Python 中，我们使用 **`pyproject.toml`** 来实现此目的。

> **注意**：`pyproject.toml` 是现代推荐方式。虽然早期打包方法如 `requirements.txt` 或 `setup.py` 仍然被支持，但应尽可能优先使用 `pyproject.toml`。

以下是一个为库提供命令行工具的 `pyproject.toml` 示例：

```toml
[project]
name = "greeting"
version = "0.1.0"
description = "A simple greeting library"
dependencies = ["typer>=0.9"]

[project.scripts]
greet = "greeting:cli"

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```

对应的 `greeting.py`：

```python
import typer

def greet(name: str) -> str:
    return f"Hello, {name}!"

def cli():
    typer.run(greet)

if __name__ == "__main__":
    cli()
```

有了这个文件，就可以构建 wheel 了：

```bash
$ uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/greeting-0.1.0.tar.gz
Successfully built dist/greeting-0.1.0-py3-none-any.whl
$ ls dist/
greeting-0.1.0-py3-none-any.whl  greeting-0.1.0.tar.gz
```

- `.whl` 文件是 wheel（一个具有特定结构的 zip 压缩包）
- `.tar.gz` 是源代码分发包，供需要从源代码构建的系统使用

查看 wheel 的内容：

```bash
$ unzip -l dist/greeting-0.1.0-py3-none-any.whl
Archive:  dist/greeting-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
      150  2024-01-15 10:30   greeting.py
      312  2024-01-15 10:30   greeting-0.1.0.dist-info/METADATA
       92  2024-01-15 10:30   greeting-0.1.0.dist-info/WHEEL
        9  2024-01-15 10:30   greeting-0.1.0.dist-info/top_level.txt
      435  2024-01-15 10:30   greeting-0.1.0.dist-info/RECORD
      998                     5 files
```

将这个 wheel 交给别人，他们可以通过以下方式安装：

```bash
$ uv pip install ./greeting-0.1.0-py3-none-any.whl
$ greet Alice
Hello, Alice!
```

#### 2.3 局限性

这种方法有局限性。特别是，如果库依赖平台特定的库（如 GPU 加速的 CUDA），那么工件**只适用于安装了这些特定库的系统**，可能需要为不同平台（Linux、macOS、Windows）和架构（x86、ARM）构建单独的 wheel。

#### 2.4 从源代码安装 vs 预构建二进制

安装软件时，有一个重要区别：
- **从源代码安装**：下载原始代码并在你的机器上编译——需要安装编译器和构建工具，大型项目可能需要很长时间
- **安装预构建二进制**：下载别人已编译好的工件——更快更简单，但二进制必须匹配你的平台和架构

例如，[ripgrep 的发布页面](https://github.com/BurntSushi/ripgrep/releases) 展示了针对 Linux（x86_64、ARM）、macOS（Intel、Apple Silicon）和 Windows 的预构建二进制。

---

### 三、发布与版本控制（Releases & Versioning）

代码是在持续过程中构建的，但以离散的方式发布。在软件开发中，**开发环境**和**生产环境**之间有明显的区别。代码需要在开发环境中被证明有效，才能交付到生产环境。

发布过程涉及多个步骤：测试、依赖管理、版本控制、配置、部署和发布。

#### 3.1 语义化版本（Semantic Versioning）

软件库不是静态的，会随着修复和新功能而演变。我们通过**离散的版本标识符**来跟踪这种演变，这些标识符对应库在特定时间点的状态。

库行为的变化范围从修补非关键功能的补丁、扩展功能的新特性，到破坏向后兼容性的更改。**更新日志（Changelog）** 记录了版本引入的变化。

为了简化版本管理问题，业界有版本控制惯例，其中最流行的是**语义化版本（SemVer）** 。

在语义化版本下，版本标识符的形式为 **`MAJOR.MINOR.PATCH`**：
- **PATCH**（如 1.2.3 → 1.2.4）：只包含 bug 修复，完全向后兼容
- **MINOR**（如 1.2.3 → 1.3.0）：以向后兼容的方式添加新功能
- **MAJOR**（如 1.2.3 → 2.0.0）：表示可能需修改代码的破坏性变化

> **注意**：这是简化版本，建议阅读完整的 SemVer 规范以理解更多细节。

#### 3.2 Python 中的版本约束

Python 打包原生支持语义化版本。在 `pyproject.toml` 中指定依赖版本时，可以使用不同的约束范围：

```toml
[project]
dependencies = [
    "requests==2.32.3",        # 精确版本——只有这个特定版本
    "click>=8.0",              # 最低版本——8.0 或更新
    "numpy>=1.24,<2.0",        # 范围——至少 1.24 但小于 2.0
    "pandas~=2.1.0",           # 兼容版本——>=2.1.0 且 <2.2.0
]
```

`~=` 操作符是 Python 的“兼容版本”操作符——`~=2.1.0` 表示“与 2.1.0 兼容的任何版本”，即 `>=2.1.0` 且 `<2.2.0`。这大致相当于 npm 和 cargo 中的 `^` 操作符。

#### 3.3 其他版本方案

并非所有软件都使用语义化版本。一种常见的替代方案是**日历版本（CalVer）** ，版本基于发布日期而非语义含义。例如，Ubuntu 使用 `24.04`（2024年4月）和 `24.10`（2024年10月）这样的版本。

---

### 四、可重现性（Reproducibility）

在现代软件开发中，你写的代码坐落在大量抽象层之上：编程语言运行时、第三方库、操作系统，甚至硬件本身。**任何一层的差异都可能改变代码的行为，甚至使其无法按预期工作。** 

#### 4.1 锁定版本（Pinning）

**锁定（Pinning）** 一个库是指指定精确版本而非范围，例如 `requests==2.32.3` 而不是 `requests>=2.0`。

包管理器的工作是考虑依赖项（及传递依赖）提供的所有约束，然后生成一个满足所有约束的有效版本列表。这个具体的版本列表可以保存到一个文件中以保证可重现性——这些文件被称为**锁文件（lock files）** 。

```bash
$ uv lock
Resolved 12 packages in 45ms
$ cat uv.lock | head -20
version = 1
requires-python = ">=3.11"

[[package]]
name = "certifi"
version = "2024.8.30"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/...", hash = "sha256:..." }
wheels = [
    { url = "https://files.pythonhosted.org/...", hash = "sha256:..." },
]
...
```

#### 4.2 库 vs 应用

处理依赖版本控制和可重现性时，一个关键区别是**库**与**应用/服务**之间的差异：
- **库**：旨在被其他代码导入和使用，这些代码可能有自己的依赖。指定过于严格的版本约束可能与用户的其他依赖产生冲突。对于库，好的做法是**指定版本范围**以最大化与更广泛包生态系统的兼容性。
- **应用**：是软件的最终消费者，通常通过用户界面或 API 暴露功能，而非编程接口。对于应用，**锁定精确版本**确保可重现性——每个运行应用的人都使用完全相同的依赖。

#### 4.3 封闭式构建（Hermetic Builds）

对于需要最大可重现性的项目，**Nix** 和 **Bazel** 等工具提供**封闭式构建（hermetic builds）** ——每个输入（包括编译器、系统库，甚至构建环境本身）都被锁定并内容寻址。这保证了无论何时何地运行构建，都能产生逐位相同的输出。

> **NixOS**：你甚至可以使用 NixOS 来管理整个计算机安装，从而可以轻松地启动计算机设置的新副本，并通过版本控制的配置文件管理其完整配置。

#### 4.4 依赖更新的挑战

软件开发中一个永无止境的张力是：新软件版本会引入（有意或无意的）破坏，而旧软件版本会随着时间推移出现安全漏洞。

应对方法包括：
- 使用**持续集成流水线**（CI）针对新软件版本测试应用
- 使用 **Dependabot** 等自动化工具检测依赖新版本的发布
- 即使有 CI 测试，升级软件版本时仍可能出现问题，通常是因为开发环境和生产环境之间不可避免的不匹配。此时最佳做法是**拥有回滚计划**——回退版本升级，重新部署已知良好的版本。

---

### 五、虚拟机与容器（VMs & Containers）

随着依赖变得越来越复杂，依赖可能会超出包管理器能处理的范围。一个常见原因是需要与特定的系统库或硬件驱动交互。例如，在科学计算和 AI 中，程序通常需要专门的库和驱动来利用 GPU 硬件。

#### 5.1 虚拟机（VMs）

传统上，这种更广泛的依赖问题是通过**虚拟机（VM）** 解决的。VM 抽象了整个计算机，提供了一个完全隔离的环境，拥有自己专用的操作系统。

#### 5.2 容器（Containers）

一种更现代的方法是**容器**。容器将应用及其依赖、库和文件系统打包在一起，但**共享主机的操作系统内核**，而非虚拟化整个计算机。容器比 VM 更轻量级，因为它们共享内核，启动更快，运行更高效。

最流行的容器平台是 **Docker**。Docker 引入了一种标准化的方式来构建、分发和运行容器。底层，Docker 使用 `containerd` 作为容器运行时——这也是 Kubernetes 等其他工具使用的行业标准。

运行容器很简单。例如，在容器内运行 Python 解释器：

```bash
$ docker run -it python:3.12 python
Python 3.12.7 (main, Nov 5 2024, 02:53:25) [GCC 12.2.0] on linux
>>> print("Hello from inside a container!")
Hello from inside a container!
```

> `-it` 标志使容器交互式地连接终端。退出时，容器停止。

#### 5.3 Dockerfile

在实践中，程序可能依赖整个文件系统。我们可以使用**容器镜像**来打包整个应用程序的文件系统作为工件。容器镜像是通过 **Dockerfile** 以编程方式创建的：

```dockerfile
FROM python:3.12
RUN apt-get update
RUN apt-get install -y gcc
RUN apt-get install -y libpq-dev
RUN pip install numpy
RUN pip install pandas
COPY . /app
WORKDIR /app
RUN pip install .
```

**重要区别**：**Docker 镜像**是打包的工件（像一个模板），而**容器**是该镜像的运行实例。你可以从同一个镜像运行多个容器。

镜像以**层**的方式构建，Dockerfile 中的每条指令（`FROM`、`RUN`、`COPY` 等）都会创建一个新层。Docker 会缓存这些层，因此如果你更改 Dockerfile 中的一行，只有该层及后续层需要重建。

#### 5.4 Dockerfile 最佳实践

前面的 Dockerfile 有几个问题：使用完整 Python 镜像而非 slim 变体、分离的 `RUN` 命令创建了不必要的层、版本未锁定、没有清理包管理器缓存。

改进版本：

```dockerfile
FROM python:3.12-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc libpq-dev && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /app
ENV PATH="/app/.venv/bin:$PATH"
COPY pyproject.toml uv.lock ./
RUN uv sync --locked --no-dev --no-install-project
COPY . .
RUN uv sync --locked --no-dev
```

这里使用了**构建者模式（builder pattern）** ——从 `ghcr.io/astral-sh/uv:latest` 镜像复制预构建的 `uv` 二进制文件，而不是从源代码安装。这样就不需要打包编译代码所需的所有工具，只需要运行应用所需的最终二进制文件。

#### 5.5 Docker 的局限性

Docker 有一些重要的局限性：
1. **平台特定**：为 `linux/amd64` 构建的镜像不能在 `linux/arm64`（Apple Silicon Mac）上原生运行，需要仿真（速度慢）
2. **需要 Linux 内核**：在 macOS 和 Windows 上，Docker 实际上在底层运行一个轻量级 Linux VM，增加了开销
3. **隔离性较弱**：容器共享主机内核，在多租户环境中存在安全风险

---

### 六、配置（Configuration）

软件本质上是可配置的。程序通过标志、环境变量或配置文件（dotfiles）接收选项。对于更复杂的应用，也有管理配置的成熟模式。

**软件配置不应嵌入代码中，而应在运行时提供。** 

#### 6.1 环境变量

通过环境变量配置的示例：

```python
import os

DATABASE_URL = os.environ.get("DATABASE_URL", "sqlite:///local.db")
DEBUG = os.environ.get("DEBUG", "false").lower() == "true"
API_KEY = os.environ["API_KEY"]  # 必需——若未设置会抛出异常
```

#### 6.2 配置文件

通过配置文件配置的示例（如 Python 程序加载 `yaml.load`）：

```yaml
# config.yaml
database:
  url: "postgresql://localhost/myapp"
  pool_size: 5
server:
  host: "0.0.0.0"
  port: 8080
debug: false
```

**一个好的经验法则**：同一代码库应该能够仅通过配置更改（而非代码更改）部署到不同环境（开发、预发布、生产）。

**敏感数据**（如 API 密钥）需要小心处理以避免意外暴露，**绝不能包含在版本控制中**。

---

### 七、服务与编排（Services & Orchestration）

现代应用很少孤立存在。一个典型的 Web 应用可能需要数据库做持久存储、缓存提高性能、消息队列处理后台任务，以及各种其他支持服务。

#### 7.1 微服务架构

现代架构通常将功能分解为独立服务，这些服务可以独立开发、部署和扩展，而不是将所有东西捆绑到一个单体应用中。

例如，如果确定应用可以从缓存中受益，可以使用现有的成熟解决方案如 **Redis** 或 **Memcached**。我们可以将 Redis 作为依赖嵌入应用中（在容器中构建），但这意味着要协调 Redis 和应用之间的所有依赖——这可能具有挑战性甚至不可行。

替代方案是将每个应用分别部署在自己的容器中。这通常被称为**微服务架构**，每个组件作为独立服务运行，通过网络（通常通过 HTTP API）进行通信。

#### 7.2 Docker Compose

**Docker Compose** 是定义和运行多容器应用的工具。你可以在一个 YAML 文件中声明所有服务，并一起编排它们：

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - REDIS_URL=redis://cache:6379
    depends_on:
      - cache
  cache:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
volumes:
  redis_data:
```

使用 `docker compose up`，两个服务一起启动，Web 应用可以使用主机名 `cache` 连接到 Redis（Docker 的内部 DNS 会自动解析服务名称）。

Docker Compose 让我们声明如何部署一个或多个服务，并处理启动它们、设置网络以及管理数据持久化共享卷的编排。

#### 7.3 生产部署

对于生产部署，通常希望 Docker Compose 服务在启动时自动启动并在失败时重启。一种常见方法是使用 **systemd** 管理 Docker Compose 部署：

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down

[Install]
WantedBy=multi-user.target
```

这个 systemd 单元文件确保应用在系统启动时启动（在 Docker 就绪后），并提供 `systemctl start myapp`、`systemctl stop myapp` 和 `systemctl status myapp` 等标准控制。

#### 7.4 Kubernetes

随着部署需求变得更加复杂——需要跨多台机器的可扩展性、服务崩溃时的容错性以及高可用性保证——组织会转向复杂的容器编排平台，如 **Kubernetes（k8s）** ，它可以在机器集群中管理数千个容器。不过，Kubernetes 学习曲线陡峭，运营开销大，对于较小的项目通常是过度工程。

#### 7.5 服务间通信

这种多容器设置之所以部分可行，是因为现代服务通过标准化 API 相互通信，主要是 **HTTP REST API**。例如，当程序与 OpenAI 或 Anthropic 等 LLM 提供商交互时，底层就是向他们的服务器发送 HTTP 请求并解析响应：

```bash
$ curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "content-type: application/json" \
  -H "anthropic-version: 2023-06-01" \
  -d '{"model": "claude-sonnet-4-20250514", "max_tokens": 256, "messages": [{"role": "user", "content": "Explain containers vs VMs in one sentence."}]}'
```

---

### 八、发布（Publishing）

一旦你的代码被证明可以工作，你可能会有兴趣分发它供他人下载和安装。分发有多种形式，与编程语言和运行环境密切相关。

#### 8.1 简单的分发方式

最简单的分发形式是**上传工件供他人下载和本地安装**。这仍然很常见，例如在 [Ubuntu 的包归档](http://archive.ubuntu.com/ubuntu/pool/main/) 中，本质上是一个 `.deb` 文件的 HTTP 目录列表。

如今，**GitHub** 已成为发布源代码和工件的事实标准平台。虽然源代码通常是公开的，但 **GitHub Releases** 允许维护者将预构建的二进制文件和其他工件附加到标记的版本上。

#### 8.2 从 GitHub 安装

包管理器有时支持直接从 GitHub 安装，无论是从源代码还是从预构建的 wheel：

```bash
# 从源代码安装（会克隆并构建）
$ pip install git+https://github.com/psf/requests.git
# 从特定标签/分支安装
$ pip install git+https://github.com/psf/requests.git@v2.32.3
# 从 GitHub Release 直接安装 wheel
$ pip install https://github.com/user/repo/releases/download/v1.0/package-1.0-py3-none-any.whl
```

#### 8.3 去中心化分发

有些语言使用**去中心化的分发模型**。例如，Go 模块直接从其源代码仓库分发——模块路径如 `github.com/gorilla/mux` 指示代码所在位置，`go get` 直接从那里获取。

然而，大多数包管理器（如 `pip`、`cargo` 或 `brew`）都有预打包项目的**中央索引**，以便于分发和安装。

#### 8.4 Wheel 的平台特异性

当我们用 `uv pip install requests` 时，可以看到从何处获取 wheel：

```bash
$ uv pip install requests --verbose --no-cache 2>&1 | grep -F '.whl'
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/.../requests-2.32.5-py3-none-any.whl
```

注意文件名中的 `py3-none-any`——这意味着这个 wheel 适用于任何 Python 3 版本、任何操作系统、任何架构。

对于包含编译代码的包，wheel 是**平台特定的**：

```bash
$ uv pip install numpy --verbose --no-cache 2>&1 | grep -F '.whl'
DEBUG Selecting: numpy==2.2.1 [compatible] (numpy-2.2.1-cp312-cp312-macosx_14_0_arm64.whl)
```

这里的 `cp312-cp312-macosx_14_0_arm64` 表示这个 wheel 专门用于 CPython 3.12、macOS 14+、ARM64（Apple Silicon）。如果你在不同的平台上，`pip` 会下载不同的 wheel 或从源代码构建。