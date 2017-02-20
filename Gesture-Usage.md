## ReactNative GestureResponderSystem

### 一、创建方式
> 有两种使用方式, 重点记录PanResponder因为比较👍

View.props
    
```
// 定义手势
this._gestureHandlers = {
	onStartShouldSetResponderCapture: () => false,
	onMoveShouldSetResponderCapture: ()=> true,
	...
}

//调用
<View {...this._gestureHandlers} />

```
* 响应者的生命周期

	* View.props.onStartShouldSetResponder: (evt) => true, - 在用户开始触摸的时候（手指刚刚接触屏幕的瞬间），是否愿意成为响应者？
	
	* View.props.onMoveShouldSetResponder: (evt) => true, - 如果View不是响应者，那么在每一个触摸点开始移动（没有停下也没有离开屏幕）时再询问一次：是否愿意响应触摸交互呢？
	* View.props.onResponderGrant: (evt) => {} - View现在要开始响应触摸事件了。这也是需要做高亮的时候，使用户知道他到底点到了哪里。
	* View.props.onResponderReject: (evt) => {} - 响应者现在“另有其人”而且暂时不会“放权”，请另作安排。
	* View.props.onResponderMove: (evt) => {} - 用户正在屏幕上移动手指时（没有停下也没有离开屏幕）。
	* View.props.onResponderRelease: (evt) => {} - 触摸操作结束时触发，比如"touchUp"（手指抬起离开屏幕）。
	* View.props.onResponderTerminationRequest: (evt) => true - 有其他组件请求接替响应者，当前的View是否“放权”？
	* View.props.onResponderTerminate: (evt) => {} - 响应者权力已经交出。这可能是由于其他View通过onResponderTerminationRequest请求的，也可能是由操作系统强制夺权（比如iOS上的控制中心或是通知中心）。
	* evt
		* nativeEvent
			* changedTouches - 在上一次事件之后，所有发生变化的触摸事件的数组集合（即上一次事件后，所有移动过的触摸点）
			* identifier - 触摸点的ID
			* locationX - 触摸点相对于父元素的横坐标
			* locationY - 触摸点相对于父元素的纵坐标
			* pageX - 触摸点相对于根元素的横坐标
			* pageY - 触摸点相对于根元素的纵坐标
			* target - 触摸点所在的元素ID
			* timestamp - 触摸事件的时间戳，可用于移动速度的计算
			* touches - 当前屏幕上的所有触摸点的集合
			
* 捕获ShouldSet事件处理(捕获期)
	> 响应系统在从最底层的组件开始冒泡之前，会首先执行一个“捕获期”，在此期间会触发on*ShouldSetResponderCapture系列事件。因此，如果某个父View想要在触摸操作开始时阻
	止子组件成为响应者，那就应该处理onStartShouldSetResponderCapture事件并返回true值。
	* View.props.onStartShouldSetResponderCapture: (evt) => true,
	* View.props.onMoveShouldSetResponderCapture: (evt) => true,

		```
		// 如果在_viewFather设置捕获期拦截 那么_viewChild就不会相应
		<View key='_viewFather' {...this._gestureHandlers} style={[styles.rect2,{"backgroundColor": this.state.bg}]}>
		  <View key='_viewChild' {...this._gestureHandlers2} style={[styles.rect,{"backgroundColor": this.state.bg2}]} />
		</View>
		```
		[https://github.com/naivehhr/RNPanResponder](https://github.com/naivehhr/RNPanResponder)
		
		![Markdown](http://p1.bpimg.com/1949/fb55e60846aef9d0.gif)

****

PanResponder

```
// 定义手势
this._panResponder = PanResponder.create({
  // 要求成为响应者：
  onStartShouldSetPanResponder: (evt, gestureState) => true,
  ...
}）

//调用
<View {...this._panResponder.panHandlers}} />

```




```
// 应该和上面差不多，简单备注下
onMoveShouldSetPanResponder: (e, gestureState) => bool //移动时是否愿意成为响应者
onMoveShouldSetPanResponderCapture: (e, gestureState) => bool //移动时是否捕获
onStartShouldSetPanResponder: (e, gestureState) => bool //一开始是否响应手势事件
onStartShouldSetPanResponderCapture: (e, gestureState) => bool //一开始是否捕获
onPanResponderReject: (e, gestureState) => {...} //拒绝放权
onPanResponderGrant: (e, gestureState) => {...} //点击反馈
onPanResponderStart: (e, gestureState) => {...} //手势开始时
onPanResponderEnd: (e, gestureState) => {...} //手势结束时
onPanResponderRelease: (e, gestureState) => {...} //手势释放时
onPanResponderMove: (e, gestureState) => {...} //手势移动时
onPanResponderTerminate: (e, gestureState) => {...} //交权后
onPanResponderTerminationRequest: (e, gestureState) => {...} //有其他组件请求权限是否放权
onShouldBlockNativeResponder: (e, gestureState) => bool // 返回一个布尔值，决定当前组件是否应该阻止原生组件成为JS响应者 默认返回true。目前暂时只支持android

```
* nativeEvent
	* changedTouches - 在上一次事件之后，所有发生变化的触摸事件的数组集合（即上一次事件后，所有移动过的触摸点）
	* identifier - 触摸点的ID
	* locationX - 触摸点相对于父元素的横坐标
	* locationY - 触摸点相对于父元素的纵坐标
	* pageX - 触摸点相对于根元素的横坐标
	* pageY - 触摸点相对于根元素的纵坐标
	* target - 触摸点所在的元素ID
	* timestamp - 触摸事件的时间戳，可用于移动速度的计算
	* touches - 当前屏幕上的所有触摸点的集合
* gestureState
	* stateID - 触摸状态的ID。在屏幕上有至少一个触摸点的情况下，这个ID会一直有效。
	* moveX - 最近一次移动时的屏幕横坐标
	* moveY - 最近一次移动时的屏幕纵坐标
	* x0 - 当响应器产生时的屏幕坐标
	* y0 - 当响应器产生时的屏幕坐标
	* dx - 从触摸操作开始时的累计横向路程
	* dy - 从触摸操作开始时的累计纵向路程
	* vx - 当前的横向移动速度
	* vy - 当前的纵向移动速度
	* numberActiveTouches - 当前在屏幕上的有效触摸点的数量

> 结合动画完成更多交互