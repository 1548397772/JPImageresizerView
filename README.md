# JPImageresizerView

[![CI Status](http://img.shields.io/travis/ZhouJianPing/JPImageresizerView.svg?style=flat)](https://travis-ci.org/ZhouJianPing/JPImageresizerView)
[![Version](https://img.shields.io/cocoapods/v/JPImageresizerView.svg?style=flat)](http://cocoapods.org/pods/JPImageresizerView)
[![License](https://img.shields.io/cocoapods/l/JPImageresizerView.svg?style=flat)](http://cocoapods.org/pods/JPImageresizerView)
[![Platform](https://img.shields.io/cocoapods/p/JPImageresizerView.svg?style=flat)](http://cocoapods.org/pods/JPImageresizerView)

## 简介

仿微信裁剪图片的一个裁剪小工具。

    目前功能：
        1.能自适应裁剪区域的缩放；
        2.高自由度的参数设定，包括裁剪区域的间距、裁剪宽高比、是否自适应缩放等；
        3.支持最多8个拖拽方向的裁剪区域；
        4.支持上左下右的旋转；
        5.水平和垂直的镜像翻转；
        6.两种边框样式；
        7.自定义遮罩颜色，或两种高斯模糊的遮罩

    注意：
        1.由于自动布局不利于手势控制，所以目前使用的是frame布局，暂不支持自动布局；
        2.目前仅支持竖屏操作。
        
    之后的更新内容：
        1.Swift版本；
        2.适配横竖屏切换；
        3.更多新的边框和遮罩样式；
        4.更多的参数设定；
        5.实现苹果自带的裁剪功能中的自由拖拽旋转方向的效果

![image](https://github.com/Rogue24/JPImageresizerView/raw/master/Cover/h05JLQ3kCA.gif)

## 0.3.0 更新内容

    1.修正旋转水平方向时自动整体自适应缩小的问题，现在为图片宽度比图片高度小时才自适应，也可以手动设定；
    2.新增锁定功能，裁剪区域可锁定，无法继续拖动；
    3.新增镜像功能，可进行垂直方向和水平方向镜像操作。

## 0.2.3 更新内容

    1.修复相册照片方向错乱的bug；
    2.修复水平方向边框点和线有所缩小的问题；
    3.更正属性的注释。

## 0.2.0 更新内容

    1.新增高斯模糊的遮罩样式；
    2.可设置动画曲线；
    3.可设置裁剪区域的内边距；
    4.新增JPImageresizerConfigure类，更加方便设定参数。

## 如何使用

#### 初始化
```ruby
// 方式一：使用工厂方法配置参数（裁剪的图片、frame、遮罩样式、边框样式、动画曲线、裁剪线颜色、背景色、遮罩透明度、垂直和水平的间距、裁剪的宽高比、裁剪区域的内边距、可否重置的回调）

JPImageresizerView *imageresizerView = [[JPImageresizerView alloc]
                                        initWithResizeImage:[UIImage imageNamed:@"Girl.jpg"]
                                        frame:frame
                                        maskType:JPConciseFrameType
                                        frameType:JPConciseFrameType
                                        animationCurve:JPAnimationCurveLinear
                                        strokeColor:[UIColor whiteColor]
                                        bgColor:[UIColor blackColor]
                                        maskAlpha:0.75
                                        verBaseMargin:10
                                        horBaseMargin:10
                                        resizeWHScale:0
                                        contentInsets:UIEdgeInsetsZero
                                        imageresizerIsCanRecovery:^(BOOL isCanRecovery) {
                                            // 可在这里监听到是否可以重置
                                            // 注意循环引用
                                        }];

// 方式二：使用JPImageresizerConfigure配置好参数再创建

JPImageresizerConfigure *configure = [JPImageresizerConfigure defaultConfigureWithResizeImage:image make:^(JPImageresizerConfigure *configure) {
    // 到这里已经有了默认参数值，可以在这里另外设置你想要的参数值（使用了链式编程方式）
    configure.jp_resizeImage([UIImage imageNamed:@"Kobe.jpg"]).
    jp_maskAlpha(0.5).
    jp_strokeColor([UIColor yellowColor]).
    jp_frameType(JPClassicFrameType).
    jp_contentInsets(contentInsets).
    jp_bgColor([UIColor orangeColor]).
    jp_isClockwiseRotation(YES).
    jp_animationCurve(JPAnimationCurveEaseOut);
}];

JPImageresizerView *imageresizerView = [JPImageresizerView imageresizerViewWithConfigure:self.configure imageresizerIsCanRecovery:^(BOOL isCanRecovery) {
    // 可在这里监听到是否可以重置
    // 注意循环引用
}];

// 添加到视图上
[self.view addSubview:imageresizerView];
self.imageresizerView = imageresizerView;

// 创建后也可以修改以上部分参数（除了maskType和contentInsets）
self.imageresizerView.resizeImage = [UIImage imageNamed:@"Kobe.jpg"];
self.imageresizerView.resizeWHScale = 16.0 / 9.0;
```
#### 更改边框样式
![image](https://github.com/Rogue24/JPImageresizerView/raw/master/Cover/JPConciseFrameTypeCover.jpeg)
![image](https://github.com/Rogue24/JPImageresizerView/raw/master/Cover/JPClassicFrameTypeCover.jpeg)

```ruby
// 目前只提供两种边框样式，分别是简洁样式JPConciseFrameType，和经典样式JPClassicFrameType
// 可在初始化或直接设置frameType属性来修改边框样式
self.imageresizerView.frameType = JPClassicFrameType;
```

#### 镜像翻转
![image](https://github.com/Rogue24/JPImageresizerView/raw/master/Cover/ggseHhuRnt.gif)

```ruby
// 垂直镜像，YES->沿着Y轴旋转180°，NO->还原
BOOL isVerticalityMirror = !self.imageresizerView.verticalityMirror;
[self.imageresizerView setVerticalityMirror:isVerticalityMirror animated:YES];

// 水平镜像，YES->沿着X轴旋转180°，NO->还原
BOOL isHorizontalMirror = !self.imageresizerView.horizontalMirror;
[self.imageresizerView setHorizontalMirror:isHorizontalMirror animated:YES];
```


#### 旋转
```ruby
// 默认逆时针旋转，旋转角度为90°
[self.imageresizerView rotation];

// 若需要顺时针旋转可设置isClockwiseRotation属性为YES
self.imageresizerView.isClockwiseRotation = YES;
```

#### 重置
```ruby
// 重置为初始状态，方向垂直向上
[self.imageresizerView recovery];
```

#### 裁剪
```ruby
// 裁剪过程是在子线程中执行，回调则切回主线程执行
// 调用可添加提示...
[self.imageresizerView imageresizerWithComplete:^(UIImage *resizeImage) {
    // 裁剪完成，resizeImage为裁剪后的图片
    // 注意循环引用
}];
```

#### 其他
```ruby
// 锁定裁剪区域，锁定后无法拖动裁剪区域，NO则解锁
self.imageresizerView.isLockResizeFrame = YES;

// 旋转至水平方向时是否自适应裁剪区域大小
// 当图片宽度比图片高度小时，该属性默认YES，可手动设为NO
self.imageresizerView.isAutoScale = NO;
```

## 安装

JPImageresizerView 可通过[CocoaPods](http://cocoapods.org)安装，只需添加下面一行到你的podfile：

```ruby
pod 'JPImageresizerView'
```

## 反馈地址

    邮箱：zhoujianping24@hotmail.com
    博客：https://www.jianshu.com/u/2edfbadd451c

## License

JPImageresizerView is available under the MIT license. See the LICENSE file for more info.

