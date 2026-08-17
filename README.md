# my-project

代谢建模用的 Python 包是 **cobra**（[COBRApy](https://opencobra.github.io/cobrapy/)），不是 `cobra.exe`。

`cobra.exe` 是 Windows 可执行文件名，PyPI 上没有这个包。`pip` 会把它当成包名 `cobra-exe` 去搜，所以会报 `No matching distribution found`。

## 安装

**不要在 notebook 里用 `%pip install`。** 内核经常解析不了 `pypi.org`，会报 `NameResolutionError`，这和包名无关。

在终端安装（PowerShell）：

```powershell
pip install cobra
```

或按本仓库锁定版本：

```powershell
pip install -r requirements.txt
```

本机若也解析不了 `pypi.org`，改用清华镜像：

```powershell
pip install cobra -i https://pypi.tuna.tsinghua.edu.cn/simple
```

推荐用项目虚拟环境（notebook 才能稳定找到包）：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Linux：

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Notebook 报 `No module named 'cobra'`

内核是隔离的，看不到 `pip --user` 装到 `~/.local` 的包。不要 `%pip`。

1. 右上角把内核切到 **`.venv`**（或系统 `Python 3.12`）
2. **Restart Kernel**
3. 打开 `hello_cobra.ipynb` 只跑 `import cobra`

```python
import sys
print(sys.executable)
import cobra
print(cobra.__version__)
```

`sys.executable` 应是 `.venv` 里的 python，或 `/usr/bin/python3`。
