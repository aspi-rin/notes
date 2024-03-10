---
{"dg-publish":true,"permalink":"/What I Learned/2024-M1/Git/Git 配置文件 git config/","tags":["Git","环境配置"]}
---


Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 git config 时带上 `--system` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 你可以传递 `--global` 选项让 Git 读写此文件，这会对你系统上 所有 的仓库生效。
3. 当前使用仓库的 Git 目录中的 config 文件（即 `.git/config`）：针对该仓库。 你可以传递 --local 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）

```bash
# 查看所有的配置以及它们所在的文件：
git config --list --show-origin
```

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置。每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量（同变量名，最后一个配置起作用）。

---

## 常用 Git 别名

```bash
git config --global alias.co checkout
git config --global alias.br branch
```

```bash
# 查看最后一次提交
git config --global alias.last 'log -1 HEAD'
```
