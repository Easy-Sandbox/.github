<div align="center">

**Welcome to Easy Sandbox**

</div>

## 📦 Repositories

| Repository | Description |
|:--|:--|
| [`serverless-sandbox`](https://github.com/serverless-sandbox/serverless-sandbox) | Core SDK & CLI — sandbox lifecycle, file I/O, code execution, agent tools, and more. |
| [`awesome-templates`](https://github.com/serverless-sandbox/awesome-templates) | Community-curated sandbox templates — Python, Node.js, data science, code interpreters. |

## 🚀 Quick Start

```bash
pip install easy-sandbox[all]
```

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

> Need the CLI? `pip install "serverless-sandbox[cli]"` — then run `sbox create --template python-base`.
