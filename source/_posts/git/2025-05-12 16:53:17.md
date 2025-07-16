---
title: git 常用命令
date: 2025-5-27 10:53:38
tags:
  - "git"
  - "python"
categories:
  - "git"
---

# git 常用命令

> 从命令行创建一个新的仓库

```bash
echo "# song.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:sixonepawnacct/song.github.io.git
git push -u origin main
```

## 一、初始化仓库

```bash
git init
```

## 📥 二、克隆远程仓库

```bash
git clone <仓库地址>
# 示例：
git clone https://github.com/user/project.git
```

> 作用：下载一个远程仓库到本地。

## 📂 三、查看当前状态

```bash
git status
```

> 作用：显示工作目录和暂存区的状态。

## 📁 四、添加到暂存区

```bash
git add <文件名>     # 添加单个文件
git add .            # 添加所有修改的文件
```

> 作用：将文件添加到暂存区，准备提交。

## 💾 五、提交代码

```bash
git commit -m "提交信息"
```

> 作用：将暂存区的文件提交到本地仓库。

## 📝 六、查看提交历史

```bash
git log
git log --oneline      # 简洁模式
```

> 作用：显示提交历史记录。

## 🔁 七、修改最后一次提交说明

```bash
git commit --amend -m "新的提交信息"
```

> 作用：修改最后一次提交的说明。

## 🔄 八、推送到远程仓库

```bash
git push <远程仓库名> <分支名>
# 示例：
git push origin main
```

> 作用：将本地分支推送到远程仓库。

⬇️ 九、拉取远程仓库最新内容

```bash
git pull <远程仓库名> <分支名>
# 示例：
git pull origin main
```

> 作用：从远程仓库拉取最新内容到本地。

## 🌿 十、分支管理

```bash
git branch        # 查看所有分支
git branch <分支名>   # 创建新分支
git checkout <分支名> # 切换到指定分支
git merge <分支名>   # 合并指定分支到当前分支
git branch -d <分支名> # 删除分支
```

> 作用：管理分支，包括创建、切换、合并和删除。

## 🔀 十一、合并分支

```bash
git merge <分支名>
```

## ❌ 十二、撤销与回退

```bash
# 撤销未暂存的修改
git checkout -- <文件名>
# 撤销已 add 的文件
git reset HEAD <文件名>
# 回退到上一个版本
git reset --hard HEAD^
```

## 🔐 十三、配置用户信息（首次使用需要）

````bash
git config --global user.name "Your Name"
```bash
git config --global user.name "Your Name"
git config --global user.email "EMAILgit config --global user.email "your_email@example.com"
````

> 作用：配置本地 Git 环境的用户名和邮箱。

### 临时推送，如果这个是误报或者你清楚不会泄露信息，你也可以在 GitHub 上手动放行本次提交：

https://github.com/sixonepawnacct/songyi.github.io/security/secret-scanning/unblock-secret/2xexyuRKp70BsD85VvNzQNaZLH6
