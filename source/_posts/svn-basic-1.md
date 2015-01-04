title: svn基础(1)
date: 2014-12-25 13:43:45
tags: svn, Revision Control System
categories: RCS
toc: true
---
## 版本控制介绍

### 什么是版本控制

版本控制系统用于保存编写开发应用程序时的文档的各个修订版（revision）。

版本控制也称作Revision Control System(RCS)。

**名词解释：**

* 修订版（revision）：可以认为是某个文件在其生命周期内各个保存的快照，每个快照和一个时间区间对应。
* 版本库（Repository）：存放修订版的数据库
* 本地工作拷贝（Local working copy）：修订版在本地的副本
* 版本的检入（Check in）：本地副本提交到服务器的版本库
* 检出（Check out）：从服务器的版本库中取出修订版成为本地副本
* 版本号的来源：有两种策略，基于文件的计数和基于仓库的计数，subversion使用后者
* 标签（Tags）：为版本加一个名字，便于检出
* 分支（Branches）：修订版打分支，以后可以平行修改，互不干扰
* 合并（Merging）：将分支的修订版合并为一个新的修订版
* 锁（Locking）：为修订版枷锁
* 冲突（Conflict）：并发版本控制时防止修订版混乱的错误机制

### 使用版本控制的好处

对团队和个人都有好处：

* 为团队提供了所有项目文档的回退按钮；
* 支持多个开发人员在可控的方式下为相同的代码工作；
* 版本控制系统保存了文档在过去的各个改动，便于查找是谁，在何时，因为什么修改了文档；
* 支持在文档的主线上同时有多个版本；
* 支持查询在某个时点上项目各个文档的状态，可用于研究生产效率等，也可用于对以前的软件发行版的再发行。
 

### 常见的版本控制系统

* VSS：Visual Source Save，微软公司的版本控制器软件，http://msdn2.microsoft.com/zh-cn/vstudio/aa718670.aspx
* CVS：Concurrent Versions System，开源免费，http://www.nongnu.org/cvs/
* Subversion，开源免费，http://subversion.tigris.org/
* ClearCase，IBM公司，http://www-306.ibm.com/software/awdtools/clearcase/

## Subversion介绍

Subversion是新一代的版本控制工具，正逐步替代CVS。

**资源：**

* [官方网址：](http://subversion.tigris.org/)
* [subversion中文站：](http://www.subversion.org.cn/)
* [中文手册：](http://www.subversion.org.cn/svnbook/)
* [subversion与cvs的对比：](http://www.uml.org.cn/pzgl/200705251.asp)

## Subversion基本使用

### Subversion安装

Subversion是典型的C/S模式应用程序。

[Windows环境下的安装包](http://subversion.tigris.org/files/documents/15/41687/svn-1.4.6-setup.exe)

安装过程很简单，图形界面，默认选择即可。

输入svn命令查看安装是否成功：

    svn --version
svn命令是subversion程序的客户端

`svn server`命令可以启动svn服务器，用于搭建简易的svn服务器环境
见[这里](http://www.easymorse.com/bbs/viewthread.php?tid=95&extra=page%3D1)

### 服务器端

以下是搭建简易的服务器端环境的做法，正式一般配合apache通过http访问。

#### 创建版本库

创建服务器端版本库，相当于DBMS创建数据库示例。

命令行：

    svnadmin create file_path/repo_name
 
#### 启动服务器

    svnserve.exe -d  -r file_path
    -d 后台执行
    -r 版本库的根目录
    访问该版本库的url：svn//localhost/repo_name


### 客户端

#### 初始导入（import）

通过命令行导入：

    svn import -m "init import" http://10.0.0.6/svn/teaching/
该命令可将当前路径下文件导入到版本库中。

#### 检出（checkout）

通过命令行检入：

    svn co http://hibernate3demo.googlecode.com/svn/tags/helloworld_r1
或者：

    svn checkout http://hibernate3demo.googlecode.com/svn/tags/helloworld_r1
或者：通过第三方图形工具的检出，比如[tortoiseSVN](http://tortoisesvn.tigris.org/), 将svn服务器的最新修订版下载到本地成为本地工作拷贝。

#### 保持更新（update）

命令行：

    svn update
或者

    svn up
或者通过tortoiseSVN
或者通过eclipse插件，[subclipse](http://subclipse.tigris.org/)，[在线安装](http://subclipse.tigris.org/update_1.2.x/)

用svn服务器的最新修订版更新本地工作拷贝。

**多人合作时：**

更新要经常频繁的做，尽量让问题及早暴露，便于处理。
提交代码前要更新，否则容易产生版本冲突。

#### 添加（add）
命令行：

    svn add file_path
或者通过tortoiseSVN，eclipse插件。

告知svn服务器，添加目录和/或文件到服务器上，这个操作类似SQL的insert，但是并没有真的操作，直到commit。

#### 提交改动

相当于通用概念：检入（checkin）。
命令行：

    svn commit
或者：
    
    svn ci
或者通过tortoiseSVN，eclipse插件。
提交本地工作拷贝的所有改动，而且是原子性的。
要求：一般要注明修改的原因

    svn ci -m "修改bug #224"
**要求：提交之前要做更新**

    svn up
    svn ci -m "修改bug #224"

#### 还原改动

对应提交(commit)，要有类似回滚（rollback）的操作。

    svn revert
或者通过tortoiseSVN，eclipse插件。

这个操作对开发人员十分有用，在改动被人很多代码后可以“一键恢复”。

**“还原”已提交的改动**

revert只适合未提交的情况。如果已经提交，发现问题，要回退到之前的修订版。首先需要：

    svn up
让本地工作拷贝更新到最新状态。然后：

    svn log your_file_path
查看文件日志，这时候提交时填写的说明信息就派上用场了。
查看两个修订版之间的不同：

    svn diff -r 旧修订版序号:新修订版序号 your_file_path
或者通过tortoiseSVN，eclipse插件。
决定用哪个旧的修订版号后，用旧的修订版号文件覆盖新的修订版号文件。

    svn merge -r 新修订版序号:旧修订版序号 your_file_path
还需要：

    svn commit -m "恢复到某修订版（某修订版作废）"
或者通过tortoiseSVN，eclipse插件。
这个还原是所谓的，不是用旧的版本号替代，而是将旧文件覆盖新文件。

#### 拷贝文件和目录

命令行：

    svn copy path/file_name newpath/new_file_name
    svn commit -m "xxxx"
或者：

    svn cp path/file_name newpath/new_file_name
    svn commit -m "xxxx"
或者：利用windows的资源管理器/unix的cp命令
或者通过tortoiseSVN，eclipse插件。
svn的copy，是很重要的工具，版本分支和标签等概念都通过它实现。
svn的copy，是廉价的拷贝。

#### 重命名目录/文件

命令行：

    svn move file_name new_file_name
或者：

    svn mv file_name new_file_name
 
#### 处理合并冲突

svn默认不对文件加锁。如果不同人编辑了同一个文件的不同部分，提交时会自动合并。如果不同人编辑了同一个文件的同一部分，后提交者会报告合并冲突。

**解决方法（人工仲裁）：**

* 放弃改动；
* 坚持你的改动，找到.mine的文件名，恢复为原文件名，然后执行：

    svn resolved file_name
 
#### 删除文件

将本地工作拷贝删除。
命令行：

    svn delete file_path
或者：

    svn del file_path

## Subversion高级内容
### 文件锁

一般用于二进制内容，因为无法合并。
如果某个文件加锁，其他用户的本地工作拷贝（更新后）将是只读的。当该用户提交后，其他用户的本地工作拷贝（更新后）才可以写操作。

其他用户可以“撬锁”，然后进行写操作。
高级配置可以配置“撬锁”权限，使不是什么人都可以“撬锁”。

### 版本库创建策略

单一的版本库保存一个项目。
单一的版本库保存多个项目。
多个版本库。

### 使用标签和分支

在svn中标签和分支都源于copy命令。

**3个约定俗成的目录：**
* trunk：主干
* branches：分支
* tags：标签

#### 发布分支：

    svn cp -m "创建用于实现radio标签的分支" https://easymorse-simpletag.googlecode.com/svn/branches/simpletag_select_1 https://easymorse-simpletag.googlecode.com/svn/branches/simpletag_select_2

#### 切换分支：

    svn switch https://easymorse-simpletag.googlecode.com/svn/branches/simpletag_select_2

#### 合并分支：

**合并操作**

    svn merge -r 33:HEAD https://easymorse-simpletag.googlecode.com/svn/branches/simpletag_select_2
或者：

    svn merge https://easymorse-simpletag.googlecode.com/svn/trunk/simpletag@HEAD https://easymorse-simpletag.googlecode.com/svn/branches/simpletag_select_1@HEAD
提交

## 未讲到的内容

* svn备份
* subversion属性
* 与apache集成的配置和使用
* svn的恢复与删除操作：学习的：

1. 本地删除
   本地删除，指的是在客户端delete了一个文件，但还没有commit，使用revert来撤销删除。
2. 服务器删除   
    1.通过本地删除后提交服务器
    a)Update上一个版本，但只适用于修改较少的情况；
    b) 通过show Log,找到被删的项，选中，点击右键->Revert changes from this revision;

    2.在Repo-Browser中删除
        这个删除比较厉害，让你在Repo-Browser都看不到被删除的项了。事实上项目并没有被真正删除。
        a)本地项目还存在，点击右键->TortoiseSVN->Update To Revision,输入被删时的Revision，确定，然后再Import上去即可。
        b)本地项目也不在了
    点击右键->TortoiseSVN->Export..，
    Url of repository填写项目的地址;
    Export Directory填写要备份的路径;
    Revision一项，默认是Head Revision，但是这里不能选这项。因为项目已经被delete了，Head Revision只对现存的项目有效，你最好是记住了删除前的版本。如果实在忘记了，一个个看，直到找到你需要的版本。
点击OK，如果没有该目录会提示你建立一个，确定后就OK 了。