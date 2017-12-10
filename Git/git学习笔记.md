# Git的学习#

【**2017-11-14**】

**资料来源： <https://git-scm.com/book/zh/v2>**

## 一、起步##

###1.git有三种状态###

| 已提交 committed | 数据已经安全的保存在本地数据库中                |
| ------------- | ------------------------------- |
| 已暂存 staged    | 对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中 |
| 已修改 modified  | 修改了文件，但还没保存到数据库中                |

#### 2.Git 项目的三个工作区域的概念####

Git 仓库、暂存区域、工作目录。

![WX20171114-131641](/Users/fairy23/Project/TrainingPlanStudy/Git/PictureInformation/WX20171114-131641.png )

####3.基本的 Git 工作流程如下：####

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

####4.git 初始配置####

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 `--system` 选项的 `git config` 时，它会从此文件读写配置变量。
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 可以传递 `--global` 选项让 Git 读写此文件。
3. 当前使用仓库的 Git 目录中的 `config` 文件（就是 `.git/config`）：针对该仓库。

每一个级别覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。

#### 5.用户信息、文本编辑器、检查配置信息 ####

```$ git config --global user.name "John Doe"$ git config --global user.email johndoe@example.com
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com

$ git config --global core.editor emacs //配置默认文本编辑器,默认是Vim。

$ git config --list //检查配置
```

#### 6.打开使用手册####

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>  //q为退出键

$ git help config //config命令的手册
```

或者在Freenode IRC 服务器（ irc.freenode.net ）的 `#git` 或 `#github` 频道寻求帮助。

## 二、Git基础##

####1.获取仓库的两种方法####

a.在现有项目或目录下导入所有文件到 Git 中; 

```
//对于现有空项目
//该命令将创建一个名为 .git 的子目录，此时项目里的文件还没有被跟踪。
$ git init 

//对于已经存在文件的文件夹还需使用以下命令
$ git add *.c //跟踪
$ git add LICENSE
$ git commit -m 'initial project version' //提交
```

现在，你已经得到了一个实际维护（或者说是跟踪）着若干个文件的 Git 仓库。

b.从一个服务器克隆一个现有的 Git 仓库。

```
$ git clone [url]

//例如
//本地创建的仓库名字变为 libgit2
$ git clone https://github.com/libgit2/libgit2 
//本地创建的仓库名字变为 mylibgit
$ git clone https://github.com/libgit2/libgit2 mylibgit 
```

####2.git文件的生命周期####

![WX20171114-162804](/Users/fairy23/Project/TrainingPlanStudy/Git/PictureInformation/WX20171114-162804.png)

####3.检查当前文件状态####

```
$ git status

//文件的状态
nothing to commit, working directory clean//文件状态为unmodified

Untracked files //未被跟踪的文件
$ git add <fileName or path> //add改变文件状态untracked->staged
Changes to be committed //文件在暂存区，文件已被跟踪

Changes not staged for commit //文件在非暂存区，已跟踪的文件被修改
$ git add [fileName or path] 
Changes to be committed //文件在暂存区

注意：运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来
```

或者用紧凑的格式输出 当前文件状态

```
$ git status -s

//文件的状态
 M README 修改过的文件-该文件被修改了但是还没放入暂存区
M  lib/simplegit.rb 修改过的文件-该文件被修改了并放入了暂存区
MM Rakefile 修改过的文件-工作区被修改、提交到暂存区、在工作区中又被修改
A  lib/git.rb 新添加到暂存区中的文件
?? LICENSE.txt 新添加的未跟踪文件

$ git status --short
```

####4.忽略文件####

创建一个名为 `.gitignore` 的文件未跟踪文件列表，来忽略无需纳入 Git 管理的文件，如日志文件，或者编译过程中创建的临时文件等。

文件 `.gitignore` 的格式规范如下：

- 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

 glob 模式匹配即：shell 所使用的简化了的正则表达式

- `*`匹配零个或多个任意字符；
- `[abc]`匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；
- `?`只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）；
- `**`匹配任意中间目录，比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。

示例：

```
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

####5.查看已暂存和未暂存的修改####

`git diff`显示尚未暂存的具体改动，而不是自上次提交以来所做的所有改动`git diff --cached` OR `git diff --staged`显示已暂存的将要添加到下次提交里的内容

#### 6.提交更新####

```
//有以下几种提交命令
$ git commit
$ git commit -m "提交附带信息"
$ git commit -a //跳过暂存区，自动把所有已跟踪过的文件暂存起来一并提交
$ git commit -a -m '提交附带信息'
```

####7.移除文件####

从工作目录、已跟踪文件清单=暂存区移除，然后提交

```
$ git rm 文件名 //删除只在工作目录，不在暂存区的文件
$ git rm -f 文件名 //强制删除force，删除暂存区的文件
$ git rm --cached 文件名 //从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中，常用于忘记添加.gitignore 文件
```

#### 8.移动文件####

```
$ git mv README.md README //对文件改名，将README.md改为README

//git mv 相当于运行了下面三条命令
$ mv README.md README
$ git rm README.md
$ git add README
```

#### 9.查看提交历史####

```
$ git log
$ git log -p -2  //-p每次提交的内容差异  -2显示最近两次提交
$ git log --stat //每次提交的简略的统计信息
$ git log --pretty=oneline //指定使用不同于默认格式的方式展示提交历史
//其中online可以替换为short, full, fuller, format
$ git log --pretty=format:"%h - %an, %ar : %s" //format，可以定制要显示的记录格式
```

####10.限制git log输出长度####

```
$ git log --since=2.weeks // 列出所有最近两周内的提交
```

####11.撤销操作####

重新提交命令，编辑后保存会覆盖原来的提交信息，最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

```
$ git commit --amend
```

git add 之后想取消某一个暂存文件。

```
$ git reset HEAD <file>
```

撤销修改，即文件还未暂存，但是已修改，撤销修改后文件会返回至上次提交时的样子。

```
$ git checkout -- <file>
```

注意，在 Git 中任何 *已提交的* 东西几乎总是可以恢复的。然而，任何未提交的东西丢失后很可能再也找不到了。

####12.远程仓库的使用####

查看远程仓库

```
$ git remote
$ git remote -v //会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL
```

添加远程仓库

```
$ git remote add <shortname> <url>
$ git remote add pb https://github.com/paulboone/ticgit //例子，之后可以在命令行中使用字符串 pb 来代替整个 URL
```

从远程仓库中获得数据

 `git fetch` 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作，需要手动合并。或者使用`git pull` 命令来自动的抓取然后合并远程分支到当前分支。

```
$ git fetch [remote-name]
```

推送到远程仓库

要满足以下条件才能推送成功：1.有所克隆服务器的写入权限；2.之前没有人推送过。否则必须将其他同时推送的人的工作拉取下来并将其合并进你的工作后才能推送。【remote-name 则为天价远程仓库时定义的shortname，第一次创建时必须创建shortname和对应url后，才可顺利使用push命令】

```
$ git push [remote-name] [branch-name]
$ git push origin master //例子
```

查看远程仓库

这个命令列出了当你在特定的分支上执行 `git push` 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 `git pull` 时哪些分支会自动合并。

```
$ git remote show [remote-name]
```

远程仓库的重命名

```
$ git remote rename [原始名] [重命名]
```

远程仓库的移除

```
$ git remote rm [name]
```

####13.打标签####

Git 可以给历史中的某一个提交打上标签，以示重要。 比较有代表性的是人们会使用这个功能来标记发布结点（v1.0 等等）

列出标签

```
$ git tag
$ git tag -l 'v1.8.5*' //例子，只对 1.8.5 系列标签感兴趣
```

创建标签

a.轻量标签(lightweight),临时的标签;

```
$ git tag [v1.4-lw] //创建轻量标签，只需要提供标签名字,不需要使用 -a、-s 或 -m 选项。

$ git show [v1.4-lw] //在标签上运行 git show，你不会看到额外的标签信息，命令只会显示出提交信息。
```

 b.附注标签(annotated),存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。 通常建议创建附注标签，这样你可以拥有以上所有信息

```
$ git tag -a [v1.4] -m ['my version 1.4'] //在运行 tag 命令时指定 -a 选项

$ git show [v1.4] //通过使用 git show 命令可以看到标签信息与对应的提交信息
```

后期打标签

```
$ git log --pretty=oneline
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile

$ git tag -a v1.2 9fceb02 //在那个提交上打标签，你需要在命令的末尾指定提交的校验和
```

共享标签

默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。之后，其他人从仓库中克隆或拉取，他们也能得到你的那些标签。

```
$ git push origin [v1.5]
$ git push origin --tags //一次性推送很多标签
```

标出标签

？？？不太理解意思

```
$ git checkout -b [branchname] [tagname]
$ git checkout -b [version2] [v2.0.0] //例子
```

####14.git别名####

 Git 并不会在你输入部分命令时自动推断出你想要的命令。 如果不想每次都输入完整的 Git 命令，可以通过 `git config` 文件来轻松地为每一个命令设置一个别名。

```
// 一些例子
$ git config --global alias.ci commit //当要输入 git commit 时，只需要输入 git ci
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.st status
$ git config --global alias.unstage 'reset HEAD --'
$ git config --global alias.last 'log -1 HEAD'h
```

##N.自我实践##

创建新的仓库并将文件推送至新仓库，<括号内的内容为用户自定义>

```
//全局的初始
$ git config --global user.name "<John Doe>"
$ git config --global user.email <johndoe@example.com>
$ git config --list 

//本地文件夹的初始，创建git并提交修改文件
$ git init 
$ git add <filename>
$ git commit -m '<Task 1>'
$ git status

//创建新仓库，并将本地git版本推送至新仓库
//前提是在远程仓库中已新创建TrainingPlanStudy此仓库
$ git remote add <tps> 
<https://github.com/MyNameIsAngela/TrainingPlanStudy>
$ git remote show <tps>
$ git push <tps> <master>

//补充说明：即使这是从本地推送至远端服务器，但本地图形界面并不会显示此仓库，若想本地图形界面显示，则需从远端服务器克隆。
```