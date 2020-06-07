---
title: hexo博客如何迁移至其它电脑
date: 2019-07-20 19:06:35
categories:
- 教程
tags:
- hexo
- 个人博客
mathjax: true
---

​		当你在一台电脑上写博客写的蛮爽的，但突然有一天你的电脑坏了或者被偷了咋办？你那些本地的博客可咋整！一想到这种有可能发生的危险，我就夜不能寐呀。于是我就开始疯狂地查找资料，但网上的资料良莠不齐。于是本人就自己慢慢的摸索嘛，这是一个程序员最基本的素质(其实还是自己太菜了)。下面我就把我的总结写一下，希望能对以后的自己和大家有帮助。

# 1、在新电脑上安装NodeJs和Git

​		这一步很简单，具体怎么安装，可以参见我的上一篇博文：利用hexo框架搭建个人博客(手把手教学)

# 2、GitHub新建分支

​		在pringce.github.io仓库下：

{% asset_img 20190720191707.png %}

这里的分支名可以任意设置。

# 3、设置分支为默认仓库

当前仓库->Settings->Branches->Default Branch

{% asset_img 20190720192207.png %}

# 4、clone至本地

​		将该分支克隆至本地

```c++
git clone https://github.com/pringce/pringce.github.io.git
```

​		cd进入 clone 下来的pringce.github.io文件夹，在此文件夹目录下执行git branch命令，应该是新建的分支名 。

# 5、新电脑生成ssh key并添加至GitHub

​	依次按照下述步骤生成本机ssh key：

1. ```java
   git config --global user.email "GitHub注册并验证时的邮箱"
   ```

2. ```java
   git config --global user.name "GitHub用户名"
   ```

3. ```java
   ssh-keygen // 一直按enter就行
   ```

4. 在本机目录“C:\Users\10530\\.ssh”会生成两个文件：id_rsa(私钥)和id_rsa.pub(公钥)。打开id_rsa.pub，复制里面的内容到GitHub:

   Settings -> SSH and GPG keys -> (填写)Title -> (粘贴)Key -> Add SSH Key

   {% asset_img 20190720193605.png %}

5. 测试是否成功：

   ```java
   ssh -T git@github.com
   ```

   **输出 You’ve successfully authenticated 表示添加key 成功。**

# 6、安装hexo

```java
npm install hexo-cli -g
```

# 7、将原博客文件拷贝至pringce.github.io文件夹

​		将原博客文件夹下的文件拷贝至pringce.github.io，需要拷贝6个：_config.yml（站点配置文件）、themes\（主题）、source\（博客源文件）、scaffolds\（文章的模板）、package.json、.gitignore。

{% asset_img 20190720194114.png %}

**此时，我们不需要再用hexo init来生成博客了，因为需要的文件我们已经拷贝过来了。**

# 8、提交本地至new分支

​		进入pringce.github.io文件夹，依次执行

1. ```java
   git add .
   ```

2. ```
   git commit -m "新电脑"
   ```

3. ```
   git push
   ```

   **这样，master分支用于保存博客静态资源，提供博客页面供人访问；new分支用于备份博客部署文件，供自己修改和更新，两者在一个GitHub仓库内互不冲突。**

# 9、安装hexo依赖的包

```java
npm install
```

​	所依赖的包都在上一步中的package.json备份文件里，所以直接这一个命令就可以了，就可以把你以前配置的那些包都安装上了。

# 10、更新博客

现在，在GitHub上的仓库就有两个分支，一个new分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美( •̀ ω •́ )y！

现在我们既可以在旧电脑上更新博客，也可以在新电脑上更新博客了。

在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：

- 首先git pull下拉，更新合并本地内容
- 本地修改编辑博客
- 依次执行git add .、git commit -m "..."、git push指令将改动推送到GitHub（此时当前分支应为new）；
- 然后才执行hexo g -d发布网站到master分支上。

虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。



具体步骤如下所示：

1. ```java
   git pull
   ```

2. ```
   hexo new "name"
   ```

3. ```
   hexo clean
   ```

4. ```
   hexo g
   ```

5. ```
   hexo s(测试时候在localhost看一下)
   ```

6. ```
   hexo d(部署)
   ```

7. ```
   git add .
   ```

8. ```
   git commit -m "新电脑"
   ```

9. ```
   git push //保证new分支版本为最新
   ```

   



**本地资料丢失后的流程**

如果此时你的电脑全坏了，并且你的本地博客资料全部丢失了，那么该怎么办呢？

- 获得新电脑的ssh key，然后添加到GitHub
- 将new分支clone至本地：

```
git clone https://github.com/pringce/pringce.github.io.git

```

- 在本地新拷贝的pringce.github.io文件夹下通过Git bash依次执行下列指令，安装hexo依赖的包（如果是整个电脑丢失了，还需要提前安装好NodeJS、Git和hexo）

```
npm install
```

所依赖的包都在新拷贝的package.json文件里，所以直接这一个命令就可以了，就可以把你以前配置的那些包都安装上了

- 然后就可以根据上述步骤10里面的流程更新博客了



好啦，心理的最大隐患解除了，可以安心睡觉啦。大家晚安！！