---
title: cloudflare + V2rayN实现科学上网
date: 2024/05/27
author: 阿宽
tags: 
    - Cloudflare
    - V2rayN
    - Cloudflare Worker
    - Cloudflare Pages
---
## 前言:
免费在`Cloudflare`上面通过部署`Worker`或者`Pages`，来创建一个免费VLESS节点，看YouTuBe和Netfilex的4K视频甚至可以访问chatgpt和gemini等双向被墙的网页

1.zizifn 大佬的一个开源项目 edgetunnel ，Github项目地址：https://github.com/zizifn/edgetunnel

2.cmliu大佬在巨人的肩膀上二次创作，简化到通过上传`Pages`文件，GitHub 地址：https://github.com/cmliu/edgetunnel (墙裂建议此项)

3.v2rayN安装，添加并更新订阅，设置活跃服务器，科学浏览大功告成！

## 1.zizifn项目教程
优点：全过程自己参与，步骤清晰
缺点：步骤繁琐，网速不理想需要使用优选IP，在V2rayN更换IP

#### 1.1 注册Cloudflare账号
注册地址：https://dash.cloudflare.com/sign-up

#### 1.2 购买注册域名
2.1 建议直接在`Cloudflare`内的`域注册`-`注册域`购买一个域名，比如我的akuan.win,花了90元买了3年，好处是直接就可以在`Cloudflare`中进行托管
2.2 也可以在其他Domain域名网站购买，再托管到Cloudflare，注意的是，第一年超级便宜，后期renew续费很贵！！

#### 1.3 Worker 部署 VLESS
1.3.1 Cloudflare 首页，点击`Workers 和 Pages` - `创建应用程序` - 创建`Worker` - 自定义名称例如`vless`，然后点击`部署`
1.3.2 部署成功页面点击`编辑代码`，清空原有代码，复制 https://github.com/V2RaySSR/Free-VLESS/blob/main/worker-vless.js 中的代码到Cloudflare
1.3.3 在 https://1024tools.com/uuid 生成随机UUID，复制其中一行，例如:
```bash
2ed6baf1-1f05-4826-9122-b4bc1ee28585
```
修改代码中`let userID`的值
1.3.4 点击右上角蓝色的`部署`

#### 1.4 设置自定义域
1.4.1 返回Workers中`vless`项目里，点击`设置`-`触发器`-`添加自定义域`，输入没使用过的已经托管在CF上面的，二级域名(例如`vless.akuan.win`)
1.4.2 输入完成，若是输入框变为绿色，证明格式正确！点击添加自定义域！
1.4.3 域证书会显示：正在初始化，等待证书生效，或是直接访问 该域名，网页显示有内容，证明部署完毕！

#### 1.5获得订阅链接
浏览器访问`https://二级域名/UUID`，例如：`https://vless.akuan.win/2ed6baf1-1f05-4826-9122-b4bc1ee28585`,即可看到订阅地址
```bash
vless://2ed6baf1-1f05-4826-9122-b4bc1ee28585···com
```

## 2.cmliu项目教程(建议)
优点：简单，仅需上传Pages文件，V2rayN中自动优选IP

#### 2.1 下载源文件
下载作者的`Pages`文件：https://raw.githubusercontent.com/cmliu/edgetunnel/main/worker.zip

#### 2.2 Pages 部署 VLESS
Cloudflare 首页，点击`Workers 和 Pages`-`创建应用程序`-`Pages`-`上传资产` - 项目创建名字列如`vlesstest`，点击`创建项目` - 上传刚才下载下来的worker.zip - 点击`部署站点`

#### 2.3 添加UUID变量
回到`vlesstest`项目,找到`设置` – `环境变量` – `添加变量`，变量名称：`UUID` ，变量值为刚才生成的 `UUID` 或在前面1.3.3中 ，点击保存！

#### 2.4 重新部署 Pages
回到`vlesstest`项目，找到`部署`，点击下面的`创建新部署`，再次上传刚才的worker.zip文件，保存并部署
这样，我们点击上图中的 访问站点，若是有内容出现，证明部署成功

#### 2.5获得订阅链接
我们可以访问 https://域/UUID ，来查看我们的节点,例如`https://vlesstest-2n4.pages.dev/7a1a374f-286f-4807-9756-ee3f90227bb0`
参照1.4 加入一个自定义域，比如`https://vless.akuan.win`即可轻松访问,后面加入刚刚的UUID，例如：`https://vless.akuan.win/2ed6baf1-1f05-4826-9122-b4bc1ee28585`,即可看到vless://2ed6baf1-1f05-4826-9122-b4bc1ee28585···com的节点信息

## 3.v2rayN
windows: https://github.com/2dust/v2rayN/releases
安卓(v2rayNG): https://github.com/2dust/v2rayNG/releases ，不知道下载哪个时，请直接下载带 arm64-v8a.apk
IOS: 使用国外账号，App Store 搜 Shadowrocket

#### 3.1 下载
下载地址: https://github.com/2dust/v2rayN/releases ，第一次安装建议下载带内核版本`v2rayN-With-Core.zip`，并解压缩

#### 3.2 安装Microsoft .NET 8.0 Desktop Runtime
V2rayN的代码需要.NET 8.0以上支持，下载地址:https://dotnet.microsoft.com/zh-cn/download/dotnet/8.0

#### 3.3 添加订阅
管理员运行`v2rayN.exe` - `订阅分组` - `订阅分组设置` - `添加` - 别名随便填，可选地址为获得的订阅地址，例如：`vless://2ed6baf1-1f05-4826-9122-b4bc1ee28585···com` - `确定`

#### 3.4 连接节点
`订阅分组` - `更新全部订阅(不通过代理)` - `右键选择其中一个节点` - `设为活动服务器`或`选中按Enter键` - 最下方有个系统代理，选择`自动配置系统代理` - 路由选择 `V2-绕过大陆(Whitelist)`

### 脱离信息茧房，开启你的全球视野吧！！
