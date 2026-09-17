# 2026 Tencent Game Security Competition — PC Client Security (Preliminary) Writeup

Writeup for the **PC Client Security** track of the **2026 Tencent Game Security
Technology Competition** (腾讯游戏安全技术竞赛), preliminary round. This work
**advanced to the finals**.

> Author: Li Jieheng · Category: PC Client Security / Windows kernel reverse engineering

## The challenge

The challenge ("ShadowGate") ships a Windows **kernel driver** (`ShadowGateSys.sys`)
plus a user-mode console. The driver hides an encrypted "maze" and never returns
results directly — all information has to be recovered through **side channels**.
The goal is to break through the maze and extract the flag.

The driver is heavily obfuscated (VMProtect-style: junk code, opaque predicates),
so static analysis alone is not enough — the solution combines static reverse
engineering (IDA Pro) with **live kernel debugging** (WinDbg) and custom runtime
instrumentation.

## What this repo contains

- **`writeup.pdf`** — the full 32-page writeup: environment setup, driver loading,
  static/dynamic analysis, discovery and verification of **all 5 side channels**,
  IOCTL reconstruction, full maze recovery, an automated exploration + shortest-path
  solver, and the final flag extraction (with the mistakes and dead-ends kept in).

To solve the challenge I wrote custom C++ tooling — a side-channel detector, a DLL
injector with IOCTL hooking, a state visualizer, and an automated maze solver. That
process is documented in the writeup; the tool source is **not** included in this
repository.

## Disclaimer

This is **educational security-research** material produced for a sanctioned
Capture-the-Flag competition. All work was carried out against the competition's own
challenge binary inside an isolated virtual machine. It is shared as a learning
resource for reverse engineering and Windows internals, and is not intended to be
used against any third-party software.

## License

Released under the [MIT License](LICENSE).

---

## 中文说明

这是 **2026 腾讯游戏安全技术竞赛 · PC 客户端安全方向 · 初赛**的 Writeup（已晋级决赛）。

**题目**：一个 Windows **内核驱动**（`ShadowGateSys.sys`）加用户态控制台，驱动内部隐藏加密迷宫，
不直接返回结果，只能通过**侧信道**获取信息，目标是突破迷宫取出 Flag。驱动被重度混淆（疑似 VMProtect），
单靠静态分析不够，解题结合了 IDA Pro 静态逆向、WinDbg 内核动态调试与自写的运行时工具。

**内容**：`writeup.pdf` 是完整 32 页记录（环境搭建、驱动加载、静/动态分析、5 种侧信道的发现与验证、
IOCTL 还原、完整迷宫复原、自动化探索＋最短路径求解、最终取 Flag，含所有踩坑与纠错）。解题时自写的
工具（侧信道检测器、DLL 注入/IOCTL Hook、可视化、自动求解器）在 writeup 里有说明，**源码不放进本仓库**。

**声明**：本仓库为正规 CTF 竞赛的**安全研究学习资料**，所有工作都在隔离虚拟机中针对比赛自带的题目程序进行，
仅供逆向与 Windows 内核学习参考，不针对任何第三方软件。
