title: ios7的UINavigationViewController转场动画
date: 2014-08-13 10:26:49
tags: objc
---
ios7中的UINavigationController新增加了两个协议方法  

```objective-c  
- (id <UIViewControllerAnimatedTransitioning>)navigationController:(UINavigationController *)navigationController
                                   animationControllerForOperation:(UINavigationControllerOperation)operation
                                                fromViewController:(UIViewController *)fromVC
                                                  toViewController:(UIViewController *)toVC


- (id <UIViewControllerInteractiveTransitioning>)navigationController:(UINavigationController *)navigationController
                          interactionControllerForAnimationController:(id <UIViewControllerAnimatedTransitioning>) animationController 
```


其中第一个协议方法是实现自动动画时用到的,第二个是用来实现互动动画的协议方法,第二个的优先级比第一个高,就是说你同时实现这两个协议方法的情况下,只有第二个互动动画效果有效

<!--more-->

这两个协议方法要求的返回实现指定协议的对象,因此我们可以创建一个类用来实现转场动画,比如创建一个叫HATransitionController的类,很明显你要实现上面两个协议中的其中一个,或者两个都实现也可以,所以类声明的时候要加上协议
@interface HATransitionController : NSObject  <UIViewControllerAnimatedTransitioning, UIViewControllerInteractiveTransitioning, UIGestureRecognizerDelegate>


这个类本身也可以定义一些协议方法,这样互动性会变的很强.先看看UIViewControllerAnimatedTransitioning这个协议要实现哪些方法
只有两个  

  
` - (NSTimeInterval)transitionDuration:(id <UIViewControllerContextTransitioning>)transitionContext;
- (void)animateTransition:(id <UIViewControllerContextTransitioning>)transitionContext;`
 

很明细第一个方法肯定就是返回动画的时长.
第二就是实现动画效果,有一个参数transitionContext,这个参数包含了一个容器(UIView),这个容器包含了转场涉及到的两个viewController,可以通过下面这种方式获取到两个vc
先获取容器

 
`UIView *containerView = [transitionContext
                             containerView];`



通过容器获取两个vc  
`    UIViewController *fromViewController = [transitionContext
                                            viewControllerForKey:UITransitionContextFromViewControllerKey
                                            ];
    UIViewController *toViewController = [transitionContext
                                          viewControllerForKey:UITransitionContextToViewControllerKey];
`
只有就可以通过这两个vc做你想要的动画效果.
