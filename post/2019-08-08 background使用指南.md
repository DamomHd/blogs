>微信公众号：**[前端公虾米]**

>关注`前端公虾米`。问题或建议，请公众号留言。

### 关于background的相关属性

>所有的浏览器都支持background属性 拿起了我多年前用过的xmind导出了张属性说明图 


![](https://user-gold-cdn.xitu.io/2019/9/3/16cf2bb34675601a?w=3551&h=2619&f=png&s=737808)

>试用模式，我太难了，公众号回复【**background**】获取原图

![](https://user-gold-cdn.xitu.io/2019/8/30/16ce1d6b13176b40?w=440&h=440&f=jpeg&s=25291)
***

>下面的演示案例将直接使用`background`这个属性，不适用单属性设置，因为这个属性在浏览器的支持更好，更简洁（能少一行是一行emmmm...）

![](https://user-gold-cdn.xitu.io/2019/9/2/16cf264773a5cb57?w=1116&h=650&f=png&s=58532)
![](https://user-gold-cdn.xitu.io/2019/8/30/16ce1dc0b0de9bd5?w=440&h=440&f=jpeg&s=24174)

### 经典场景
> 奉上原图参照物 

![](https://user-gold-cdn.xitu.io/2019/8/30/16ce1ef952f84e7d?w=550&h=352&f=jpeg&s=41204)

（图源/网络）
 #### 居中

![](https://user-gold-cdn.xitu.io/2019/8/30/16ce1f770849b62d?w=1254&h=414&f=png&s=660191)

### 不重复平铺 顺时针八个点
![](https://user-gold-cdn.xitu.io/2019/8/30/16ce20955968b5e4?w=1679&h=841&f=png&s=169295)

```!
注：其中的`left top right bottom`为`background-position`的属性,可以设置为px 或者百分比
```


### 重复平铺  

```
//x轴重复平铺
background: transparent url(./5.jpg) repeat-x
```
![](https://user-gold-cdn.xitu.io/2019/8/31/16ce566d22ab4c5a?w=1244&h=400&f=png&s=96599)

### 背景图指定大小
在日常开发中，我们常常需要对背景图进行一个尺寸的缩放，上面的常规操作是无法满足我们的需求的。
如果是多行属性的操作中，我们可能是这样写：

```
background: transparent url(./5.jpg) no-repeat bottom left;
background-size:150px auto;
```

![](https://user-gold-cdn.xitu.io/2019/9/2/16cf27b394084144?w=421&h=414&f=png&s=31526)

而在单行属性声明中，我们仅需这样写：

```
background: transparent url(./5.jpg) no-repeat bottom left/150px;
```
也可以达到一样的效果
![](https://user-gold-cdn.xitu.io/2019/9/2/16cf27c1d6995a68?w=410&h=416&f=png&s=29288)

><span style="color:red;">注意事项:</span>`bg-size`必须跟在`bg-position`后并以'<span style="color:red;">/</span>'分隔，如`'center/100px'`

### 关于bg-clip与bg-origin的属性说明
>`background-origin`:背景图的原点位置的背景相对区域
>`background-clip`:背景图是否延伸到边框下面

>在单行`background`属性声明中，由于两者属性值相同，在仅出现了一个属性值的时候，他同时为 `background-origin`和`background-clip`设定属性，如果出现了两次，则分别给`background-origin`、`background-clip`设定.



![](https://user-gold-cdn.xitu.io/2019/9/8/16d0f16041fba5dc?w=1012&h=845&f=png&s=489041)

><span style="color:red;font-weight:bold;">小结：</span>

* `background-origin`决定了背景图原点从哪开始(**起始点**)
* `background-clip`决定了背景图的绘制到边框外沿、padding外延或者是content外延终止(**终止点**)
* **从我个人的理解上，起始点和终止点的对角线连线即为背景图可绘制的最大区域**
* 当使用 `background-attachment` 为`fixed`时，`background-origin`属性将被忽略不起作用。
* **`background-clip`可以将背景图设置为文字的前景色**

![](https://user-gold-cdn.xitu.io/2019/9/16/16d37c5740da8f7d?w=648&h=140&f=png&s=11769)
![](https://user-gold-cdn.xitu.io/2019/9/8/16d0f2137d09dca7?w=282&h=278&f=png&s=27644)

### background 多背景图片使用
CSS如下：
```
     background: url('./9.jpg') left center/100px no-repeat,
                url('./7.jpg') top center/100px no-repeat,
                url('./7.jpg') bottom center/100px no-repeat,
                url('./9.jpg') right center/100px no-repeat red;
```
效果图如下：

![](https://user-gold-cdn.xitu.io/2019/9/8/16d0f28b1620e141?w=287&h=288&f=png&s=37266)
> <span style="color:red;font-weight:bold;">注</span>：因为整个元素只能有一个背景色，所以在定义多背景图片的时候`bg-color`只能在**最后一个属性上声明**，否则会**导致**整个`background`属性声明失效；

>在多张背景图存在重叠的情况下，先声明的背景图优先级高
>CSS如下：

```
 background: url('./5.jpg') center/100px no-repeat,
                url('./7.jpg') top center/100px no-repeat,
                url('./4.jpg') bottom center/150px no-repeat,
                url('./7.jpg') bottom center no-repeat,
                url('./9.jpg') right center/100px no-repeat red;
```

![](https://user-gold-cdn.xitu.io/2019/9/8/16d0f32c53dea84c?w=881&h=179&f=png&s=25902)
效果图如下：
![](https://user-gold-cdn.xitu.io/2019/9/8/16d0f35683995bad?w=553&h=289&f=png&s=110448)

有关background的单行属性声明使用就讲到这里，后续会继续补充，写的不好的麻烦见谅...新手作者
# Object-fit实现img图片尺寸设置
本文主要针对的是background属性中的图片常规操作，讲到图片，这边也浅谈一下img的一个图片尺寸设定的小技巧，让我们的小伙伴们从

```
    display: inline-block;
    width: 100%;
    height: auto;
```
解脱出来
>CSS部分

![](https://user-gold-cdn.xitu.io/2019/9/16/16d37ecc8d87c8cc?w=1065&h=668&f=png&s=62764)
>HTML部分

![](https://user-gold-cdn.xitu.io/2019/9/16/16d37ed20c70950a?w=1113&h=462&f=png&s=59500)
>效果图

![](https://user-gold-cdn.xitu.io/2019/9/16/16d37ee2d67bf599?w=1087&h=329&f=png&s=305533)

该属性分别有`fill`、`contain`、`cover`、`none`、`scale-down`五个，从效果图上来看，应该很好理解，这边只对`scale-down`做一下说明。
>`scale-down`属性值的内容尺寸会与`none`或者`contain`其中一个一样，至于取决于哪个值，取决于`none`和`contain`哪个得到的内容尺寸更小一些。当前的none属性黑眼圈较大影响工作，所以选择了contain黑眼圈较小的来安慰自己


# 小结
本文仅仅是总结了下background在实际开发中使用背景图片的相关单行属性声明的使用技巧
希望我的内容能被大家喜欢，不足之处还望理解和希望大佬指出赐教。
谢谢大家！

如果你觉得这篇文章不错，请别忘记点个**赞**跟**关注**哦~😊

# 交流
微信公众号<span style="color:red">「前端公虾米」</span>，将不定时更新最新、实用的开发小技巧和相关前端干货
关注公众号，回复"**资料**"获取全套前端视频教程
![](https://user-gold-cdn.xitu.io/2019/9/16/16d380f57fc3ca55?w=900&h=500&f=png&s=73546)
