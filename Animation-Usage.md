## ReactNative Animation



### InteractionManager

> 这里提及InteractionManager是因为InteractionManager的使用可以帮助我们实现更好的动画交互

***

#### 几种动画交互优化的实现

* InteractionManager //通常用于场景切换时的过场动画和异步数据加载
* requestAnimationFrame(): 用来执行在一段时间内控制视图动画的代码
* setImmediate/setTimeout/setInterval(): 在稍后执行代码。注意这有可能会延迟当前正在进行的动画。
* runAfterInteractions(): 在稍后执行代码，不会延迟当前进行的动画。

> 第三种实测无太大影响，但尽量避免并行动画时使用setTimeout(由于其不确定性，有可能导致动画执行顺序更改)

1、用法

```
InteractionManager.runAfterInteractions(() => {
  // ...耗时较长的同步的任务...
});

var handle = InteractionManager.createInteractionHandle();
// 执行动画... (`runAfterInteractions`中的任务现在开始排队等候)
// 在动画完成之后 执行动画的方法机会写在这个中间就行了
InteractionManager.clearInteractionHandle(handle);
// 在所有句柄都清除之后，现在开始依序执行队列中的任务
```
> runAfterInteractions接受一个普通的回调函数，或是一个PromiseTask对象，该对象需要带有名为gen的方法，并返回一个Promise。如果提供的参数是一个PromiseTask， 那么即便它是异步的它也会阻塞任务队列，直到它（以及它所有的依赖任务，哪怕这些依赖任务也是异步的）执行完毕后，才会执行下一个任务。

>默认情况下，排队的任务会在一次setImmediate方法中依序批量执行。如果你调用了setDeadLine方法并设定了一个正整数值，则任务只会在设定的时间到达后开始执行。在此之前，任务会通过setTimeout来挂起并阻塞其他任务执行，这样可以给诸如触摸交互一类的事件留出时间，使应用可以更快地响应用户。


* runAfterInteractions(callback: Function) 安排一个函数在所有的交互和动画完成之后运行。返回一个可取消的promise。
* createInteractionHandle() 通知管理器有某个动画或者交互开始了。
* clearInteractionHandle(handle: Handle) 通知管理器有某个动画或者交互已经结束了。
* setDeadline(deadline: number) 如果设定了一个正整数值，则会使用setTimeout来挂起所有尚未执行的任务。在eventLoopRunningTime到达设定时间后，才开始使用一个setImmediate方法来批量执行所有任务。

2、属性

* Events
* addListener

```
// jest中的示例用法，自己并未具体实现
beforeEach(() => {
	jest.resetModules();
	InteractionManager = require('InteractionManager');
		
	interactionStart = jest.fn();
	interactionComplete = jest.fn();
		
	InteractionManager.addListener(
	  InteractionManager.Events.interactionStart,
	  interactionStart
	);
	InteractionManager.addListener(
	  InteractionManager.Events.interactionComplete,
	  interactionComplete
	);
});


```


### Animations

1、 LayoutAnimation 

> LayoutAnimation 神器😁。 允许你在全局范围内创建和更新动画，这些动画会在下一次渲染或布局周期运行

```
//android 
UIManager.setLayoutAnimationEnabledExperimental && UIManager.setLayoutAnimationEnabledExperimental(true);
//在需要的地方设置当前页面动画
LayoutAnimation.configureNext(LayoutAnimation.Presets.spring);
```
* easeInEaseOut //缓入缓出
* linear //线性
* spring //弹跳
* easeIn //缓入
* easeOut //缓出
* keyboard //键入

> 也可以自定义动画效果

2、Animated.Value

> 最基本的一个动画使用方式是创建一个Animated.Value,将该动画绑定到一个或者多个样式属性到动画组件中，然后通过开启动画运行

```
this.state = {bounceValue: new Animated.Value(0),}
Animated.spring(
  this.state.bounceValue,
  {
    toValue: 0.8,
    friction: 1
  }
).start();
```
// 东西略多。。。

[0](http://reactnative.cn/docs/0.41/animations.html)

[1](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8Banimated%E5%8A%A8%E7%94%BB%E5%BA%93%E8%AF%A6%E8%A7%A3-%E5%9F%BA%E7%A1%80%E7%AF%8764/)

[2](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8Banimated%E5%8A%A8%E7%94%BB%E5%BA%93%E8%AF%A6%E8%A7%A3-%E5%AE%9E%E4%BE%8B%E7%AF%8765/)