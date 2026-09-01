
## 调试与性能分析

> 编程中的一条黄金法则是：代码不会做你期望它做的事，而是做你告诉它做的事。弥补这之间的差距有时是相当困难的。本讲涵盖处理有 Bug 的代码和资源消耗型代码的有用技术：**调试** 和 **性能分析**。

---

## 一、调试（Debugging）

### 1. Printf 调试与日志（Printf Debugging and Logging）

最有效的调试工具仍然是**仔细的思考**，加上**精心放置的 print 语句**。

第一种方法是：在发现问题的地方周围添加 print 语句，不断迭代直到提取到足够的信息来理解问题原因。

第二种方法是：在程序中使用**日志（logging）**，而不是临时的 print 语句。日志本质上是“更用心的打印”，通常通过日志框架实现，内置支持：

- 将日志（或日志子集）定向到其他输出位置
- 设置严重级别（如 INFO、DEBUG、WARN、ERROR 等），并允许根据这些级别过滤输出
- 支持与日志条目相关的**结构化日志**，便于事后提取数据

日志语句通常是在编程时**主动放置**的，因此调试所需的数据可能已经在那里了！一旦使用 print 语句发现并修复了问题，通常值得在删除它们之前将这些 print 转换为适当的日志语句。这样，如果将来出现类似的 Bug，你已经有了所需的诊断信息，无需修改代码。

> **第三方日志**：许多程序支持 `-v` 或 `--verbose` 标志来打印更多信息，有助于发现命令失败的原因。有些甚至允许重复使用该标志以获得更多细节。调试服务（数据库、Web 服务器等）问题时，检查它们的日志——在 Linux 上通常在 `/var/log/` 中。使用 `journalctl -u <服务名>` 查看 systemd 服务的日志。对于第三方库，检查它们是否通过环境变量或配置支持调试日志。

### 2. 调试器（Debuggers）

当你**不确定需要什么信息**、Bug 仅在**难以复现**的条件下出现，或者修改和重启程序**代价高昂**（启动时间长、复杂状态需要重建等）时，调试器变得非常有价值。

调试器是让你与正在执行的程序交互的程序，允许你：

- 在到达某一行时暂停执行
- 单步执行（一次一条指令）
- 崩溃后检查变量的值
- 在满足给定条件时条件性地暂停执行
- 以及更多高级功能

大多数编程语言都支持（或附带）某种形式的调试器。最通用的是 **GDB**（GNU Debugger）和 **LLDB**（LLVM Debugger），可以调试任何原生二进制文件。许多语言也有与运行时更紧密集成的语言特定调试器（如 Python 的 `pdb` 或 Java 的 `jdb`）。

**GDB 常用命令**：

| 命令                                    | 说明               |
| --------------------------------------- | ------------------ |
| `run`                                 | 启动程序           |
| `b {function}` 或 `b {file}:{line}` | 设置断点           |
| `c`                                   | 继续执行           |
| `step` / `next` / `finish`        | 步入 / 步过 / 步出 |
| `p {variable}`                        | 打印变量值         |
| `bt`                                  | 显示回溯（调用栈） |
| `watch {expression}`                  | 值变化时中断       |

> 考虑使用 GDB 的 TUI 模式（`gdb -tui` 或在 GDB 内按 `Ctrl-x a`）获得分屏视图，同时显示源代码和命令提示符。

### 3. 记录-重放调试（Record-Replay Debugging）

最令人沮丧的 Bug 之一是 **Heisenbugs**：当你试图观察它们时，它们似乎消失或改变行为。竞态条件、时序相关的 Bug 以及只在特定系统条件下出现的问题都属于这一类。传统调试通常在这里毫无用处，因为再次运行程序会产生不同的行为（例如，print 语句可能减慢代码速度，使得竞态不再发生）。

**记录-重放调试**通过记录程序的执行过程，允许你按需**确定性重放**来解决这个问题。更好的是，你可以在执行过程中**反向执行**，以精确定位出错的位置。

**[rr](https://rr-project.org/)** 是 Linux 上一个强大的工具，它记录程序执行并允许具有完整调试能力的确定性重放。它与 GDB 配合使用。

**基本用法**：

```bash
# 记录程序执行
rr record ./my_program

# 重放记录（打开 GDB）
rr replay
```

**反向调试命令**：

| 命令                           | 说明                     |
| ------------------------------ | ------------------------ |
| `reverse-continue`（`rc`） | 反向运行直到命中断点     |
| `reverse-step`（`rs`）     | 反向单步执行一行         |
| `reverse-next`（`rn`）     | 反向单步，跳过函数调用   |
| `reverse-finish`             | 反向运行直到进入当前函数 |

这对于调试非常强大。假设你有一个崩溃——你可以：

- 运行到崩溃点
- 检查损坏的状态
- 在损坏的变量上设置观察点
- 使用 `reverse-continue` 精确定位它被损坏的位置

**何时使用 rr**：

- 间歇性失败的不稳定测试
- 竞态条件和线程 Bug
- 难以复现的崩溃
- 任何你希望“回到过去”的 Bug

> **注意**：rr 仅适用于 Linux，需要硬件性能计数器。它在不暴露这些计数器的虚拟机中无法工作（如大多数 AWS EC2 实例），也不支持 GPU 访问。对于 macOS，可以查看 [Warpspeed](https://warpspeed.dev/)。
>
> **rr 与并发**：由于 rr 确定性记录执行过程，它会序列化线程调度。这意味着某些依赖特定时序的竞态条件在 rr 下可能不会显现。rr 对于调试竞态仍然有用——一旦捕获了一次失败运行，你可以可靠地重放它——但可能需要多次记录尝试来捕获间歇性 Bug。对于不涉及并发的 Bug，rr 最为出色：你总是可以重现确切的执行过程，并使用反向调试来追踪损坏。

### 4. 系统调用跟踪（System Call Tracing）

有时你需要了解程序如何与操作系统交互。程序通过**系统调用**向内核请求服务——打开文件、分配内存、创建进程等。跟踪这些调用可以揭示程序为何挂起、尝试访问哪些文件，或在哪里等待耗时。

#### strace（Linux）和 dtruss（macOS）

`strace` 让你观察程序发出的每一个系统调用：

```bash
# 跟踪所有系统调用
strace ./my_program

# 只跟踪文件相关的调用
strace -e trace=file ./my_program

# 跟踪子进程（对于启动其他程序的程序很重要）
strace -f ./my_program

# 跟踪正在运行的进程
strace -p <PID>

# 显示时序信息
strace -T ./my_program
```

在 macOS 和 BSD 上，使用 `dtruss`（包装了 `dtrace`）实现类似功能。

> 关于 strace 的深入探讨，可以查看 Julia Evans 的 [strace zine](https://jvns.ca/strace-zine-unfolded.pdf)。

#### bpftrace 和 eBPF

**eBPF**（extended Berkeley Packet Filter）是一项强大的 Linux 技术，允许在内核中运行沙箱程序。

**bpftrace** 为编写 eBPF 程序提供了高级语法。这些是在内核中运行的任意程序，因此具有巨大的表达能力（尽管语法有些类似 awk）。最常见的用例是调查正在调用的系统调用，包括聚合（如计数或延迟统计）或内省（甚至过滤）系统调用参数。

```bash
# 系统范围内跟踪文件打开（立即打印）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'

# 按名称统计系统调用（Ctrl-C 时打印摘要）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* { @[probe] = count(); }'
```

你还可以使用 **BCC** 工具链直接用 C 编写 eBPF 程序，它附带了许多方便的工具，如 `biosnoop`（打印磁盘操作的延迟分布）或 `opensnoop`（打印所有打开的文件）。

`strace` 的优势在于“开箱即用”，而当你需要更低的开销、想要跟踪内核函数、或需要做任何聚合时，应该使用 `bpftrace`。注意 `bpftrace` 必须以 root 身份运行，并且通常监控整个内核，而不仅仅是特定进程。

要定位特定程序，可以按命令名或 PID 过滤：

```bash
# 按命令名过滤（Ctrl-C 时打印摘要）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /comm == "bash"/ { @[probe] = count(); }'

# 使用 -c 从启动开始跟踪特定命令（cpid = 子进程 PID）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /pid == cpid/ { @[probe] = count(); }' -c 'ls -la'
```

#### 网络调试（Network Debugging）

对于网络问题，**tcpdump** 和 **Wireshark** 可以捕获和分析网络数据包：

```bash
# 捕获端口 80 上的数据包
sudo tcpdump -i any port 80

# 捕获并保存到文件供 Wireshark 分析
sudo tcpdump -i any -w capture.pcap
```

对于 HTTPS 流量，加密使得 `tcpdump` 不那么有用。像 **mitmproxy** 这样的工具可以充当拦截代理来检查加密流量。浏览器开发者工具（Network 标签）通常是调试 Web 应用 HTTPS 请求的最简单方式——它们显示解密后的请求/响应数据、头部和时序。

### 5. 内存调试（Memory Debugging）

内存 Bug——缓冲区溢出、释放后使用、内存泄漏——是最危险且最难调试的。它们通常不会立即崩溃，而是以在很久以后导致问题的方式损坏内存。

#### Sanitizers

一种发现内存 Bug 的方法是使用 sanitizers，它们是编译器特性，通过插桩代码在运行时检测错误。

**AddressSanitizer（ASan）** 检测：

- 缓冲区溢出（栈、堆和全局）
- 释放后使用
- 返回后使用
- 内存泄漏

```bash
# 使用 AddressSanitizer 编译
gcc -fsanitize=address -g program.c -o program
./program
```

其他有用的 sanitizers：

- **ThreadSanitizer（TSan）**：检测多线程代码中的数据竞争（`-fsanitize=thread`）
- **MemorySanitizer（MSan）**：检测未初始化内存的读取（`-fsanitize=memory`）
- **UndefinedBehaviorSanitizer（UBSan）**：检测未定义行为，如整数溢出（`-fsanitize=undefined`）

Sanitizers 需要重新编译，但速度足够快，可以在 CI 流水线和日常开发中使用。

#### Valgrind：当你无法重新编译时

**Valgrind** 在类似虚拟机的环境中运行你的程序来检测内存错误。它比 sanitizers 慢，但不需要重新编译：

```bash
valgrind --leak-check=full ./my_program
```

在以下情况使用 Valgrind：

- 你没有源代码
- 你无法重新编译（第三方库）
- 你需要 sanitizers 未提供的特定工具

Valgrind 实际上是一个功能强大的受控执行环境，我们将在性能分析部分再次看到它。

### 6. AI 辅助调试

大语言模型已成为令人惊讶的有用调试助手。它们在以下方面表现出色：

- **解释隐晦的错误信息**：编译器错误，尤其是 C++ 模板或 Rust 的借用检查器错误，可能非常隐晦。LLM 可以将它们翻译成通俗英语并建议修复。
- **跨越语言和抽象边界**：如果你在调试一个跨越多种语言的问题（例如，一个通过 Python 绑定暴露的 C 库中的 Bug），LLM 可以帮助导航不同层次。它们特别擅长理解 FFI 边界、构建系统问题和跨语言调试。
- **将症状与根因关联**：“我的程序工作正常但内存使用量是预期的 10 倍”这类模糊症状，LLM 可以帮助调查，建议可能的原因和需要查看的内容。
- **分析崩溃转储和堆栈跟踪**：粘贴堆栈跟踪并询问可能的原因。

> **关于调试符号的说明**：为了有意义的堆栈跟踪和调试，请确保你的二进制文件（以及任何链接的库）使用调试符号编译（`-g` 标志）。调试信息通常以 DWARF 格式存储。此外，使用帧指针编译（`-fno-omit-frame-pointer`）使堆栈跟踪更可靠，尤其是对性能分析工具。没有这些，堆栈跟踪可能只显示内存地址或不完整。这对于原生编译的程序（C++、Rust）比 Python 或 Java 更重要。

**需要牢记的限制**：

- LLM 可能产生听起来合理但错误的解释
- 它们可能建议掩盖 Bug 而非修复它的修复方案
- 始终用实际的调试工具验证建议
- 它们最好作为理解代码的补充，而非替代品

> 这与开发环境讲座中涉及的[通用 AI 编码能力](/2026/development-environment/#ai-powered-development)不同。这里我们专门讨论将 LLM 用作调试辅助工具。

---

## 二、性能分析（Profiling）

即使你的代码在功能上按预期工作，如果它消耗了所有 CPU 或内存，那仍然不够好。算法课程通常教授大 O 表示法，但不教授如何找到程序中的热点。

### 1. 计时（Timing）

测量性能的最简单方法是**计时**。在许多场景中，打印代码两点之间的时间就足够了。

然而，**墙钟时间（wall clock time）** 可能具有误导性，因为你的计算机可能同时运行其他进程或等待事件发生。

`time` 命令区分三种时间：

- **Real**：从开始到结束的墙钟时间，包括等待时间
- **User**：在 CPU 上运行用户代码的时间
- **Sys**：在 CPU 上运行内核代码的时间

```bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real    0m0.272s
user    0m0.079s
sys     0m0.028s
```

这里请求花费了近 300 毫秒（real time），但只有 107 毫秒的 CPU 时间（user + sys）。其余时间在等待网络。

### 2. 资源监控（Resource Monitoring）

分析程序性能的第一步通常是了解其实际资源消耗情况。程序在资源受限时通常运行缓慢。

- **通用监控**：**htop** 是 `top` 的改进版本，显示当前运行进程的各种统计信息。有用按键：`<F6>` 排序，`t` 显示树层次结构，`h` 切换线程。还有 **btop** 监控更多内容。
- **I/O 操作**：**iotop** 显示实时 I/O 使用信息。
- **内存使用**：**free** 显示总空闲和已用内存。
- **打开的文件**：**lsof** 列出进程打开的文件信息。对于检查哪个进程打开了特定文件很有用。
- **网络连接**：**ss** 监控网络连接。常见用例是找出哪个进程在使用给定端口：`ss -tlnp | grep :8080`。
- **网络使用**：**nethogs** 和 **iftop** 是用于按进程监控网络使用的好用的交互式 CLI 工具。

### 3. 可视化性能数据（Visualizing Performance Data）

人类在图表中发现模式的速度比在数字表格中快得多。分析性能时，绘制数据通常会揭示在原始数字中不可见的趋势、峰值和异常。

**使数据可绘制**：在添加用于调试的 print 或 log 语句时，考虑格式化输出以便以后轻松绘图。简单的带时间戳的 CSV 格式值（`1705012345,42.5`）比散文句子更容易绘图。JSON 结构化日志也可以以最小的努力进行解析和绘图。

**使用 gnuplot 快速绘图**：

```bash
# 绘制简单的带时间戳、值的 CSV
gnuplot -e "set datafile separator ','; plot 'latency.csv' using 1:2 with lines"
```

**使用 matplotlib 和 ggplot2 进行迭代探索**：对于更深入的分析，Python 的 **matplotlib** 和 R 的 **ggplot2** 支持迭代探索。与一次性绘图不同，这些工具允许你快速切片和转换数据以调查假设。`ggplot2` 的分面图特别强大——你可以按类别（例如，按端点或一天中的时间对请求延迟进行分面）将单个数据集分割到多个子图中，以揭示原本隐藏的模式。

**示例用例**：

- 绘制随时间变化的请求延迟可以揭示周期性减速（垃圾回收、cron 任务、流量模式），而这些是原始百分位数所掩盖的
- 可视化增长数据结构的插入时间可以暴露算法复杂度问题——当后备数组翻倍时，向量插入的绘图会显示特征性尖峰
- 按不同维度（请求类型、用户群组、服务器）分面指标通常可以揭示一个“系统范围”的问题实际上被隔离到某一个类别

### 4. CPU 分析器（CPU Profilers）

大多数时候人们提到分析器，指的是 CPU 分析器。主要有两种类型：

- **跟踪分析器（Tracing profilers）**：记录程序发出的每一次函数调用
- **采样分析器（Sampling profilers）**：定期探测程序（通常每毫秒一次）并记录程序的堆栈

采样分析器开销较低，通常更适合生产环境使用。

#### perf：采样分析器

`perf` 是标准的 Linux 分析器。它可以在不重新编译的情况下分析任何程序。

`perf stat` 提供时间花费的快速概览：

```bash
$ perf stat ./slow_program
Performance counter stats for './slow_program':
      3,210.45 msec task-clock          # 0.998 CPUs utilized
            12 context-switches         # 3.738 /sec
             0 cpu-migrations           # 0.000 /sec
           156 page-faults              # 48.587 /sec
12,345,678,901 cycles                   # 3.845 GHz
 9,876,543,210 instructions             # 0.80 insn per cycle
 1,234,567,890 branches                 # 384.532 M/sec
    12,345,678 branch-misses            # 1.00% of all branches
```

真实世界的分析器输出包含大量信息。人类是视觉动物，非常不擅长阅读大量数字。**火焰图（Flame graphs）** 是一种使分析数据更容易理解的可视化方式。

火焰图在 Y 轴上显示函数调用的层次结构，在 X 轴上显示与时间成比例的长度。它们是交互式的——你可以点击放大程序的特定部分。

从 `perf` 数据生成火焰图：

```bash
# 记录分析数据
perf record -g ./my_program

# 生成火焰图（需要 flamegraph 脚本）
perf script | stackcollapse-perf.pl | flamegraph.pl > flamegraph.svg
```

> 考虑使用 [Speedscope](https://www.speedscope.app/) 作为交互式基于 Web 的火焰图查看器，或使用 [Perfetto](https://perfetto.dev/) 进行全面的系统级分析。

#### Valgrind 的 Callgrind：跟踪分析器

`callgrind` 是一个记录程序调用历史和指令计数的分析工具。与采样分析器不同，它提供精确的调用计数，并可以显示调用者和被调用者之间的关系：

```bash
# 使用 callgrind 运行
valgrind --tool=callgrind ./my_program

# 使用 callgrind_annotate（文本）或 kcachegrind（GUI）分析
callgrind_annotate callgrind.out.<pid>
kcachegrind callgrind.out.<pid>
```

Callgrind 比采样分析器慢，但提供精确的调用计数，并且可以选择模拟缓存行为（使用 `--cache-sim=yes`）。

> 如果你使用特定语言，可能有更专门的分析器。例如，Python 有 **cProfile** 和 **py-spy**，Go 有 **go tool pprof**，Rust 有 **cargo-flamegraph**（实际上适用于任何编译的程序！）。

### 5. 内存分析器（Memory Profilers）

内存分析器帮助你理解程序如何随时间使用内存，并发现内存泄漏。

#### Valgrind 的 Massif

`massif` 分析堆内存使用情况：

```bash
valgrind --tool=massif ./my_program
ms_print massif.out.<pid>
```

这显示随时间变化的堆使用情况，有助于识别内存泄漏和过量分配。

> 对于 Python，[memory-profiler](https://pypi.org/project/memory-profiler/) 提供逐行的内存使用信息。

### 6. 基准测试（Benchmarking）

当你需要比较不同实现或工具的性能时，**hyperfine** 非常适合对命令行程序进行基准测试：

```bash
$ hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'
Benchmark #1: fd -e jpg
  Time (mean ± σ):      51.4 ms ±   2.9 ms    [User: 121.0 ms, System: 160.5 ms]
  Range (min … max):    44.2 ms …  60.1 ms    56 runs

Benchmark #2: find . -iname "*.jpg"
  Time (mean ± σ):      1.126 s ±  0.101 s    [User: 141.1 ms, System: 956.1 ms]
  Range (min … max):    0.975 s …  1.287 s    10 runs

Summary
  'fd -e jpg' ran 21.89 ± 2.33 times faster than 'find . -iname "*.jpg"'
```

> 对于 Web 开发，浏览器开发者工具包括优秀的分析器。参见 [Firefox Profiler](https://profiler.firefox.com/docs/) 和 [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/rendering-tools) 文档。

---

## 三、练习

### 调试练习

1. **调试排序算法**：下面的伪代码实现了归并排序但包含一个 Bug。用你选择的语言实现它，然后使用调试器（gdb、lldb、pdb 或你的 IDE 调试器）找到并修复 Bug。

   ```python
   function merge_sort(arr):
       if length(arr) <= 1:
           return arr
       mid = length(arr) / 2
       left = merge_sort(arr[0..mid])
       right = merge_sort(arr[mid..end])
       return merge(left, right)

   function merge(left, right):
       result = []
       i = 0, j = 0
       while i < length(left) AND j < length(right):
           if left[i] <= right[j]:
               append result, left[i]
               i = i + 1
           else:
               append result, right[i]   # Bug: 应该是 right[j]
               j = j + 1
       append remaining elements from left and right
       return result
   ```

   测试向量：`merge_sort([3, 1, 4, 1, 5, 9, 2, 6])` 应返回 `[1, 1, 2, 3, 4, 5, 6, 9]`。使用断点单步执行 merge 函数，找到选择错误元素的位置。
2. **使用 rr 进行反向调试**：安装 `rr` 并使用反向调试查找损坏 Bug。

   ```c
   #include <stdio.h>

   typedef struct {
       int id;
       int scores[3];
   } Student;

   Student students[2];

   void init() {
       students[0].id = 1001;
       students[0].scores[0] = 85;
       students[0].scores[1] = 92;
       students[0].scores[2] = 78;
       students[1].id = 1002;
       students[1].scores[0] = 90;
       students[1].scores[1] = 88;
       students[1].scores[2] = 95;
   }

   void curve_scores(int student_idx, int curve) {
       for (int i = 0; i < 4; i++) {   // Bug: 应该是 i < 3，越界写入
           students[student_idx].scores[i] += curve;
       }
   }

   int main() {
       init();
       printf("=== Initial state ===\n");
       printf("Student 0: id=%d\n", students[0].id);
       printf("Student 1: id=%d\n", students[1].id);

       curve_scores(0, 5);

       printf("\n=== After curving ===\n");
       printf("Student 0: id=%d\n", students[0].id);
       printf("Student 1: id=%d\n", students[1].id);

       if (students[1].id != 1002) {
           printf("\nERROR: Student 1's ID was corrupted! Expected 1002, got %d\n", students[1].id);
           return 1;
       }
       return 0;
   }
   ```

   使用 `gcc -g corruption.c -o corruption` 编译并运行。Student 1 的 ID 被损坏了，但损坏发生在一个只操作 student 0 的函数中。使用 `rr record ./corruption` 和 `rr replay` 找出罪魁祸首。在 `students[1].id` 上设置观察点，并在损坏后使用 `reverse-continue` 精确定位是哪行代码覆盖了它。
3. **使用 AddressSanitizer 调试内存错误**：

   ```c
   #include <stdio.h>
   #include <stdlib.h>
   #include <string.h>

   int main() {
       char *greeting = malloc(32);
       strcpy(greeting, "Hello, world!");
       printf("%s\n", greeting);
       free(greeting);
       greeting[0] = 'J';   // 释放后使用（use-after-free）
       printf("%s\n", greeting);
       return 0;
   }
   ```

   首先在不使用 sanitizer 的情况下编译运行：`gcc uaf.c -o uaf && ./uaf`。它可能看起来正常工作。现在使用 AddressSanitizer 编译：`gcc -fsanitize=address -g uaf.c -o uaf && ./uaf`。阅读错误报告。ASan 发现了什么 Bug？修复它。
4. **使用 strace 跟踪系统调用**：使用 `strace`（Linux）或 `dtruss`（macOS）跟踪 `ls -l` 等命令的系统调用。它发出了哪些系统调用？尝试跟踪更复杂的程序，看看它打开了哪些文件。
5. **使用 LLM 帮助调试**：尝试复制一个编译器错误（尤其是 C++ 模板或 Rust 的）并询问解释和修复方案。尝试将 `strace` 或 AddressSanitizer 的一些输出输入其中。

### 性能分析练习

1. **使用 perf stat**：对你选择的程序使用 `perf stat` 获取基本性能统计信息。不同计数器代表什么？
2. **使用 perf record 和火焰图**：

   ```c
   #include <stdio.h>
   #include <math.h>

   double slow_computation(int n) {
       double result = 0;
       for (int i = 0; i < n; i++) {
           for (int j = 0; j < 1000; j++) {
               result += sin(i * j) * cos(i + j);
           }
       }
       return result;
   }

   int main() {
       double r = 0;
       for (int i = 0; i < 100; i++) {
           r += slow_computation(1000);
       }
       printf("Result: %f\n", r);
       return 0;
   }
   ```
   使用调试符号编译：`gcc -g -O2 slow.c -o slow -lm`。运行 `perf record -g ./slow`，然后 `perf report` 查看时间花费在哪里。尝试使用 flamegraph 脚本生成火焰图。
3. **使用 hyperfine 进行基准测试**：使用 `hyperfine` 对同一任务的两种不同实现进行基准测试（例如，`find` vs `fd`，`grep` vs `ripgrep`，或你自己的代码的两个版本）。
4. **使用 htop 监控系统**：在运行资源密集型程序时使用 `htop` 监控系统。尝试使用 `taskset` 限制进程可以使用的 CPU：`taskset --cpu-list 0,2 stress -c 3`。为什么 `stress` 没有使用三个 CPU？
5. **查找占用端口的进程**：一个常见问题是想要监听的端口已被另一个进程占用。学习如何发现该进程：首先执行 `python -m http.server 4444` 在端口 4444 上启动一个最小的 Web 服务器。在另一个终端上运行 `ss -tlnp | grep 4444` 找到该进程。用 `kill` 终止它。
