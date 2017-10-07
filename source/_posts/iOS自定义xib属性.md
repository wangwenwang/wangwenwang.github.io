---
title: iOS自定义xib属性
date: 2017-04-12 16:43:59
tags: iOS
---

XIB和Storyboard提供操作View的属性还是很少，例如XIB中不支持view设置圆角（Key Path非可视化），而iOS8中为我们带来了新的可能。


### Objective-C IBDesignable/IBInspectable

```
#import <UIKit/UIKit.h>
@interface IB_UIView : UIView 
@property (nonatomic, assign)IBInspectable CGFloat cornerRadius; 
@end

```


```
IB_DESIGNABLE
@implementation IB_UIView

- (void)setCornerRadius:(CGFloat)cornerRadius {
    _cornerRadius = cornerRadius;
    self.layer.cornerRadius  = _cornerRadius;
    self.layer.masksToBounds = YES;
}
@end

```


### Swift IBDesignable/IBInspectable

```
import UIKit
@IBDesignable class IB_UIView: UIView {

    @IBInspectable var cornerRadius: CGFloat
}

```

效果如下：

![LM](/images/BBD09B73-E90B-4943-8E2F-AA6CCAD492C3.png)
