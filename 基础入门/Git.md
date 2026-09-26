# Git 学习笔记

![[仓库构成.png]]

## 四大区域

- **Workspace 工作区**：你正在编辑的项目文件夹，所有改动最先发生在这里
- **Index / Stage 暂存区**：`git add` 之后，更改暂存在这里，等待被提交
- **Repository 本地仓库**：`git commit` 之后，版本记录在这里（就是项目里隐藏的 `.git` 文件夹）
- **Remote 远程仓库**：GitHub、Gitee 等服务器上的仓库，用来备份和多人协作

数据流向：工作区 → add → 暂存区 → commit → 本地仓库 → push → 远程仓库（反方向用 pull / fetch / clone）

## 需要用到的指令

- [ ] `git init`    在当前文件夹初始化一个全新的本地仓库（从零开始建项目的第一步）
- [ ] `git clone URL`    将远程仓库完整复制到本地（含全部提交历史），并自动关联远程
- [ ] `git add .`    将工作区的更改暂存到暂存区（`.` 表示全部更改，也可以 `git add 文件名` 只加一个）
- [ ] `git commit -m "备注"`    将暂存区的内容提交到本地仓库并写备注（必须带 `-m`，否则会进入编辑器）
- [ ] `git push`    将本地仓库的内容推送到关联的远程仓库（新分支首次推送用 `git push -u origin 分支名`）
- [ ] `git pull`    从**远程仓库**拉取最新更改并合并到当前分支（相当于 fetch + merge）
- [ ] `git checkout 分支名`    切换到指定分支（`git switch -c name` 新建并切换分支）

## 常用辅助指令

- [ ] `git status`    查看当前状态：哪些文件改了、哪些已暂存、哪些还没被跟踪
- [ ] `git log --oneline`    查看提交历史，一行显示一条
- [ ] `git diff`    查看工作区中还没 add 的具体改动内容
- [ ] `git branch`    查看所有分支；`git branch name` 新建；`git branch -d name` 删除
- [ ] `git merge name`    把 name 分支的更改合并到当前分支
- [ ] `git remote add origin URL`    把本地仓库关联到远程仓库（clone 下来的项目已自动关联，不用再执行）
- [ ] `git restore 文件名`    撤销某文件在工作区中尚未暂存的修改
- [ ] `git reset --soft HEAD~1`    撤销上一次 commit，改动退回暂存区（`--hard` 会连改动一起删，慎用）

## 新建仓库常用流程(需要先在Github等平台上先新建一个远程仓库)

- echo "# Git-Learning" >> README.md
- git init
- git add .
- git commit -m "Initial commit"
- git branch -M main
- git remote add origin `URL`
- git push -u origin main

## 小知识点

- **首次使用先配置**：`git config --global user.name "名字"` 和 `git config --global user.email "邮箱"`，否则无法 commit
- **HEAD**：指向"你当前所在的分支 / 提交"的指针，switch / checkout 本质上就是在移动 HEAD
- **fetch 与 pull 的区别**：fetch 只下载远程更新、不合并，更安全；pull = fetch + merge，更省事
- **.gitignore**：把不想被管理的文件（临时文件、密码配置、编译产物）路径写进 `.gitignore` 文件，git 会自动忽略它们

## 一次典型的日常流程

`git pull` → 写代码 → `git add .` → `git commit -m "备注"` → `git push`

## 本地代码修改与远程仓库冲突如何解决?

- 1. git pull origin main 终端会提示 `CONFLICT (content): Merge conflict in <文件名>`,同时工作区文件会被标记为未合并状态
- 2. 手动解决冲突 报错文件会出现如下冲突标记
  <!-- !-- 
  <<<<<<< HEAD
  本地的修改内容
  =======
  远程拉取下来的修改内容
  >>>>>>> origin/main 
  -->
  - 删除所有 <<<<<<<、=======、>>>>>>> 标记行
  - 保留正确的代码
  - 保存文件

- 3. 标记冲突已解决并提交
    git add .
    git commit -m "Resolve merge conflict"

- 4. 推送到远程 git push origin main

## 不同分支之间代码如何合并

### 方式一：Merge（合并，保留完整历史）

- 1. 切换到目标分支 git checkout main
- 2. 将源分支合并进来 git merge feature-login
- 3. 如果有冲突，按上述流程解决后提交
- 4. 推送 git push origin main

### 方式二：Rebase（变基，保持线性历史）

- 1. 在源分支上执行
  - git checkout feature-login
  - git rebase main

- 2. 完成后切回目标分支快进合并
  - git checkout main
  - git merge feature-login
  - git push origin main