## Android原生模块封装

封装Android组件

http://www.jianshu.com/p/73ef53244a7b
http://www.jianshu.com/p/07b928feee3b
http://www.liuchungui.com/blog/2016/05/08/reactnativezhi-yuan-sheng-mo-kuai-kai-fa-bing-fa-bu-androidpian/

1、用Android Studio打开创建RN项目中的Android项目(比如成为A)
2、在A的根目录中新建一个Module(右键或者File -> New -> New Module)，这里称为BModule
3、在B中添加自己要封装的模块比如Toast的实现
4、B中的依赖要和母RN项目一致或不要太高
5、引用的话就直接react-native link 或者在三个地方分别添加就好了

注意：所谓平台的区分不是和RN项目有关系，上传到npm中完全可以是一个单独的库，IOS的也是一样。当然，如果是双平台的模块，完全可以放到一个专门的库中(copy过去应该就行了吧~)

卡了好几天。。。NND