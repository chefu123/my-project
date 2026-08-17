# my-project

代谢建模用的 Python 包是 **cobra**（[COBRApy](https://pypi.org/project/cobra/)），不是 `cobra.exe`。

Notebook 在 **JupyterHub 服务器**（`/opt/jhub/venv`）上跑。包必须装进该内核的 Python。

## 不要在 notebook 里 pip

内核 **没有外网 DNS**。`pypi.org` 和清华镜像都会 `NameResolutionError`。换源、`%pip`、`!pip` 都没用。

## 1. 诊断（notebook 里跑）

```python
import os, socket, sys
print("python:", sys.executable)
for k in ["http_proxy", "https_proxy", "HTTP_PROXY", "HTTPS_PROXY"]:
    print(k, "=", os.environ.get(k))
for host in ["pypi.org", "pypi.tuna.tsinghua.edu.cn"]:
    try:
        print(host, socket.getaddrinfo(host, 443)[0][4])
    except Exception as e:
        print(host, "FAIL", type(e).__name__, e)
```

把输出留下来。若两个 host 都 FAIL，就不要再 pip。

## 2. 可选装法

**A. SSH 登录节点再装（优先试）**

Jupyter 内核常被隔离，SSH 登录节点有时能上网：

```bash
getent hosts pypi.tuna.tsinghua.edu.cn
/opt/jhub/venv/bin/python3 -m pip install cobra==0.32.1 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

优点：对准内核 Python。缺点：登录节点也可能没 DNS；`/opt/jhub/venv` 可能没写权限。

**B. 本机能上网时，离线 wheel 再上传（DNS 全挂时用这个）**

本机 PowerShell：

```powershell
mkdir cobra-wheels
pip download cobra==0.32.1 -d cobra-wheels -i https://pypi.tuna.tsinghua.edu.cn/simple
scp -r cobra-wheels 用户名@服务器:~/
```

服务器 SSH：

```bash
/opt/jhub/venv/bin/python3 -m pip install --no-index --find-links="$HOME/cobra-wheels" cobra==0.32.1
```

若 Permission denied：

```bash
/opt/jhub/venv/bin/python3 -m pip install --user --no-index --find-links="$HOME/cobra-wheels" cobra==0.32.1
```

优点：不依赖服务器 DNS。缺点：wheel 要和服务器 Python 3.x / Linux 架构匹配；`--user` 有时仍被内核隔离。

**C. 让管理员装进 `/opt/jhub/venv`**

优点：所有 notebook 都能 `import cobra`。缺点：要等管理员。

装完后 **Restart Kernel**，再 `import cobra`。
