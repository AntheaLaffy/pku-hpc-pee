---
tags:
  - 教程
  - git
---
> SIPOC |codex|----

## 核心价值与思想

git的最大价值在于高效协同工作

git的核心思想是分布式管理系统，数据结构的模式是个单向只读图，多对一。

推荐学习资料:（如果你不是只想用，也想了解背后的原理的话）

  课程:https://missing-semester-cn.github.io/
  作业:https://missing-semester-cn.github.io/
  书籍:《Pro Git》
  助教: Chatgpt大人
  参考手册:https://github.com/AntheaLaffy/thu-hpc-pee/blob/main/git%E6%95%99%E7%A8%8B.md
  Agent Skills:https://github.com/AntheaLaffy/the-missing-semester-skills
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

## 忽略本地配置

`.gitignore` 是仓库根目录中的规则文件，不是 Git 命令。它用于避免把只属于个人设备的配置、缓存或临时文件提交到公共仓库。

Obsidian 的 `.obsidian/` 同时包含个人界面配置和知识库的共享配置，不能直接忽略整个目录。本仓库只忽略主题、字体、窗口布局和标签页状态；插件开关、关系图等可能影响文章组织和共同使用方式的配置仍由 Git 跟踪。

在仓库根目录的 `.gitignore` 中加入：

```gitignore
/.obsidian/appearance.json
/.obsidian/workspace*.json
```

开头的 `/` 表示规则只作用于仓库根目录下的 `.obsidian/`。`workspace*.json` 同时覆盖桌面端和移动端可能生成的工作区状态文件。

Markdown 文件中的 `tags` 等属性直接保存在 `.md` 文件里，不受以上忽略规则影响：

```yaml
---
tags:
  - 教程
  - git
---
```

如果这些个性化配置从未提交过，添加忽略规则后正常提交 `.gitignore` 即可：

```bash
git add .gitignore
git commit
git push
```

如果个性化配置已经被 Git 跟踪，仅修改 `.gitignore` 不会生效。需要逐项将它们从 Git 的索引中移除，不要解除整个 `.obsidian/` 的跟踪：

```bash
git rm --cached .obsidian/appearance.json
git rm --cached .obsidian/workspace.json
git add .gitignore
git commit
git push
```

`--cached` 只解除 Git 对这些文件的跟踪，不会删除执行命令者的本机文件。已经克隆过旧版本的协作者应在首次拉取这个变更前备份自己的个性化配置；从该变更开始，Git 不再记录这些文件，后续本地主题和工作区布局不会进入提交。

检查某个文件是否已经被忽略：

```bash
git check-ignore -v .obsidian/workspace.json
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
git remote add upstream https://github.com/AntheaLaffy/thu-hpc-pee.git
```

---

如月风铃作为仓库的拥有者只需要一个远程
```text
origin -> AntheaLaffy/thu-hpc-pee
```

长劫的 fork 需要两个远程
```text
origin   -> Kisjerry/pku-hpc-pee
upstream -> AntheaLaffy/thu-hpc-pee
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
  base repository: AntheaLaffy/thu-hpc-pee
  base:            main
  head repository: AntheaLaffy/thu-hpc-pee
  compare:         fuurin
  ```

  长劫创建 PR 时选择：
```text
  base repository: AntheaLaffy/thu-hpc-pee
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

#### 实例：一边修改文件，另一边停止跟踪文件

本仓库的 PR #2 曾出现过一次 `modify/delete` 冲突：公共分支 `main` 修改了
`.obsidian/workspace.json`，而 `fuurin` 分支为了避免同步个人界面配置，删除了 Git
索引中的同一文件。Git 无法替作者判断应该保留公共分支的修改，还是采用源分支的删除，
因此 GitHub 会继续提示冲突。

  仅把文件写入 `.gitignore` 不能解决这类冲突。`.gitignore` 只阻止未被跟踪的文件在以后
重新加入 Git，不会替已被跟踪的文件选择冲突结果。

要解决 PR 的冲突，应在 PR 的源分支 `fuurin` 上合入最新的远端公共分支：

```bash
git switch fuurin
git fetch origin
git merge origin/main
```

这里使用 `origin/main`，因为它表示最近一次 `git fetch` 获取到的远端 `main` 状态；
本地 `main` 可能仍然落后。发生冲突后，先用 `git status` 确认冲突文件。如果最终意图是
停止跟踪个人工作区配置，应选择删除结果：

```bash
git rm .obsidian/workspace.json
git commit
git push
```

`git push` 会更新 `fuurin` 及其关联的 PR，不需要关闭或重新创建 PR。

更新本地 `main` 是另一件事，并不是解除这个 PR 冲突的前提。需要同步本地 `main` 时，
可以另外执行：

```bash
git switch main
git pull --ff-only origin main
```

`--ff-only` 只允许本地 `main` 直接快进到远端位置；如果本地存在远端没有的提交，Git 会
拒绝自动合并，从而避免在公共分支上意外产生合并提交。

如果想放弃本次合并：

```bash
git merge --abort
```
