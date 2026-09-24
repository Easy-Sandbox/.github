# 欢迎使用 Easy Sandbox

本目录包含 Easy Sandbox SDK 的完整使用示例，覆盖从基础操作到高级场景。

## 准备

### 1. 安装 SDK

```bash
# 完整安装（推荐，包含 CLI + 声明式装饰器）
pip install easy-sandbox[all]

# 或按需安装
pip install easy-sandbox[cli]          # 仅 CLI
pip install easy-sandbox[declarative]  # 仅 @sandbox 装饰器

# 从源码安装
pip install -e .
```

### 2. 配置 API Key

```bash
# 方式一：环境变量
export E2B_API_KEY="your-api-key"

# 方式二：CLI 配置（持久化到 ~/.ebx/config.toml）
ebx config set api_key your-api-key

# 如需运行 Codex Agent 示例，还需配置：
export OPENAI_API_KEY="your-openai-key-here"
```

## 快速开始

运行任意示例：

```bash
# 基础示例
python examples/quickstart/01_hello.py

# 数据分析
python examples/quickstart/04_data_analysis.py

# @sandbox 装饰器
python examples/quickstart/05_decorator_usage.py

# E2B 兼容
python examples/compat-demos/e2b_data_analysis.py
```

## 核心 API 速览

```python
from easy_sandbox import Sandbox

# 创建沙箱（推荐使用 async with 自动管理生命周期）
async with await Sandbox.create(template="base", api_key="...") as sandbox:

    # 执行命令
    result = await sandbox.commands.run("echo hello")
    print(result.stdout, result.exit_code)

    # 文件操作
    await sandbox.files.write("/app/data.txt", "内容")
    content = await sandbox.files.read("/app/data.txt")

    # 执行代码
    code_result = await sandbox.run_code("print(1 + 1)")
    print(code_result.text)

    # 端口访问
    url = sandbox.network.get_url(3000)
```

```python
# @sandbox 装饰器 — 声明式远程执行
from easy_sandbox.declarative import sandbox

@sandbox(template="code-interpreter", packages=["numpy"])
def compute(n: int) -> float:
    import numpy as np
    return float(np.random.random(n).mean())

result = compute(1000)  # 自动在远程沙箱中执行
```
