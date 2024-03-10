---
{"dg-publish":true,"permalink":"/What I Learned/2024-M1/Git/Git 基础/","tags":["Git"]}
---


> [!tip]
> ChatGPT 写的 git 命令已经很好了。
> 善用 `git status` 的提示。

---

## 什么是 Git

- 分布式版本控制系统（DVCS，Distributed Version Control System）
    - 分布式：不需要联网，在自己的机器上就可以使用
    - 版本控制：记录、管理、回溯文件的修改历史

## Git 模型

把 Git 看作是一个维护文件系统的系统，记录文件系统随时间变化的**快照**。

![](https://s2.loli.net/2024/01/13/8MrcRCpoUkPW7s5.jpg)

## config - Git 基础配置

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址

账号配置：

- 多人合作，区分每个人
- 让 GitHub 可以识别你

```bash
# 若针对某个 repo 单独进行配置，不加 --global
git config --global user.name "xxx"
git config --global user.email "xxx@xxx.com"

git config --list
git config --list --show-origin
```

```bash
git config --global http.proxy http://127.0.0.1:10809
git config --global https.proxy http://127.0.0.1:10809
git config --global http.sslVerify false
```

```bash
git config --global --unset http.proxy 
git config --global --unset https.proxy
```

关于配置文件 [[What I Learned/2024-M1/Git/Git 配置文件 git config\|Git 配置文件 git config]]

## status - 检查文件状态

工作目录中的文件可能分三个类别：未跟踪（Untracked）、已追踪（Tracked）、被忽略（Ignored）

![](https://pic.aspi-rin.top/2024/03/1ad805fdda88e35946e5bcaaf0007571.jpeg)

```bash
$ git status -s
 M README  # Modified, not staged
MM Rakefile  # Modified and staged and modified again
A  lib/git.rb  # New file added to stage
M  lib/simplegit.rb  # Modified and staged
?? LICENSE.txt  # Untracked
```

新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。 输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。

## add - 将文件加入暂存区（Stage）

```bash
git add <file/folder>
```

这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为「**精确地将内容添加到下一次提交中**」而不是「将一个文件添加到项目中」要更加合适。

- 暂存区：已经修改、等待后续提交的文件
- 只会添加修改过的文件
- add 之后对文件又进行修改，需要重新 add 以将最新的内容 stage
- **删除文件**的几种情况：
    - 版本库中不存在的文件，直接从本地删除：`rm`
    - 同时删除本地和版本库中的文件：`git rm`
    - 将一个已暂存的新文件取消暂存：`git rm --cached <file>`
- 重命名文件：`git mv`（等价于 `mv + git rm + git add`）

## commit - 提交

```bash
git commit -m "message"
git commit -a -m "stage all unignored files and commit"
```

- 显示上一个提交的基本信息（复查上一个提交）`git show`
- 查看
- 查看提交历史：`git log`
    - --oneline：每一个提交一行
    - --graph：显示分支结构
    - --stat：显示文件删改信息
    - -p：显示详细的修改内容
- 每个提交都有一个唯一的 sha-1 标识符/ID（40 位十六进制数）
    - `git show <id>`：显示提交详细信息（id 在不重复前提下可以只写前几位）
- 检出之前的某一版本：`git checkout <id>`

### commit message 规范

[gitmoji | An emoji guide for your commit messages](https://gitmoji.dev)

[约定式提交](https://www.conventionalcommits.org/zh-hans/v1.0.0)

```
fix: 修复了一个 bug
feat: 新增了一个功能
docs: 文档
style: 用于修改代码的样式，例如调整缩进、空格、空行等
refactor: 重构代码，如修改代码结构、变量名、函数名等但不修改功能逻辑
perf: 提升代码的性能、减少内存占用等
test: 添加、删除、修改代码的测试用例等
```

[Angular 规范](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format)

```
<type>(<scope>): <short summary>
<BLANK LINE>
[body]
<BLANK LINE>
[footer]
```

- type：更改类型（build|ci|docs|feat|fix|perf|refactor|test）
	- **feat**: A new feature
	- **fix**: A bug fix
	- **refactor**: A code change that neither fixes a bug nor adds a feature
	- **test**: Adding missing tests or correcting existing tests
	- **docs**: Documentation only changes
	- perf：改进性能的代码变更
	- build：影响构建系统或外部依赖关系的变更
	- ci：更改我们的 CI 配置文件和脚本
    - 重大更改可以写 BREAKING CHANGE 或 DEPRECATED（全大写）
- scope：影响范围（可选，比如具体影响的模块等）
- summary：更改的简要描述，英文一般现在时，首字母小写句末无句号
- body：详细描述，可选
- footer：解决 issue 了可以写 `Fixes #<issue number>` 或 `Closes #<pr number>`

## 分支

- 创建分支
    - `git branch <branch name>`：基于当前 HEAD
    - `git branch <branch name> <id>`：基于 `id` 提交
- 查看分支
    - `git branch`（带 -a 显示远程分支）
    - `git show-branch` 更详细
- 切换分支
    - `git checkout/switch <branch name>`
    - `git checkout -b <branch name>`：创建并切换（`git switch` 没有 `-b` 的用法）
- 内容比较
    - `git diff <branch1> <branch2>`：比较两个分支
    - `git diff <branch>`：比较工作区和分支
    - `git diff`：比较工作区和暂存区

### 其他切换分支的方法：

`git checkout <id>`：切换到某个特定的提交 [[What I Learned/2024-M1/Git/Git 基础#detached HEAD 问题\|detached HEAD 问题]]

#### 相对引用

![](https://pic.aspi-rin.top/2024/03/05086819076749c37ef15fbe098523be.jpeg)

- 使用 `^` 向上移动 1 个提交记录
- 使用 `~<num>` 向上移动多个提交记录，如 `~3`

> [!example]
> `main^` 相当于 `main` 的 parent 节点
> `main^^` 是 `main` 的第二个 parent 节点
> 可以使用 `HEAD^` 一直向上移动

## 合并

将多个分支的更改都**合并到当前分支**：`git merge <branch1> [branch2...]`

- 当前分支只比被合并分支多提交：already up-to-date
- 被合并分支只比当前分支多提交：fast-forward（将 HEAD 指向被合并分支）
- 都有新的提交：产生一个 merge commit（一个特殊的提交记录，它有两个 parent 节点）
    - 有冲突需要手动解决冲突（add 后再次 commit 生成 merge commit）

![](https://pic.aspi-rin.top/2024/03/f27b3a6e47125cc8e5c2ac9c020783b7.jpeg)

实际上 merge 操作一般都在 GitHub 上通过 PR 完成

两种**特殊的 merge 方法**：

- squash merge：将目的分支多出的所有提交压缩为一个新提交并入当前分支
	- dev 就不能 fast forward 了
- **rebase**：变基
    - 命令行直接 rebase 会**将当前分支接到目标分支后**（原有的分支依然存在）
	    - 优势就是可以创造更线性的提交历史
        - 这种情况会导致提交历史更改，同步会有冲突，合作时不推荐
    - 通过 GitHub PR rebase merge 会将目标分支接到当前分支后

![](https://pic.aspi-rin.top/2024/03/92a76e4ad9cdc8a58f65a21c2242fcda.jpeg)

## .gitignore

[常用的 gitignore 模板](https://github.com/github/gitignore)

### 语法

[[What I Learned/2024-M1/Git/glob 模式\|glob 模式]]

- # 开头的行：注释
- * 通配多个字符，** 通配中间目录（有或无）
    - \*.c 匹配所有 C 文件，a/\*\*/b 匹配 a/b、a/x/b、a/x/y/b 等
- / 开头只匹配根目录，否则匹配所有目录
- ! 取消忽略
- `git check-ignore -v [file]`：查看某个文件是否被忽略，以及匹配的规则

```.gitignore
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

## 「版本」控制

### 使用 tag

- 创建标签：
    - 轻量标签 lightweight：`git tag <tag> [id]`（id 可选，默认为 HEAD）
    - 附注标签 annotated：`git tag -a <tag> -m "message" <id>`
- 查看标签：`git tag`
- 默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上：`git push origin v1.5` 或者推送所有不在远程的标签 `git push origin --tags`
- 如果你想查看某个标签所指向的文件版本，可以使用 `git checkout` 命令， 虽然这会使你的仓库处于“分离头指针（detached HEAD）”的状态

### 版本命名规范

[语义化版本 2.0.0 | Semantic Versioning](https://semver.org/lang/zh-CN)

- `v<主版本号>.<次版本号>.<修订号>[-<预发布版本号>]`
- 修订号：兼容修改，修正不正确的行为
- 次版本号：添加新功能，但是保持兼容
- 主版本号：不兼容的 API 修改
    - 为 0 时表示还在开发阶段，不保证稳定性
- 预发布版本号：alpha/beta/rc.1/rc.2/...
- e.g. v1.0.0-beta < v1.0.0-rc.1 < v1.0.0 < v1.0.1

---

## detached HEAD 问题

- 什么是 HEAD：当前工作区在提交历史中的**指针**，HEAD 通常情况下是指向分支名的
- 什么是 detached HEAD：HEAD 指向某个历史提交，而不是某个“分支”

![](https://pic.aspi-rin.top/2024/03/686b3f1b40c63f13e3ad6639dbae6f6e.jpeg)

- 什么情形会出现 detached HEAD
    - `git checkout <SHA1 hash>`，此后的修改不会出现在任何分支
    - 切换回 master 后会出现一条不属于任何分支的提交（相当于修改会丢失）

![](https://s2.loli.net/2024/01/13/tqhQ1pHbFfdcOo5.jpg)

> 因此当指定某个 tag（特定提交）为 submodule 时，如果修改了 submodule，就会产生 detached HEAD

- 如何解决：在 F 的位置上 `git checkout -b <branch_name>` 创建并检出新分支

---

## 进阶 - 修改历史/撤销操作

- git revert _id_（用新提交抹掉以前提交的**效果**）
- `git commit --amend`（~~修复旧提交~~，**用一个新的提交替换上一个提交**）
- git reset _id_（回到某一提交的状态）
- git rebase -i _id_（交互式 rebase 变基，修改提交历史）

![](https://pic.aspi-rin.top/2024/03/ea23e0b3c51efb4011487384bfa74b5e.jpeg)

---

## Git 自学资料

- [lec2 - 2023秋冬实用技能拾遗](https://slides.tonycrane.cc/PracticalSkillsTutorial/2023-fall-ckc/lec2)
- [Pro Git (2nd Edition) 中文版, Scott Chacon, et al.](https://git-scm.com/book/zh/v2)
- [Git Reference](https://git-scm.com/docs)
- [Git飞行规则(Flight Rules)](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-CN.md)
- 动手学习
    - [Learning Git Branching](https://learngitbranching.js.org/?locale=zh_CN)
    - [Gazler/githug](https://github.com/Gazler/githug)
