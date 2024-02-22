---
{"dg-publish":true,"permalink":"/What I Learned/2024-M2/Windows WSL 开发环境/"}
---

## 安装

[Install WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/install#install-wsl-command)

在管理员 Terminal 下：

```bash
wsl --install
```

## Python

### Anaconda

如果忘记在设置环境时选 yes，可以手动修改 `.bashrc`

```bash
source ~/anaconda3/etc/profile.d/conda.sh                                
eval "$(/home/aspi-rin/anaconda3/bin/conda shell.bash hook)"
```

然后 `source .bashrc`

## LazyGit

https://github.com/jesseduffield/lazygit#ubuntu
