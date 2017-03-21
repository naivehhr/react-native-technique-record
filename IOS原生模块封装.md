## IOS 原生模块封装

1、创建一个用来调试模块的RN项目

2、在项目的node_module目录下创存放组件的目录比如A

3、XCODE打开RN项目中的IOS项目, 完成原生模块的封装及测试

4、封装完成后, 使用XCODE在目录A中创建静态库项目(模块名称最好和上面调试模块的名称一致)

5、把RN->IOS项目中封装模块的代码.h.m拷贝到静态库文件中

6、设置静态库项目的header search path路径(添加React的依赖)

	* $(SRCROOT)/../../react-native/React