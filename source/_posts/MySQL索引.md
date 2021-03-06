---
title: MySQL索引
date: 2020-04-15 15:13:29
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

### 为何要有索引

 一般的应用系统，读写比例在10:1左右，而且插入操作和一般的更新操作很少出现性能问题，在生产环境中，遇到最多的，也是最容易出问题的，还是一些复杂的查询操作，因此**对查询语句的优化**显然是重中之重。说起加速查询，就不得不提到索引了。所以**索引的作用就是对查询语句的效率进行优化**

索引(key)是存储引擎用于快速找到记录的一种数据结构。它和一本书中目录的工作方式类似——当要查找一行记录时，先在索引中快速找到行所在的位置信息，然后再直接获取到那行记录。
在MySQL中，**索引是在存储引擎层而不是服务器层实现的，所以不同的存储引擎对索引的实现和支持都不相同**。



### 什么是索引

索引在MySQL中也叫做“键”或者"key"（primary key，unique key，还有一个index key），是存储引擎用于快速找到记录的一种数据结构。MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构。提取句子主干，就可以得到索引的本质：**索引是数据结构**。其中primary key和unique key，除了有加速查询的效果之外，还有约束的效果，primary key 不为空且唯一，unique key 唯一，而index key只有加速查询的效果，没有约束效果

索引优化应该是对查询性能优化最有效的手段了。索引能够轻易将查询性能提高好几个数量级。 索引相当于字典的音序表，如果要查某个字，如果不使用音序表，则需要从几百页中逐页去查

**强调：一旦为表创建了索引，以后的查询最好先查索引，再根据索引定位的结果去找数据**



### 索引的数据结构

先问个问题：我们需要这种数据结构能做些什么？其实很简单：**就是每次查找数据时把磁盘IO次数控制在一个很小的数量级，最好是常数数量级**。目前大部分数据库系统及文件系统都采用B-树或B+树作为索引结构（B+树是通过二叉查找树，再由平衡二叉树，B-树演化而来）。

下面我们就分别介绍一下B-树和B+树



#### B-树

B-树就是我们常说的B树。

B-树是一种多路搜索树（并不一定是二叉的）

一棵m阶B-树是一棵平衡的m路搜索树。它或者是空树，或者是满足下列性质的树：

- 根节点至少有两个子女，根节点的子女个数为[2,m]
- 每个非根节点（内节点和叶节点）包含的关键字个数k满足：ceil(m/2)-1 <= k <= m-1;（ceil代表向上取整）
- 除根节点以外的所有节点（不包含叶子节点）的孩子个数为关键字个数加1
- **所有叶子节点都位于同一层**
- 每个节点中的元素从小到大排列，节点当中k-1个元素正好是k个孩子包含的元素的值域分划。
- 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] < K[i+1]；
- 非叶子结点的指针：P[1], P[2], …, P[M]；其中P[1]指向关键字小于K[1]的子树，P[M]指向关键字大于K[M-1]的子树，其它P[i]指向关键字属于(K[i-1], K[i])的子树；
- B-树高度h（n个关键字，阶数为m）的取值范围：

$$
log_m(n+1)<=h<=log_{ceil(m/2)}((n+1)/2)+1
$$

B-树的结构如图所示：

{% asset_img 1.png %}

B-树有以下性质：

- 关键字集合分布在整棵树中
- 任何一个关键字出现且只出现在一个结点中
- 搜索有可能在非叶子节点结束
- 其搜索性能等价于在关键字全集中做一次二分查找



#### B+树

B+树是B-树的变体，也是一种多路搜索树，其定义基本与B-树相同。除了以下几点：

- 非叶子节点的子树指针与关键字个数相同（B-树中子树指针比关键字多一个）；每个元素不保存数据，只用来索引，所有数据都保存在叶子节点。
- 非叶子节点的子树指针P[i]，指向关键字值属于[K[i], K[i+1])的子树（B-树是开区间）；
- 所有的叶子结点中包含了全部元素的信息，及指向含这些元素记录的指针，且叶子结点本身依关键字的大小自小而大顺序链接。
- 所有的中间节点元素都同时存在于子节点，在**子节点元素中是最大（或最小）元素**。
- **叶子节点间会建立双向链表**，B-树中没有这个双向链表

B+树的结构如图所示：

{% asset_img 2.jpg %}

注意：上图每一个父节点都出现在子节点中，是子节点最大或者最小的元素。在这里，根节点中最大的元素是15，也就是整个树中最大的元素。以后无论插入多少元素要始终保持最大元素在根节点当中。也可以使子节点中的最小的元素出现在父节点中（**最大或最小选一种实现即可**）。

B+树的特性：

- 所有关键字都出现在叶子节点的链表中，且链表中的关键字恰好是有序的
- 不可能在非叶子节点民众
- 非叶子结点相当于是叶子结点的索引，叶子结点相当于是存储（关键字）数据的数据层；
- 实际用户记录其实都存放在B+树的最底层的节点上，这些节点也被称为`叶子节点`或`叶节点`，其余用来存放`目录项`的节点称为`非叶子节点`或者`内节点`，其中`B+`树最上边的那个节点也称为`根节点`。
- 更适合文件索引系统



#### **B-树和B+树的区别**

1，B-树的叶子节点和内节点存在的都是数据行的所有信息，B+树的内节点值存放键（索引）信息，数据都在叶子节点上。

2，由于B-树键和值的所有信息，所以每页的存储的数据行相对较少，随数据发展，该树发成为一个**高瘦**的树；相反，B+树的内节点只存放键值，所以会成为一个**矮胖**的树。所以就搜索而言，B+树的效率比B树的效率要高。

3，B-树的查询效率和所查的键在B-树中的位置有关；而B+树的复杂度对于某个B+树来说是固定的，所以B+树的查询效率相对B-树比较稳定

4，B-树中所有的数据只存储一次。B+树种除了叶子节点存储所有数据以外，还需要内节点存储键的数据。所以在占用空间方面，B+树的方式比B-树占用空间要多一些，但是B+树的方式提升了整体性能。



### 为什么用B-树或B+树做索引？

红黑树等数据结构也可以用来实现索引，但是文件系统及数据库系统普遍采用B-/+Tree作为索引结构，这一节将结合计算机组成原理相关知识讨论B-/+Tree作为索引的理论基础。

一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储的磁盘上。这样的话，索引查找过程中就要产生磁盘I/O消耗，相对于内存存取，I/O存取的消耗要高几个数量级，所以评价一个数据结构作为索引的优劣最重要的指标就是在查找过程中磁盘I/O操作次数的时间复杂度。换句话说，索引的结构组织要尽量减少查找过程中磁盘I/O的存取次数。下面先介绍内存和磁盘存取原理，然后再结合这些原理分析B-/+Tree作为索引的效率。

#### 主存存取原理

目前计算机使用的主存基本都是随机读写存储器（RAM），现代RAM的结构和存取原理比较复杂，这里本文抛却具体差别，抽象出一个十分简单的存取模型来说明RAM的工作原理。

{% asset_img 3.png %}

从抽象角度看，主存是一系列的存储单元组成的矩阵，每个存储单元存储固定大小的数据。每个存储单元有唯一的地址，现代主存的编址规则比较复杂，这里将其简化成一个二维地址：通过一个行地址和一个列地址可以唯一定位到一个存储单元。上图展示了一个4 x 4的主存模型。

**主存的存取过程如下：**

当系统需要读取主存时，则将地址信号放到地址总线上传给主存，主存读到地址信号后，解析信号并定位到指定存储单元，然后将此存储单元数据放到数据总线上，供其它部件读取。

写主存的过程类似，系统将要写入单元地址和数据分别放在地址总线和数据总线上，主存读取两个总线的内容，做相应的写操作。

这里可以看出，主存存取的时间仅与存取次数呈线性关系，因为不存在机械操作，两次存取的数据的“距离”不会对时间有任何影响，例如，先取A0再取A1和先取A0再取D3的时间消耗是一样的。



#### 磁盘存取原理

索引一般以文件形式存储在磁盘上，索引检索需要磁盘I/O操作。与主存不同，磁盘I/O存在机械运动耗费，因此磁盘I/O的时间消耗是巨大的。

磁盘结构如下图所示：

{% asset_img 4.png %}

{% asset_img 5.png %}

一个磁盘由大小相同且同轴的圆形盘片组成，磁盘可以转动（各个磁盘必须同步转动）。在磁盘的一侧有磁头支架，磁头支架固定了一组磁头，每个磁头负责存取一个磁盘的内容。磁头不能转动，但是可以沿磁盘半径方向运动（实际是斜切向运动），每个磁头同一时刻也必须是同轴的，即从正上方向下看，所有磁头任何时候都是重叠的（不过目前已经有多磁头独立技术，可不受此限制）。

盘片被划分成一系列同心环，圆心是盘片中心，每个同心环叫做一个磁道，所有半径相同的磁道组成一个柱面。磁道被沿半径线划分成一个个小的段，每个段叫做一个扇区，每个扇区是磁盘的最小存储单元。为了简单起见，我们下面假设磁盘只有一个盘片和一个磁头。

当需要从磁盘读取数据时，系统会将数据逻辑地址传给磁盘，磁盘的控制电路按照寻址逻辑将逻辑地址翻译成物理地址，即确定要读的数据在哪个磁道，哪个扇区。为了读取这个扇区的数据，需要将磁头放到这个扇区上方，为了实现这一点，磁头需要移动对准相应磁道，这个过程叫做**寻道**，所耗费时间叫做**寻道时间**，然后磁盘旋转将目标扇区旋转到磁头下，这个过程耗费的时间叫做**旋转时间**。



#### 局部性原理和磁盘预读

由于存储介质的特性，磁盘本身存取就比主存慢很多，再加上机械运动耗费，磁盘的存取速度往往是主存的几百分分之一，因此为了提高效率，要尽量减少磁盘I/O。为了达到这个目的，**磁盘往往不是严格按需读取，而是每次都会预读，即使只需要一个字节，磁盘也会从这个位置开始，顺序向后读取一定长度的数据放入内存**。这样做的理论依据是计算机科学中著名的局部性原理：当一个数据被用到时，其附近的数据也通常会马上被使用。

由于磁盘顺序读取的效率很高（不需要寻道时间，只需很少的旋转时间），因此对于具有局部性的程序来说，预读可以提高I/O效率。

预读的长度一般为**页（page）的整倍数**。**页是计算机管理存储器的逻辑块**，硬件及操作系统往往将主存和磁盘存储区分割为连续的大小相等的块，**每个存储块称为一页（在许多操作系统中，页得大小通常为4k或8k）**，主存和磁盘以页为单位交换数据。当程序要读取的数据不在主存中时，会触发一个缺页异常，此时系统会向磁盘发出读盘信号，磁盘会找到数据的起始位置并向后连续读取一页或几页载入内存中，然后异常返回，程序继续运行。



#### B-树和B+树性能分析

前面提到了**一般使用磁盘I/O次数评价索引结构的优劣**。

先从B-树分析，根据B-Tree的定义，可知检索一次最多需要访问h个节点（h为树的高度）。数据库系统的设计者巧妙利用了**磁盘预读**原理，**将一个节点的大小设为等于一个页，这样每个节点只需要一次I/O就可以完全载入**。为了达到这个目的，在实际实现B-Tree还需要使用如下技巧：

每次新建节点时，直接申请一个页的空间，这样就保证一个节点物理上也存储在一个页里，加之计算机存储分配都是按页对齐的，就实现了一个node只需一次I/O。

**B-树中一次检索最多需要h-1次I/O（根节点常驻内存）**。由于B-树的高度一般都很矮，3层即可存储超过百万的数据，所以用B-树作为索引结构效率是非常高的。



再来看一下B+树，由于B+树的内节点中不存储数据，数据只存储在叶子节点中。所以一个内节点中存储的关键字相对于B-树要多很多，因此B+树相对于B-树将更加的矮胖，树的高度更低，所以I/O次数将更少，检索效率更高。

真实环境中一个页存放的记录数量是非常大的，假设，假设，假设所有存放用户记录的**叶子节点**代表的数据页可以存放100条用户记录，所有存放目录项记录的**内节点**代表的数据页可以存放1000条目录项记录，那么：

- 如果`B+`树只有1层，也就是只有1个用于存放用户记录的节点，最多能存放`100`条记录。
- 如果`B+`树有2层，最多能存放`1000×100=100000`条记录。
- 如果`B+`树有3层，最多能存放`1000×1000×100=100000000`条记录。
- 如果`B+`树有4层，最多能存放`1000×1000×1000×100=100000000000`条记录。

你的表里能存放`100000000000`条记录么？所以一般情况下，我们用到的`B+`树都不会超过4层，那我们通过索引关键字去查找某条记录最多只需要做4个页面内的查找（查找3个目录项页和一个用户记录页）。



而红黑树这种结构，由于逻辑上很近的节点（父子）物理上可能很远，无法利用局部性，h明显要深的多，所以红黑树的I/O渐进复杂度也为O(h)，效率明显比B-Tree差很多。



### MySQL的索引实现

在MySQL中，索引属于存储引擎级别的概念，不同存储引擎对索引的实现方式是不同的。这里主要讨论一下MyISAM和InnoDB两个存储引擎的索引实现方式

这里先明确一件事情，无论是MyISAM还是InnoDB，都采用B+树作为索引的数据结构。有如下特点：

- 只在叶子节点中存储信息（MyISAM存储对应该行数据信息的地址，InnoDB的聚集索引中存储该行数据信息的全部数据，InnDB的二级索引中存储的是对应的主键值）。**每个叶节点中存储的都是用户信息记录**
- 非叶节点（根节点除外）也称为内节点，**每个叶节点中存储的是目录项记录。**每个目录项包括以下两部分：
  - 索引键值
  - 所指向的下一层的页号
- 无论是内节点、根节点还是叶节点都被称为数据页，只不过叶节点层数据页中存储的是用户记录信息，根节点和内节点数据页中存储的是目录项记录信息

#### MyISAM索引实现方式

MyISAM引擎使用B+Tree作为索引结构，**叶节点的data域存放的是数据记录的地址**。MyISAM的索引结构如下图所示：

{% asset_img 6.png %}

这里设表一共有三列，假设我们以Col1为主键，则上图是一个MyISAM表的主索引（Primary key）示意。可以看出MyISAM的索引文件仅仅保存数据记录的地址。**在MyISAM中，主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而辅助索引的key可以重复**。如果我们在Col2上建立一个辅助索引，则此辅助索引的结构如下图所示：

{% asset_img 7.png %}

上图同样也是一颗B+Tree，data域保存数据记录的地址。因此，MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。

MyISAM的索引方式也叫做**非聚集索引**，之所以这么称呼是为了与InnoDB的**聚集索引**区分。

使用`MyISAM`存储引擎的表会把索引信息另外存储到一个称为`索引文件`的另一个文件中。`MyISAM`会单独为表的主键创建一个索引，只不过在索引的叶子节点中存储的不是完整的用户记录，而是`主键值 + 行号`的组合。也就是先通过索引找到对应的行号，再通过行号去找对应的记录！

这一点和`InnoDB`是完全不相同的，在`InnoDB`存储引擎中，我们只需要根据主键值对`聚簇索引`进行一次查找就能找到对应的记录，而在`MyISAM`中却需要进行一次**回表操作**，意味着`MyISAM`中建立的索引相当于全部都是`二级索引`！



#### InnoDB索引实现方式

虽然InnoDB也使用B+Tree作为索引结构，但具体实现方式却与MyISAM截然不同。

第一个重大区别是**InnoDB的数据文件本身就是索引文件**。MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，**因此InnoDB表数据文件本身就是主索引**。

{% asset_img 8.png %}

上图即为InnoDB主索引（同时也是数据文件）的示意图，可以看到叶节点包含了完整的数据记录。这种索引叫做**聚集索引**。因为InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列（UNIQUE）作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。

第二个与MyISAM索引的不同是**InnoDB的辅助索引data域存储相应记录主键的值而不是地址**。换句话说，I**nnoDB的所有辅助索引都引用主键作为data域。**例如，下图为定义在Col3上的一个辅助索引：

{% asset_img 9.png %}

聚集索引这种实现方式使得按主键的搜索十分高效，但是**辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录**。

了解不同存储引擎的索引实现方式对于正确使用和优化索引都非常有帮助，例如知道了InnoDB的索引实现后，就很容易明白为什么不建议使用过长的字段作为主键，因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用自增字段作为主键则是一个很好的选择。



##### InnoDB索引注意事项

###### 1.  B+树根节点位置不变

B+树的形成过程是这样的：

- 每当为某个表创建一个`B+`树索引（聚簇索引不是人为创建的，默认就有）的时候，都会为这个索引创建一个`根节点`页面。最开始表中没有数据的时候，每个`B+`树索引对应的`根节点`中既没有用户记录，也没有目录项记录。
- 随后向表中插入用户记录时，先把用户记录存储到这个`根节点`中。
- 当`根节点`中的可用空间用完时继续插入记录，此时会将`根节点`中的所有记录复制到一个新分配的页，比如`页a`中，然后对这个新页进行`页分裂`的操作，得到另一个新页，比如`页b`。这时新插入的记录根据键值（也就是聚簇索引中的主键值，二级索引中对应的索引列的值）的大小就会被分配到`页a`或者`页b`中，而`根节点`便升级为存储目录项记录的页。



注意：**一个B+树索引的根节点自诞生之日起，便不会再移动（保存在内存中，这样在索引查询的时候，磁盘I/O的次数将是B+树的高度减1）。**这样只要我们对某个表建立一个索引，那么它的`根节点`的页号便会被记录到某个地方，然后凡是`InnoDB`存储引擎需要用到这个索引的时候，都会从那个固定的地方取出`根节点`的页号，从而来访问这个索引。



###### 2. 内节点中目录项记录的唯一性

我们知道`B+`树索引的内节点中目录项记录的内容是`索引列 + 页号`的搭配，但是这个搭配对于二级索引来说有点儿不严谨。因为二级索引可能不是唯一的，可能存在重复值，在插入的时候如何插入是个问题。

为了让新插入记录能找到自己在那个页里，我们需要保证在B+树的同一层内节点的目录项记录除`页号`这个字段以外是唯一的。所以对于二级索引的内节点的目录项记录的内容实际上是由三个部分构成的：

- 索引列的值
- 主键值
- 页号

也就是我们把`主键值`也添加到二级索引内节点中的目录项记录了，这样就能保证`B+`树每一层节点中各条目录项记录除`页号`这个字段外是唯一的



### 索引的分类

#### 聚集索引

它有两个特点：

- 使用记录**主键值**的大小进行记录和页的排序，这包括三个方面的含义：
  - 页内的记录是按照主键的大小顺序排成一个单向链表。
  - 各个存放用户记录的页也是根据页中用户记录的主键大小顺序排成一个双向链表。
  - 存放目录项记录的页分为不同的层次，在同一层次中的页也是根据页中目录项记录的主键大小顺序排成一个双向链表。
- `B+`树的叶子节点存储的是**完整的用户记录**。所谓完整的用户记录，就是指这个记录中存储了所有列的值（包括隐藏列）。

我们把具有这两种特性的`B+`树称为`聚集索引`，所有完整的用户记录都存放在这个`聚集索引`的叶子节点处。这种`聚集索引`并不需要我们在`MySQL`语句中显式的使用`INDEX`语句去创建（后边会介绍索引相关的语句），`InnoDB`存储引擎会自动的为我们创建聚集索引（InnoDB中，聚集索引就是主键索引）。另外有趣的一点是，在`InnoDB`存储引擎中，`聚集索引`就是数据的存储方式（所有的用户记录都存储在了`叶子节点`），也就是所谓的**索引即数据，数据即索引**。

**在InnoDB中，聚集索引与主键索引等价！**



#### 非聚集索引

上面在MyISAM中介绍的索引实现方式就是非聚集索引。叶子节点中存储的不是用户记录，而是用户记录的地址，还需要进行一次**回表**才能找到对应的用户记录数据

非聚集索引又可以称之为非主键索引，辅助索引，二级索引。**MyISAM中的索引文件和数据文件分开存储，所以MyISAM中的所有索引都是非聚集索引。**



#### 二级索引

大家有木有发现，上边介绍的`聚簇索引`只能在搜索条件是主键值时才能发挥作用，因为`B+`树中的数据都是按照主键进行排序的。那如果我们想以别的列作为搜索条件该咋办呢？难道只能从头到尾沿着链表依次遍历记录么？

不，我们可以多建几棵`B+`树，不同的`B+`树中的数据采用不同的排序规则。当采用非主键建立的B+树叫做**二级索引**。

二级索引的B+树和聚集索引的B+树有几处不同：

- 使用记录二级索引列的大小进行记录和页的排序，而非主键
- `B+`树的叶子节点存储的并不是完整的用户记录，而只是`二级索引列+主键`这两个列的值。
- 非叶子结点中不再是`主键+页号`的搭配，而变成了`二级索引列+页号`的搭配



**注意**：MyISAM中的主键索引和二级索引没啥区别，叶子节点中存的都是数据地址；而在InnoDB中，主键索引是聚集索引，二级索引中叶子节点中存储的是对应的主键值，还需要**回表**到聚集索引中再查一遍。

为什么我们还需要一次`回表`操作呢？直接把完整的用户记录放到`叶子节点`不就好了么？你说的对，如果把完整的用户记录放到`叶子节点`是可以不用`回表`，但是太占地方了呀～相当于每建立一棵`B+`树都需要把所有的用户记录再都拷贝一遍，这就有点太浪费存储空间了。因为这种按照`非主键列`建立的`B+`树需要一次`回表`操作才可以定位到完整的用户记录，所以这种`B+`树也被称为`二级索引`（英文名`secondary index`），或者`辅助索引`。



#### 联合索引

我们也可以同时以多个列的大小作为排序规则，也就是同时为多个列建立索引，比方说我们想让`B+`树按照`c2`和`c3`列的大小进行排序，这个包含两层含义：

- 先把各个记录和页按照`c2`列进行排序。
- 在记录的`c2`列相同的情况下，采用`c3`列进行排序



需要注意以下几点：

- 非叶子结点中的每条`目录项记录`都由`c2`、`c3`、`页号`这三个部分组成，各条记录先按照`c2`列的值进行排序，如果记录的`c2`列相同，则按照`c3`列的值进行排序。
- `B+`树叶子节点处的用户记录由`c2`、`c3`和主键列组成。

千万要注意一点，以c2和c3列的大小为排序规则建立的B+树称为联合索引，本质上也是一个二级索引。它的意思与分别为c2和c3列分别建立索引的表述是不同的，不同点如下：

- 建立`联合索引`只会建立1棵`B+`树。
- 为c2和c3列分别建立索引会分别以`c2`和`c3`列的大小为排序规则建立2棵`B+`树。



#### 覆盖索引

我们前面提到了一个**回表**的概念，即如果通过二级索引查询数据时，会先查询到主键值，然后再去聚集索引中查询具体的数据，整个查询流程需要扫描两次索引，显然回表是一个非常耗时的操作

覆盖索引指一个查询语句的执行只用从索引中就能够取得，不必从数据表中读取。当一条查询语句符合覆盖索引条件时，MySQL只需要通过索引就可以返回查询所需的数据，这样避免了查到索引后的**回表操作**，减少磁盘I/O次数，提高了检索效率

例如：InnoDB引擎中表covering_index_sample中有一个普通索引idx_key1_key2(key1,key2)。当我们通过SQL语句：select key2 from covering_index_sample where key1 = 'keytest';的时候，就可以通过覆盖索引查询，无需回表。

**总结：如果一个索引包含(或覆盖)所有需要查询的字段的值，称为覆盖索引。即只需扫描索引而无须回表。**



#### 唯一索引

就是索引键值不允许重复的索引，比如主键索引、UNIQUE索引



#### 非唯一索引

键值可以重复的索引



### 查询优化器

一条SQL查询语句，可以有不同的执行方案（因为存在很多个索引），至于最终选择哪种方案，需要通过优化器进行选择，选择执行成本最低的方案。

在一条单表查询语句真正执行之前，MySQL的查询优化器会找出执行该语句所有可能使用的方案，对比之后找出成本最低的方案。这个成本最低的方案就是所谓的**执行计划**（用explain查看）。优化过程如下：

1. 根据搜索条件，找出所有可能使用的索引；
2. 计算全表扫描的代价；
3. 计算使用不同索引执行查询的代价；
4. 对比各种执行方案的代价，找出成本最低的哪一个



### MySQL中创建和删除索引的语句

`InnoDB`和`MyISAM`会自动为主键或者声明为`UNIQUE`的列去自动建立`B+`树索引，但是如果我们想为其他的列建立索引就需要我们显式的去指明。为啥不自动为每个列都建立个索引呢？别忘了，每建立一个索引都会建立一棵`B+`树，每插入一条记录都要维护各个记录、数据页的排序关系，这是很费性能和存储空间的。

#### 创建索引

- 在创建表的时候执行需要建立索引的单个列或者建立联合索引的多个列：

```mysql
CREATE TALBE 表名 (
    各种列的信息 ··· , 
    [KEY|INDEX] 索引名 (需要被索引的单个列或多个列)
)
```
其中的KEY和INDEX是同义词，任意选用一个就可以

- 在修改表结构的时候添加索引：

```mysql
/*添加主键索引*/
alter table 表名 add primary key(id);

/*添加唯一索引*/
alter table 表名 add unique key 索引名(id);

/*添加普通索引*/
alter table 表名 add index 索引名(id);
或
create index 索引名 on 表名(id);
```



#### 删除索引

```mysql
/*删除主键索引*/
alter table 表名 drop primary key;

/*删除唯一索引*/
alter table 表名 drop unique key 索引名;

/*删除普通索引*/
alter table 表名 drop index 索引名;
或
drop index 索引名 on 表名;
```



### 索引的代价

虽然索引是个好东西，可不能乱建，在介绍如何更好的使用索引之前先要了解一下使用这玩意儿的代价，它在空间和时间上都会拖后腿：

- 空间上的代价

  这个是显而易见的，每建立一个索引都要为它建立一棵`B+`树，每一棵`B+`树的每一个节点都是一个数据页，一个页默认会占用`16KB`的存储空间，一棵很大的`B+`树由许多数据页组成，那可是很大的一片存储空间呢。

- 时间上的代价

  每次对表中的数据进行增、删、改操作时，都需要去修改各个`B+`树索引。`B+`树每层节点都是按照索引列的值从小到大的顺序排序而组成了双向链表。不论是叶子节点中的记录，还是内节点中的记录（也就是不论是用户记录还是目录项记录）都是按照索引列的值从小到大的顺序而形成了一个单向链表。而增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需要额外的时间进行一些记录移位，页面分裂、页面回收啥的操作来维护好节点和记录的排序。如果我们建了许多索引，每个索引对应的`B+`树都要进行相关的维护操作，这还能不给性能拖后腿么？

所以说，一个表上索引建的越多，就会占用越多的存储空间，在增删改记录的时候性能就越差。



### 索引的使用

首先建一个表

```mysql
CREATE TABLE person_info(
    id INT NOT NULL auto_increment,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name, birthday, phone_number)
);
```

对于这个`person_info`表我们需要注意两点：

- 表中的主键是`id`列，它存储一个自动递增的整数。所以`InnoDB`存储引擎会自动为`id`列建立聚簇索引。
- 我们额外定义了一个二级索引`idx_name_birthday_phone_number`，它是由3个列组成的**联合索引**。所以在这个索引对应的`B+`树的叶子节点处存储的用户记录只保留`name`、`birthday`、`phone_number`这三个列的值以及主键`id`的值，并不会保存`country`列的值。

上述的聚合索引会建立一个B+树，，这个`idx_name_birthday_phone_number`索引对应的`B+`树中页面和记录的排序方式就是这样的：

- 先按照`name`列的值进行排序。
- 如果`name`列的值相同，则按照`birthday`列的值进行排序。
- 如果`birthday`列的值也相同，则按照`phone_number`的值进行排序。



下面分析一下索引使用的各种情况

#### 全值匹配

如果我们的搜索条件中的列和索引列一致的话，这种情况就称为全值匹配，比方说下边这个查找语句：

```mysql
SELECT * FROM person_info WHERE name = 'Ashburn' AND birthday = '1990-09-27' AND phone_number = '15123983239';
```

我们建立的`idx_name_birthday_phone_number`索引包含的3个列在这个查询语句中都展现出来了。那么如何利用这个联合索引快速查询呢？我这里加入了个人理解（不只是针对联合索引，针对所有的索引都是通过这个思路进行快速查询）

- 把联合索引当中的三个列的值当成是一个，把它们三个想象成一个对象，实现了comparable接口，在接口中对三个信息进行比较，比较规则是先比较name，name相同的时候再比较birthday，如果name和birthday都相同的话，再比较phone_number
- 通过上面的比较接口可以定位到叶子节点中第一条符合条件的记录
- 之后的满足要求的记录都是沿着单向链表向后搜索



`WHERE`子句中的几个搜索条件的顺序对查询结果有啥影响么？也就是说如果我们调换`name`、`birthday`、`phone_number`这几个搜索列的顺序对查询的执行过程有影响么？比方说写成下边这样：

```mysql
SELECT * FROM person_info WHERE birthday = '1990-09-27' AND phone_number = '15123983239' AND name = 'Ashburn';
```

答案是：没影响哈。`MySQL`有一个叫查询优化器的东西，会分析这些搜索条件并且按照可以使用的索引中列的顺序来决定先使用哪个搜索条件，后使用哪个搜索条件。



#### 匹配左边的列

其实在我们的搜索语句中也可以不用包含全部联合索引中的列，只包含左边的就行，比方说下边的查询语句：

```mysql
SELECT * FROM person_info WHERE name = 'Ashburn';
```

或者包含多个左边的列也行：

```mysql
SELECT * FROM person_info WHERE name = 'Ashburn' AND birthday = '1990-09-27';
```

但是如果没有出现左边的列，就用不到这个B+树索引

```mysql
SELECT * FROM person_info WHERE birthday = '1990-09-27';
```

这就是**最左前缀匹配原理**！！

为什么不出现左边的列就不能用该索引呢？因为`B+`树的数据页和记录先是按照`name`列的值排序的，在`name`列的值相同的情况下才使用`birthday`列进行排序，也就是说`name`列的值不同的记录中`birthday`的值可能是无序的。而现在你跳过`name`列直接根据`birthday`的值去查找肯定是不行的~



**注意：如果我们想使用联合索引中尽可能多的列，搜索条件中的各个列必须是联合索引中从最左边连续的列**。比方说联合索引`idx_name_birthday_phone_number`中列的定义顺序是`name`、`birthday`、`phone_number`，如果我们的搜索条件中只有`name`和`phone_number`，而没有中间的`birthday`，比方说这样：

```mysql
SELECT * FROM person_info WHERE name = 'Ashburn' AND phone_number = '15123983239';
```

这样只能用到`name`列的索引，`birthday`和`phone_number`的索引就用不上了，因为`name`值相同的记录先按照`birthday`的值进行排序，`birthday`值相同的记录才按照`phone_number`值进行排序。**这样只能先通过name快速查找到符合name条件的数据，再依次遍历找到符合phone_number的数据。**



#### 匹配列前缀

我们前边说过为某个列建立索引的意思其实就是在对应的`B+`树的记录中使用该列的值进行排序，比方说`person_info`表上建立的联合索引`idx_name_birthday_phone_number`会先用`name`列的值进行排序。

字符串排序的本质就是比较哪个字符串大一点儿，哪个字符串小一点，比较字符串大小就用到了该列的字符集和比较规则，这个我们前边儿唠叨过，就不多唠叨了。这里需要注意的是，一般的比较规则都是逐个比较字符的大小，也就是说我们比较两个字符串的大小的过程其实是这样的：

- 先比较字符串的第一个字符，第一个字符小的那个字符串就比较小。
- 如果两个字符串的第一个字符相同，那就再比较第二个字符，第二个字符比较小的那个字符串就比较小。
- 如果两个字符串的第二个字符也相同，那就接着比较第三个字符，依此类推。

也就是说这些字符串的前n个字符，也就是前缀都是排好序的，所以对于字符串类型的索引列来说，我们只匹配它的前缀也是可以快速定位记录的，比方说我们想查询名字以`'As'`开头的记录，那就可以这么写查询语句：

```mysql
SELECT * FROM person_info WHERE name LIKE 'As%';
```

但是需要注意的是，如果只给出后缀或者中间的某个字符串，比如这样：

```mysql
SELECT * FROM person_info WHERE name LIKE '%As%';
```

`MySQL`就无法快速定位记录位置了，因为字符串中间有`'As'`的字符串并没有排好序，所以只能全表扫描了。



#### 匹配范围值

在`idx_name_birthday_phone_number`索引中，所有记录都是按照索引列的值从小到大的顺序排好序的，所以这极大的方便我们查找索引列的值在某个范围内的记录。比方说下边这个查询语句：

```mysql
SELECT * FROM person_info WHERE name > 'Asa' AND name < 'Barlow';
```

由于`B+`树中的数据页和记录是先按`name`列排序的，所以我们上边的查询过程其实是这样的：

- 找到`name`值为`Asa`的记录。
- 找到`name`值为`Barlow`的记录。
- 哦啦，由于所有记录都是由链表连起来的（记录之间用单链表，数据页之间用双链表），所以他们之间的记录都可以很容易的取出来喽～
- 找到这些记录的主键值，再到`聚簇索引`中`回表`查找完整的记录。

不过在使用联合索引进行范围查找的时候需要注意，**如果对多个列同时进行范围查找的话，只有对索引最左边的那个列进行范围查找的时候才能用到`B+`树索引（除最左边的列，其余的列都不能用索引）**，比方说这样：

```mysql
SELECT * FROM person_info WHERE name > 'Asa' AND name < 'Barlow' AND birthday > '1980-01-01';
```

上边这个查询可以分成两个部分：

1. 通过条件`name > 'Asa' AND name < 'Barlow'`来对`name`进行范围，查找的结果可能有多条`name`值不同的记录，
2. 对这些`name`值不同的记录继续通过`birthday > '1980-01-01'`条件依次进行过滤。

这样子对于联合索引`idx_name_birthday_phone_number`来说，只能用到`name`列的部分，而用不到`birthday`列的部分，因为只有`name`值相同的情况下才能用`birthday`列的值进行排序，而这个查询中通过`name`进行范围查找的记录中可能并不是按照`birthday`列进行排序的，所以在搜索条件中继续以`birthday`列进行查找时是用不到这个`B+`树索引的。



#### 精确匹配某一列并范围匹配另外一列

对于同一个联合索引来说，虽然对多个列都进行范围查找时只能用到最左边那个索引列，但是如果左边的列是精确查找，则右边的列可以进行范围查找，比方说这样：

```mysql
SELECT * FROM person_info WHERE name = 'Ashburn' AND birthday > '1980-01-01' AND birthday < '2000-12-31' AND phone_number > '15100000000';
```

这个查询的条件可以分为3个部分：

1. `name = 'Ashburn'`，对`name`列进行精确查找，当然可以使用`B+`树索引了。
2. `birthday > '1980-01-01' AND birthday < '2000-12-31'`，由于`name`列是精确查找，所以通过`name = 'Ashburn'`条件查找后得到的结果的`name`值都是相同的，它们会再按照`birthday`的值进行排序。所以此时对`birthday`列进行范围查找是可以用到`B+`树索引的。
3. `phone_number > '15100000000'`，通过`birthday`的范围查找的记录的`birthday`的值可能不同，所以这个条件无法再利用`B+`树索引了，只能遍历上一步查询得到的记录。

**注意**：这里和匹配范围值的区别是，匹配范围值只能用最左边的列进行范围匹配；而这里左边的列可以先进行精确匹配，直到遇见第一个范围（必须从最左开始，连续，而且第一个范围后面无如果再加精确匹配仍可使用索引，但如果再加范围匹配就不能使用索引了）



#### 用于排序

我们在写查询语句的时候经常需要对查询出来的记录通过`ORDER BY`子句按照某种规则进行排序。一般情况下，我们只能把记录都加载到内存中，再用一些排序算法，比如快速排序、归并排序、吧啦吧啦排序等等在内存中对这些记录进行排序，有的时候可能查询的结果集太大以至于不能在内存中进行排序的话，还可能暂时借助磁盘的空间来存放中间结果，排序操作完成后再把排好序的结果集返回到客户端。在`MySQL`中，把这种在内存中或者磁盘上进行排序的方式统称为文件排序（英文名：`filesort`），跟`文件`这个词儿一沾边儿，就显得这些排序操作非常慢了（磁盘和内存的速度比起来，就像是飞机和蜗牛的对比）。但是如果`ORDER BY`子句里使用到了我们的索引列，就有可能省去在内存或文件中排序的步骤，比如下边这个简单的查询语句

```mysql
SELECT * FROM person_info ORDER BY name, birthday, phone_number LIMIT 10;
```

这个查询的结果集需要先按照`name`值排序，如果记录的`name`值相同，则需要按照`birthday`来排序，如果`birthday`的值相同，则需要按照`phone_number`排序。大家可以回过头去看我们建立的`idx_name_birthday_phone_number`索引对应的B+树，因为这个`B+`树索引本身就是按照上述规则排好序的，所以直接从索引中提取数据，然后进行`回表`操作取出该索引中不包含的列就好了。简单吧？是的，索引就是这么牛逼。



##### 使用联合索引进行排序注意事项

对于`联合索引`有个问题需要注意，`ORDER BY`的子句后边的列的顺序也必须按照索引列的顺序给出，如果给出`ORDER BY phone_number, birthday, name`的顺序或`ORDER BY birthday`等，那也是用不了`B+`树索引，这种颠倒顺序就不能使用索引的原因我们上边详细说过了，这就不赘述了。

同理，`ORDER BY name`、`ORDER BY name, birthday`这种**匹配索引左边的列**的形式可以使用部分的`B+`树索引。当联合索引左边列的值为常量，也可以使用后边的列进行排序，比如这样：

```mysql
SELECT * FROM person_info WHERE name = 'A' ORDER BY birthday, phone_number LIMIT 10;
```

这个查询能使用联合索引进行排序是因为`name`列的值相同的记录是按照`birthday`, `phone_number`排序的，说了好多遍了都。



##### 不可以使用索引进行排序的几种情况

###### ASC、DESC混用

对于使用联合索引进行排序的场景，我们要求各个排序列的排序顺序是一致的，也就是要么各个列都是`ASC`规则排序，要么都是`DESC`规则排序。

如果查询中的各个排序列的排序顺序是一致的，比方说下边这两种情况：

- `ORDER BY name, birthday LIMIT 10`

  这种情况直接从索引的最左边开始往右读10行记录就可以了。

- `ORDER BY name DESC, birthday DESC LIMIT 10`，

  这种情况直接从索引的最右边开始往左读10行记录就可以了。

但是如果我们查询的需求是先按照`name`列进行升序排列，再按照`birthday`列进行降序排列的话，比如说这样的查询语句：

```mysql
SELECT * FROM person_info ORDER BY name, birthday DESC LIMIT 10;
```

这样如果使用索引排序的话过程就是这样的：

- 先从索引的最左边确定`name`列最小的值，然后找到`name`列等于该值的所有记录，然后从`name`列等于该值的最右边的那条记录开始往左找10条记录。
- 如果`name`列等于最小的值的记录不足10条，再继续往右找`name`值第二小的记录，重复上边那个过程，直到找到10条记录为止。

累不累？累！重点是这样不能高效使用索引，而要采取更复杂的算法去从索引中取数据，设计`MySQL`的大叔觉得这样还不如直接文件排序来的快，所以就规定使用联合索引的各个排序列的排序顺序必须是一致的。



###### WHERE子句中出现非排序使用到的索引列

如果WHERE子句中出现了非排序使用到的索引列，那么排序依然是使用不到索引的，比方说这样：

```mysql
SELECT * FROM person_info WHERE country = 'China' ORDER BY name LIMIT 10;
```

这个查询只能先把符合搜索条件`country = 'China'`的记录提取出来后再进行排序，是使用不到索引。



###### 排序列包含非同一个索引的列

有时候用来排序的多个列不是一个索引里的，这种情况也不能使用索引进行排序，比方说：

```mysql
SELECT * FROM person_info ORDER BY name, country LIMIT 10;
```

`name`和`country`并不属于一个联合索引中的列，所以无法使用索引进行排序



#### 用于分组

有时候我们为了方便统计表中的一些信息，会把表中的记录按照某些列进行分组。比如下边这个分组查询：

```mysql
SELECT name, birthday, phone_number, COUNT(*) FROM person_info GROUP BY name, birthday, phone_number;
```

这个查询语句相当于做了3次分组操作：

1. 先把记录按照`name`值进行分组，所有`name`值相同的记录划分为一组。
2. 将每个`name`值相同的分组里的记录再按照`birthday`的值进行分组，将`birthday`值相同的记录放到一个小分组里，所以看起来就像在一个大分组里又化分了好多小分组。
3. 再将上一步中产生的小分组按照`phone_number`的值分成更小的分组，所以整体上看起来就像是先把记录分成一个大分组，然后把`大分组`分成若干个`小分组`，然后把若干个`小分组`再细分成更多的`小小分组`。

然后针对那些`小小分组`进行统计，比如在我们这个查询语句中就是统计每个`小小分组`包含的记录条数。如果没有索引的话，这个分组过程全部需要在内存里实现，而如果有了索引的话，恰巧这个分组顺序又和我们的`B+`树中的索引列的顺序是一致的，而我们的`B+`树索引又是按照索引列排好序的，这不正好么，所以可以直接使用`B+`树索引进行分组。

和使用`B+`树索引进行排序是一个道理，分组列的顺序也需要和索引列的顺序一致，也可以只使用索引列中左边的列进行分组



### 回表的代价

上边的讨论对`回表`这个词儿多是一带而过，可能大家没啥深刻的体会，下边我们详细唠叨下。还是用`idx_name_birthday_phone_number`索引为例，看下边这个查询：

```mysql
SELECT * FROM person_info WHERE name > 'Asa' AND name < 'Barlow';
```

在使用`idx_name_birthday_phone_number`索引进行查询时大致可以分为这两个步骤：

1. 从索引`idx_name_birthday_phone_number`对应的`B+`树中取出`name`值在`Asa`～`Barlow`之间的用户记录。
2. 由于索引`idx_name_birthday_phone_number`对应的`B+`树用户记录中只包含`name`、`birthday`、`phone_number`、`id`这4个字段，而查询列表是`*`，意味着要查询表中所有字段，也就是还要包括`country`字段。这时需要把从上一步中获取到的每一条记录的`id`字段都到聚簇索引对应的`B+`树中找到完整的用户记录，也就是我们通常所说的`回表`，然后把完整的用户记录返回给查询用户。

由于索引`idx_name_birthday_phone_number`对应的`B+`树中的记录首先会按照`name`列的值进行排序，所以值在`Asa`～`Barlow`之间的记录在磁盘中的存储是相连的，集中分布在一个或几个数据页中，我们可以很快的把这些连着的记录从磁盘中读出来，这种读取方式我们也可以称为`顺序I/O`。根据第1步中获取到的记录的`id`字段的值可能并不相连，而在聚簇索引中记录是根据`id`（也就是主键）的顺序排列的，所以根据这些并不连续的`id`值到聚簇索引中访问完整的用户记录可能分布在不同的数据页中，这样读取完整的用户记录可能要访问更多的数据页，这种读取方式我们也可以称为`随机I/O`。一般情况下，顺序I/O比随机I/O的性能高很多，所以步骤1的执行可能很快，而步骤2就慢一些。所以这个使用索引`idx_name_birthday_phone_number`的查询有这么两个特点：

- 会使用到两个`B+`树索引，一个二级索引，一个聚簇索引。
- 访问二级索引使用`顺序I/O`，访问聚簇索引使用`随机I/O`。

需要回表的记录越多，使用二级索引的性能就越低，甚至让某些查询宁愿使用全表扫描也不使用`二级索引`。比方说`name`值在`Asa`～`Barlow`之间的用户记录数量占全部记录数量90%以上，那么如果使用`idx_name_birthday_phone_number`索引的话，有90%多的`id`值需要回表，这不是吃力不讨好么，还不如直接去扫描聚簇索引（也就是全表扫描）。

那什么时候采用全表扫描的方式，什么时候使用采用`二级索引 + 回表`的方式去执行查询呢？这个就是传说中的查询优化器做的工作，查询优化器会事先对表中的记录计算一些统计数据，然后再利用这些统计数据根据查询的条件来计算一下需要回表的记录数，需要回表的记录数越多，就越倾向于使用全表扫描，反之倾向于使用`二级索引 + 回表`的方式。当然优化器做的分析工作不仅仅是这么简单，但是大致上是个这个过程。一般情况下，限制查询获取较少的记录数会让优化器更倾向于选择使用`二级索引 + 回表`的方式进行查询，因为回表的记录越少，性能提升就越高，比方说上边的查询可以改写成这样：

```mysql
SELECT * FROM person_info WHERE name > 'Asa' AND name < 'Barlow' LIMIT 10;
```

添加了`LIMIT 10`的查询更容易让优化器采用`二级索引 + 回表`的方式进行查询。

对于有排序需求的查询，上边讨论的采用`全表扫描`还是`二级索引 + 回表`的方式进行查询的条件也是成立的，比方说下边这个查询：

```mysql
SELECT * FROM person_info ORDER BY name, birthday, phone_number;
```

由于查询列表是`*`，所以如果使用二级索引进行排序的话，需要把排序完的二级索引记录全部进行回表操作，这样操作的成本还不如直接遍历聚簇索引然后再进行文件排序（`filesort`）低，所以优化器会倾向于使用`全表扫描`的方式执行查询。如果我们加了`LIMIT`子句，比如这样：

```mysql
SELECT * FROM person_info ORDER BY name, birthday, phone_number LIMIT 10;
```

这样需要回表的记录特别少，优化器就会倾向于使用`二级索引 + 回表`的方式执行查询。

**注意：解决回表问题，覆盖索引是一种很好的解决方式！！**



### 建立索引的参考原则

- 只为用于搜索、排序或分组的列创建索引
- 当列的选择性较低时不建议建立索引。列的选择性指的是不重复的列与表记录的比值。可以用这种方式计算

```mysql
SELECT count(DISTINCT(列名))/count(*) AS Selectivity FROM 表名;
```

显然选择性的取值范围为(0, 1]，选择性越高的索引价值越大，这是由B+Tree的性质决定的。假设某个列的选择性为0，就是所有记录在该列中的值都一样，那为该列建立索引是没有用的，因为所有值都一样就无法排序，无法进行快速查找了～ 而且如果某个建立了二级索引的列的重复值特别多，那么使用这个二级索引查出的记录还可能要做回表操作，这样性能损耗就更大了。所以结论就是：**最好为那些列的选择性大的列建立索引，为选择性太小列的建立索引效果可能不好。**

- 索引列的类型尽量小。如果我们想要对某个整数列建立索引的话，在表示的整数范围允许的情况下，尽量让索引列使用较小的类型，比如我们能使用`INT`就不要使用`BIGINT`，能使用`MEDIUMINT`就不要使用`INT`～ 这是因为：
  - 数据类型越小，在查询时进行的比较操作越快（这是CPU层次的东西）
  - 数据类型越小，索引占用的存储空间就越少，在一个数据页内就可以放下更多的记录，从而减少磁盘`I/O`带来的性能损耗，也就意味着可以把更多的数据页缓存在内存中，从而加快读写效率。
  - 这个建议对于表的主键来说更加适用，因为不仅是聚簇索引中会存储主键值，其他所有的二级索引的节点处都会存储一份记录的主键值，如果主键适用更小的数据类型，也就意味着节省更多的存储空间和更高效的`I/O`。



### 主键插入顺序

我们知道，对于一个使用`InnoDB`存储引擎的表来说，在我们没有显式的创建索引时，表中的数据实际上都是存储在`聚簇索引`的叶子节点的。而记录又是存储在数据页中的，数据页和记录又是按照记录主键值从小到大的顺序进行排序，所以如果我们插入的记录的主键值是依次增大的话，那我们每插满一个数据页就换到下一个数据页继续插，而如果我们插入的主键值忽大忽小的话，这就比较麻烦了，假设某个数据页存储的记录已经满了，它存储的主键值在`1~100`之间：

{% asset_img 10.png %}

如果此时再插入一条主键值为`9`的记录，那它插入的位置就如下图：

{% asset_img 11.png %}

可这个数据页已经满了啊，再插进来咋办呢？我们需要把当前页面分裂成两个页面，把本页中的一些记录移动到新创建的这个页中。页面分裂和记录移位意味着什么？意味着：性能损耗！所以如果我们想尽量避免这样无谓的性能损耗，最好让插入的记录的主键值依次递增，这样就不会发生这样的性能损耗了。所以我们建议：**让主键具有`AUTO_INCREMENT`，让存储引擎自己为表生成主键，而不是我们手动插入** 

