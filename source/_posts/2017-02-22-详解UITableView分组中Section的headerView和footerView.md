title: 详解UITableView分组中Section的headerView和footerView
date: 2017-02-22 13:02:00
tags: [UITableView,section,header,footer,原创]
---

首先我们直接填充分组数据,看看默认的分组样式是什么样的
![](http://images.wgq.name/tableSectionHeaderFooterView1.png)

图中可以看到分组数据1和2之间有个空白的隔断,但是分组1和tableview的headerview之间没有任何间隔,是连在一起的,这样的话我们根本不清楚section的header和footer到底是哪部份.我们知道uitableView的协议方法可以自定义header和footer的视图,通过改变颜色来看看到底是怎么回事(为了看的更清楚,多加一组数据进去)

```
-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    UIView *headView = [[UIView alloc] init];
    headView.backgroundColor = [UIColor orangeColor];
    return headView;
}

-(UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
{
    UIView *headView = [[UIView alloc] init];
    headView.backgroundColor = [UIColor blueColor];
    return headView;
}
```

效果:

![](http://images.wgq.name/tableSectionHeaderFooterView2.png)

可以看出,之前数据1和数据2之间的空白隔断是由两部份组成的,分别是section的header和footer各占一半,
由图中可以看出另外一个很重要的信息,那么就是第一个分组数据是没有header的,那么如果我们想让第一个分组数据也有headerView怎么办?简单,指定header的高度就行了,我们测试一下把header高度定为40  
```
-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    return 40;
}
```

![](http://images.wgq.name/tableSectionHeaderFooterView3.png)

look,第一个分组数据的header也出来了,所以如果我们想把footer隐藏不就很简单了,把高度footer的高度设置为0不就搞定了,靠,我真聪明,动手

```
-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
    return 0;
}
```

运行看看.....
哇靠,什么情况?没有任何变化??
这里其实是个坑,理论上确实是指定为0就可以把footer隐藏,但是xcode却在这里扭捏了一把,你直接指定为0的话他是用默认高度的,所以你要指定为一个非0值就行了,比如0.01,再试试

```
-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
    return 0.01;
}
```

完美!

![](http://images.wgq.name/tableSectionHeaderFooterView4.png)

最后有个tips,你在-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section协议方法中创建header视图时可以指定frame,里面包含了高度,如果你没有实现heightForHeaderInSection协议方法时这里的高度是有效的,但是当你实现了这个协议方法后,该协议方法返会的高度优先级最高

### 原创文章,转载请注明出处,谢谢! ###
