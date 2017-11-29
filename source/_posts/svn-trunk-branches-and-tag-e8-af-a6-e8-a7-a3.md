---
title: 'SVN trunk, branches and tag详解'
id: 288
categories:
  - Svn
  - 版本控制
date: 2011-08-26 01:35:28
tags:
---

<div id="blog_text">

**trunk** ：表示开发时版本存放的目录，即在开发阶段的代码都提交到该目录上。

**branches** ：表示发布的版本存放的目录，即项目上线时发布的稳定版本存放在该目录中。

**tags** ：表示标签存放的目录。

&nbsp;

在这需要说明下分三个目录的原因，如果项目分为一期、二期、三期等，那么一期上线时的稳定版本就应该在一期完成时将代码copy到branches 上，这样二期开发的代码就对一期的代码没有影响，如新增的模块就不会部署到生产环境上。而branches上的稳定的版本就是发布到生产环境上的代码，如 果用户使用的过程中发现有bug，则只要在branches上修改该bug，修改完bug后再编译branches上最新的代码发布到生产环境即可。 tags的作用是将在branches上修改的bug的代码合并到trank上时创建个版本标识，以后branches上修改的bug代码再合并到 trunk上时就从tags的version到branches最新的version合并到trunk，以保证前期修改的bug代码不会在合并。

### [SVN—patch的应用](http://xiebh.iteye.com/blog/347458)

**1.create patch **
使用create patch可以生成一个或者多个修改过的文件和当前版本差异的patch（支持目录树）
通常情况下，create patch将修改保存为.patch或.diff文件
可以将.patch或.diff文件的内容复制出来，发给需要审查的人
.patch或.diff文件中记录了发生这个patch的版本号以及具体修改的内容
针对某个文件或某几个文件的若干种修改，可以生成多个.patch或.diff文件
**2.apply patch **
可以将.patch或.diff文件应用到对应版本的项目，就像打补丁一样
同一个项目/文件夹下，可以选择应用需要的patch
通常来说，应用一个patch时文件版本和生成这个patch时文件的版本是一致的；如果不一致，也可以强制应用，svn会自动进行diff（这时候需要手动合并）
linux下，可以使用系统的patch命令来应用patch，eg: patch -p0 &lt;xxx.patch
**3.使用 **
暂时不需要提交或不允许提交的修改，可以选择create patch来保存修改的内容
选择create patch来保存修改的内容并且提交patch，通过审查后，(在服务器端)应用patch
当一个功能有多种解决方案时，可以生成多个patch，（提交后）分别经过测试，再决定应用哪个patch
多个功能分别需要改同一个文件的不同地方（即没有同一行），可以做成多个patch，应用patch的顺序没有要求（在linux下应用也一样成功，只是会生成多个.orig文件）
多个连续性的功能，他们修改的文件都与一个base作patch，例：p1在v1的基础上开发v2，生成v2和v1之间的patch1；p2在v2的基础上开发v3，生成v3和v1之间的patch2，这样只要应用patch2也就应用了patch1。
**4.带来的问题 **
一个较早的patch，在经过多轮提交后，如果想再要应用，需要严格的diff
如果两个patch分别改了同一行代码，应用第一个patch后要再应用第二个patch时，仍然需要diff。如果在linux下，会产生冲突，生成.orig和.rej两个文件（此时仍然需要手动进行比较合并）
第3部分提到的连续性，要准确的预见到，比较困难
第3部分提到的多个连续的功能，后做的功能的某个文件更新了先做的功能的内容，但先做的功能可能还涉及到其他文件，容易造成漏更新文件的情况.

</div>
&nbsp;

=================================================================

&nbsp;

**翻译者：zwws
原　文：**  [**SVN trunk, branches and tags**  ](http://www.jmfeurprier.com/blog/2010/02/08/svn-trunk-branches-and-tags/)
**译　言：[http://article.yeeyan.org/view/132319/81358](http://article.yeeyan.org/view/132319/81358)  **
**转载请注明[原链接](http://www.zvv.cn/blog/show-111-1.html)  ，谢 谢。**

_因水平所限，如果翻译得和原文有差，敬请评论指正。_

在本篇文章中, 我将会详细说明我是如何应用SVN trunk(树干)、branches(分支)和tags(标记)。这种方法同样被称为“branch always”，两者非常接近。可能我所介绍的并不是最好的方法，但是它会给新手一些解释说明，告诉他们trunk、branches和tags是什么， 并且该如何去应用它们。

当然，如果本文有些要点需要澄清/确认，亦或者有一些错误的观点，还请你评论，自由发表自己的观点。

**——简单的对比**

SVN的工作机制在某种程度上就像一颗正在生长的树：

*   一颗有树干和许多分支的树
*   分支从树干生长出来，并且细的分支从相对较粗的树干中长出
*   一棵树可以只有树干没有分支（但是这种情况不会持续很久，随着树的成长，肯定会有分支啦，^^）
*   一颗没有树干但是有很多分支的树看起来更像是地板上的一捆树枝
*   如果树干患病了，最终分支也会受到影响，然后整棵树就会死亡
*   如果分支患病了，你可以剪掉它，然后其他分支还会生长出来的哦！
*   如果分支生长太快了，对于树干它可能会非常沉重，最后整棵树会垮塌掉
*   当你感觉你的树、树干或者是分支看起来很漂亮的时候，你可以给它照张相，这样就就可以记得它在那时是多么的赞。
**——Trunk**

Trunk是放置稳定代码的主要环境，就好像一个汽车工厂，负责将成品的汽车零件组装在一起。

以下内容将告诉你如何使用SVN trunk：

*   <div>除非你必须处理一些容易且能迅速解决的BUG，或者你必须添加一些无关逻辑的文件（比如媒体文件：图像，视频，CSS等等），否则永远 不要在trunk直接做开发</div>
*   <div>不要因为特殊的需求而去对先前的版本做太大的改变，如何相关的情况都意味着需要建立一个branch（如下所述）</div>
*   <div>不要提交一些可能破坏trunk的内容，例如从branch合并</div>
*   <div>如果你在某些时候偶然间破坏了trunk，bring some cake the next day (”with great responsibilities come… huge cakes”)</div>
**——Branches**

一个branch就是从一个SVN仓库中的子树 所作的一份普通拷贝。通常情况它的工作类似与UNIX系统上的符号链接，但是你一旦在一个SVN branch里修改了一些文件，并且这些被修改的文件从拷贝过来的源文件独立发展，就不能这么认为了。当一个branch完成了，并且认为它足够稳定的时 候，它必须合并回它原来的拷贝的地方，也就是说：如果原来是从trunk中拷贝的，就应该回到trunk去，或者合并回它原来拷贝的父级branch。

以下内容将告诉你如何使用SVN branches：

*   <div>如果你需要修改你的应用程序，或者为它开发一个新的特性，请从trunk中创建一个新的branch，然后基于这个新的分支进行开发</div>
*   <div>除非是因为必须从一个branch中创建一个新的子branch，否则新的branch必须从trunk创建</div>
*   <div>当你创建了一个新branch，你应当立即切换过去。如果你没有这么做，那你为什么要在最初的地方创建这个分支呢？</div>
**——Tags**

从表面上看，SVN branches和SVN tags没有什么差别，但是从概念上来说，它们有许多差别。其实一个SVN tags就是上文所述的“为这棵树照张相”：一个trunk或者一个branch修订版的命名快照。

以下内容将告诉你如何使用SVN tags：

*   <div>作为一个开发者，永远不要切换至、取出，或者向一个SVN tag提交任何内容：一个tag好比某种“照片”，并不是实实在在的东西，tags只可读，不可写。</div>
*   <div>在特殊或者需要特别注意的环境中，如：生产环境（production）、？（staging）、测试环境（testing）等等，只 能从一个修复过的（fixed）tag中checkout和update，永远不要commit至一个tag。</div>
*   <div>对于上述提及到的环境，可以创建如下的tags：“production”，“staging”，“testing”等等。你也可以根 据软件版本、项目的成熟程度来命名tag：“1.0.3”，“stable”，“latest”等等。</div>
*   <div>当trunk已经稳定，并且可以对外发布，也要相应地重新创建tags，然后再更新相关的环境（production, staging, etc）</div>
**——工作流样例**

假设你必须添加了一个特性至一个项目，且这个项目是受版本控制的，你差不多需要完成如下几个步骤：

1.  <div>使用SVN checkout或者SVN switch从这个项目的trunk获得一个新的工作拷贝（branch）</div>
2.  <div>使用SVN切换至新的branch</div>
3.  <div>完成新特性的开发（当然，要做足够的测试，包括在开始编码前）</div>
4.  <div>一旦这个特性完成并且稳定（已提交），并经过你的同事们确认，切换至trunk</div>
5.  <div>合并你的分支至你的工作拷贝（trunk），并且解决一系列的冲突</div>
6.  <div>重新检查合并后的代码</div>
7.  <div>如果可能的话，麻烦你的同事对你所编写、更改的代码进行一次复查（review）</div>
8.  <div>提交合并后的工作拷贝至trunk</div>
9.  <div>如果某些部署需要特殊的环境（生成环境等等），请更新相关的tag至你刚刚提交到trunk的修订版本</div>
10.  <div>使用SVN update部署至相关环境</div>