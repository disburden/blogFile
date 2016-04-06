title: UIMenuController使用方法
date: 2015-07-26 18:35:07
tags:
---
1UIMenuController 可以方便的生成弹出菜单,就是uitextview长按弹出的文本选择,复制,粘贴那种样式的菜单
用法  
```
        [self.btnContent becomeFirstResponder];  
        UIMenuController *menu = [UIMenuController sharedMenuController];    
        UIMenuItem *item1 = [[UIMenuItem alloc] initWithTitle:@"删除"  action:@selector(deleteMessage:)];    
        [menu setMenuItems:[[NSArray arrayWithObjects:item1, nil] mutableCopy]];    
        [menu setTargetRect:self.btnContent.frame inView:self.btnContent.superview];    
        [menu setMenuVisible:NO];    
        [menu setMenuVisible:YES animated:YES];   
        [self.btnContent resignFirstResponder];    
```
  
1.必须要把menu要弹出的view设置为第一响应者(有时候必须设置父视图为第一响应者,比如在cell中,如果你添加了button点击弹出菜单,那么你设置button为第一响应者无效时,就要设置cell本身为第一响应者)  
```  
    [self.btnContent becomeFirstResponder];
```

<!--more-->

2.可以自己添加任意的菜单项(参考例子中删除的添加)

3.设置菜单弹出  
```
        [menu setMenuVisible:NO];
        [menu setMenuVisible:YES animated:YES];
```
前面一句可以不用,(我加上这一句是因为不知道什么原因,在第一次弹出的时候位置不正确,第二次才会正常,所以加上这一句调整一下)

4.解决执行菜单项对应的方法代码时遇到的怪问题  
```
    [self.btnContent resignFirstResponder];
```
加上这一句的原因是遇到一个奇怪的现象,就是我例子中的item1对应的方法deleteMessage,执行的目的是让代理类执行一个删除uitabelview中一条cell的方法,当数据源移除了记录后,[tableview reloadData]方法无法正常执行,就是不会进入uitableview的那些协议方法.在stackoverfollow上找到了这个解决办法,具体的原因是什么不是很清楚,强力建议把这一句可以放在更合理的地方,就是deleteMessage方法中,在方法的最前面调用这个语句就行了

后来弄明白了,是由于我的button是继承UIButton自定义的一个按钮,里面有实现了类似的方法造成的问题,先放着给有遇到的人提个醒.

5.以上这些只是创建menu,但是执行时会发现不会弹出菜单,因为还有一些事情要处理

6.要让当前的view允许成为第一响应者,实现下面这个方法就行了  
```
    - (BOOL) canBecomeFirstResponder {
    return YES;
    }
```
7.前面添加菜单项的时候我们会设置点击该菜单中要执行的事件方法(本例中为deleteMessage:),你还要通过实现以下这个方法来管理是否允许执行该事件,同时也是通过是否允许执行该事件来确定弹出菜单时是否显示该菜单项  
```
    - (BOOL) canPerformAction:(SEL)selector withSender:(id) sender {
    if ( selector == @selector(deleteMessage:) ) {
        return YES;
    }
    return NO;
    }
```
新版的xcode方法名改为:-(BOOL)canPerformAction:(SEL)action withSender:(id)sender 第一个参数名称不一样


8.附:如果第7点中的方法不做判断,直接返回YES,会发现出现一大堆系统默认功能的菜单
这些菜单主要是UIResponderStandardEditActions协议中定义的那些功能

9.本例中添加了一个自定义的菜单项"删除",其实在UIResponderStandardEditActions协议中拷贝和删除都有了,可以直接用默认的就行了,按照第8条说的,只要找到拷贝和删除在协议中定义的方法名称,返回YES就会出现这两个菜单,要实现自己的拷贝和删除逻辑,可以重载这两个方法(在UITableViewCell中没成功)  
```
    -(void)copy:(id)sender
    -(void)delete:(id)sender
```
