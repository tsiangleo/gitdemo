# gitdemo

## git常见命令
可以使用如何命令查看指定命令的用法。
```
git help <command>
```
常见的command有：
- init
- clone
- add
- diff
- status
- commit
- log
- branch
- checkout
- merge
- tag
- remote
- fetch
- pull
- push


## 创建本地仓库

### git init
Git 使用 git init 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行。
```
git init [directory]
```
该命令在```[directory]```目录下创建一个Git仓库，并在```[directory]```目录下生成一个 ```.git``` 目录，```.git```目录包含了资源的所有元数据。如果不指定```[directory]```，则默认以当前目录作为Git仓库。

### git clone

将已有的仓库拷贝到目录中。
```
git clone <repo> <directory>
```
参数：
- repo：Git 仓库。
- directory：本地目录。


## 操作本地仓库

### git add
git add 命令可将该文件添加到缓存区。
```
$ touch README
$ touch hello.php
$ ls
README		hello.php
$ git status -s
?? README
?? hello.php
$ git add README hello.php 
$ git status -s
A  README
A  hello.php
$ 
```

### git status
git status 以查看在你上次提交之后是否有修改。加-s 参数，以获得简短的结果输出。如果没加该参数会详细输出内容。


### git diff 

该命令显示已写入缓存与已修改但尚未写入缓存的改动的区别


### git commit


## 同步本地仓库至远程仓库

我们在本地创建了一个Git仓库，现在想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。

我们在本地仓库已经创建了分支和标签，现在想将这些信息同步到远程仓库，该怎么操作呢？

### 添加远程库
要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用,命令格式如下：
```
git remote add <alias> <url>
```
参数说明：
- alias：远程仓库的名字，方便本地引用而已，可任意取名，一般为origin。
- url：远程仓库的地址，比如：git@github.com:tsiangleo/gitdemo.git

要查看当前配置有哪些远程仓库，可以用命令：
```
git remote
```
例子：
```
$ git remote
origin
$ git remote -v
origin  git@github.com:tsiangleo/gitdemo.git (fetch)
origin  git@github.com:tsiangleo/gitdemo.git (push)

```
执行时加上 -v 参数，你还可以看到每个别名的实际链接地址。

### 提取远程仓库

一旦远程主机的版本库有了更新(Git术语叫做commit)，需要将这些更新取回本地，这时就要用到git fetch命令。
```
git fetch <alias>
```
上面命令将某个远程仓库```<alias>```的更新，全部取回本地。
默认情况下，git fetch取回所有分支(branch)的更新。如果只想取回特定分支的更新，可以指定分支名
```
git fetch <alias> <branch>
```
然后可以执行如下命令本地分支上合并远程分支：
```
git merge <alias>/<branch>
```
该命令将远程仓库```<alias>```的```<branch>```分支上的任何更新合并到本地仓库的当前分支。


上述的```git fetch```加```git merge```的功能等同于```git pull```的功能。
git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。
```
$ git pull <远程仓库名> <远程分支名>:<本地分支名>
```
比如，取回远程origin仓库的next分支，与本地的master分支合并，需要写成下面这样。
```
$ git pull origin next:master
```
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
```
$ git pull origin next
```
上面命令表示，取回origin/next分支，再与当前分支合并。实质上，这等同于先做git fetch，再做git merge。
```
$ git fetch origin
$ git merge origin/next
```

### 推送至远程仓库

git push命令用于将本地分支的更新，推送到远程仓库。它的格式与git pull命令相仿。
```
$ git push <远程仓库名> <本地分支名>:<远程分支名>
```
注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。
如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)，如果该远程分支不存在，则会被新建。
```
$ git push origin master
```
上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
```
$ git push origin :master
```
等同于
```
$ git push origin --delete master
```
上面命令表示删除origin主机的master分支。

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
```
$ git push origin
```
上面命令表示，将当前分支推送到origin主机的对应分支。

如果当前分支只有一个追踪分支，那么仓库名都可以省略。
```
$ git push
```
如果当前分支与多个仓库存在追踪关系，则可以使用-u选项指定一个默认仓库，这样后面就可以不加任何参数使用git push。
```
$ git push -u origin master
```
上面命令将本地的master分支推送到origin仓库，同时指定origin为默认仓库，后面就可以不加任何参数使用git push了。

不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。
```
$ git config --global push.default matching
```
或者
```
$ git config --global push.default simple
```
还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用–all选项。
```
$ git push --all origin
```
上面命令表示，将所有本地分支都推送到origin主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用–force选项。
```
$ git push --force origin
```
上面命令使用–force选项，结果导致在远程主机产生一个”非直进式”的合并(non-fast-forward merge)。除非你很确定要这样做，否则应该尽量避免使用–force选项。

最后，git push不会推送标签(tag)，除非使用–tags选项。
```
$ git push origin --tags
```


## 参考链接

- [菜鸟教程-Git教程](http://www.runoob.com/git/git-tutorial.html)
- [易百教程-Git教程](http://www.yiibai.com/git/)
- [Git把Tag推送到远程仓库](http://blog.csdn.net/hustpzb/article/details/8056518)
- [廖雪峰-Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
