## 学习资料
[Git下载地址](https://git-for-windows.github.io) 针对windows系统

[Pro Git](https://git-scm.com/book/zh/v2) git教程 

## 学习思路

### 什么是Git

> Git 更像是把数据看作是对小型文件系统的一组快照。 每次你提交更新，或在 Git 中保存项目状态时，**它主要对当时的全部文件制作一个快照并保存这个快照的索引**。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 快照流。

> Git 中所有数据在存储前都计算校验和，然后以校验和来引用。 这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。

> Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。 这是一个由 40 个十六进制字符（0-9 和 a-f）组成字符串，基于 Git 中文件的内容或目录结构计算出来。

### 工作流程

基本的 Git 工作流程如下：

- 在工作目录中修改文件。
	Working directory，没有执行任何命令之前，修改过文件的状态为已修改（modified）；修改和新增的文件都是未跟踪，所以不能直接提交。

- 暂存文件，将文件的快照放入暂存区域。
	Staging area，执行 git add 具体文件名 命令之后，文件状态为已暂存（staged），暂存文件都是已跟踪。

- 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
	Repository，使用 git commit 命令，表示已经安全的保存在本地数据库中。

> 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。

#### 第一次使用

配置用户信息（用户名称和邮件地址），目的每个Git仓库提交都需要这些信息，配置一次之后，就不需要再次输入。

		$ git config --global user.name "example"
		$ git config --global user.email example@example.com

如果某个项目需要其他账户信息，其当前目录修改配置信息，就是去掉`--global`即可。

查看配置信息，命令如下：

		$ git config --list

#### 帮助，获取相关指令帮助信息

		$ git help <verb>
		$ git <verb> --help
		$ man git-<verb> 

#### 获取 Git 仓库

两种方式获取Git仓库

1. 现有目录下创建Git仓库。
	
		$ git init

2. 一个服务器克隆一个现有的Git仓库。

		$ git clone https://github.com/example

		//or

		$ git clone https://github.com/example otherName //修改当前仓库名为otherName

#### 文件状态

根据文件的变化（新增、修改、提交），即一系列的周期状态变化。所以查看当前文件处于什么状态：
		
		$ git status

		//or 

		$ git status -s // $ git status --short 输出简洁形式的状态

**注意的是本地文件要修改后要执行保存操作，不然查看不了文件状态变化。**

#### 对于新增和修改文件操作

1. 对于新增的文件，属于未跟踪的文件，意思是

> 意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”， 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。 

所以，使用命令

		$ git add 具体文件名

用以上命令，添加到Git来跟踪这个文件。同样，改命令的参数是目录的路径，将递归地跟踪该目录下的所有文件。

2. 对修改的文件，属于已跟踪的文件，但是一旦修改的文件属于工作目录（属于新的要暂存版本），同样需要运行`git add`命令，可以理解为

>  “添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 

进一步理清暂存区概念，一旦有新增、修改的文件，都属于非暂存区，提交时提示有文件新增和修改，和要添加到暂存区。在暂存区里都是确定已经可以提交文件。

> 实际上 Git 只不过暂存了你运行 git add 命令时的版本， 如果你现在提交，CONTRIBUTING.md 的版本是你最后一次运行 git add 命令时的那个版本，而不是你运行 git commit 时，在工作目录中的当前版本。 所以，运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来。

#### 2.2忽略文件

需要创建`.gitignore`文件，文件里每一行使用正则表达式来忽略上传的文件。

		$ cat .gitignore //查看.gitignore文件

#### 查看未暂存和已暂存的文件修改

使用`git diff`可以查看未暂存文件修改过，使用`git diff --staged`来查看已暂存文件修改。

#### 提交更新

注意，每次提交更新之前，要查看文件状态，因为提交的是暂存区，而不是非暂存区的文件（新增和修改）。

		$ git commit //之后打开文本编辑器以便输入本次提交的说明

		//or

		$ git commit -m 'update'

同样，可以跳过使用暂存区域，使用以下命名：

		$ git commit -a -m 'added new'

Git会自动把所有已经跟踪过的文件暂存起来一并提交。

#### 删除文件

当删除文件，要注意不同区域的文件。对未跟踪的文件，可以直接手工在工作目录中删除文件；对暂存区的文件，有两种情况：

	1. 如果已经手工删除工作目录中文件，只需要用命令执行：

		$ git rm //+ 相应文件名称

	2. 如果工作目录没有删除文件，需要用如下命令：

		$ git rm -f //f:force

	就是强制删除，不用参数-f，则会提示错误文件已暂存；执行之后会连同工作目录的文件删除。


如果只是想删除暂存区里文件，可以执行如下命令：

		$ git rm --catched //+ 相应文件名称 or 文件目录/正则

#### 重名文件

重名文件的命令`git mv`，针对暂存区的文件。具体如下：

		$ git mv <source's file> <destination's file>

#### [查看提交历史记录](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)

		$ git log //使用一些参数过滤提交信息或一些其他具体信息，如提交统计信息

#### 撤消操作

	当提交之后，发现其他文件没有提交或者信息写错，再次文件暂存，然后，执行如下命令：

		$ git commit --amend

	将会代替之前提交信息。

#### 取消暂存的文件

	如果取消文件保留在暂存区，可以用如下命令：

		$ git reset HEAD <file>...

	如果想重置文件，任何文件修改信息将会消失，如：

		$ git checkout -- <file>

---

#### 查看远程仓库

> 如果想查看你已经配置的远程仓库服务器，可以运行 git remote 命令。其中origin，这是 Git 给你克隆的仓库服务器的默认名字。指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

#### 添加远程仓库

> 运行 git remote add [shortname] [url] 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写。

#### 从远程仓库中抓取与拉取

> 从远程仓库中获得数据（拉取所有你还没有的数据），命令会将数据拉取到你的本地仓库，它并不会自动合并或修改你当前的工作。如下命令：

		$ git fetch [remote-name]

> 而运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

#### 推送到远程仓库

> 当你想分享你的项目时，必须将其推送到上游。 这个命令很简单：git push [remote-name] [branch-name]。**只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。**

#### 查看远程仓库

> 查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。

#### 远程仓库的移除与重命名

> 重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。如下例子：

		$ git remote rename <old name> <new name>

> 移除一个远程仓库： 

		$ git remote rm <remote url or name>

---

#### 打标签

> 这个功能来标记发布结点（v1.0 等等）。

#### 列出标签

输入 git tag，输出标签列表，已字母排序。可以查找特定的的标签，例如：

		$ git tag -l 'v1.8.5*'

#### 创建标签

> Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。

> 一个轻量标签很像一个不会改变的分支 - 它只是一个特定提交的引用。

> 然而，附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。 通常建议创建附注标签，这样你可以拥有以上所有信息；但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，轻量标签也是可用的。

#### 附注标签

		$ git tag -a v1.4 -m 'my version 1.4'

	-a指定一个版本，-m指定标签信息。

> 通过使用 git show 命令可以看到标签信息与对应的提交信息。

> 轻量标签本质上是将提交校验和存储到一个文件中，没有保存任何其他信息。如下命令：

		$ git tag v1.4-lw

#### 后期打标签

		$ git tag -a v1.2 9fceb02 

	9fceb02是提交时的提交的校验和（或部分校验和）。

#### 共享标签

> git push 命令并不会传送标签到远程仓库服务器上。运行 git push origin [tagname]打上标签。如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令。 这将会把所有不在远程仓库服务器上的标签全部传送到那里。

### 检出标签

> 想要工作目录与仓库中特定的标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支。

#### Git 别名

对Git内部命令，起用别名：

		$ git config --global alias.ci commit

同样，可以用别名一串字符命令：

		$ git config --global alias.unstage 'reset HEAD --'

对Git外部命令，在命令前加`!`调用外部命名:

		$ git config --global alias.visual '!gitk'

---

### 分支

> Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。

> Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。

#### 分支创建

		$ git branch myBranch

但是注意:

> 因为 git branch 命令仅仅 创建 一个新分支，并不会自动切换到新分支中去。

> Git 又是怎么知道当前在哪一个分支上呢？ 也很简单，它有一个名为 HEAD 的特殊指针。

#### 分支切换

		$ git checkout myBranch

当前工作目录切换myBranch，还有一个命令可以同时创建和切换到新的分支：

		$ git checkout -b newestBranch

注意的是要当前目录上提交，才会记录当前分支上。

#### 删除分支

		$ git branch -d myBranch

#### 分支的合并

合并有两种情况：不冲突和冲突合并；其中不冲突分为`快进（fast-forward）`和三方合并。

1. 不冲突合并
	a.如果当前master不处于分叉中，只会简单的将指针向前推进（指针右移）
	b.如果处于分叉中，两个分叉中会自动寻找父节点，则三者进行合并，并提交和Head指向它。

		$ git merge [分支]

以上的命令，执行合并和提交操作。

2. 冲突合并

执行`git merge`，**如果修改同一个文件且同一个部分代码进行合并，则会发生冲突现象**，只会执行合并操作，不会执行提交操作，当手动操作解决冲突之后，再运行命令`git add`来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

执行`git commit`来完成合并提交。

#### 分支管理

1. 查看分支

		$ git branch

		//or 

		$ git branch --all //同时列出远程分支

	执行上面命令，则列出所有分支，并前面带有星号，表示当前目录。

2. 查看已合并和未合并分支

		$ git branch --merged 与 --no-merged

	已合并的分支可以删除，未合并的分支不可以进行删除，但是可以强制删除，大写参数D，如下命令：

		$ git branch -D [分支]

#### 远程分支

如下命令，来显式地获得远程引用（对远程仓库的引用（指针），包括分支、标签等等）的完整列表：

		$ git ls-remote (remote)

远程分支以(remote)/(branch) 形式命名。

		$ git fetch (remote)

		//or

		$ git fetch --all //抓取所有远程分支

以上命令，是从远程分支抓取到本地没有的数据，远程分支在本地属于一个分支，并不会自动合并当期工作目录分支；如果使用

		$ git pull (remote)

不仅抓取远程分支，且合并本地分支。

在推送之前，必要检查是否远程分支数据是否有更新，就是执行抓取和合并操作之后，才能进行推送：

		$ git push (remote) (branch)

#### 如何避免每次输入密码

>  最简单的方式就是将其保存在内存中几分钟，可以简单地运行`git config --global credential.helper cache`来设置它。

#### 跟踪分支

> 如果不想跟踪默认的分支，运行`git checkout -b [branch] [remotename]/[branch]`。 这是一个十分常用的操作所以 Git 提供了 --track 快捷方式：

		$ git checkout --track origin/serverfix

如果想要查看设置的所有跟踪分支:

		$ git branch -vv

#### 删除远程分支

从服务器上删除 serverfix 分支，基本上这个命令做的只是从服务器上移除这个指针：

		$ git push origin --delete serverfix

### 变基

变基功能也是分支合并作用（第二种方法），原理是一个分支移动master分支之后，然后进行合并。

		$ git checkout b1 //切换到分支
		$ git rebase master //使用变基操作：b1链接到master

		$ git checkout master
		$ git merge b1

好处是提交历史是一条直线，方便后续人快速合并，但是变基操作会丢弃一些提交操作，存在风险。

遵守一条准则：

**不要对在你的仓库外有副本的分支执行变基。**

总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作。