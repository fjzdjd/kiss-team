# Git

## 基本 (Basic)

```bash
git config [--global|--system] <git-config>

# 创建
git clone <repo-url> [<repo-name>]
git init

# 分支 (Branch)
git branch [-d|-D] <branch>
git checkout <branch>
git merge <branch>
git rebase <branch-or-commit>

# 提交 (Commit)
git diff [--cached]
git add|rm|mv <file ... or dir>
git status
git commit -m <commit-message>
git log
git pull [--rebase]
git push
git push [-f] <remote-repo> :<remote-branch>

# Git补丁 (Git Patch)
git format-patch -M <upstream-branch> <current-branch>.patch
git am <current-branch>.patch

# 标准补丁 (Standard Patch)
git diff <upstream-branch> > <current-branch>.patch
git apply --check <current-branch>.patch
git apply <current-branch>.patch

# 标签 (Tag)
git tag -a <tag-name>
git push origin :refs/tags/<tag-name>

# 子模块 (submodule)
git submodule update --init
git submodule add <submodule-repo-url> <submodule-repo-name>
```

## 提交注解 (Commit Message)

```
This update contains improvement and bug fixes, including:

    - Fixed bugs where user cannot signup

    - Fixed bugs that prevent A from doing sth

    - Fixed issues that could cause B crash when ...

    - Improved reliability of C when ...

    - Improved performance of D when ..
```

## 工作流 (Work Flow)

### Mainstream

Branch `master` contains codes only for official releases with some version
tags.

**NOTE**: The perils of rebasing: **DONOT** `rebase` commits that you have pushed to a public repository.

```bash
git checkout master
git pull --rebase
git merge --no-ff dev

# If any upstream repo (e.g., GitHub Fork)
git remote add upstream <git-url>
git fetch upstream
git merge --no-ff upstream/master

git tag -a <tag> -m '<tag-comment>' # e.g., git tag -a 'v0.1' -m 'v0.1 - Initial version'

git push
git push --tag
```

### Bugfix for Mainstream

```bash
git checkout -b <bugfix-A> master
# ... (git add/rm)
git diff [--cached] <path ...>
git diff --check # Avoid white space issues commit
# ... (git commit -m)
git diff <bugfix-A>..master

git checkout master
git pull --rebase
git rebase <bugfix-A>
git branch -d <bugfix-A>
```

### Features & Topics

I think that it is better for same features or topics by using `rebase`, while for different ones by using `merge`. Since it will keep the branch structure clean.

```bash
git checkout <feature-A> dev
git pull --rebase
# ... (git add/rm)
git diff [--cached] <path ...>
git diff --check # Avoid white space issues commit
# ... (git commit -m)
git diff <feature-A>..dev

git checkout dev
git pull --rebase
git merge --no-ff <feature-A>
```

### Bugfix for Features & Topics

```bash
git checkout <feature-A>
git pull --rebase

git checkout -b <feature-A-bugfix>
# ... (git add/rm)
git diff [--cached] <path ...>
git diff --check # Avoid white space issues commit
# ... (git commit -m)
git diff <feature-A>..<feature-A-bugfix>

git checkout <feature-A>
git pull --rebase
git rebase <feature-A-bugfix>
git branch -d <feature-A-bugfix>
```
 
## 推荐读物 (Recommended Reading)

- [Pro Git](http://www.git-scm.com/book/)
- [GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html)
- [GitHub Flow Guide](https://guides.github.com/introduction/flow/index.html)
