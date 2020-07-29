# 常用的git操作

## 撤销操作

### 修改commit信息

有时候我们提交完了才发现**漏掉了几个文件没有添加，或者提交信息写错了**。 此时，可以运行带有 --amend 选 项的提交命令尝试重新提交:

```shell
git commit --amend
```

这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此 命令），那么快照会保持不变，而你所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。 例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作:

```shell
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```

### 撤销暂存的文件

`git add .`之后，`git status`会提示：

```shell
git add *
git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
```

撤销某一文件/文件夹：

```shell
git reset HEAD <filename>
```

撤销所有`add`的文件：

```shell
git reset HEAD .
```

如果在调用时加上 `--hard` 选项可以令 `git reset` 成为一个危险的命令，可能导致工作目录中所有当前进 度丢失。

### 撤消对文件的修改

```shell
git checkout -- <filename>
```

这是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。

## 参考

- [Pro Git v2](https://bingohuang.gitbooks.io/progit2/content/)
- [如何在 Git 里撤销（几乎）任何操作](https://linux.cn/article-5713-1.html)

