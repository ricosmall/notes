# git pull --rebase的正确使用

在多人使用同一个远程分支合作开发的时候，很可能出现 push 代码的时候出现以下问题：

```sh
$ git push origin master

# 结果如下
To github.com:hello/demo.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:hello/demo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

很明显此时远程分支有新的 commit 未同步到本地，无法推送。正常情况下我们会执行以下操作：

```sh
$ git pull origin master

# 结果如下
remote: Counting objects: 2, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (2/2), done.
From github.com:hello/demo
   3824da0..f318a05  master     -> origin/master
Merge made by the 'recursive' strategy.
 one.md | 2 ++
 1 file changed, 2 insertions(+)
 
$ git push origin master

# 结果如下
Enumerating objects: 10, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 1.24 KiB | 1.24 MiB/s, done.
Total 8 (delta 5), reused 0 (delta 0)
To github.com:hello/demo.git
   f318a05..1aefef1  master -> master
```

确实 push 成功了，但是此时用 `git log` 查看以下提交记录：

```sh
$ git log

# 结果如下
commit 1aefef1a2bedbd3ebd82db8dcf802011a35a9888 (HEAD -> master, origin/master, origin/HEAD)
Merge: 24cfa5c f318a05
Author: hello <hello@qq.com>
Date:   Tue Jul 23 09:53:47 2019 +0800

    Merge branch 'master' of github.com:hello/demo

commit 24cfa5c3ad271e85ff0e64793bf2bcc9d700c233
Author: hello <hello@qq.com>
Date:   Tue Jul 23 09:50:06 2019 +0800

    feat: 新功能提交

commit f318a05b1a4cbc0a6cf8d7dc7d3fb99cbafb0363
Author: world <world@qq.com>
Date:   Tue Jul 23 09:48:20 2019 +0800

    feat: 其他功能提交

...
```

你会发现多出了一条 merge commit，这个 commit 就是在执行 `git pull origin master` 的时候自动生成的。如果多人多次如此操作，那么提交记录就会出现很多条这种自从生成的 merge commit，非常难看。

要解决以上问题，不再出现自动生成的 merge commit，那么只要在执行 `git pull origin master` 的时候带上 `--rebase` 即可：

```sh
$ git pull --rebase origin master

$ git push origin master
```

## 项目示例

现在通过一个示例项目来示范以上命令的用法。项目（demo）的结构如下：

```sh
# 在 demo 目录下执行以下命令

$ ls

# 结果如下
one.md  two.md

$ cat one.md

# 结果如下
hello one

$ cat two.md

#结果如下
hello two
```

A、B两位开发同学使用一个远程分支（master）合作开发。A同学修改了 `one.md` 文件，提交了一个 commit 并且已经推送到远程分支 master：

```sh
$ cat one.md

# 结果如下
hello one

new feature for one

$ git push origin master

# 结果如下
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.24 KiB | 1.24 MiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
To github.com:hello/demo.git
   f318a05..1aefef1  master -> master
```

此时B同学不知情，也并未将远程分支的更新拉到本地。B同学修改了 `two.md` 文件，也提交了一个 commit 并尝试推送到远程分支 master：

```sh
$ cat two.md

# 结果如下
hello two

new feature for two

$ git push origin master

# 结果如下
To github.com:hello/demo.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:hello/demo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

发现报错，无法推送。此时执行以下命令：

```sh
$ git pull --rebase

$ git push origin master
```

即可成功推送。

## 注意事项

- **执行 `git pull --rebase` 的时候必须保持本地目录干净**。即：不能存在状态为 `modified` 的文件。（存在`Untracked files`是没关系的）
- **如果出现冲突，可以选择手动解决冲突后继续 `rebase`，也可以放弃本次 `rebase`**

### 详细说明

1.执行 `git pull --rebase` 的时候必须保持本地目录干净

本地有受版本控制的文件改动的时候，执行以上命令：

```sh
$ git pull --rebase

#结果如下
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.
```

会出现以上报错，无法操作。
