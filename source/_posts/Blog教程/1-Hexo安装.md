---
title: Hexo安装 # 代码演示名称
date: 2022-04-21 22:48:20 # 代码演示创建时间
description: 详解Hexo的安装步骤 # 描述，用来做主页文章内容节选
cover: "./img/MD2.png"
categories: Blog
tags: 
    - Hexo
    - 教程
    - 安装
---

# Hexo安装

## 前提：安装node.js，git，npm（基本安装和使用就不介绍了，不懂的可网上专门学习安装和使用）

### 一、安装并检查hexo

```
#安装hexo
npm install hexo -g

#检查hexo版本
hexo -v
```

### 二、初始化hexo文件

```
#初始化命令
hexo init
```

### 三、启动hexo服务

```
#启动服务
hexo server或者hexo s

#退出服务
 ctrl + c 
```

### 四、常用hexo命令如下

```
#清楚缓存文件，重新启动使用
hexo clean

# 本地启动预览
hexo server/s

# 生成静态文件：public文件夹，这个文件夹就是我们托管到码云上用的文件夹
hexo generate/g

#打包上传到git
hexo d
```

### 五、生成SSH添加到GitHub（第五节要用鼠标右键里的”Git Bash Here“来输入命令行）

```
# "yourname"
git config --global user.name "123456"
# "youremail"
git config --global user.email "123456@qq.com"
```

这里的yourname输入你的GitHub的用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对:

```
git config user.name
git config user.email
```

然后创建SSH,会询问一堆询问，直接一路回车即可。

```
ssh-keygen -t rsa -C "840658280@qq.com"
```

这时会在“C:\Users\陈勇\”文件夹下生成“.ssh”的文件夹，找到文件夹内“id_rsa.pub”文件里的内容就是公钥。（如果要第二次使用生成公钥，要先将“.ssh”文件夹删掉再重新生成）

接着将公钥添加到Github上：“Settings”里找到“SSH and GPG keys”,点击“New SSH key”,将公钥复制进去，保存即可。

最后，在gitbash中，查看是否成功（会有一个询问，输入yes）

```
ssh -T git@github.com
```

### 六、在hexo根目录下安装hexo-deployer-git，并打包发版到github

```
npm install hexo-deployer-git --save
hexo clean
hexo g
hexo d
```

### 七、仓储命名注意点

在GIthub上建的用来做Github的仓储必须要命名后缀必须是“.github.io”。例如“cyblog.github.io”

### 八、设置github pages

进入博客的仓储，“Settings”里点击“Pages”,配置“Source”里的分支路径，保存。便会在上方绿框内生成一个Github pages的url连接。

### 九、绑定github pages

这时候如果直接访问github pages的url，会发现发布到github上的博客没有样式，原因是hexo根目录的“_config.yaml”文件里url没有修改成github pages的地址。

```
url: https://cywarrior666.github.io/cyblog.github.io
```

### 十、Hexo源代码托管备份（第十节也要用鼠标右键里的”Git Bash Here“来输入命令行）

源代码托管备份非常重要，如果源文件损坏可以从代码托管上再重新拉取下来。和hexo d的部署有本质上的不同，要区分开来，不能混淆。

操作前如果不放心可以先在本地新建个文件夹把Hexo源代码文件全都复制黏贴本地备份一下。

1、在github上新建存储库（如：cyblogcode）。

2、在本地Hexo源文件根目录下先新建个”README.md“文件，打开”Git Bash Here“按github上”…or create a new repository on the command line“输入以下命令：

```
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/CYwarrior666/cyblogcode.git
git push -u origin main
```

3、然后就可以用git进行提交、推送、拉取、同步。（如何用git提交、推送、拉取、同步，这里就不介绍了，不懂的可网上查找资料学习）

4、源代码要克隆下来时候，要”npm install“一下，会出现俩个报错：

```
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@2.3.2 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
```

原因是这个包是mac上可以选择安装的依赖，但是你使用的是window的电脑也安装了该依赖。

解决方案：可以看看你的”package.json“和”package-lock.json“文件中是不是有”fsevents“的相关依赖，全部删除就好；其实这个警告是因为mac才需要这个包，但是你是在windows环境下，所以可以忽略这个警告。

5、再重新在本地运行一下，验证是否能可以正常打开即可

```
hexo clean
hexo s
```

6、第一次 Hexo s 时候会有报错，如下：

```
(node:18216) Warning: Accessing non-existent property 'column' of module exports inside circular depen
(node:18216) Warning: Accessing non-existent property 'filename' of module exports inside circular dep
(node:18216) Warning: Accessing non-existent property 'lineno' of module exports inside circular depen
(node:18216) Warning: Accessing non-existent property 'column' of module exports inside circular depen
(node:18216) Warning: Accessing non-existent property 'filename' of module exports inside circular dep
```

引起报错是stylus的问题，说node版本太高，要降低版本，解决方案如下：

找到”node_modules\stylus\lib\nodes\index.js“文件，加上一下代码：

```
exports.lineno = null;
exports.column = null;
exports.filename = null;
```

成功解决~