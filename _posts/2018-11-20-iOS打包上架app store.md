--- 
layout: post
title: 'IOS Xcode打包'
date: 2018-11-20
author: GorgeousNi
color: RGB(117,190,210)
cover: '/assets/doc_img/xcode.jpeg'
tags: IOS
---


# iOS Xcode打包 上架app store实践分享

网上的xcode打包ipa教程大多太旧而且又不完整，所以整理了一个最新的完整详细的xcode打包APP的图文教程分享；

1、真机测试调试APP（上架前一定要先测试好APP，要不app一堆bug，上架也审核不过，浪费时间）
2、上传APP到App Store审核

上架基本需求资料
* 苹果开发者账号（如还没账号先申请-苹果开发者账号申请教程）
*   备注 ：企业账号一般可用于内部测试开发，不用设备号即可安装，不可提交app store;公司账号用于提交app store 上架；
* 开发好的APP


分为5 步进行

1、申请iOS证书

2、导入证书到钥匙串

3、xcode配置iOS证书

4、配置xcode打包环境

5、打包并导出IPA包

三、xcode配置iOS证书和打包环境

1、用xocde打开你的项目，点击进入设置证书界面。

有两个地方都要设置

选择Code Signing下面的release（发布版）Debugs是测试版，上架App Store选择发布版的。

然后选择你刚上传的对应iOS发布证书

2、回到基本信息设置界面，Bundie 这项填写，最先创建的那个appid，跟创建iOS描述文件时选择的要一样。

现在下面还有个错误提示，因为还没有导入iOS描述文件。

3、选择xcode菜单栏如果图所示

4、把Archived修改为Release

5、点击选择设备，选择为打包设备。

四、项目打包IPA包导出


1、选择菜单栏如图所示，如果Archive还是灰色的，说明之前的配置没有生效，退出重新打开下。

点击Archive，开始打包。

2、打包进度条走完后，会弹出以下界面，点击Expcrt

