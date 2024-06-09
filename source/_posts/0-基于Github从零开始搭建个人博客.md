---
title: Hexo + Github Pages从零开始搭建个人博客
author: 阿宽
top: true
hide: false
tags: 
  - Github Pages
  - Hexo
  - Markdown
---
## 大概流程：安装node.js,git,hexo框架,github ssh密钥，github代码仓，在GitHub pages上搭建个人博客.以下以windows为例
### 1.安装Node.js

访问Node.js官方网站：https://nodejs.org/，下载适合您操作系统的安装包，并按照安装程序的指示完成安装。

完成后，您可以通过以下命令来验证 Node.js 和 npm（Node包管理器）的安装是否成功：
管理员权限打开 `cmd` 或者 `powershell` 
```bash
node -v
npm -v
```
这两个命令会输出已安装的 Node.js 和 npm 的版本号

### 2.安装Git

访问Git官方网站：https://git-scm.com/，载适合您操作系统的安装包，并按照安装程序的指示完成安装。
完成后，您可以通过以下命令来验证 Git 的安装是否成功：
```bash
git --version
```
该命令会输出已安装的Git版本号

### 3.安装hexo框架

```bash
npm install -g hexo-cli
```

### 4.新建一个空文件夹，创建一个新的Hexo项目

```bash
hexo init
npm install
```

### 5.生成静态文件

```bash
hexo g
# 或
hexo generate
```

### 6.启动本地服务预览

```bash
hexo s
# 或
hexo server
```

在浏览器输入：`127.0.0.1:4000`，即可看到：hexo 的默认 blog


### 7.变更默认的Hexo 主题

如果喜欢其他主题，可以在 https://hexo.io/themes/index.html 挑选更换。
下载好后放在```themes```文件夹内，同时在 ```_config.yml``` 中的theme: 更改主题名
这里采用了主体： https://blinkfox.github.io/2018/09/28/qian-duan/hexo-bo-ke-zhu-ti-zhi-hexo-theme-matery-de-jie-shao/

在根目录的`themes`目录下执行：
```bash
git clone https://github.com/blinkfox/hexo-theme-matery.git
```

原来有一个主题为：landscape，克隆完成后可以看到一个新的主题：hexo-theme-matery

### 8.修改 _config.yml

修改根目录下的配置文件 ```_config.yml``` ，替换：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape
```

变更为：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: hexo-theme-matery
```

#### 9.使主题生效

重新生成文件，并启动服务：

```bash
  hexo clean
  hexo g
  hexo s
```

再刷新浏览器可以看到崭新的页面：，如果没有变化ctrl+F5强制刷新

### 10.配置本地SSH Key

生成新的SSH Key
```bash
ssh-keygen -t rsa -C "your_email@youremail.com"
# "your_email@youremail.com"为可更改注释
```
按回车键后，会提示您输入保存文件的路径。默认情况下，它会保存在 C:\Users\YourUsername\.ssh\id_rsa。您可以直接按回车键使用默认路径。

再打开生成的公钥.pub 将内容复制到Github上去，或者输入下面命令获得刚刚生成的SSH Key
```bash
cat ~/.ssh/id_rsa.pub
```
### 11.把生成的SSH Key添加到github

Github账户，找到 ```settings ---> SSH and GPG keys```。new 一个新的SSH Key，并把刚刚生成的本地SSH Key复制进去

### 12.安装 hexo-deployer-git 部署插件

在根目录下执行(不是git bash里)：
```bash
npm install hexo-deployer-git --save
```
### 13.Github新建一个仓库(Create a new repository)

在Github新建一个仓库，仓库名例如`1234`，创建后会得到`SSH`值为 `git@github.com:likuan123/1234.git`

### 14.修改_config.yml

blog目录中的```_config.yml``` 文件的 `deploy` 项需要修改：
初始为
```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type:
```
增加或修改`repo`为 `SSH`值和`branch: master`,冒号后有空格，代码如下

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:likuan123/1234.git
  branch: master  
```
保存修改

### 15.部署到Github

```shell
hexo c
hexo g
# 部署到服务上
hexo d
```

注意，如果出现无法上传，根据报错内容，主要有2个原因：
1.22端口被封，此时把`repo`的值改为 GitHub 上的`http`值，再不行改端口映射
2.secret问题，点击报错链接，在跳转的页面选择其中某一项，关闭即可

然后再重新输入上方代码即可

### 16.尝试访问Github Pages

浏览器输入：https://likuan123.github.io/1234 ，(可能需要翻墙)，即可出现自己的博客了

### 17.GitHub Pages 网址到个人域名解析(墙内建议)

1.在Github `1234`项目 的`Settings -- pages -- Custom domain` --输入自己的二级域名例如`1234.akuan.win`

2.在域名托管网站，如`cloudflare`，DNS记录里，新建一个CNAME，名称1234，内容`likuan123.github.io`，保存

等几分钟既可以通过1234.akuan.win访问博客了

### 保存代码(可选)

将本地主目录推送到远程其他分支，保存代码。

因为发布的分支选的是master，所以每次发布都会影响，所以我们选择将所有的代码上传到其他分支。

比如我们新建一个分支 prd，以后都在prd分支上修改，新建文章，发布的时候会发布到master分支上，这样一来，编写与发布互不干扰了。



