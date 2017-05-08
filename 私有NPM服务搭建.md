# 私有NPM库及使用
***


## 搭建服务

### 获取项目代码

```
#从github获取源代码
$ git clone git://github.com/cnpm/cnpmjs.org.git
$ cd cnpmjs.org

# 创建mysql表结构
$ mysql -u yourname -p
mysql> create database cnpmjs
mysql> use cnpmjs;
mysql> source docs/db.sql

```

### 编辑配置文件
```
vim config/index.js
```

```
enableCluster: true,
bindingHost: '172.18.22.198',
  // default system admins
admins: {
    // name: email
    fengmk2: 'fengmk2@gmail.com',
    admin: 'guopengb@chanjet.com',
    dead_horse: 'dead_horse@qq.com',
},  
  database: {
    db: 'cnpmjs',
    username: 'root',
    password: 'baby',

    // the sql dialect of the database
    // - currently supported: 'mysql', 'sqlite', 'postgres', 'mariadb'
    dialect: 'mysql',

    // custom host; default: 127.0.0.1
    host: '127.0.0.1',

    // custom port; default: 3306
    port: 3306,

      // public mode: all users can publish
  enablePrivate: false,

  // registry scopes, if don't set, means do not support scopes
  scopes: [ '@cjt','@cnpm', '@cnpmtest', '@cnpm-test' ],
  // sync mode select
  // none: do not sync any module, proxy all public modules from sourceNpmRegistry
  // exist: only sync exist modules
  // all: sync all modules
  syncModel: 'exist', // 'none', 'all', 'exist'
    // sync devDependencies or not, default is false
  syncDevDependencies: true
  
```

### 安装以依赖
```
 npm install
```

### 启动服务

```
node dispatch.js --harmony_generators
或者npm run start 等脚维护服务
```

注: 7001 端口是下载端口 7002 是web管理端口

## 发布私有包

### 安装cnpm客户端
```
sudo npm install cnpm -g
```

### 更改默认的registry，指向私有的registry
```
cnpm set registry http://172.18.22.198:7001
```

### 用npm账号登录你的私有registry

这里的账号中www.npmjs.com的账号，没有请注册

```
$ cnpm login
Username: guopeng
Password: ***
Email: (this IS public) ggpp224@sina.com
```

### 创建私有测试npm包
```
$ cd /tmp
$ mkdir helloworld && cd helloworld
$ cnpm init
name: @cjt/helloworld
version: 1.0.0
```

### 生成package.json :
```
{
  "name": "@cjt/helloworld",
  "version": "1.0.0",
  "description": "my first scoped package",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

注意，包名helloworld前必须要以公司名@cjt为前缀， @cjt在 config/index.js的scopes中配置

```
// registry scopes, if don't set, means do not support scopes
  scopes: [ '@cjt','@cnpm', '@cnpmtest', '@cnpm-test' ],

  如果在管理员列表中添加用户在没有添加包的公司作用域也没有发布包的权限，修改 `middleware/publishable.js`中没有包作用域判断权限代码
```

### 发布

```
cnpm publish
```
通过浏览器访问：http://172.18.22.198:7002/ 可查看和操作私有CNPM服务器


[参考](https://ggpp224.gitbooks.io/web-notes/content/CNPM%E7%A7%81%E6%9C%89%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%90%AD%E5%BB%BA.html)


## 使用

不要使用cnpm安装(RN中有问题)

### 设置特定源的指向路径

```
npm login --registry http://10.12.116.40:7001 --sco
pe=@faegroup
```

```
localhost:MyCounter aran.hu$ npm config list
; cli configs
scope = ""
user-agent = "npm/4.2.0 node/v7.6.0 darwin x64"

; userconfig /Users/admin/.npmrc
//10.12.116.40:7001/:always-auth = false
//10.12.116.40:7001/:email = "1903283729@qq.com"
//10.12.116.40:7001/:username = "aran.hu"
//registry.npm.taobao.org/:always-auth = false
//registry.npm.taobao.org/:email = "1903283729@qq.com"
//registry.npm.taobao.org/:username = "aranhu"
@faegroup:registry = "http://10.12.116.40:7001/"
disturl = "https://npm.taobao.org/dist"
registry = "https://registry.npmjs.org/"

; builtin config undefined
prefix = "/usr/local"

; node bin location = /usr/local/bin/node
; cwd = /Users/admin/Desktop/MyCounter
; HOME = /Users/admin
; "npm config ls -l" to show all defaults.

localhost:MyCounter aran.hu$

```
安装命令

```
npm install ** 

```
[参考](https://ggpp224.gitbooks.io/web-notes/content/cnpmClientUse.html)