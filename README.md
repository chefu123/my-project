# my-project

代谢建模用的 Python 包是 **cobra**（[COBRApy](https://opencobra.github.io/cobrapy/)），不是 `cobra.exe`。

Notebook 在**远程 Linux 服务器**上跑。包必须装进**那台服务器、那个 Jupyter 内核用的 Python**，装在本机 Windows 上无效。

## 不要在 notebook 里 `%pip`

服务器上的 notebook 内核解析不了 `pypi.org`（`NameResolutionError`），`%pip` 会失败。到**服务器终端**（SSH）里装。

## 1. 先看内核用的是哪个 Python

在 notebook 里只跑这一格（不需要 cobra）：

```python
import sys
print(sys.executable)
```

记下打印出来的路径，下面叫 `<PYTHON>`。

## 2. 在服务器终端（SSH）里跑，不要贴进 Python 单元格

这是 **bash**，不是 Python。路径前面要有 `/`。

JupyterHub 常见路径：

```bash
/opt/jhub/venv/bin/python3 -m pip install cobra==0.32.1 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

如果一定要在 notebook 里装，必须用 `!` 开头（还是 shell，不是 Python）：

```python
!/opt/jhub/venv/bin/python3 -m pip install cobra==0.32.1 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

没有 `!`、也没有 SSH 终端，直接粘贴会变成 `python3(...)` 函数调用，报 `SyntaxError`。

`/opt/jhub/venv` 多半是共享环境。若提示 `Permission denied`，不要硬装进系统 venv，改用自己的环境或让管理员装。

服务器若有 conda：

```bash
conda install -c conda-forge cobra
```

装完后 notebook **Restart Kernel**，再 `import cobra`。不要再 `%pip`。

## 常见失败

| 现象 | 原因 | 做法 |
| --- | --- | --- |
| `Failed to resolve 'pypi.org'` | 服务器 DNS 到不了 PyPI | 用清华镜像 `-i https://pypi.tuna.tsinghua.edu.cn/simple` |
| `No module named 'cobra'` | 装到了另一个 Python，或 `pip --user` 被内核隔离 | 必须用 `sys.executable` 那个解释器 `-m pip install` |
| `No matching distribution found for cobra.exe` | 包名写错 | 用 `cobra`，不要 `cobra.exe` |
