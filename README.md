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

装完后 notebook 点 **Restart Kernel**，不要再跑 `%pip`。

## 检查是否装好

打开 `hello_cobra.ipynb`，或直接：

```python
import cobra
print(cobra.__version__)
```
