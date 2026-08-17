# my-project

代谢建模用的 Python 包是 **cobra**（[COBRApy](https://opencobra.github.io/cobrapy/)），不是 `cobra.exe`。

`cobra.exe` 是 Windows 可执行文件名，PyPI 上没有这个包。`pip` 会把它当成包名 `cobra-exe` 去搜，所以会报 `No matching distribution found`。

## 安装

PowerShell：

```powershell
pip install cobra
```

或按本仓库锁定版本：

```powershell
pip install -r requirements.txt
```

Notebook 里装完后需要 **Restart Kernel**。

## 检查是否装好

```python
import cobra
print(cobra.__version__)
```
