---
tags:
  - 教程
  - git
---
> SIPOC |codex|----

## 我们的协作结构是

如月风铃:  fuurin 分支 ──PR──> AntheaLaffy/main
长劫:  Kisjerry fork 分支 ──PR──> AntheaLaffy/main

---

main分支作为公共分支，任何改动都需要提交pr，由另一人审核

> 注：下方只提供所有git命令最原始的状态，参数请自行探索

## 常用命令

- 查看当前状态
```bash
git status
```

- 查看尚未暂存的修改
```bash
git diff
```

- 将修改后的文件加入暂存区
```bash
git add
```

- 保存提交快照
```bash
git commit
```

- 推送分支
```bash
git push
```

- 查看提交历史
```bash
git log
```

## 本地分支切换

- 查看所有本地分支
```bash
git branch
```

- 切换到已有的分支
```bash
git switch fuurin #这里以我的fuurin分支为例
```

- 将特定分支推送到github
```bash
git push # 第一次以后可以省略下面一条的写法，但记得切换到对应的分支
```

```bash
git push -u origin fuurin #首次推送这么写，自动在github上创建对应分支
```

## 远程协作

### 远程仓库配置

- 查看远程仓库
```bash
git remote
```

- 添加上游仓库配置
```bash
git remote add upstream https://github.com/AntheaLaffy/pku-hpc-pee.git
```

---

如月风铃作为仓库的拥有者只需要一个远程
```text
origin -> AntheaLaffy/pku-hpc-pee
```

长劫的 fork 需要两个远程
```text
origin   -> Kisjerry/pku-hpc-pee
upstream -> AntheaLaffy/pku-hpc-pee
```

 其中：

  - `origin` 表示自己可以直接推送的仓库。
  - `upstream` 表示 fork 来源，即公共仓库。
  - 长劫首次克隆 fork 后，需要添加 `upstream`。

---

### 获取远程更新

- 捕获远程仓库更新状态在.git📁，但是不更新当前工作区
```bash
git fetch
```

- 拉取与本地分支相对应的远程分支，并合并到本地
```bash
git pull
```

- 合并本地中其他分支到本分支
```bash
git merge 
```


**例子**

  如月风铃：将公共 `main` 分支更新到 `fuurin`:
```bash
git switch fuurin
git fetch
git merge origin/main
git push
```

  长劫：将公共 `main` 同步到自己 fork 的 `main`:
```bash
git fetch upstream
git merge upstream/main 
git push
```

### 创建 PR

  PR的目标分支统一为:
```text
AntheaLaffy/main
```

  如月风铃创建 PR 时选择：
  ```text
  base repository: AntheaLaffy/pku-hpc-pee
  base:            main
  head repository: AntheaLaffy/pku-hpc-pee
  compare:         fuurin
  ```

  长劫创建 PR 时选择：
```text
  base repository: AntheaLaffy/pku-hpc-pee
  base:            main
  head repository: Kisjerry/pku-hpc-pee
  compare:         main  
```

---

GitHub 网页操作流程：

1. 打开公共仓库的 `Pull requests` 页面。
2. 点击 `New pull request`。
3. 根据上面的结构选择 `base` 和 `compare`。
4. 检查 `Commits` 和 `Files changed`。
5. 填写标题和修改原因。
6. 点击 `Create pull request`。
7. 在右侧 `Reviewers` 中选择另一位协作者。

### 审核 PR

>原则: PR者不自审

- 如月风铃的 PR 由长劫审核。
- 长劫的 PR 由如月风铃审核。

 审核者打开 PR 的 `Files changed` 页面，检查修改后点击 `Review changes`，可以选择：

- `Comment`：仅提出意见，不阻止合并。
- `Request changes`：要求作者修改，阻止当前 PR 合并。
- `Approve`：认可当前修改，允许进入合并流程。

 需要修改时，应由 PR 作者继续修改原来的源分支，而不是重新创建 PR。

### 更新 PR

PR 与它的源分支保持关联。作者向同一个源分支推送新提交后，PR 会自动更新。

  如月风铃更新自己的 PR：

```bash
git switch fuurin
git add
git commit
git push
```

  长劫更新自己的 PR：

```bash
git add
git commit
git push
```

  更新完成后，需要在 PR 页面重新请求另一位协作者审核。

### PR 落后于 main

如果 PR 页面提示源分支落后于 `main`，应先把最新的 `main` 合入源分支。

  如月风铃：
```bash
git switch fuurin
git fetch origin
git merge origin/main
git push
```

  长劫：
```bash
git fetch upstream
git merge upstream/main
git push
```

  推送完成后，PR 会自动更新，不需要关闭或重新创建。

### 合并 PR

 满足以下条件后才能合并：

- PR 已由另一人 `Approve`。
- 没有尚未解决的修改请求。
- 分支没有合并冲突。
- 仓库配置的检查已经通过。

 确认无误后，在 PR 页面点击：

```text
Merge pull request
```

### 处理合并冲突

  同步 `main` 时如果发生冲突，先查看冲突文件：

```bash
git status
```

  手动编辑冲突文件并保留正确内容，然后：

```bash
git add
git commit
git push
```

  如果想放弃本次合并：

```bash
git merge --abort
  ```