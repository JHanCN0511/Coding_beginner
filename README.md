# Python 科研计算环境配置指南：VS Code + Miniconda

> **适用对象**：机械工程、物理及相关理工科专业学生
> **目标**：搭建一套轻量、现代且可扩展的 Python 编程环境，替代传统的 Anaconda + Spyder 组合。
> **核心工具**：Miniconda (环境管理) + VS Code (代码编辑与交互)

---

## 第一部分：安装 Miniconda (Python 环境管理器)

Miniconda 是 Anaconda 的精简版，只包含最基础的 Python 和 Conda 管理器，避免了安装大量无用的库，更加轻量高效。

### 1. 下载

推荐使用清华大学开源软件镜像站下载，速度快且稳定。

* **下载地址**：[Miniconda 清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)
* **选择文件**：找到 `Miniconda3-latest-Windows-x86_64.exe` 并下载。

### 2. 安装设置 (关键步骤)

运行安装包，在 **"Advanced Options"** 界面，请务必按照以下方式勾选：

* [ ] `Add Miniconda3 to my PATH environment variable` -> **不要勾选** (避免污染系统环境，防止软件冲突)。
* [x] `Register Miniconda3 as my default Python 3.x` -> **务必勾选** (让 VS Code 能自动识别)。

---


## 第二部分：配置环境与解决网络问题

由于国内网络环境原因，直接使用默认源下载库经常会出现 `IncompleteRead` 或 `Connection broken` 错误。我们需要先配置国内镜像源。

### 1. 换源 (使用清华源)

在开始菜单搜索并打开 **"Anaconda Prompt (Miniconda3)"** (黑色终端窗口)，**逐行复制**以下命令并回车执行：

```bash
conda config --add channels [https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/)
conda config --add channels [https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/)
conda config --set show_channel_urls yes
```

### 2. 创建通用的科研环境 (Research)
我们不建议直接在 `base` 环境中安装库。建立一个独立的 `research` 环境用于日常数据处理。

在终端中执行以下命令：

```Bash

# 1. 创建环境 (指定 Python 3.9 为例，兼容性较好)
conda create -n research python=3.9

# 2. 激活环境 (激活后命令行前缀会变成 (research))
conda activate research

# 3. 安装常用的科研“三剑客”及 Jupyter 内核
# 注意：ipykernel 是 VS Code 交互式运行所必须的
conda install numpy pandas matplotlib scikit-learn openpyxl ipykernel
```

进阶提示：为什么要建立独立环境？ 如果未来涉及深度学习（Deep Learning），不同项目对 Python 和 CUDA 版本要求不同。建议届时新建环境（如 conda create -n deeplearning python=3.10），避免与通用环境冲突。

## 第三部分：安装 Visual Studio Code (VS Code)
### 1. 下载与安装
- 官网下载：[Visual studio code](https://code.visualstudio.com/)

- 安装设置 (提升效率的关键)： 在安装过程中，务必勾选以下选项，以便直接通过右键打开项目：

  * [x] Add "Open with Code" action to Windows Explorer file context menu

  * [x] Add "Open with Code" action to Windows Explorer directory context menu


### 2. 安装核心插件
启动 VS Code，点击左侧边栏的 方块图标 (Extensions) (快捷键 Ctrl+Shift+X)，搜索并安装以下 4 个插件（认准 Microsoft 官方发布）：

  1. Python (核心插件，识别 Python 环境)

  2. Jupyter (提供类似 Spyder/MATLAB 的分块运行体验)

  3. Pylance (提供智能代码补全)

  4. Chinese (Simplified) (中文语言包，安装后需重启软件)

## 第四部分：编写并运行第一个程序
### 1. 建立项目工作区
VS Code 推荐以“文件夹”为单位管理代码，而不是单个文件。

  1. 在电脑上新建文件夹，例如 `Lab_Code`。

  2. 右键该文件夹，选择 `"Open with Code"`。

### 2. 新建脚本与选择解释器 (最易错点！)
  1. 新建一个文件 `demo.py`。

  3. **关键步骤**：点击 VS Code 界面右下角的状态栏（通常显示 Python 版本号或 Select Interpreter）。

  4. 在弹出的顶部列表中，选择我们刚才创建的环境： `Python 3.9.x ('research': conda)`

### 3. 编写交互式代码
VS Code 支持使用 `# %%` 标记将代码分块，体验与 MATLAB 的 Cell Mode 一致。复制以下代码测试：

```Python

# %% 第一块：导入库
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

print("环境配置成功！开始计算...")

# %% 第二块：生成并处理数据
x = np.linspace(0, 10, 100)
y_signal = np.sin(x)
y_noise = np.random.normal(0, 0.1, 100)
df = pd.DataFrame({'Time': x, 'Signal': y_signal + y_noise})

# %% 第三块：绘图
plt.figure(figsize=(10, 6))
plt.plot(df['Time'], df['Signal'], label='Noisy Data')
plt.plot(x, y_signal, 'r--', label='True Signal', linewidth=2)
plt.legend()
plt.grid(True)
plt.title("VS Code Environment Test")
plt.show()
```

### 4. 运行与查看变量
- 点击代码块上方的 "Run Cell" 运行。

- 查看变量：在右侧弹出的 Interactive Window 顶部，点击 "Variables" 按钮，即可像 Spyder 一样查看矩阵和数据表。

## 常见问题 (FAQ)
Q1: 安装库时报错 `Connection broken` / `IncompleteRead`? A: 这是网络连接海外服务器不稳定导致的。请检查是否已按照第二部分配置了清华镜像源。如果依然报错，尝试在安装命令后加 `--remote-read-timeout-secs 1000`。

Q2: 右下角找不到 'research' 环境？ A: 尝试重启 VS Code。如果还不行，按 `Ctrl+Shift+P` 输入 `Python: Select Interpreter` 手动查找。

Q3: 如何切换到深度学习环境？ A: 在 VS Code 右下角点击当前环境，从列表中选择其他已创建的 Conda 环境（如 `deeplearning`）即可一键切换。
