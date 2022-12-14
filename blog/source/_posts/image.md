---
title: Typora+picGo+Github实现图片自动上云
date: 2022-12-1 10:36:15
tags: 学习
categories: 前端
top: 101
typora-copy-images-to: upload
---

### GitHub

首先在`GitHub`上创建一个仓库用来存放图片，需要注意仓库必须是公开的，不然别人无法访问到你的图片
<!--more-->
#### 在`GitHub`上生成一个`token`用来给`picGO`上传图片

点击在左上角的头像上悬浮鼠标找到下拉框中的`settings`菜单

![image-20221213142347612](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213142347612.png)

进去之后往下滑找到`Developer setting`菜单并点击![image-20221213142704198](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213142704198.png)

找到`Personal access tokens`下的`Tokens（classic）`菜单，然后点击`Cenerate new token`按钮

![image-20221213143024388](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213143024388.png)

随便写一个名字然后选中rope，滑倒最下方点击绿色的`Genderate toekn`按钮

![image-20221213143231493](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213143231493.png)

把token复制并保存，以为下次进来就看不到token的值了

![image-20221213143346949](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213143346949.png)

### PicGo客户都安（[PicGo下载地址](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FMolunerfinn%2Fpicgo%2Freleases)）

安装好了之后设置一下将GitHub设置为默认图床

![image-20221213143852918](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213143852918.png)

- 仓库名为：`GitHub用户名/仓库名`
- 分支名默认为主分支`master`
- token使用刚刚在GitHub上复制的token值
- 存储路径就是你的文件在GitHub上方照片的文件路径，可不填

### Typora设置

点击文件找到下拉菜单中的偏好设置

![image-20221213144346349](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213144346349.png)

勾上途中画圈的两个选项，然后点击验证图片上传选项那个按钮验证能否成功上传，Typora会上传一个图片，当返回成功的消息就好了。

![image-20221213145041883](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213145041883.png)

