## 起因

如果你有使用过 fir.im/蒲公英 等这类的应用托管平台，就会知道他们现在要求必须实名后才能上传应用包，比如说这样
![verified](https://raw.githubusercontent.com/1ilI/1ilI.github.io/master/resource/2018-04/verified.jpg)

然鹅我的个人账号没实名已经很久了= = ，最开始的时候这些平台都是注册即可使用的，然后到非实名认证后每日有下载次数限制，再到非实名认证无法上传应用包，没办法，要想使用它们终究得实名了。

但是，我的需求就只是想让那些有 UDID 的 100 台设备进行下载测试，也不给别人使用，再说了，为什么要把自己的私密信息交给他们呢，万一泄漏了呢（手动眼斜 → →）。其实技术上苹果这边是没有问题的，就差一个服务器存储相关资源和一个下载界面了，怎么办，自己动手搭呗。若是嫌麻烦的话还是用托管平台吧，不得不说 GitHub 真是个好东西，又自由又免费，还能实现这一需求，哈哈(￣▽￣)

## 介绍

**[TestMyipa](https://1ili.github.io/TestMyipa/)** 是我自制的一个基于 GitHub Page 和 jekyll 的测试下载平台（对，你没有看错，就是那套常用来搭博客的工具）

主要是想用于 iOS 的包管理，对 $299 账号或者那 100 台设备提供一个类似于 蒲公英/fir.im 那样的下载界面，方便下载测试使用。<br/>如果不是这两家要实名认证（拿着身份证拍照的那种）才能上传 ipa 文件，我也不会心血来潮的做这个 =_=

有了这个，你就可以不需受托管平台的约束、也不需要有自己的服务器，一切数据的存储与下载都可以交给 GitHub。当然图标和 ipa资源文件也可不上传至 GitHub ，iOS 仅验证 plist 所在位置是否启用SSL，其余也可放到自己的服务器上，只要 plist 写好引用路径即可。

## 效果

![result](https://raw.githubusercontent.com/1ilI/1ilI.github.io/master/resource/2018-04/result.gif)

## 使用

1. 使用 Xcode 打好 Development 包，获取 ipa 文件
2. 把 ipa 文件以及项目的图标上传到 GitHub 并获取地址
3. 根据上述地址填写相关信息，生成 `manifast.plis` 文件（这个文件也可由第一步打包时填写生成）
4. 把 `manifast.plis` 文件上传，获取其地址
5. 生成最后 iPhone 能用的下载地址，格式如下： <br/>
`itms-services://?action=download-manifest&url=manifest.plist的地址`
6. 最后使用 GitHub Page 引用这个地址，写成一个页面，手机端 safari 浏览器打开，点击安装

若在打包时勾选了 **Incloud manifest for over-the-air installtion** 的话，会让你输入 ipa的下载地址、图标的地址等信息，最后会生成 plist 文件，这样就不用自己手动新建啦

![xcode-step](https://raw.githubusercontent.com/1ilI/1ilI.github.io/master/resource/2018-04/xcode-step.png)

最后的 `manifast.plis` 文件，手写也行，生成的也可，其中内容可如下所示：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>
		<dict>
			<key>assets</key>
			<array>
				<dict>
					<key>kind</key>
					<string>software-package</string>
					<key>url</key>
					<string> ipa文件的地址 </string>
				</dict>
				<dict>
					<key>kind</key>
					<string>display-image</string>
					<key>url</key>
					<string> 图标地址（小图，可用一个） </string>
				</dict>
				<dict>
					<key>kind</key>
					<string>full-size-image</string>
					<key>url</key>
					<string> 图标地址（大图，可用一个） </string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string> Xcode上对应的 Bundle Identifier </string>
				<key>bundle-version</key>
				<string> Xcode上对应的 Version </string>
				<key>kind</key>
				<string>software</string>
				<key>title</key>
				<string> Xcode上对应的 Display Name </string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
```

我的实现方式是这样的：

<img alt="my-install" src="https://raw.githubusercontent.com/1ilI/1ilI.github.io/master/resource/2018-04/install-my.png" width="50%"/>

对应代码：
```md
## 安装方式

* 在 safari 浏览器中，[点击安装](itms-services://?action=download-manifest&url=https://raw.githubusercontent.com/1ilI/TestMyipa/master/resource/PunishmentAider/manifest.plist)

* 使用「相机」扫描二维码安装☟

<img alt="downloadImage" src="https://raw.githubusercontent.com/1ilI/TestMyipa/master/resource/PunishmentAider/download.png" width="50%"/>
```

>**关于最后一步，你可以直接 clone、下载或 fork 我这个仓库的代码 [https://github.com/1ilI/TestMyipa](https://github.com/1ilI/TestMyipa)，把里面的资源换成你自己的，主要是 `resource` 和 `_post` 这两个文件夹内的东西，然后部署上去，当然你也可自己找主题做这个界面。如果你觉得还不错，请给我个 start 以示鼓励吧，欢迎大家使用～**

## 附则：

今天才发现，邮箱里被 GitHub 警告了好多次(ｰ ｰ;)，大意就是说，他们虽然给你构建完页面了，但是检测到你好像用它来分发 ipa 包，他们强烈建议你用 GitHub 的方式分发应用包。
![github-warning](https://raw.githubusercontent.com/1ilI/1ilI.github.io/master/resource/2018-04/github-warning.jpg)

虽然页面什么的看起来都没有问题，但是每次上传部署后，GitHub 都会发一封这样的邮件给你，邮箱不得爆炸了 <br/> (╯°□°）╯︵ ┻━┻

最后解决方式是 再建一个仓库，不开 GitHub Pages，把之前的包资源文件，我放在 `resource` 这个文件夹里的东西，全都挪到新仓库里去，那里只存储所有的包资源，然后修改一下引用地址就 OK 啦。

## 最后

本站使用了 [Cayman Blog](https://github.com/lorepirri/cayman-blog) 主题，在此感谢～

关于使用 GitHub + jekyll 搭建博客的，网上有许多教程，在此就不赘述了。

我也搭了一个，地址在这：[https://1ili.github.io](https://1ili.github.io) (｡ì _ í｡)
