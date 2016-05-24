---
title: 让Github项目支持CocoaPods
date: 2016-05-24 20:51:56
tags: CocoaPods
---

# 让Github项目支持CocoaPods

---

以LMProgressView为例

clone

```
$ git clone https://github.com/xxx

```
cd到clone的目录

```
$ cd LMProgressView

```


创建一个podspec文件

```
$ pod spec create LMProgressView

```

编辑 podspec文件，这里是用nano打开的

```
$ nano LMProgressView.podspec

```

创建之后会自动生成一个模板，粘贴一下自己的配置（这个文件很严格，不能出现中文）

```
Pod::Spec.new do |s|
  s.name         = 'LMProgressView'
  s.version      = '1.0.3'
  s.license      = 'MIT'
  s.summary      = 'This is a progress bar prompts, let the user know the background doing'
  s.homepage     = 'https://github.com/wangwenwang/LMProgressView'
  s.authors      = { 'wangwenwang' => '372266373@qq.com' }
  s.source       = { :git => 'https://github.com/wangwenwang/LMProgressView.git', :tag => '1.0.3' }
  s.requires_arc = true
  s.platform     = :ios
  s.source_files = 'Third/LMProgressView.{h,m}'
  s.framework    = 'Foundation', 'UIKit'
end

```

验证podspec文件

```
$ pod spec lint LMProgressView.podspec

```

![LM](/images/B54CF307-8C7A-45AA-B67D-12715FF8A1B1.png)

验证通过后，创建tag，推送到github

```
$ git add .
$ git commit -m "1.0.1"
$ git tag 1.0.1
$ git push --tags
$ git push origin master

```

如果是第一次提交，需要先注册（CocoPods会给你发邮件）

```
$ pod trunk register 邮箱 '名字' --description='描述'

```

提交到CocoaPods

```
$ pod trunk push LMProgressView.podspec

```

![LM](/images/77BF3F65-EE0A-4E4F-9C73-8F5C6CA6B4AB.png)

好了，最后放大招 pod search

