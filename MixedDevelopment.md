#RN集成到原生项目
***
> 原生项目中使用RN开发还是很开心的一件事，不是么？

##IOS

###原生混和开发：
#### 一、开发配置

> [参考](http://www.jianshu.com/p/3dc9d70a790f)

##### 1. IOS项目中搭建RN开发环境(见参考)
##### 2.创建package.json
```
{
  "name": "MyRNApp",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start"
  },
  "dependencies": {
    "react": "15.2.1",
    "react-native": "0.31.0"
  }
}
====> npm install 	
```
##### 3. 安装CocoaPods

```
# Uncomment this line to define a global platform for your project
# platform :ios, '9.0'

target 'MyRNApp' do
  # Uncomment this line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for MyRNApp

    pod 'React', :path => 'node_modules/react-native', :subspecs => [
    'ART',
    'RCTActionSheet',
    'RCTAdSupport',
    'RCTGeolocation',
    'RCTImage',
    'RCTNetwork',
    'RCTPushNotification',
    'RCTSettings',
    'RCTText',
    'RCTVibration',
    'RCTWebSocket',
    'RCTLinkingIOS'
    # Add any other subspecs you want to use in your project
    ]

  target 'MyRNAppTests' do
    inherit! :search_paths
    # Pods for testing
  end

  target 'MyRNAppUITests' do
    inherit! :search_paths
    # Pods for testing
  end

end

注意path的路径设置,默认根目录,也可文件夹中
====> pod install 喽~
```

#### 二、开始开发

##### 1. 创建index.ios.js

```
'use strict';
import React, {Component} from 'react'

import {
    Text,
    View,
    AppRegistry
} from 'react-native';

class NativeRNApp extends React.Component {
  render() {
    return (
      <View >
        <Text>This is a simple application.</Text>
      </View>
    )
  }
}
AppRegistry.registerComponent('NativeRNApp', () => NativeRNApp);
```

##### 2. 设置JS入口

> 注: 可以直接就设置到默认的ViewController,也可以自己写个ReactViewController

```
@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self initRNView];
   }

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
}

-(void)initRNView {
   NSString * strUrl = @"http://localhost:8081/index.ios.bundle?platform=ios&dev=true";
    NSURL * jsCodeLocation = [NSURL URLWithString:strUrl];

    RCTRootView * rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation
                                                         moduleName:@"NativeRNApp"
                                                  initialProperties:nil
                                                      launchOptions:nil];
    self.view = rootView;
}

===> moduleName:@"NativeRNApp" 注意这个名称要和index.ios.js 中的对应上
```

##### 3. 一些问题记录
> [参考](http://blog.csdn.net/u014410695/article/details/50635842)

报错： App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure 报错原因是需要开启内网http的访问权限（iOS9之后会有这个问题）。
> 在Info.plist中添加 NSAppTransportSecurity 类型 Dictionary ; 
在 NSAppTransportSecurity 下添加 NSAllowsArbitraryLoads 类型Boolean ,值设为 YES; 
参考地址：

-

报错：RCTStatusBarManager module requires that the UIViewControllerBasedStatusBarAppearance key in the Info.plist is set to NO 错误信息说的很清楚了。
> 在Info.plist中添加View controller-based status bar appearance为NO。
http://www.cnblogs.com/chglog/p/4746683.html

![Markdown](http://i4.buimg.com/1949/3fb2e1d5b4f654d5.png)
***
### 集成已有RN模块(jsbundle)：
#### 一、开发配置

- 1
- 2
- 3
- 4
- 5 

#### 二、开始开发

- 1
- 2
- 3
- 4
- 5 

## Android

####原生混和开发：
#### 一、开发配置

- 1
- 2
- 3
- 4
- 5 

#### 二、开始开发

- 1
- 2
- 3
- 4
- 5 

####集成已有RN模块(jsbundle)：
#### 一、开发配置

- 1
- 2
- 3
- 4
- 5 

#### 二、开始开发

- 1
- 2
- 3
- 4
- 5
