---
layout: post
title:  "github利用coocapods创建iOS程序的依赖管理"
date:   2016-08-01 15:14:54
categories: cocoapods
comments: true
---

* content
{:toc}

## 1.创建项目
在github上创建自己的开源项目。此部分不是说明的重点，具体步骤略过。

## 2.添加cocospods支持
trunk需要CocoaPods 0.33版本以上，(现在到1.0.0版本了)。用pod --version命令查看版本，如果版本低，需要升级：

	sudo gen install cocoapods
	pod setup
	xxxxxx$ pod //可直查看帮助

未安装过的，具体安装过程可参考以下两个链接<br>
[http://www.jianshu.com/p/071d30a3af02](http://www.tuicool.com/articles/7VvuAr3)<br>
[http://www.tuicool.com/articles/7VvuAr3](http://www.tuicool.com/articles/7VvuAr3)


## 3.注册trunk
	pod trunk register xiaomk7758@sina.com 'MK Shaw' --description='' --verbose

把邮箱和名字以及描述替换成你的，加上--verbose可以输出详细debug信息，方便出错时查看。
注册后CocoaPods会给你的邮箱发送验证链接，点击后就注册成功了，可以用pod trunk me命令查看自己的注册信息。

	bogon:~ xiaomk$ pod trunk me
	- Name:     MK Shaw
	- Email:    xiaomk7758@sina.com
	- Since:    August 2nd, 21:14
	- Pods:     None
	- Sessions:
    - August 2nd, 21:14 - December 8th, 21:15. IP: 58.22.120.58 Description: MK

## 4.部署你的Pod
创建 podspec

	pod spec create MKActionSheet

会在当前目录下生成 MKActionSheet.podspec 文件，然后我们编辑这个文件。
生成的这个文件里有详细的配置说明。
在这里我贴一个`MJExtension`的配置。基本上参照生成文件里的注释看一下就懂了。

	Pod::Spec.new do |s|
		s.name         = "MJExtension"
		s.version      = "3.0.13"
		s.ios.deployment_target = '6.0'
		s.osx.deployment_target = '10.8'
		s.summary      = "A fast and convenient conversion between JSON and model"
		s.homepage     = "https://github.com/CoderMJLee/MJExtension"
		s.license      = "MIT"
		s.author             = { "MJLee" => "199109106@qq.com" }
		s.social_media_url   = "http://weibo.com/exceptions"
		s.source       = { :git => "https://github.com/CoderMJLee/MJExtension.git", :tag => s.version }
		s.source_files  = "MJExtension"
		s.requires_arc = true
	end

 配置完成之后，需要把你的源码push到github上，tag一个版本号并且发布一个release版本，这样podspec文件中的s.source的值才能是准确的。

## 5.提交
pod trunk push 命令会首先验证你本地的podspec文件(是否有错误)，之后会上传spec文件到trunk，最后会将你上传的podspec文件转换为需要的json文件
第一步验证podspec文件也可以自己去做 pod spec lint Peanut.podspec

## 6.补充 Claim your Pod
如果你之前提交过pod，那么trunk之后你需要去(Claim your Pod)认领
在这个页面: [https://trunk.cocoapods.org/claims/new](https://trunk.cocoapods.org/claims/new)
send之后就开始等待，官方提示是过了过渡期就你就可以提交新版本了


## 7.错误提示
如果出现

	source: The version should be included in the Git tag.

应该是因为下面这两行 的 版本数字不一致导致的

	s.version          = "0.1.0"
	s.source           = { :git => "http://gitlab...git", :tag => '0.1.1' }  

s.version 与 tag=>'0.1.1' 不一致。
可以直接   

	s.source           = { :git => "http://gitlab...git", :tag => s.version }  

## 8.完成后 pod search
完成后 pod search xxx 一直搜索不到自己的项目。（找了网上好多都是交重装cocoapods的，坑了我半小时）
最后清除下 缓存就OK了

	rm ~/Library/Caches/CocoaPods/search_index.json
	
后在一次输入：pod search xxxx 就找到自己的啦，
怎么使用coocapod 网上很多久不赘述了。


## end

参考链接：

[http://www.tuicool.com/articles/6FF7fi](http://www.tuicool.com/articles/6FF7fi)

[http://blog.csdn.net/skylin19840101/article/details/50426822](http://blog.csdn.net/skylin19840101/article/details/50426822)

[https://guides.cocoapods.org/making/private-cocoapods.html](https://guides.cocoapods.org/making/private-cocoapods.html)