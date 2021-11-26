### 1. Git
Git是一个免费的、开源的分布式版本控制系统，可以快速高效的处理从小型到大型的各种项目
Git易于学习，占地面积小，性能极快。它具有廉价的本地库，方便的暂存区和多个工作流分支等特性。其性能优于Subversion、CVS、Perforce和ClearCase等版本控制工具。


### 2. Git工作机制
![在这里插入图片描述](https://img-blog.csdnimg.cn/d8c949b8aa9a4f5287c91a9593bb6b59.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_8,color_FFFFFF,t_70,g_se,x_16)
一旦提交到本地库，在代码无法删除。本地库通过```git pushhttps://git-scm.com/```推送到远程托管中心。

### 3 Git和代码托管中心

- 局域网
	- GitLab
- 互联网
	- GitHub(外网)
	- Gitee码云(国内网站)

### 4 Git 安装
 [Git官网下载](https://git-scm.com/)

查看GNU协议，可以直接点击下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ddae343b7ea4a2fbe98f5b0084dd629.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
选择 Git 安装位置，要求是非中文并且没有空格的目录，然后下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ecc00e1f4c04d8e946593fa93dbdff1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)

Git 选项配置，推荐默认设置，然后下一步。


![在这里插入图片描述](https://img-blog.csdnimg.cn/7b5c5933bbb4488681d81660a544fab3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
Git 安装目录名，不用修改，直接点击下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/f83096a30e364db98da67659e1e9bcd8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
Git 的默认编辑器，建议使用默认的 Vim 编辑器，然后点击下一步

![在这里插入图片描述](https://img-blog.csdnimg.cn/74d2cd0766054bc18032ab44e2c9309d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
默认分支名设置，选择让 Git 决定，分支名默认为 master，下一步

![在这里插入图片描述](https://img-blog.csdnimg.cn/4da04005dcf848f8a2098114f16e45cb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)

修改 Git 的环境变量，选第一个，不修改环境变量，只在 Git Bash 里使用 Git。

![在这里插入图片描述](https://img-blog.csdnimg.cn/047eea25b69140be993c99840dac26bf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
选择后台客户端连接协议，选默认值 OpenSSL，然后下一步。
![在这里插入图片描述](https://img-blog.csdnimg.cn/241f8257539a4a579cfeb55216c26696.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
配置 Git 文件的行末换行符，Windows 使用 CRLF，Linux 使用 LF，选择第一个自动转换，然后继续下一步。


![在这里插入图片描述](https://img-blog.csdnimg.cn/811a0f0f263c4e44b45f15d5fb34ea00.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
选择 Git 终端类型，选择默认的 Git Bash 终端，然后继续下一步。
![在这里插入图片描述](https://img-blog.csdnimg.cn/80d632a7278442fbbe489a8f592ada55.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
选择 Git pull 合并的模式，选择默认，然后下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/7410095fc8e54b9b9f38dd3ab54c3d27.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)

选择 Git 的凭据管理器，选择默认的跨平台的凭据管理器，然后下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8b7006884ef34544be8ed6143c313a70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
其他配置，选择默认设置，然后下一步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c5ef535758a4922941df9b8a25e6fb7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
实验室功能，技术还不成熟，有已知的 bug，不要勾选，然后点击右下角的 Install
按钮，开始安装 Git。
![在这里插入图片描述](https://img-blog.csdnimg.cn/eeaabce448b04abdb4a9b11fe8da97dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
点击 Finsh 按钮，Git 安装成功

![在这里插入图片描述](https://img-blog.csdnimg.cn/019474a0e0bf4ff2ac71a6d0818abcf5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)
右键任意位置，在右键菜单里选择 Git Bash Here 即可打开 Git Bash 命令行终端

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd4862f574ca40a5975203c4269584f5.png)
在 Git Bash 终端里输入 git --version 查看 git 版本，如图所示，说明 Git 安装成功。

![在这里插入图片描述](https://img-blog.csdnimg.cn/b4b70fdcf0a34f88821705dc2436301a.png)
### 5 Git常用命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/e74324710d2f451f858b9200844c39c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 设置用户签名

```git
git config --global user.name 用户名
git config --global user.email 邮箱
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d3afa56532cb435cb2cde10dc7738e9b.png)


说明：签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。Git 首次安装必须设置一下用户签名，否则无法提交代码。这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

#### 初始化本地库

```git
git init
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/78b651977abc4a6dadac93189fb4508b.png)
初始化成功后该目录生成```.git```文件夹

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee75660e4d374ebca3455bcea7d6a77a.png)
#### 查看本地仓库状态
```git
git status
```
首次查看（工作区没有任何文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/05f7c7ef38cd46faa37845c1f8426a13.png)
#### 新增文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/506a6205c169425d828fcf5b88a8dca5.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/80331a0285c341caa80692086ae88ab2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 再次查看（检测到未追踪文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/1a1e87324cae49f1aba83fd4cec10cf5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
红色文件名表示未添加至暂存区

#### 添加文件到暂存区

```git
git add 文件名
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/91b3403a66b147d8a76b7e4525ea8c49.png)
#### 查看状态（检测到暂存区有新文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/1c158ce353e54fed95e495539a89e7ca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
绿色文件名表示未提交到本地仓库

#### 将暂存区文件提交到本地仓库

```git
//日志信息不能少
git commit -m "日志信息" 文件名
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7fd0bb98f2334d1ca8095e9dee9461aa.png)
#### 查看状态（没有文件需要提交）
![在这里插入图片描述](https://img-blog.csdnimg.cn/618b6c6328114067a59ad96ec2f28f3e.png)
#### 修改文件（hello.txt）
![在这里插入图片描述](https://img-blog.csdnimg.cn/91880aeaf65c49dfa3f2f3c09bf80c20.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_14,color_FFFFFF,t_70,g_se,x_16)

#### 查看状态（检测到工作区有文件被修改）
![在这里插入图片描述](https://img-blog.csdnimg.cn/a3d2f94350094d81909c9a62fc2b9165.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 将修改的文件再次添加暂存区并查看状态（工作区的修改添加到了暂存区）
![在这里插入图片描述](https://img-blog.csdnimg.cn/1402e708864e496ead3814bdc53270b1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 再次提交到本地仓库并查看状态（没有文件需要提交）
![在这里插入图片描述](https://img-blog.csdnimg.cn/e03337aca1ee4510bedc18a722a98251.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 6 历史版本
#### 查看历史版本
```git
//查看版本信息
git reflog  
//查看详细版本信息
git log
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a867ae4227b44ea193042336b7a663e9.png)
前面7位为版本号前7位，```HEAD```表示当前指针位置，后面位版本提交时输入的日志信息

#### 版本穿梭
```git
git reset --hard 版本号前7位
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bd1d643b42264cc1b30a216a11e9c15b.png)
此时hello.txt已经回到没有添加1111111的第一个版本的时候
![在这里插入图片描述](https://img-blog.csdnimg.cn/46d240631d2043669d7de2127a4145ff.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
Git 切换版本，底层其实是移动的 HEAD 指针，具体原理如下图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/512b19dafc184188ab6fda88e22a69b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_17,color_FFFFFF,t_70,g_se,x_16)

### 7 Git分支操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/90055a52922a48e1bcaa97f8b53ce1ed.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

![在这里插入图片描述](https://img-blog.csdnimg.cn/1abd21c40520497cbc07079ad5a2d4f2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)
- 分支的好处
	- 同时并行推进多个功能开发，提高开发效率。
	- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/303647a303ae4d0d888900a01bf31d6c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 查看分支
```git
git branch -v
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e61202e6a49a4fd991ece0a5778ad5d8.png)
```*```代表当前所在分支

#### 创建分支
```git
git branch 分支名
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7faa6c049ecf437fbb6edaaea15db7d7.png)
#### 切换分支

```git
git checkout   分支名
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/471d122f73664b149a98dada25b749c4.png)
在```hot-fix```分支下修改hello.txt文件内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/b5ae02bbf5c1498b9953f2698bbbd0ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_11,color_FFFFFF,t_70,g_se,x_16)
在```hot-fix```分支下添加到暂存区，并提交到本地仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f6a53c60dcb44478db60168c421526f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
切换到master分支可以看到，hello.txt文件内容并没有被修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ff94aa05de1495cb5e85a1b908801bd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 无冲突合并分支
```git
git merge 分支名
```
在master分支上合并hot-fix分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/a4751f4f6c5043f1a6cfb35b8c362ba0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 有冲突合并分支
合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改。Git无法替我们决定用哪一个，必须人为决定。

冲突产生的表现：后面状态为 ```MERGING```

在master分支上修改hello.txt文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/97f5245efa5b4db98d4ebf920e636dee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_13,color_FFFFFF,t_70,g_se,x_16)
在master分支上提交到本地仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/78742c2fd0f44929b9f7b64ee211652d.png)
在hot-fix分支上修改hello.txt文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/2b6b164cadf845c4b810fb2daa1a4563.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_14,color_FFFFFF,t_70,g_se,x_16)
提交到本地仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/733e64ba0e4740b5a4920a23586a8ce9.png)

因为此时master和hot-fix修改了同一个问价的同一个位置，在master分支上合并hot-fix时产生冲突

![在这里插入图片描述](https://img-blog.csdnimg.cn/0d14d362b4104148ba360e570d39c653.png)
#### 解决冲突

直接通过```vim hello.txt```人为修改hello.txt文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/b76c1267083a4b55a074a5e0d9b9e6f4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
修改为
![在这里插入图片描述](https://img-blog.csdnimg.cn/77989e8b9592417a880ec9e427f75f11.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_15,color_FFFFFF,t_70,g_se,x_16)
合并成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/bccd1b845675434ca1b991e6b918e914.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 8 GitHub操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/bfeb6dd0bb6b4afc8b65d60c5a9b9cf9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)

- ```git clone 远程地址```会做如下操作：
	- 拉去代码
	- 初始化本地仓库
	- 创建别名， 默认```orign```

#### SSH免密登陆

在windows下的当前用户目录```C:\Users\Administrator```下打开打开git
执行```ssh-keygen -t rsa -C zf@fjnu.com```生成SSH密钥
执行```cat id_rsa.pub```查看公钥

复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys

![在这里插入图片描述](https://img-blog.csdnimg.cn/94c2dfcbe49a45798f8cabce259f5315.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c0dd6f7f92a43dfa1155819fd3fb992.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3480b5c19d8d4d41b9d16541fd042de0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcHVtcGtpbjk4NDE=,size_19,color_FFFFFF,t_70,g_se,x_16)
接下来再往远程仓库 push 东西的时候使用 SSH 连接就不需要登录了。
