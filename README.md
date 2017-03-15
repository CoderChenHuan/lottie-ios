Lottie 应用于iOS、MacOS(以及[Android](https://github.com/airbnb/lottie-android) 和[React Native](https://github.com/airbnb/lottie-react-native))
------
* 本文由CRAnimation团队翻译
* 本项目iOS版本原地址：[airbnb/lottie-ios](https://github.com/airbnb/lottie-ios)
* 本文译文地址：[CRAnimation/lottie-ios](https://github.com/CRAnimation/lottie-ios)
* 翻译：小9
* 校正：熊熊
* QQ群：547897182（iOS动效特工队）

Lottie 是一个可应用于Andriod和iOS的动画库，它通过[bodymovin](https://github.com/bodymovin/bodymovin)插件来解析[Adobe After Effects ](http://www.adobe.com/products/aftereffects.html)动画并导出为json文件，继而在手机上通过React Native加载该json文件从而渲染出矢量动画。

这是前所未有的方式，设计师可以创作并且运行优美的动画而不需要工程师煞费苦心地通过手动调整的方式来重现动画。由于动画是通过json来加载的，使得动画源文件只需占用极小的空间就能完成相当复杂的效果！Lottie可以用于播放动画、调整尺寸、循环播放、加速、减速、甚至是精致的交互。

Lottie 也创造性地支持原生的UIViewController 的转场动画。

这里仅仅列出了强大的Lottie一些小小的例子：

![](https://github.com/airbnb/lottie-ios/raw/master/_Gifs/Examples1.gif)
![](https://github.com/airbnb/lottie-ios/raw/master/_Gifs/Examples2.gif)
![](https://github.com/airbnb/lottie-ios/raw/master/_Gifs/Community%202_3.gif)
![](https://github.com/airbnb/lottie-ios/raw/master/_Gifs/Examples3.gif)
![](https://github.com/airbnb/lottie-ios/raw/master/_Gifs/Examples4.gif)

##使用Lottie

Lottie支持iOS 8 及其以上系统。当我们把动画需要的images资源文件添加到Xcode的目标工程的后，Lottie 就可以通过JSON文件或者包含了JSON文件的URL来加载动画。

最简单的方式是用LOTAnimationView来使用它
```
LOTAnimationView *animation = [LOTAnimationView animationNamed:@"Lottie"];
[self.view addSubview:animation];
[animation playWithCompletion:^(BOOL animationFinished) {
  // Do Something
}];
```
如果你使用到了多个bundle文件，你可以这么做：
```
LOTAnimationView *animation = [LOTAnimationView animationNamed:@"Lottie" inBundle:[NSBundle YOUR_BUNDLE]];
[self.view addSubview:animation];
[animation playWithCompletion:^(BOOL animationFinished) {
  // Do Something
}];
```
或者你可以用代码通过NSURL来加载
```
LOTAnimationView *animation = [[LOTAnimationView alloc] initWithContentsOfURL:[NSURL URLWithString:URL]];
[self.view addSubview:animation];
```
Lottie 支持iOS中的`UIViewContentModes`的 aspectFit, aspectFill 和 scaleFill这些属性。

你可以控制动画的进度
```
CGPoint translation = [gesture getTranslationInView:self.view];
CGFloat progress = translation.y / self.view.bounds.size.height;
animationView.animationProgress = progress;
```
 ////////想要任意视图来给Lottie View中的动画图层做遮罩吗 ？只要你知道After Effects中对应的图层的名字，那就是小菜一碟的事了：
```
UIView *snapshot = [self.view snapshotViewAfterScreenUpdates:YES];
[lottieAnimation addSubview:snapshot toLayerNamed:@"AfterEffectsLayerName"];
```
Lottie 带有一个`UIViewController`动画控制器，可以用来自定义转场动画！
```
#pragma mark -- View Controller Transitioning

- (id<UIViewControllerAnimatedTransitioning>)animationControllerForPresentedController:(UIViewController *)presented
                                                                  presentingController:(UIViewController *)presenting
                                                                      sourceController:(UIViewController *)source {
  LOTAnimationTransitionController *animationController = [[LOTAnimationTransitionController alloc] initWithAnimationNamed:@"vcTransition1"
                                                                                                          fromLayerNamed:@"outLayer"
                                                                                                            toLayerNamed:@"inLayer"];
  return animationController;
}

- (id<UIViewControllerAnimatedTransitioning>)animationControllerForDismissedController:(UIViewController *)dismissed {
  LOTAnimationTransitionController *animationController = [[LOTAnimationTransitionController alloc] initWithAnimationNamed:@"vcTransition2"
                                                                                                          fromLayerNamed:@"outLayer"
                                                                                                            toLayerNamed:@"inLayer"];
  return animationController;
}
```
如果你的动画会很频繁地重用，`LOTAnimationView`有一个内置的LRU缓存策略。

##Swift 支持
----
Lottie 在Swift下也表现的很好！在你的swift 类上方简单地`improt Lottie`，然后就可以按照下面的方式使用Lottie了：
```
let animationView = LOTAnimationView.animationNamed("hamburger")
self.view.addSubview(animationView!)

animationView?.play(completion: { (finished) in
  // Do Something
})
```
##支持的After Effects 的特性
-----
###关键帧插值（Keyframe Interpolation）
- 线性插值(Linear Interpolation)
- 贝塞尔插值(Bezier Interpolation)
- 冻结关键帧插值(Hold Interpolation)
- 浮动关键帧插值(Rove Across Time)
- 空间贝塞尔曲线(Spatial Bezier)

###固体(Solids)
----------
- 锚点变换(Transform Anchor Point)
- 位置变换(Transform Position)
- 缩放变换(Transform Scale)
- 旋转变换(Transform Rotation)
- 透明度变换(Transform Opacity)

###遮罩(Masks)
-----
- 路径(Path)
- 透明度(Opacity)
- 多个遮罩(叠加的) [Multiple Masks (additive)]

###轨道遮罩(Track Mattes)
-----
- Alpha蒙版(Alpha Matte)

###父子级关系(Parenting)
-----
- 多个父子级关系(Multiple Parenting)
- 无父子级关系(Nulls)

###形状图层(Shape Layers)
-----
- 锚点(Anchor Point)
- 位置(Position)
- 缩放(Scale)
- 旋转(Rotation)
- 透明度(Opacity)
- 路径(Path)
- 组变换(锚点、位置、比例等) [Group Transforms (Anchor point, position, scale etc)]
- 矩形(所有属性) [Rectangle (All properties)]
- 椭圆(所有属性）[Elipse (All properties)]
- 多条路径的组合 (Multiple paths in one group)

####描边(shape layer) ［Stroke (shape layer)］
-----
- 描边颜色(Stroke Color)
- 描边透明度(Stroke Opacity)
- 描边宽度(Stroke Width)
- 描边接头样式(Line Cap)
- 虚线(Dashes)

####填充(shape layer)
-----
- 填充颜色(Fill Color)
- 填充透明度(Fill Opacity)

####裁切路径(shape layer) [Trim Paths (shape layer)]
-----
- 裁切路径的起始点(Trim Paths Start)
- 裁切路径的终点(Trim Paths End)
- 裁切路径的偏移(Trim Paths Offset)

####图层特性(Layer Features)
-----
- 图层组合(Precomps)
- 图像图层(Image Layers)
- 形状图层(Shape Layers)
- 空图层(Null Layers)
- 固体图层(Solid Layers)
- 父子级关系图层Layers(Parenting Layers)
- Alpha蒙板图层(Alpha Matte Layers)

##目前还不支持的After Effects 特性
-----
- 奇偶绕组路径(Even-Odd winding paths)
- 合并形状(Merge Shapes)
- 裁切路径中的个别裁切形状功能(Trim Shapes Individually feature of Trim Paths)
- 表达式(Expressions)
- 3D图层支持(3d Layer support)
- 渐变(Gradients)
- 多边形形状（有一种临时方案是通过转换为矢量路径来解决）[Polystar shapes (Can convert to vector path as a workaround)]
- 反相Alpha蒙板(Alpha inverted mask)

##安装Lottie
-----
###CocoaPods
在你的podfile中添加：
`pod 'lottie-ios'`

运行

`pod install`

###Carthage

安装Carthage([](https://github.com/Carthage/Carthage)) 向你的Cartfile添加Lottie：
`github "airbnb/lottie-ios" "master"`
运行
`carthage update`

##试试看
-----
Lottie入驻了Cocoapods！通过Cocoapod获取或克隆这个仓库,下载完成后导入Lottie`#import <Lottie / Lottie.h>`，并尝试使用示例代码。

用Carthage尝试。 在应用程序目标“常规”选项卡部分，从Carthage新生成的Carthage / Build / iOS目录中将lottie-ios.framework 拖放“Linked Frameworks and Libraries”下。

##社区贡献
- [Xamarin bindings](https://github.com/martijn00/LottieXamarin)
- [NativeScript bindings](https://github.com/bradmartin/nativescript-lottie)
- [Appcelerator Titanium bindings](https://github.com/m1ga/ti.animation)
- 由 [Alex Pawlowski](https://github.com/pawlowskialex)添加的对MacOS的支持

##替补方案
-----
1. 手动地创建动画。手动创建动画对于设计师以及iOS、Android工程师而言意味着付出巨额的时间。通常很难，即使花费这么多时间来开发动画也几乎很难达到理想的效果。
2. [Facebook Keyframes](https://github.com/facebookincubator/Keyframes)。 Keyframes是专门用来构建用户界面的， 是FaceBook的一个很棒，很新的库。但是Keyframes不支持一些Lottie所能支持的特性，比如： 遮罩，蒙版，裁切路径，虚线样式还有很多。
3. Gifs。Gifs 占用的大小是bodymovin生成的JSON大小的2倍还多，并且渲染的尺寸是固定的，并不能放大来适应更大更高分辨率的屏幕。
4. Png序列桢动画。 Png序列桢动画 甚至比gifs更糟糕，它们的文件大小通常是 bodymovin json文件大小的30-50倍，并且也不能被放大。

##为什么叫Lottie？
Lottie以德国电影导演(剪影动画的最早的开拓者)命名。 她最着名的电影是“阿赫迈德王子历险记”（1926年） - 最古老的长篇动画电影。比华特迪士尼的长篇电影白雪公主和七个小矮人（1937）早十多年[The art of Lotte Reineger](https://www.youtube.com/watch?v=LvU55CUw5Ck&feature=youtu.be)

##贡献

贡献者是非常受欢迎的。 只需上传项目并且包含您的更改说明。
如果你想添加更多的JSON文件，随意这样做！

#####问题或者功能需求？
可以在github的issues专栏上提出任何超乎预期崩溃问题。如果After Effects 文件并没有起作用的话，请在issue上附上它。没有源文件来排除故障是相当困难的。

##技术路线(没有特别的顺序)
- 添加了对交互转场动画的支持
- 添加了对父类自动添加子类，移动／缩放的支持
- 通过代码来改变动画的行为
- 动画 断点/查找点
- 渐变
- LOTAniamtedButton
- 重复创建对象机制
