## 介绍

**[TestMyipa](https://1ili.github.io/TestMyipa/)** 是我自制的一个基于 GitHub Page 和 jekyll 的测试下载平台（对，你没有看错，就是那套常用来搭博客的工具）

主要是想用于 iOS 的包管理，对 $299 账号或者那 100 台设备提供一个类似于 蒲公英/fir.im 那样的下载界面，方便下载测试使用。<br/>如果不是这两家要实名认证（拿着身份证拍照的那种）才能上传 ipa 文件，我也不会心血来潮的做这个 =_=

有了这个，你可以不需受托管平台的约束、也不需要有自己的服务器，一切数据的存储与下载都可以交给 GitHub。当然图标和 ipa资源文件也可不上传至 GitHub ，iOS 仅验证 plist 所在位置是否启用SSL，其余也可放到自己的服务器上，只要 plist 写好引用路径即可。

## 效果

![result](https://raw.githubusercontent.com/1ilI/TestMyipa/master/resource/images/result.gif)

## 使用

1. 使用 Xcode 打好 Development 包，获取 ipa 文件
2. 把 ipa 文件以及项目的图标上传到 GitHub 并获取地址
3. 根据上述地址填写相关信息，生成 `manifast.plis` 文件（这个文件也可由第一步打包时填写生成）
4. 把 `manifast.plis` 文件上传，获取其地址
5. 生成最后 iPhone 能用的下载地址，格式如下： <br/>
itms-services://?action=download-manifest&url=`manifest.plist的地址`
6. 最后使用 GitHub Page 引用这个地址，写成一个页面，手机端 safari 浏览器打开，点击安装

若在打包时勾选了 **Incloud manifest for over-the-air installtion** 的话，会让你输入 ipa的下载地址、图标的地址等信息

![xcode-step](https://raw.githubusercontent.com/1ilI/TestMyipa/master/resource/images/xcode-step.png)

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

## 最后

本站使用了 [Cayman Blog](https://github.com/lorepirri/cayman-blog) 主题，在此感谢～

关于使用 GitHub + jekyll 搭建博客的，网上有许多教程，在此就不赘述了。

我也搭了一个，地址在这：[https://1ili.github.io](https://1ili.github.io) (｡ì _ í｡)
