自从跳槽以后，工作上接触 TS 也是越来越多，所以对 TS 关注也是有所增加。社会上有种效应叫做“视网膜效应”，说的是越关注什么就越出现什么，当你开始对某些方面增加关注时，相同的事物就会在你眼前不断出现。TS 对于近期的我而言，便是如此。

---
好了废话不多说，近期也是关注到 TypeScript4.3 发布了，简单给大家介绍下该版本。

当然，如果你还不清楚什么是 TypeScript，小编这里也不会科普。（因为我猜你打不到我 emmm...）感兴趣的小伙伴可以去官网 get 一下啦。

最新版的 TS4.3，需要使用或者更新的伙伴，直接通过 npm 或者 yarn 下载/更新即可。

接下来让我带着愉悦的心情，一起 see see Typescript4.3 给我们带来了啥新特性？你好奇吗？（小编写完了，所以不好奇了，小声 BB）

## 新特性预览

- 支持将属性单独读写指定类型
- 增加了关键字 overrride，以保证基础类中的方法不会被覆盖
- 模版字符串类型的改进
- 扩展了类中可被赋予#private #name 的元素，使它们在运行时能够真正私有化。除了属性以外，方法和访问器也可以被赋予私有名称。
- ConstructorParameters 类型帮助现在可以在抽象类中使用。
- 泛型的上下文范围得到缩小。
- Always-Truthy 检查
- static 静态索引签名
- .tsbuildinfo 文件大小优化减少，加快构建速度
- --incremental 和 --watch 中计算优化，提高编译速度
- 导入、导出语句优化
- 编辑器支持@link 标签
- 非 js 文件文件路径跳转，获取快速信息
- lib.d.ts 变更

下面简单聊聊其中几个变化。

### 增加了关键字 overrride

在扩展类时，我们很容易覆盖原有基础类的方法。
比如：

```typescript
class Animals {
  eat () {
    // ...
  }
  sleep () {
    // ...
  }
}

class Pig extends Animals {
  eat () {
    // ...
  }
  sleep () {
    // ...
  }
}
```

继承之后如果是这样的处理方案，无法知悉使用者是添加对应的新的方法亦或是覆盖现有基础类上的方法。这便是 Ts4.3 添加 override 关键字的原因。

```
class Pig extends Animals {
  override eat () {
    // ...
  }
  override sleep () {
    // ...
  }
}
```

当一个方法标记为 override 时，Ts 总是会确保基类中存在同名方法。

同时，Ts4.3 提供一个新的选项 --noImplicitOverride，开启此选项后，重写超类的任何方法将会抛错，除非显式使用关键字 override。

### .tsbuildinfo 文件大小优化

Ts4.3 中，.tsbuildinfo 文件的优化，归功于内部格式的优化，即创建使用了数字标示符的表，而不再是完整路径等来做处理。

该文件的优化减少体积，毋庸置疑也意味着构建速度的大大提高。

### 导入导出优化

在现有使用的版本里，我们知道导入的时候如果不写 from 路径的话很难为我们自动匹配可能需要导入的文件列表。

而在 Ts4.3 中，这一块做的更加智能了，哪怕你只是 coding 下 import 关键字，也会自动为你匹配可能需要导入的文件列表以及补全对应的文件路径。大大提升了开发者日常开发导入模块的痛点，可以在最新的 vs code 中去尝试了！为 Ts 团队点赞 👍。

### 支持 link 标签快速获取信息

Ts4.3 以后，将完全可以理解@link 标签，并尝试解析它们所链接到的生命，这将意味着我们可以直接通过悬停@link 标签来获得快速信息。在支持的编辑器里，也可以一键跳转到对应的函数声明中。将会是十分便捷的一项新功能。

### lib.d.ts 改变

兼容来删除没有浏览器实现的 api，虽然我们平常可能不一定用到。

- Account
- AssertionOptions
- RTCStatsEventInit
- MSGestureEvent
- DeviceLightEvent
- MSPointerEvent
- ServiceWorkerMessageEvent
- WebAuthentication
  以上的均在接下来的版本里从 lib.d.ts 中删除。

本文只是做一个简短的介绍，相关更加详尽的介绍还得靠各位德智体美劳优异的小朋友们。[传送门](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-3.html "TypeScript4.3")



---
据本台可靠消息，虽然 TypeScript4.3 刚发布，但是相关团队已经在开展 TpyeScript4.4 的工作了。就问问你还学的动么？
