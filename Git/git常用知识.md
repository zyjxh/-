# 「一劳永逸」一张脑图带你掌握Git命令

## 前言

最近在`网易`工作之余，遇到Git上面一些问题，趁这次的机会，补一补Git基础知识。

公众号**前端UpUp**，回复Git，即可获取脑图。

联系👉TianTianUp,有惊喜噢❤️

脑图👇



![Git脑图](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/145b0cdfa98a4a9cb724d745a1466c47~tplv-k3u1fbpfcp-zoom-1.image)



**本文更多梳理的是Git常用命令，非原理篇，需要这部分知识的可以往下看。**

------

首先我们的了解Git通常的操作流程，网上流行的不错一张图👇



![Git经典流程图](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1d538d63559402fbcfd82d68b08061c~tplv-k3u1fbpfcp-zoom-1.image)



------

## 基本概念



![Git基本命令](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b675e7bb00d24232a2338f87d85d00af~tplv-k3u1fbpfcp-zoom-1.image)



基于上面的图，我们就有接下来一些概念👇

- 版本库👉`.git`
  - 当我们使用git管理文件时，比如`git init`时，这个时候，会多一个`.git`文件，我们把这个文件称之为版本库。
  - `.git文件`另外一个作用就是它在创建的时候，会自动创建master分支，并且将HEAD指针指向master分支。
- 工作区
  - 本地项目存放文件的位置
  - 可以理解成图上的workspace
- 暂存区 (Index/Stage)
  - 顾名思义就是暂时存放文件的地方，通过是通过add命令将工作区的文件添加到缓冲区
- 本地仓库（Repository）
  - 通常情况下，我们使用commit命令可以将暂存区的文件添加到本地仓库
  - 通常而言，HEAD指针指向的就是master分支
- 远程仓库（Remote）
  - 举个例子，当我们使用GitHub托管我们项目时，它就是一个远程仓库。
  - 通常我们使用clone命令将远程仓库代码拷贝下来，本地代码更新后，通过push托送给远程仓库。

------

## Git文件状态

- 通常我们需要查看一个文件的状态

```bash
git status
复制代码
```

- ```
  Changes not staged for commit
  ```

  - 表示得大概就是工作区有该内容，但是缓存区没有，需要我们`git add`

- ```
  Changes to be committed
  ```

  - 一般而言，这个时候，文件放在缓存区了，我们需要`git commit`

- ```
  nothing to commit, working tree clean
  ```

  - 这个时候，我们将本地的代码推送到远端即可

------

## 常见命令

### git配置命令



![Git配置命令](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



- 列出当前配置

```bash
git config --list   
复制代码
```

- 列出Repository配置

```bash
git config --local --list
复制代码
```

- 列出全局配置

```bash
git config --global --list
复制代码
```

- 列出系统配置

```bash
git config --system --list
复制代码
```

通过上述的命令，发现你并没有配置用户信息的话，接下来配置一下👇

- 配置用户名

```bash
git config --global user.name "your name"
复制代码
```

- 配置用户邮箱

```bash
git config --global user.email "youremail@github.com"
复制代码
```

------

### 分支管理



![Git分支管理](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



- 查看本地分支

```bash
git branch
复制代码
```

- 查看远程分支

```bash
git branch -r
复制代码
```

- 查看本地和远程分支

```bash
git branch -a
复制代码
```

- 从当前分支，切换到其他分支

```bash
git checkout <branch-name>
// 举个例子
git checkout feature/tiantian
复制代码
```

- 创建并切换到新建分支

```bash
git checkout -b <branch-name>
// 举个例子👇
git checkout -b feature/tiantian
复制代码
```

- 删除分支

```bash
git branch -d <branch-name>
// 举个例子👇
git branch -d feature/tiantian
复制代码
```

- 当前分支与指定分支合并

```bash
git merge <branch-name>
// 举个例子👇
git merge feature/tiantian
复制代码
```

- 查看哪些分支已经合并到当前分支

```bash
git branch --merged
复制代码
```

- 查看哪些分支没有合并到当前分支

```bash
git branch --no-merged
复制代码
```

- 查看各个分支最后一个提交对象的信息

```bash
git branch -v
复制代码
```

- 删除远程分支

```bash
git push origin -d <branch-name>
复制代码
```

- 重命名分支

```bash
git branch -m <oldbranch-name> <newbranch-name>
复制代码
```

- 拉取远程分支并创建本地分支

```bash
git checkout -b 本地分支名x origin/远程分支名x

// 另外一种方式,也可以完成这个操作。
git fetch origin <branch-name>:<local-branch-name>
// fetch这个指令的话,后续会梳理
复制代码
```

------

### fetch指令



![Git命令fetch](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



我理解的就是将远程仓库内容更新到本地，最近与师姐开发项目过程中，使用的就是这个命令。

具体是这样子的👇

#### fetch推荐写法

```bash
git fetch origin <branch-name>:<local-branch-name>
复制代码
```

- 一般而言，这个origin是远程主机名，一般默认就是origin。
- `branch-name` 你要拉取的分支
- `local-branch-name` 通常而言，就是你本地新建一个新分支，将origin下的某个分支代码下载到本地分支。

举个例子👇

```bash
git fetch origin feature/template_excellent:feature/template_layout
// 你的工作目录下，就会有feature/template_layout
// 一般情况下,我们需要做的就是在这个分支上开发新需求
// 完成代码后,我们需要做的就是上传我们的分支
复制代码
```

#### fetch其他写法

- 将某个远程主机的更新，全部取回本地。

```bash
git fetch <远程主机名> 
复制代码
```

- 这样子的话，取回的是所有的分支更新，如果想取回特定分支，可以指定分支名👇

```bash
git fetch <远程主机名> <分支名>
复制代码
```

- 当你想将某个分支的内容取回到本地下某个分支的话，如下👇

```
git fetch origin :<local-branch-name>
// 等价于👇
git fetch origin master:<local-branch-name>
复制代码
```

------

### 花式撤销



![Git花式撤销](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



- 撤销**工作区**修改

  - git checkout --

- 暂存区文件撤销 (不覆盖工作区)

  - git reset HEAD

- 版本回退

  - git reset --(soft | mixed | hard ) < HEAD ~(num) > |

  - | 指令    | 作用范围                                |
    | ------- | --------------------------------------- |
    | --hard  | 回退全部，包括HEAD，index，working tree |
    | --mixed | 回退部分,包括HEAD，index                |
    | --soft  | 只回退HEAD                              |

------

### 状态查询

- 查看状态
  - git status
- 查看历史操作记录
  - git reflog
- 查看日志
  - git log

------

### 文档查询

- 展示Git命令大纲
  - git help (--help)
- 展示Git命令大纲全部列表
  - git help -a
- 展示具体命令说明手册
  - git help

------

### 文件暂存



![Git命令文件暂存](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



- 添加改动到stash
  - git stash save -a “message”
- 删除暂存
  - git stash drop [stash@{ID}](mailto:stash@{ID})
- 查看stash列表
  - git stash list
- 删除全部缓存
  - git stash clear
- 恢复改动
  - git stash pop [stash@{ID}](mailto:stash@{ID})

------

### 差异比较



![Git文件比较](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



- 比较工作区与缓存区
  - git diff
- 比较缓存区与本地库最近一次commit内容
  - git diff -- cached
- 比较工作区与本地最近一次commit内容
  - git diff HEAD
- 比较两个commit之间差异
  - git diff

------

## 分支命名



![Git分支管理规范](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



**master分支**

1. 主分支，用于部署生产环境的分支，确保稳定性。
2. master分支一般由develop以及hotfix分支合并，任何情况下都不能直接修改代码。

**develop 分支**

1. develop为开发分支，通常情况下，保存最新完成以及bug修复后的代码。
2. 开发新功能时，feature分支都是基于develop分支下创建的。

**feature分支**

1. 开发新功能，基本上以develop为基础创建feature分支。
2. 分支命名：feature/ 开头的为特性分支， 命名规则: feature/user_module、 feature/cart_module。

**这点我深有体会，我在网易，mentor就是这么教我的，**通常建一个feature分支。

**release分支**

1. release 为预上线分支，发布提测阶段，会release分支代码为基准提测。

**hotfix分支**

1. 分支命名：hotfix/ 开头的为修复分支，它的命名规则与 feature 分支类似。
2. 线上出现紧急问题时，需要及时修复，以master分支为基线，创建hotfix分支，修复完成后，需要合并到master分支和develop分支。

参考来着：[稻草叔叔](https://juejin.im/post/6844903635533594632)

------

## 基本操作

有了上述的基本了解后，那么我们就来看看整体的一个流程吧。

- 创建本地仓库 git init

  > git init

- 链接本地仓库与远端仓库

  > git remote add origin
  >
  > origin默认是远端仓库别名 url 可以是**可以使用https或者ssh的方式新建**

- 检查配置信息

  - git config --list

- Git user name 与email

  > git config --global user.name "yourname"
  >
  > git config --global user.email "your_email"

- 生成SSH密钥

  > ssh-keygen -t rsa -C "这里换上你的邮箱"
  >
  > cd ~/.ssh 里面有一个文件名为id_rsa.pub,把里面的内容复制到git库的我的SSHKEYs中

- 常看远端仓库信息

  - git remote -v

- 远端仓库重新命名

  - git remote rename old new

- 提交到缓存区

  - git add . 全部上传到缓存区
  - git add 指定文件

- 提交到本地仓库

  - git commit -m 'some message'

- 提交远程仓库

  - git push <远程主机名> <本地分支名>:<远程分支名>

- 查看分支

  - git branch

- 创建新分支

  - git branch

- 切换分支

  - git checkout

- 创建分支并切换

  - git checkout -b

- 删除分支

  - git branch -d

- 删除远程分支

  - git push -d

- 切换分支

  - git checkout

## 忽略文件 .gitignore

这个文件的作用，会去忽略一些不需要纳入Git管理这种，我们也不希望出现在未跟踪文件列表。

那么我们来看看如何配置该文件信息。

```
# 此行为注释 会被Git忽略

# 忽略 node_modules/ 目录下所有的文件
node_modules


# 忽略所有.vscode结尾的文件
.vscode

# 忽略所有.md结尾的文件
*.md

# 但README.md 除外
!README.md

# 会忽略 doc/something.txt 但不会忽略doc/images/arch.txt
doc/*.txt

# 忽略 doc/ 目录下所有扩展名为txt文件

doc/**/*.txt

复制代码
```

------

## 参考

- [git基本操作，一篇文章就够了！](https://juejin.im/post/6844903598522908686#heading-5)
- [Git 常用操作总结](https://juejin.im/post/6844903586120335367)
- [您必须知道的 Git 分支开发规范](https://juejin.im/post/6844903635533594632#heading-3)
- [如何优雅地使用 Git](https://juejin.im/post/6844903546104135694#heading-5)
- [优雅的提交你的 Git Commit Message](https://juejin.im/post/6844903606815064077)
- [详解git pull和git fetch的区别](https://blog.csdn.net/weixin_41975655/article/details/82887273?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-6.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-6.channel_param)
- [十分钟的时间，武装你的代码库](https://juejin.im/post/6861769180736126983)
- [「备忘录」60+Git常用命令行](https://juejin.im/post/6844904149465710599)