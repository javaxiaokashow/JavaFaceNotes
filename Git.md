## Git

#### 1.什么是Git？

我建议你先通过了解 git 的架构再来回答这个问题，如下图所示，试着解释一下这个图：

- Git 是分布式版本控制系统（DVCS）。它可以跟踪文件的更改，并允许你恢复到任何特定版本的更改。
- 与 SVN 等其他版本控制系统（VCS）相比，其分布式架构具有许多优势，一个主要优点是它不依赖于中央服务器来存储项目文件的所有版本。
- 每个开发人员都可以“克隆”我在图中用“Local repository”标注的存储库的副本，并且在他的硬盘驱动器上具有项目的完整历史记录，因此当服务器中断时，你需要的所有恢复数据都在你队友的本地 Git 存储库中。
- 还有一个中央云存储库，开发人员可以向其提交更改，并与其他团队成员进行共享，如图所示，所有协作者都在提交更改“远程存储库”。

#### 2.Git 工作流程

本章节我们将为大家介绍 Git 的工作流程。

一般工作流程如下：

- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

下图展示了 Git 的工作流程：

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-process.png)

#### 3.在 Git 中提交的命令是什么？

用于写入提交的命令是 **`git commit -a`**。

现在解释一下 `-a` 标志， 通过在命令行上加 `-a` 指示 git 提交已修改的所有被跟踪文件的新内容。还要提一下，如果你是第一次需要提交新文件，可以在在 `git commit -a` 之前先 **`git add <file>`**。

#### 4.什么是 Git 中的“裸存储库”？

你应该说明 “工作目录” 和 “裸存储库” 之间的区别。

Git 中的 “裸” 存储库只包含版本控制信息而没有工作文件（没有工作树），并且它不包含特殊的 `.git` 子目录。相反，它直接在主目录本身包含 `.git` 子目录中的所有内容，其中工作目录包括：

1. 一个 `.git` 子目录，其中包含你的仓库所有相关的 Git 修订历史记录。
2. 工作树，或签出的项目文件的副本。

#### 5.Git 是用什么语言编写的？

你需要说明使用它的原因，而不仅仅是说出语言的名称。我建议你这样回答：

Git使用 C 语言编写。 GIT 很快，C 语言通过减少运行时的开销来做到这一点。

#### 6.在Git中，你如何还原已经 push 并公开的提交？

There can be two answers to this question and make sure that you include both because any of the below options can be used depending on the situation: 1
这个问题可以有两个答案，你回答时也要保包含这两个答案，因为根据具体情况可以使用以下选项：

- 删除或修复新提交中的错误文件，并将其推送到远程存储库。这是修复错误的最自然方式。对文件进行必要的修改后，将其提交到我将使用的远程存储库

```
git commit -m "commit message"
```

- 创建一个新的提交，撤消在错误提交中所做的所有更改。可以使用命令：

```
git revert <name of bad commit>
```



#### 7.git pull 和 git fetch 有什么区别？

`git pull` 命令从中央存储库中提取特定分支的新更改或提交，并更新本地存储库中的目标分支。

`git fetch` 也用于相同的目的，但它的工作方式略有不同。当你执行 `git fetch` 时，它会从所需的分支中提取所有新提交，并将其存储在本地存储库中的新分支中。如果要在目标分支中反映这些更改，必须在 `git fetch` 之后执行`git merge`。只有在对目标分支和获取的分支进行合并后才会更新目标分支。为了方便起见，请记住以下等式：

<center><h5>git pull = git fetch + git merge</h5></center>



#### 8.git中的“staging area”或“index”是什么？

For this answer try to explain the below diagram as you can see:
可以通过下图进行解释：

在完成提交之前，可以在称为“staging area”或“index”的中间区域中对其进行格式化和审查。从图中可以看出，每个更改首先在暂存区域中进行验证，我将其称为“stage file”，然后将更改提交到存储库。

![clipboard.png](https://segmentfault.com/img/bVbtc0c?w=655&h=645)

### 9.什么是 git stash?

首先应该解释 git stash 的必要性。

通常情况下，当你一直在处理项目的某一部分时，如果你想要在某个时候切换分支去处理其他事情，事情会处于混乱的状态。问题是，你不想把完成了一半的工作的提交，以便你以后就可以回到当前的工作。解决这个问题的答案是 git stash。

再解释什么是git stash。

stash 会将你的工作目录，即修改后的跟踪文件和暂存的更改保存在一堆未完成的更改中，你可以随时重新应用这些更改。

#### 10.什么是git stash drop？

通过说明我们使用 `git stash drop` 的目的来回答这个问题。

`git stash drop` 命令用于删除隐藏的项目。默认情况下，它将删除最后添加的存储项，如果提供参数的话，它还可以删除特定项。

下面举个例子。

如果要从隐藏项目列表中删除特定的存储项目，可以使用以下命令：

**git stash list：**它将显示隐藏项目列表，如：

stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert “added file_size”
stash@{2}: WIP on master: 21d80a5 added number to log

如果要删除名为 stash@{0} 的项目，请使用命令 **git stash drop stash@{0}**。

#### 11.如何找到特定提交中已更改的文件列表？

对于这个问题，不能仅仅是提供命令，还要解释这个命令究竟做了些什么。

要获取特定提交中已更改的列表文件，请使用以下命令：

**git diff-tree -r {hash}**

给定提交哈希，这将列出在该提交中更改或添加的所有文件。 `-r` 标志使命令列出单个文件，而不是仅将它们折叠到根目录名称中。

你还可以包括下面提到的内容，虽然它是可选的，但有助于给面试官留下深刻印象。

输出还将包含一些额外信息，可以通过包含两个标志把它们轻松的屏蔽掉：

**git diff-tree –no-commit-id –name-only -r {hash}**

这里 `-no-commit-id` 将禁止提交哈希值出现在输出中，而 `-name-only` 只会打印文件名而不是它们的路径。

#### 12.git config 的功能是什么？

首先说明为什么我们需要 `git config`。

git 使用你的用户名将提交与身份相关联。 `git config` 命令可用来更改你的 git 配置，包括你的用户名。

下面用一个例子来解释。

假设你要提供用户名和电子邮件 ID 用来将提交与身份相关联，以便你可以知道是谁进行了特定提交。为此，我将使用：

**git config –global user.name "Your Name":** 此命令将添加用户名。

**git config –global user.email "Your E-mail Address":** 此命令将添加电子邮件ID。

#### 13.提交对象包含什么？

Commit 对象包含以下组件，你应该提到以下这三点：

- 一组文件，表示给定时间点的项目状态
- 引用父提交对象
- SHAI 名称，一个40个字符的字符串，提交对象的唯一标识。

#### 14.Git的工作区域

对于任何一个文件，在 Git 内都只有三种区域：工作区，暂存区和本地仓库。

`工作区：表示新增或修改了某个文件，但还没有提交保存；`

`暂存区：表示把已新增或修改的文件，放在下次提交时要保存的清单中;`

`本地仓库：文件已经被安全地保存在本地仓库中了。`

#### 15.如果分支是否已合并为master，你可以通过什么手段知道？

要知道某个分支是否已合并为master，你可以使用以下命令：

`git branch –merged` 它列出了已合并到当前分支的分支。

`git branch –no-merged` 它列出了尚未合并的分支。

#### 16.什么是SubGit？

SubGit 是将 SVN 到 Git迁移的工具。它创建了一个可写的本地或远程 Subversion 存储库的 Git 镜像，并且只要你愿意，可以随意使用 Subversion 和 Git。

这样做有很多优点，比如你可以从 Subversion 快速一次性导入到 Git 或者在 Atlassian Bitbucket Server 中使用SubGit。我们可以用 SubGit 创建现有 Subversion 存储库的双向 Git-SVN 镜像。你可以在方便时 push 到 Git 或提交 Subversion。同步由 SubGit 完成。

#### 17. 如何把本地仓库的内容推向一个空的远程仓库？

首先确保本地仓库与远程之间是连同的。如果提交失败，则需要进行下面的命令进行连通：

```
git remote add origin XXXX
```

注意：XXXX是你的远程仓库地址。 如果是第一次推送，则进行下面命令：

```
git push -u origin master
```

注意：-u 是指定origin为默认主分支 之后的提交，只需要下面的命令：

```
git push origin master
```

#### 18.描述一下你所使用的分支策略？

这个问题被要求用Git来测试你的分支经验，告诉他们你在以前的工作中如何使用分支以及它的用途是什么，你可以参考以下提到的要点：

- 功能分支（Feature branching）

  要素分支模型将特定要素的所有更改保留在分支内。当通过自动化测试对功能进行全面测试和验证时，该分支将合并到主服务器中。

- 任务分支（Task branching）

  在此模型中，每个任务都在其自己的分支上实现，任务键包含在分支名称中。很容易看出哪个代码实现了哪个任务，只需在分支名称中查找任务键。

- 发布分支（Release branching）

  一旦开发分支获得了足够的发布功能，你就可以克隆该分支来形成发布分支。创建该分支将会启动下一个发布周期，所以在此之后不能再添加任何新功能，只有错误修复，文档生成和其他面向发布的任务应该包含在此分支中。一旦准备好发布，该版本将合并到主服务器并标记版本号。此外，它还应该再将自发布以来已经取得的进展合并回开发分支。

最后告诉他们分支策略因团队而异，所以我知道基本的分支操作，如删除、合并、检查分支等。

#### 19.Git 工作区、暂存区和版本库

我们先来理解下 Git 工作区、暂存区和版本库概念：

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![img](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

- 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
- 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
- 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
- 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

#### 20.打标签

   git tag -a 2.27 -m "release version 201606"
    git push origin --tags
    详解：git tag 是命令
        -a 0.1.3是增加 名为0.1.3的标签
        -m 后面跟着的是标签的注释    

     打标签的操作发生在我们commit修改到本地仓库之后。完整的例子
        git add .
        git commit -m “fixed some bugs”
        git tag -a 0.1.3 -m “Release version 0.1.3″
    
    分享提交标签到远程服务器上
       git push origin master
       git push origin --tags
       –tags参数表示提交所有tag至服务器端，普通的git push origin master操作不会推送标签到服务器端。
    
    删除标签的命令
        git push origin local_branch:remote_branch
#### 参考

https://segmentfault.com/a/1190000019315509

https://www.runoob.com/git/

https://www.cnblogs.com/rebackl/p/12990881.html

https://blog.csdn.net/junwua/article/details/82906002