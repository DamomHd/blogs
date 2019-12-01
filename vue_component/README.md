# 一个鲜为人知的高性能组件注册及实现组件排序技巧
## 背景
在使用Vue的路途中，你一定知道如何去注册并调用一个组件

通常我们会通过三个步骤来实现调用组件的一整个流程


* 通过**import**引入组件
* 在父组件的组件对象**components**中将导入的子组件注册
* 在父组件中使用该组件

```
<template>
  <div>
    <Child msg="Hello World！"/>
  </div>
</template>

<script>
import Child from "../components/Child";

export default {
  name: "page",
  components: {
    Child
  }
};
</script>
```

这可能是80%Vue友最常见的一个使用子组件的模式，没错，我也经常这么干...如果你用腻了这种方式，想换个新奇的，接着往下走，我们再学习两个不常用的但又很高效的使用方式，提升开发逼格。


## 场景预设1
想象一下一个会员中心组件，有一部分区域用来展示会员与会员差异的相关展示模块，但是这两个只会展示其中一个，会员与非会员。假设我们现在有**MemberInfo**以及**UserInfo**两个组件,我们想根据用户身份进行对应模块显示

### 实现方案1
这可能是大部分前端玩家会想到的实现方式，包括我自己也常常会如此(捂脸)

```
<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <UserInfo v-if="!isMember"/>
    <MemberInfo v-else/>
  </div>
</template>

<script>
import UserInfo from "../components/UserInfo";
import MemberInfo from "../components/MemberInfo";

export default {
  name: "user",
  data() {
    return {
      isMember: false
    };
  },
  components: {
    UserInfo,
    MemberInfo
  }
};
</script>
```
![](https://user-gold-cdn.xitu.io/2019/12/1/16ec08ef54e24ae8?w=500&h=717&f=gif&s=54088)

### 实现方案2
通过阅读官方文档，我们会发现Vue有提供一个内置组件 component ，渲染一个“元组件”为动态组件。依 <span style="color:red">**is**</span> 的值，来决定哪个组件被渲染。如果你对此还不太了解，文末会奉上官网传送门.

```
<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <component :is="userComponentName" title="component就是好用哟"/>
  </div>
</template>

<script>
import UserInfo from "../components/UserInfo";
import MemberInfo from "../components/MemberInfo";

export default {
  name: "userComponent",
  data() {
    return {
      isMember: false
    };
  },
  components: {
    UserInfo,
    MemberInfo
  },
  computed: {
    userComponentName() {
      let { isMember } = this;
      return isMember ? "member-info" : "user-info";
    }
  }
};
</script>
```

![](https://user-gold-cdn.xitu.io/2019/12/1/16ec08f5e5683d9a?w=500&h=717&f=gif&s=68093)

从方案1过渡到方案2 ，很明显我们通过计算属性配合内置组件**component**完美剔除了**v-if/v-else**的逻辑判断，也可以正常做数据传递

只是从现在看来，除了使用方法改变了，没有啥差异了。这并不是本次想介绍的。现有的两个模块组件，我们仍然必须先导入并注册才能完成调用。现在，我们就不想提前注册好所需使用的子组件，因为太麻烦了并且浪费性能，我们想尝试动态导入注册使用,让我们继续往下，冲冲冲！

### 实现方案3 
```
<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <component :is="userComponentImstance" title="component就是好用哟"/>
  </div>
</template>

<script>
export default {
  name: "userImport",
  data() {
    return {
      isMember: false
    };
  },
  computed: {
    userComponentImstance() {
      let { isMember } = this;
      let pathName = isMember ? "MemberInfo" : "UserInfo";
      //通过import动态导入组件 配合webpack实现组件分离
      return () => import(`../components/${pathName}`);
    }
  }
};
</script>
```

可以看到，我们不再是提前导入注册组件的形式来调用了，**component**直接给其提供一个完整的组件对象
通过阅读官方文档我们可以看到 is接收两种选项之一：
* **注册的组件名称**
* **组件对象**

现在，我们通过动态**import**的形式导入了子组件，配合**computed**按条件渲染对应的子组件，谁用谁知道。只有在程序运行的时候才会进行，就省去了我们预先导入和注册组件的形式，这样的效果是不是跟使用第三方组件的时候按需加载有个似曾相识的味道..嗯是的，包括我们为了对SPA性能优化的时候router懒加载有着异曲同工之妙。

当我们改变**isMember**这个变量就可以实现动态切换组件了。这样做的好处在于，当我们使用动态导入的时候，**webpack**会将与导入函数匹配的每个文件单独创建一个**chunk**，也就是我们常说的分包加载，而不会一次性加载全部组件。当前样例并不能看出具体有多大的性能提升，但实际开发中，这个优势会非常明显。

如果你熟悉**webpack**你可以很好的利用这点对代码进行拆分和DIY属于自己程序的一个加载策略。具体可以通过[https://webpack.js.org/guides/code-splitting/]()**了解Code Splitting**

## 场景预设2
之前遇到过一个需求，就是给商城首页的几个活动模块根据后端返回顺序依次渲染，当时没想明白怎么实现。而现在，当你看到这里的时候，我想你大概有一点点小思路了吧。(如果没有，那一定是我的错，文章写的不够清晰)。现在知道了内置组件**component**以及我们的动态加载，我们完全可以很轻松的实现。

假设我们父组件为Home主页面，可能存在**Seckill**秒杀、**Group**团购、**Limit**限购、**Bargain**砍价四个活动模块,模块展示及排序均由数据决定，而不再是我们**hard coded**

```
<template>
  <div>
    <p>假装这是一个商城首页</p>
    <button @click="shuffle">随机切换模块顺序</button>
    <component :is="item.imstance" v-for="(item ,i) in componentImstances" :info="item" :key="i"/>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      moduleList: [
        {
          type: "Bargain",
          title: "砍价",
          startTime: "2019-12-01",
          endTime: "2019-01-01"
        },
        {
          type: "Seckill",

          title: "秒杀",
          startTime: "2019-12-05",
          endTime: "2019-01-01"
        },
        {
          type: "Limit",

          title: "限购",
          startTime: "2019-12-07",
          endTime: "2019-01-01"
        },
        {
          type: "Group",
          title: "团购",
          startTime: "2019-12-11",
          endTime: "2019-01-01"
        }
      ]
    };
  },
  components: {},
  methods: {
    shuffle() {
      let { moduleList } = this;
      let resultArr = [];
      while (moduleList.length > 0) {
        var index = Math.floor(Math.random() * moduleList.length);
        resultArr.push(moduleList[index]);
        moduleList.splice(index, 1);
      }
      this.moduleList = resultArr;
    }
  },
  computed: {
    componentImstances() {
      let { moduleList } = this;
      return moduleList.map(item => {
        item.imstance = () => import(`../components/${item.type}`);
        return item;
      });
    }
  }
};
</script>
```

![](https://user-gold-cdn.xitu.io/2019/12/1/16ec090660b6cfe2?w=500&h=717&f=gif&s=146656)

这样就实现了我们的活动模块的自定义排序了，但是目前我们的模块动态导入是根据后端返回数据来加载的，没有人会知道中间会不会出现什么幺蛾子，为了避免动态导入的组件在未知情况下加载失败，我们可以去做一个异常模板提示。
将上面中的**componentImstances**稍作修改：
```
computed: {
    componentImstances() {
      let { moduleList } = this;
      return moduleList.map(item => {
        item.imstance = () => {
          return new Promise((resove, reject) => {
            let imstance = import(`../components/${item.type}`);
            imstance.then(res => {
              resove(res);
            });
            imstance.catch(e => {
              resove(import("../components/Error"));
            });
          });
        };
        return item;
      });
    }
  }
```

![](https://user-gold-cdn.xitu.io/2019/12/1/16ec090954b667c9?w=500&h=717&f=gif&s=137472)
我们将动态导入异常的组件捕获并输出默认模板，用户体验嘛肯定是你的夙愿

参考：

[Vue内置组件](https://cn.vuejs.org/v2/api/#component)

[webpack](https://www.webpackjs.com/)



## 总结
灵活运用component以及import，如果一个组件有许多不同的试图，那这是很有用的，易于扩展。因为它是异步的，仅在需要的时候才加载模板，使代码更加轻量。题外话，component配合keep-alive来缓存组件，效果也是顶呱呱噢，本文不再赘述。相关演示源码已更新到[Github传送门](https://github.com/VincentHhd/blogs/tree/master/vue_component)。

## ❤️ 最后

**如果你觉得这篇内容对你挺有启发：**

分享出去让更多的人也能看到这篇内容（欢迎**点赞**和**关注**😊）

关注公众号「**前端公虾米**」，每周分享前端干货


![](https://user-gold-cdn.xitu.io/2019/12/1/16ec08e38d55c89c?w=1500&h=1500&f=png&s=323590)



