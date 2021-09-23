

# VsCode开发React

## 安装node配置

1. node.js自带npm安装，最后一步勾上是安装必备其他工具
2. 在node目录下配置两个文件夹node_cache和node_global
3. cmd安装cluster模块npm install cluster -g
4. 在【系统变量】新建【NODE_PATH】，值【F:\SP\node\node_global\node_modules】
5. 将【用户变量】的【Path】修改为【F:\SP\node\node_global】
6. cmd命令require('cluster')验证安装配置成功

## 安装npm

1. cmd安装npm i cnpm

   //如果加载报错，修改npm下载地址npm set registry https://registry.npm.taobao.org

2. 再安装npm i cnpm

## 安装VsCode插件

- Vscode下载安装地址：官方下载地址网站替换为[vscode.cdn.azure.cn](https://link.zhihu.com/?target=http://vscode.cdn.azure.cn/)下载速度好了
- Chinese（Simplified）Language Pack for VS Code中文语言包
- Open in Browser 右击打开html
- JS-CSS-HTML Formatter 自动格式js css html
- Auto Rename Tag 自动配对HTML标签
- CSS Peek 跟踪至样式

## 创建配置React项目

1. 创建my-app自定义项目文件夹【确保已装Node】

   ```
   npx create-react-app my-app
   ```

2. 在src/文件夹中删掉其它，创建index.html，index.js，index.css文件【源代码】

3. 在index.js导包

   ```jsx
   //导包
   import React from 'react';
   import ReactDOM from 'react-dom';
   import './index.css';
   //测试
   const mydiv = React.createElement('div',{id:'mydiv',title:'标题mydiv'},'非常不错');
   ReactDOM.render(mydiv,document.getElementById('app'))
   ```

4. 在html测试

   ```html
   //引用
   <!-- <script src="./index.js"></script> -->
   //容器
   <div id="app"></div>
   ```

## Create React App

配置Static Server

```html
//安装
npm install -g serve
//开启服务
serve -s build
//设置端口
serve -s build -l 4000
//其他设置
serve -h
```

## GitHub页面配置上线

```jsx
//配置package.json远程github设置的pages页面链接
"homepage": "https://myusername.github.io/my-app",
//安装
yarn add gh-pages
"scripts": {
	//配置package.json启动命令
    "predeploy": "npm run build",
    //配置package.json部署命令
    "deploy": "gh-pages -b master -d build",
//开启部署
npm run deploy
//部署先设置好origin
git remote set-url origin git@github.com:qq632119185/todos.git
```



## 安装webpack配置.config.js

1. 安装

  ```
  //模板打包机
  cnpm i webpack -D
  //模板框架
  cnpm i webpack-cli -D
  //安装服务器
  cnpm i webpack-dev-server --save-dev
  //安装虚拟html文件
  cnpm i html-webpack-plugin --save-dev
  ```

2. 根据官网配置【根目录创建webpack.config.js文件】

  ```js
  //导包
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  //
  module.exports = {
    //模式设置
    mode: 'development',
    //打包设置【命令webpack，src目录是打包文件，dist目录打包好的文件】
    entry: './src/index.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'index.js',
    },
    //服务器设置
    devServer: {
      static: {
        directory: path.join(__dirname, './src'),
      },
      compress: true,
      port: 9000,
    },
    //虚拟html文件首页打开【html自动添加虚拟<script src="/main.js"></script>】
    plugins: [new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'src/index.html'
    })],
  };
  ```

3. 配置package.json【命令npm run dev开启服务器】

  ```js
  "scripts": {
    "dev": "webpack-dev-server --open --host 127.0.0.1"
   }
  ```

## 安装jsx配置

1. 安装

   ```
   cnpm i babel-core@6.26.3 babel-loader@7.1.5 babel-plugin-transform-runtime -D
   cnpm i babel-preset-env babel-preset-stage-0 babel-preset-react -D
   npm init -y
   npm install babel-cli@6 babel-preset-react-app@3
   ```
   
2. 配置Webpack.config.js

   ```
   module.exports = {
       module: { //要打包的第三方模块
               rules: [
                   { test: /\.js|jsx$/, use: 'babel-loader', exclude: /node_modules/ }
               ]
           },
   }
   ```

3. 根目录创建.babelrc内容

   ```jsx
   {
      "presets": ["env", "stage-0", "react"],
      "plugins": ["transform-runtime"]
   } 
   ```

## 安装jsx加载解析css文件模块化

1. jsx加载解析css安装插件

   ```
   cnpm i style-loader --save-dev
   cnpm i css-loader --save-dev
   ```

2. 配置

   ```jsx
   module: { //要打包的第三方模块
       rules: [
           {test: /\.css$/, use: ['style-loader', 'css-loader']},
       ]
     },
   ```

3. 安装sass

   ```
   cnpm i bootstrap@3.3.7 -S
   cnpm i url-loader -D
   cnpm i file-loader -D
   cnpm i sass-loader -D
   cnpm i node-sass -D
   ```

4. 配置

   ```jsx
   {test: /\.ttf|woff|woff2|eot|svg$/, use: 'url-loader'},
   ```

5. 自定义样式表用sass文件.scss，第三方默认用css

   ```jsx
   module: { //要打包的第三方模块
       rules: [
           //.css文件用css-loader?modules编译，再用style-loder编译
           {
             test: /\.scss$/,
             use: [
               {
                 loader: "style-loader",
               },{
                 loader: "css-loader",
                 options: {
                   modules: true,
                 },
               },{
                 loader: "sass-loader",
               }
             ]
           },
       ]
     },
   ```

6. 应用：只能id和className类名模块化

   ```jsx
   import 'bootstrap/dist/css/bootstrap.css';
   import './index.scss';
   //jsx调用css模块化
   <div id={css文件名.title}></div>
   <div classNme={css文件名.user}></div>
   /* 模块化【调用文件】.title或者:local(.title) */
   .title {
   	color: pink;
   }
   /* 不模块化【全局文件】 */
   :global(.user) {
   	color: red;
   }
   ```

## 组件传参props

- 一个文件传参

  ```jsx
  //模拟数据
  const list = {id: 1,title: '测试'}
  //创建组件
  function zujian (props) {
  	return (<div>{props.title}</div>)
  }
  //渲染组件
  ReactDOM.render(<Zujian id={list.id} title={list.title}></Zujian>,document.getElementById('app'))
  //或者...展开运算符
  ReactDOM.render(<Zujian {...list}></Zujian>,document.getElementById('app'))
  ```

- 多文件传参【function定义组件】无状态无生命周期

1. index.js文件发送props参数并引用indexapp.js文件接口组件

   ```jsx
   //导包
   import React from 'react'
   import ReactDOM from 'react-dom'
   //引用其他js文件接口
   import App from './indexapp.js'
   //模拟参数
   const list = {id: 1, title: '测试'}
   //渲染传参props
   ReactDOM.render(<App {...list}></App>,document.getElementById('app'))
   ```

2. indexapp.js文件接收porps参数并暴露接口

   ```jsx
   //导包
   import React from 'react'
   //暴露接口接收参数props
   export default function App (props) {
   //应用props参数
   	return <div>{props.title}</div>
   }
   ```

- 多文件传参【class定义组件】有状态有生命周期：可以自定义参数且可以修改

1. HTML文件index.html

   ```html
   <script src="./main.js"></script>
   <div id="main"></div>
   ```

2. 主组件main.js

   ```jsx
   //导包
   import React from 'react'
   import ReactDOM from 'react-dom'
   //【导class类组件接口】
   import MainContent from './main1.js'
   //渲染页面
   ReactDOM.render(<Main></Main>,document.getElenmentById('main'))
   ```

3. 组件一main1.js

   ```jsx
   import React from 'react'
   //【导function函数组件接口】
   import MainContent from './main2.js'
   //定义class组件【类】暴露接口
   export default class Main extends React.Component {
       constructor () {
           super (),
           this.state = {
               msg: [
                   {id: 0, name: '李四', content: '非常不错'},
                   {id: 1, name: '张艺谋', content: '著名导演演员'},
                   {id: 2, name: '邵鹏辉', content: '上班的一个好员工'},
                   {id: 3, name: '吴新奇', content: '不认识的人'},
                   {id: 4, name: '陈毅', content: '游戏设计师动画制作'}
               ]
           }
       }
       render () {
           return <div>
               <h1 style={{color: 'red', fontSize: '44px', textAlign: 'center'}}>评论列表页</h1>
               {/*组件{...item}传参*/}
               {this.state.msg.map(item => <MainContent {...item} key={item.id}></MainContent>)}
           </div>
       }
   }
   ```
   
4. 组件二main2.js

   ```jsx
   import React from 'react'
   //【导style对象接口】
   import styles from './styles.js'
   //定义function组件【函数】暴露接口
   export default function MainContent (props) {
       {/*组件props接参*/}
       return <div style={styles.item}>
           <h2 style={styles.uer}>评论人：{props.name}</h2>
           <p style={styles.content}>评论内容：{props.content}</p>
   	</div>
   }
   ```
   
5. 组件三style.js【文件名即接口】

   ```jsx
   //定义【对象】暴露接口
   export default {
       item: {border: '1px dashed #ccc', margin: '10px', padding: '10px', boxShadow: '0 0 10px #ccc'},
       uer: {fontSize: '14px'},
       content: {fontSize: '12px'}
   }
   ```

## 单项数据绑定

```jsx
<div>
    <input type="text" style={{width: '1024px'}} value={this.state.msg}></input>
    <button onClick={() => this.handleShow('测试成功')}>按钮</button>
</div>
//
handleShow = (props) => {
        //this.setState()属异步可用回调函数新参数
        this.setState({
            msg: props
        },function(){console.log(this.state.msg)})
    }
```

- 更改参数this.setastate({},callback)方法异步可用回调函数调用新的参数
- console.log是老的参数

## 双向数据绑定

```jsx
<div>
    <p>{this.state.msg.map(item => item.content)}</p>
    {/* 或者ref="txt" */}
    <input type="text" style={{width: '1024px'}} value={this.state.msg} onChange={(props) => this.handleOnchange(props)} ref="txt"></input>
</div>
//
handleOnchange = (props) => {
        this.setState({
            msg: props.target.value
            //或者 msg: this.refs.txt.value
        })
```

# git

## 本地配置

- 官网下载安装

- npm git -v查询安装版本

- 在项目文件夹右击打开Git Bash Here

- 用户配置

  ```jsx
  //配置信息
  git config --global user.name ceshi
      git config --global user.email ceshi@qq.com
  //查询配置
  git config --global --list
  //更改配置
  git config --global user.name ceshigenggai
  ```

- 管理项目文件

  ```jsx
  //在项目文件夹右击打开Git Bash Here
  //初始化git init
  ```

- 虚拟工作目录

  ```jsx
  //【虚拟目录】文件管理状态
  git status
  //【虚拟目录】添加文件
  git add index.html
  //【虚拟目录】下载文件覆盖源文件
  git chekout index.html
  //【虚拟文件】删除文件
  git rm --cached index.html
  //【虚拟目录】删除文件
  git rm -r --cached src/index.html
  ```

- 本地工作目录

  ```jsx
  //【本地目录】提交并说明
  git commit -m 第一次
  //【本地目录】提交信息
  git log
  //【本地目录】下载恢复到某个提交版本，其他之后的版本自动删除
  git reset --hard 'commit版本信息'
  ```

- 分支管理

  ```jsx
  //查看分支
  git branch
  //创建分支
  git branch develop //开发分支名
  //切换分支
  //查看【虚拟目录】文件管理状态 git status是否有未提交版本，不然会携带本分支文件至其他目录
  git checkout develop
  //合并分支
   //切换至主分支master才可以合并，只是文件合并，分支依然在
  git merge develop
  //删除分支【硬删git branch -D develop】
  //切换至主分支master并合并才可以删除，查看【虚拟目录】文件管理状态 git status是否有未提交版本
  git branch -d develop
  //剪贴板【临时切换分支】不用提交就可以切换至其他分支，不会携带当前分支文件到其他分支，完成其他分支工作再切换回来
  git stash
  ```

## 网页配置

- ssh免登陆配置

  ```jsx
  1. 命令ssh-keygen 一直回车生成文件
  2. 在C:./.用户/./.ssh文件id_rsa_pub打开源码复制，在github网站用户设置SSH标题自动生成
     //普通登录配置push后登录页面，控制面板-凭证管理器-window凭证
  ```

- 项目配置

  ```jsx
  //远程地址添加别名为origin或者origin_ssh替换长地址
  git remote add origin https://github.com/qq632119185/git-demo.git
  //或者
  git remote set-url origin https://<user>:<token>@github.com/<user>/<repo>
  //推送远程【添加提交才可以推送】
  git push origin master
  //设置其他人可以修改
  网页项目setting-collaborators添加其他账号，右上角copy invite link链接分享打开就可以修改
  //更新本地到远程最新版本，clone是第一次用
  git pull origin master //如果有冲突文件里会有标识，修改后删掉标识提交就可以解决冲突了
  //目录下创建.gitignone文件
  这个文件里的不会显示在添加目录
  //.md后缀说明文件
  命令git add .会显示在网页说明
  ```

- 其他项目

  ```jsx
  //复制远程文件
  git clone https://github.com/qq632119185/git-demo.git
  //打开git-demo文件夹
  cd git-demo
  //clone其他作者的程序，可以在网页右上角fork下载修改好push后在网页Pull requests-New pull request-Create pull request可以说明本次修改的内容说明
  ```

# Sulime Text开发React

- 安装Sublime Text编辑器

- cmd命令npm i yarn -g安装yarn

- shfit+右击，运行命令mkdir react-demos创建项目文件夹

- cd react-demos打开文件夹

- yarn init -y建立新项目生成package.json文件

- 添加依赖yarn add react react-dom @babel/standalone文件夹生成项目文件组件/渲染/解析

- Sublime Text编辑器安装AutoFileName插件，把项目文件夹拖进编辑器

- <script>引进插件

  ```html
  <script src="node_modules/@babel/standalone/babel.js"></script>
  	<script src="node_modules/react/umd/react.development.js"></script>
  	<script src="node_modules/react-dom/umd/react-dom.development.js"></script>
  ```


- <script>应用插件

  ```jsx
  <script type="text/babel">
  </script>
  ```

- 装插件babel右下角主题babel设置jsx的语句高亮

- jsx的html代码注释用{/**/}

  可以用ES6的``模板字符串

  ```jsx
  //引用bable
  <script type="text/babel">
  //对象
  const user = {
  name: '李四',
  age: 6
  }
  //函数
  function getGrtting(user) {
  if(user) {
  return <h1>正确<h1>
  }
  return <h1>错误</h1>
  }
  //调用函数
  const element = getGrtting(user)
  //调用html
  {/*在jsx的html中注释方法*/}
  //与js冲突calss用className
  const element = <div class="userclass" title={user.name}>
  {/*调用ES6模板字符串``调用${}*/}
  <p>{`hello world${user.name}`}</p>
  {/*表达式*/}
  <p>{user.age > 6 ? '成年' ： '未成年'}</p>
  </div>
  //与js冲突check用defaultChecked
  const element = <input type="checkbox" defaultChecked/>
  //与js冲突value用defaultValue
  const element = <input type="text" value="user"/>
  //与js冲突for用htmlFor
  const element = <label for="forname">用户名：</label>
  //在jsx中html未闭合的要用/>闭合
  <input type="text" id="foename"/>
  //渲染
  ReactDOM.render(elemnet,document.getElementById('app'))
  //数组按序列渲染
  //报错设置key就好了
  const arry = [
  <li key="1">黑色</li>
  <li key="2">红色</li>
  <li key="3">蓝色</li>
  <li key="4">紫色</li>
  ]
  const lists = [
  {
  id: 1,
  title: '实'
  },{
  id: 2,
  title: '战'
  },{
  id: 3,
  title: '教'
  },{
  id: 4,
  title: '学'
  }
  ]
  //forEach方法
  const list = []
  lists.forEach(item => {
  list.push(<li key={item.id}>{item.title}</li>)
  })
  const element = <div>
  <ul>
  {/*报错设置key就好了*/}
  {arry}
  </ul>
  <ul>
  {/*forEach方法*/}
  {list}
  </ul>
  <ul>
  {/*内部调用map方法*/}
  {lists.map(item => <li key={item.id}>{item.title}</li>)}
  </ul>
  </div>
  ReactDOM.render(element,document.getElementById('app'))
  //点击外部调用函数
  function alertElement() {
  window.alert('点击内部函数成功')
  }
  const element = <div>
  <button onClick={alertElement}>点击外部函数</button>
  {/*点击内部调用函数*/}
  <button onClick={() => {alert('点击内部函数成功')}}>点击内部函数</button>
  ReactDOM.render(element,document.getElementById('app'))
  </div>
  </script>
  ```

- 组件

  ```jsx
  //继承react组件方法
  class MyComponent extends React.Component {
  //参数方法
  constructor () {
  //必须
  super ()
  //自定义参数
  this.state = {
  message: 'Hello Word!'
  }
  }
  //渲染方法
  render () {
  return (
  <div>
  <p>{this.message}</p>
  //点击调用方法改其this
  <button onClick={this.handleClick.bind(this)}>点击更新</button>
  </div>
  )
  }
  //this指window
  handleClick () {
  //setState方法修改state参数
  this.setState({
  message: '更新成功'
  })
  }
  }
  //实例化组件
  const element = <Mycomponent />
  //渲染
  React.render(element,document.getElementById('app'))
  ```

- 下载安装git

- 下载项目命令git clone https://github.com/tastejs/todomvc-app-template.git --depth=1 todomvc-react

- 命令打开cd todomvc-react文件夹

- 命令yarn install安装package.json中的所有依赖

- text/babel要在服务器网址http打开

  ```html
  <script type="text/babel" src="main.js"></script>
  ```

- npm install http-server -g安装http-server

- 项目文件夹shift右击命令http-server -c-1

- 目录下新建main.js文件，新建components文件夹app.js文件

  ```html
  //html引进这两个文件
  <script type="text/babel" src="components/app.js"></script>//组件
  <script type="text/babel" src="main.js"></script>//渲染
  ```

- app.js

  ```jsx
  //调用React
  (function (React) {
  //创建列表数组
  const todos = [	
  {id: 1, title: '测试', completed: true},
  {id: 2, title: '测试', completed: false},
  {id: 3, title: '测试', completed: true},
  {id: 4, title: '测试', completed: true}
  ]
  //创建组件
  window.app = class extends React.Component {
  //继承默认
  constructor () {
  //必须
  super （）
  //自定义参数
  this.state = {
  todos
  }
  }
  //组件渲染
  render () {
  return (
  //渲染内容jsx
  <div>
  //调用事件方法，箭头函数体this指向调用者event默认对象
  <input onKeyDown={() => this.handleTodoNewKeyDown(event)} />
  <ul>
  //调用数据方法
  {this.getTodolist()}
  </ul>
  </div>
  )
  }
  //数据方法
  getTodolist () {
  return (
  this.state.todos.map(item => <li key={item.id}>{item.title}</li>)
  )
  }
  //事件方法
  handleNewTodoKeyDown (event) {
  //event是对象
  const {target,keyCode} = event
  if(keyCode !== 13) {
  return
  }
  const inputText = target.value.trim()
  if(!inputText.length) {
  return
  }
  const todoKey = todos.lenth ? todos.lenth+1 : 1
  //数组push进新成员
  this.state.todos.push({id: todoKey, title: inputText, completed: true})
  //设置state对象列表更新
  this.setState({todos: this.state.todos})
  target.value = ""
  }
  }
  })(React);
  ```

- main.js

  ```jsx
  //引用ReactDOM
  (function (RecatDOM){
  //渲染页面
  ReactDOM.render(<app />,document.getElementById('app'))
  })(ReactDOM);
  ```

# Vue安装配置

1. 安装cnpm install vue-cli -g

2. 版本vue -V

3. 创将项目文件vue init webpack vue_demo

4. 打开文件夹cd vue_demo

5. 安装依赖cnpm install

   //如果报错重新安装npm install -g npm

6. 下载 vue.js 文件https://vuejs.org/js/vue.min.js

7. 在index.html文件中引用<script src="./vue.min.js"></script>

8. html设置容器<div id="app">

9. 编译中的vue代码就可以编译<script></script>

   ```html
   <div id="app">
   	<div>
   		//v-model绑定参数数值
   		<input v-model="title" />
   		//@click绑定事件方法
   		<button @click="add">点击添加</button>
   	</div>
   	<ul>
   		//：key值防止错乱
   		//v-for遍历参数数值
   		<li :key="item.id" v-for="item in list">
   		//内容{{引用显示参数数值}}
   		<input type="checkbox" />编号：{{item.id}},标题：{{item.title}}
   		</li>
   	</ul>
   </div>
   <script>
       const vm = new Vue ({
       	//绑定html标签id名
           el: '#app',
           //参数区域
           data: {
               title: '',
               list: [
                   {id: 0, title: '测试'},
                   {id: 1, title: '测试'},
                   {id: 2, title: '测试'},
                   {id: 3, title: '测试'},
                   {id: 4, title: '测试'}
               ]
   		},
   		//方法区域
   		methods: {
   			add () {
   			var newtitle = {id: this.list.length, title: this.title}
   			//数组末尾添加
   			this.list.push(newtitle)
   			//数组开始添加
   			this.list.unshift(newtitle)
   			}
   		}
       })
   </script>
   ```


## 其他配置【非必】

- 在终端命令npm init -y初始化项目

- 操作设置

  ```jsx
  module.exports = {
      resolve: {
          extensions: ['.js','.jsx','json'],//引用这些文件名后缀可以省略
          alias: {'@': path.join(_dirname, './src')}//@代表这个拼接路径
      }
  }
  ```

- 浏览器插件react-devtools

  1. 下载react-devtools文件到本地: git clone https://github.com/facebook/react-devtools

  2. 打开文件夹cd react-devtools

  3. 安装依赖npm --registry https://registry.npm.taobao.org install

  4. 打包程序扩展；npm run build:extension:chrome

     //生成新的文件夹：react-project⁩ ▸ ⁨react⁩ ▸ ⁨react-devtools⁩ ▸ ⁨shells⁩ ▸ ⁨chrome⁩ ▸ ⁨build⁩

  5. 在Chrome扩展程序chrome://extensions/加载生成的unpacked文件夹

- 运行在终端的 JSX 预处理监听器
  1. 下载这段 **[JSX 代码](https://gist.github.com/gaearon/c8e112dc74ac44aac4f673f2c39d19d1/raw/09b951c86c1bf1116af741fa4664511f2f179f0a/like_button.js)**创建一个 `src/like_button.js` 文件
  2. 终端命令npx babel --watch src --out-dir . --presets react-app/prod

- 注释折叠

  ```jsx
  //#内容
  //内容
  //内容
  //#内容
  ```


# html结构

- 框架

  ```html
  <header> //头部
  <nav> //导航
  <article> //内容
      <section> //块级
  <aside> //侧边
  <footer> //尾部
  ```

- button

  ```css
  <button type='primary' shape='round' size='small'></button>
  ```
  
  
  
- audio

  ```html
  <audio src = “.mp3” controls autoplay loop> //显示控件
      //多格式
      <source src = “.mp3” type = “audio/mpeg” />
      <source src = “.ogg” type = “audio/ogg” />
  </audio>
  //audio设置属性
      src
      muted 静音true
          preload 是否立即加载
          controls 显示播放控件
          loop 音频循环
          autoplay
  //target.属性
      .paused 是否暂停
      .currentTime 音频当前播放时间
      .duration 音频总长度
      .ended 音频是否结束
      .volume 当前音频音量
      .readyState 音频当前的就绪状态
  //target.方式
      .play() 播放
      .pause() 暂停
  	canplay 当浏览器可以播放音频/视频时
      timeupdate 当目前的播放位置已更改时
          ended 当目前的播放列表已结束时
          abort 当音频/视频的加载已放弃时  
          canplaythrough 当浏览器可在不因缓冲而停顿的情况下进行播放时
          durationchange 当音频/视频的时长已更改时
          emptied 当目前的播放列表为空时
          error 当在音频/视频加载期间发生错误时
          loadeddata 当浏览器已加载音频/视频的当前帧时
          loadedMetadata 当浏览器已加载音频/视频的元数据时
          loadstart 当浏览器开始查找音频/视频时
          playing 当音频/视频在已因缓冲而暂停或停止后已就绪时
          progress 当浏览器正在下载音频/视频时
          ratechange 当音频/视频的播放速度已更改时
          seeked 当用户已移动/跳跃到音频/视频中的新位置时
          seeking 当用户开始移动/跳跃到音频/视频中的新位置时
          stalled 当浏览器尝试获取媒体数据，但数据不可用时
          suspend 当浏览器刻意不获取媒体数据时
          volumechange 当音量已更改时
          waiting 当视频由于需要缓冲下一帧而停止
  ```
  
- video

  ```html
  <video src = “.mp4” controls muted loop poster = “.jpg” preload = “auto”或”no”> //显示控件,静音,封面,预加载
      <source src = “.mp4” type = “video/mp4” />
      <source src = “.ogg” type = “video/ogg” />
  </video>
  ```

- form

  ```html
  <form action = ".asp" method="get"> //表单提交地址，方式
      <ul>
          <li>邮箱：<input type = “email” /></li>
          <li>网址：<input type = “url” /></li>
          <li>日期：<input type = “date” /></li>
          <li>时间：<input type = “time” /></li>
          <li>数量：<input type = “number” /></li>
          <li>手机号码：<input type = “tel” /></li>
          <li>搜索：<input type = “search” /></li>
          <li>颜色：<input type = “color” /></li>
          <li>月：<input type = “month” /></li>
          <li>周：<input type = “week” /></li>
          <li> <input type = “submit” value = “提交”></li>
      </ul>
  </form>
  ```

- input

  ```html
  <from action = ".asp" method="get"> //表单提交地址，方式
      用户名：
      <input type = "text" placeholder = "默认内容" autofocus required autocomplete = "off"> //必填,不显示字段
      <input type = "submit" value = "提交">
      上传图像
      <input type = "file" multiple = "multiple"> //多件
  </form>
  //音量条
<input type="range"/>
  ```
  

# 样式

- class

  ```css
  div[class或type等] {} //此属性div
  div[class = "icon"] {} //class属性=icon
  div[class ^= "icon"] {} //icon开头
  div[class *= "icon"] {} //包含icon
  div[class $= "icon"] {} //icon结尾
  ```

- ul li

  ```css
  ul li:first-child {} //此标签第几个
  ul li:last-child {}
  ul li:nth-child(6) {}
  ul li:first-of-type {} //此标签此类型第几个
  ul li:last-of-type {}
  ul li:nth-of-type(6) {}
  ```

- ::before ::after

  ```css
  div::before {} //前插
  div::after {
      content : "小猪佩奇";内容
      display : inline-block;
  } //后插
  ```

- :hover

  ```css
  div:hover{
      transform : translate(6px ，6px) rotate(6deg) scale(6); //变形,偏移,旋转,缩放
      transform-origin : 6px 6px或top right bottom left center //中心点
  }
  ```

- Transition（变换过渡）

  ```css
  transition: property duration timing-function delay;
  /* property：过渡的属性
              all所有属性
              property属性名以逗号分隔
  duration：过渡的持续时间
              单位为s（秒）或者ms（毫秒）
  timing-function：过渡的速率模式
              ease（逐渐变慢）默认值
              linear（匀速）)
              ease-in(加速)
              ease-out（减速
              ease-in-out（加速然后减速）
  delay：延时多久
              单位为s（秒）或者ms（毫秒）
  */
  ```
  
- @keyframes

  ```css
  @keyframes ceshi {
      0% {
          width: 6px;
      }
      100% {
          width: 66px;
      }
  }
  div {
      animation: ceshi 6s linear 6s infinite alternate; //常用
      // animation-name: //名称
      // animation-duration: //周期秒
      // //运动曲线:匀速,低快慢,低开,低尾,低开低尾,步长
      // animation-timing-function: linear ease ease-in ease-out ease-in-out steps()
      // animation-delay: //开始时间
      // animation-iteration-count: infinite; //次数
      // animation-direction: normal或altemate; //逆播放
      // animation-play-state: running或paused;
      // animation-fill-mode: forwards或backwards;
  }
  ```

- transform

  ```css
  div.cs {
      transform-style: preserve-3d; //开启,默认关闭
  }
  div.ceshi {
      transform: translate3d(66px,66px,6px);
      transform: rotate3d(1,1,1,66deg);
  }
  div.ceshi6 {
      transform: translateX(6px);
      transform: rotateZ(6deg);
  }
  ```

- a标签样式

  ```
  a:link{color:#} //未访问时的状态（鼠标点击前显示的状态）
  a:hover{color:#} //鼠标悬停时的状态
  a:visited{color:#} //已访问过的状态（鼠标点击后的状态）
  a:active{color:#} //鼠标点击时的状态
  a:focus{color:#} //点击后鼠标移开保持鼠标点击时的状态[获得焦点]（只有在<a href="#"></a>时标签中有效）
  ```
  
- 背景渐变兼容

  ```scss
  background: -moz-linear-gradient(top, #000000 0%, #ffffff 100%);
      background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#000000), color-stop(100%,#ffffff));
      background: -webkit-linear-gradient(top, #000000 0%,#ffffff 100%);
      background: -o-linear-gradient(top, #000000 0%,#ffffff 100%);
      background: -ms-linear-gradient(top, #000000 0%,#ffffff 100%);
      background: linear-gradient(to bottom, #000000 0%,#ffffff 100%);
  ```
  
- overflow:scroll 滚动条的样式

  ```css
  /* 定义滚动条样式 */
  ::-webkit-scrollbar {
    width: 6px;
    height: 6px;
    background-color: rgba(240, 240, 240, 1);
  }
    
  /*定义滚动条轨道 内阴影+圆角*/
  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
    border-radius: 10px;
    background-color: rgba(240, 240, 240, .5);
  }
    
  /*定义滑块 内阴影+圆角*/
  ::-webkit-scrollbar-thumb {
    border-radius: 10px;
    box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
    background-color: rgba(240, 240, 240, .5);
  ```
  
- range播放条的样式

  ```css
  //去除系统样式
          input[type=range] {
              -webkit-appearance: none;
              width: 300px;
              border-radius: 10px; /*这个属性设置使填充进度条时的图形为圆角*/
          }
          input[type=range]::-webkit-slider-thumb {
              -webkit-appearance: none;
          }
          //滑动轨道(track)样式
          input[type=range]::-webkit-slider-runnable-track {
              height: 2px;
              border-radius: 10px; /*将轨道设为圆角的*/
              // box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112; /*轨道内置阴影效果*/
          }
          input[type=range]:focus {
              outline: none;
          }
          //滑块(thumb)样式
          input[type=range]::-webkit-slider-thumb {
              -webkit-appearance: none;
              height: 10px;
              width: 10px;
              margin-top: -4px; /*使滑块超出轨道部分的偏移量相等*/
              background: #ffffff; 
              border-radius: 50%; /*外观设置为圆形*/
              // border: solid 0.125em rgba(205, 224, 230, 0.5); /*设置边框*/
              // box-shadow: 0 .125em .125em #3b4547; /*添加底部阴影*/
          }
  ```
  
- 插件

  图标网站使用icomoon
  
  ps-cutterman-切图神器

## less

- vs code安装easy less自动生成css

  ```
  nmp install -g less
  ```

  ```less
  a:hover{
      color: red;
  }
  //或者
  a{
      &:hover{
          color: red;
      }
  }
  ```

- 文件引用

  ```jsx
  在index.less引用ceshi.less
  @import “ceshi”
  //下载地址https://github.com/amfe/lib-flexible
  <script src = “.flexible.js”></script> //在index.html引用flexible.js
  ```

- 框架

  bootstrap框架http://www.boot css.com/

- 插件

  vscode px变rem插件cssrem

# 移动端开发

- html配置

  ```html
  <meta name = "viewport" content = "width = device-width,user-scalable = no,initial-scale = 1.0,maximum-scale = 1.0,minimum-scale = 1.0"> //是否可缩放//默认
  <meta http-equiv = “C-UA-Compatible” content = “IE=edge”> //用最新版本渲染
  <link rel="stylesheet" type="text/css" href="css.css">
  <link rel="stylesheet" type="text/css" href="normalize.css">
  ```

- css配置

  ```css
  -webkit-box-sizing: border-box; //盒子模型
  -webkit-tap-highlight-color: transparent; //点亮关闭
  -webkit-appearance: none; //按钮和框自定样式
  img,a{-webkit-touch-callout: none;} //禁按弹菜单
  //常用样式
  boby {
      margin: 0 auto;
      min-width: 320px;
      max-width: 640px;
      background: #ffff;
      font-size: 14px;
      font-family: -apple-system,Helvetica,sans-serif;
      line-height: 1.5;
      color: #666;
      overflow-x: hidden;
      -webkit-tap-highlight-color: transparent;
  }
  ```

- 插件

  移动端css初始化推荐

  normalize.css官方http://necolas.github.io/normalize.css

# flex兼容

- 方式flexible.js、less、媒体查询、rem

- flex

  ```css
  display: flex; //开启模式
  flex-direction: //设置item元素的排序方式
                  row; //左右
                  row-reverse; //右左
                  column; //上下
                  column-reverse; //下上
  justify-content: //设置tem元素在主轴上的对齐方式
                  flex-start; //头排
                  flex-end; //尾排
                  center; //居中
                  space-around; //均匀
                  space-between; //两边贴边再均匀
  align-items: //设置item元素在交叉轴的对齐方式
                  flex-start; //上下
                  flex-end; //下上
                  center; //居中
                  stretch; //拉伸
  ```

  ```css
  align-content: //侧轴排列方式【排在第二行的元素排序方式】
                  flex-start; //头排
                  flex-end; //尾排
                  center; //居中
                  space-around; //均匀
                  space-between; //两边贴边再均匀
                  stretch; //拉伸
  flex-wrap: //是否换行
                  nowrap; //不
                  wrap; //换
  span: nth-child(6) {align-self: flex-end;}独立排列方式
  ```

  

- em基于上级rem基于html

- background

  ```css
  background: linear-gradient(角度，颜色，颜色，，）；
  background: -webkit-linear-gradient(66deg,red,pink);
  ```

- em基于上级,rem基于html

  ```css
  <style>
  	//查询尺寸使用不同html样式
      @media mediatype and或not或only (min-width: 666px) {
          html {
          font-size: 66px;
      }
      @media mediatype and (min-width: 66px) {
          html {
          font-size: 6px;
      }
  </style>
  //或者
  <link rel = “stylesheet” media = “mediatype and (min-width: 666px)” href = “ceshi.css”>
  ```

- 解决ie 9对html5新标签不识别

  ```html
  <!—[if It IE 9]
  //解决ie 9对html5新标签不识别
  <script src = “https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js”></script>
  //解决ie 9对css3 Media Query不识别
  <script src = “https://oss.max Dan.com/respond/1.4.2/respond.min.js”></script>
  <![endif]—>
  ```

# js

## 类型

特殊符号

```
isNaN(ceshi);是否数字类型
\n为换行符\\,\’\”,\t,\b
字符串.length
typeof(ceshi)检测类型
表单、prompt数据是字符串类型
var可升可变let块不升可变const块不升不变
变量var x,y;
$_可作为变量名;
```

## 数值

```jsx
parseInt(‘6’);
parseFloat(‘6.6’);
Number(‘666’);
js(- * /)隐式‘6ceshi’-或*或/6;
```

方式

```js
Math.PI //圆周率
.floor() //下整
.ceil() //上整
.round() //约等
.abs() //绝对值
.max() //最大
.min() //最小
.random() //随机数
ToInteger数值的整数部分
.pow() //以第一个参数为底数、第二个参数为幂的值
.sqrt() //参数的平方根
.log() //以e为底的自然对数值
.exp() //常数e的参数次方
```

## 字符

变字符类型

```jsx
ceshi.toString();
String(ceshi);
(numceshi + “测试”）
字符数组'hello'[1];// "e"
```

方式

```js
indexOf(‘c’,6) //开始位置
lastIndexOf()
str[index]指定位置charAt(6)或charCodeAt(index)
substr(start,length) //位置截
slice(start,end)
substring(start,end)
ceshi.replace(‘x’,‘r’) //只替换第一个
ceshi.split(“,|\n|或者连接的其他正则遇到这些跳过变数组”) //变数组按给定规则分割字符;
.replace(‘pink’,‘x’); //替换pink
div.innerHTML = ceshi.replace(/pink/gi,’x’); //替换包含pink
let ceshi = ‘Hello word!’;
ceshi.startsWith(‘Hello’); //是否在头
ceshi.endsWith(‘!’); //是否在尾
‘x’.repeat(6);//xxxxxx
concat(ceshi,ceshi6)
toLowerCase //变小写
toUpperCase //变大写
match确定原字符是否匹配某个子字符index匹配位置input匹配字符，基本等同search只匹配第一个位置
localeCompare //比较
```

## 布尔

```jsx
变boolean
Boolean(‘true’);
console.log(Boolean(‘’或0或NaN或null或undefined);//flase
```

## 数组

```jsx
var ceshi = new Array();
var ceshi = [‘ceshi6’,‘ceshi666’];
```

方式

```js
//检测数组
console.log(ceshi instanceof Array);
console.log(Array.isArray(ceshi6));//返回布尔值;
push(多) //尾加
pop() //尾减
unshift(多) //头加
shift() //头减
ceshi.reverse() //颠倒顺序
//数组成员排序
ceshi.sort(function(x,x6){
    return x - x6;
    //return x6 - x;
})
ceshi.indexOf(‘pink’) //左查元素在数组中第一次出现的位置
ceshi.lastIndexOf(‘pink’) //右查第一个索引
toString() //数组变字符串
join(‘分隔符’)//指定参数作分隔符将数组连接为字符串
concat()连接多数组
slice(头，尾）//截数组
splice(位置，个数）//删数组一部分成员
[6,66,666].includes(6); //是否包括
数组的valueOf返回数组本身;
m遍历为返回值用map否用forEach; 
some只要一个成员返回值是true; 
every所有成员返回值都是true; 
reduce是从左到右处理reduceRight则是从右到左，第一个参数都是一个函数; 
```

```js
//遍历迭代
var ceshi = [6,6,666];
ceshi.forEach(function(x,x6,x666){
    console.log(x); //值
    console.log(x6); //索引
    console.log(x666); //数组
});
```

```js
//过滤查找满足返回数组
var ceshi = [6,66,666];
var ceshi6 = ceshi.filter(function(x,x6){
    return x > 6;
});
console.log(ceshi6);
```

```js
//查找满足返回布尔值
var ceshi = [‘red’,‘blue’,‘pink’];
var ceshi6 = ceshi.some(function(x){
    return x == ‘pink’;
});
console.log(ceshi6);
```

扩展

```jsx
function ceshi(x,…x6) {};
let [x,…x6] = [6,66,666];
let ceshi = [6,66,666];
console.log(…ceshi); //6 66 666
[…ceshi,…ceshi6]; //或
ceshi.push(…ceshi6);
let divs = document.getElementByTagName(‘div’);
divs = […divs];
```

## 函数

命名函数

```jsx
function ceshi() {
}
ceshi(); //调用
```

匿名函数

```jsx
var ceshi = function(ceshi6){
    console.log(ceshi6);
}
ceshi(‘ceshi6’); //调用
```

箭头函数

```jsx
const ceshi = (x,x6) => x+x6; //简头语句是返回值
```

函数小括号()

```
声明函数小括号未知为形参，调用时小括号已知为实参
函数.arguments可遍历()小括号具length属性
```

## 对象

```jsx
调用obj.p或obj['p'];
查Object.keys(obj);
删delete obj.p;
包'p' in obj;
遍历对象for...in;
new.target //属性判断调用时是否使用new命令
Object.create() //以对象为模板生成新对象用; 
```

原型对象

```jsx
//原型对象就是定义所有实例对象共享的属性和方式
obj instanceof Object;//obj对象是否Object的实例;
Object.prototype.isPrototypeOf(obj) //判断该对象是否为参数对象的原型
obj instanceof Object等同Object.prototype.isPrototypeOf(obj) 
Object.prototype.ceshi //定义所有实例对象共享的属性和方法
Object.prototype.hasOwnProperty(ceshi) //返回布尔值判断某个属性定义
Object.setPrototypeOf //为参数对象设置原型，两个参数，第一个是现有对象，第二个是原型对象
Object.getPrototypeOf //方法返回参数对象的原型
constructor //可以得知某个实例对象是哪个构造函数产生的
__proto__ //返回该对象的原型
Object.getOwnPropertyNames(obj) //方法返回一个数组，成员是参数对象本身的所有属性的键名
Object.getPrototypeOf(f)等同F.prototype
valueOf返回对象的“值”
```

添加属性

```jsx
var obj = {};
obj.x = 1;
obj.y = function () {
    return this.x + this.x;
}
```

修改属性值

```jsx
Object.defineProperty(obj, "x", {})
```

修改多个属性值

```jsx
Object.defineProperties(obj, { x : {}, y : {}, })
```

私有属性名称【可枚举的和不可枚举的】

```jsx
Object.getOwnPropertyNames(obj)
```

私有属性名称【可枚举的】

```jsx
Object.keys(obj);
```

对象属性x的描述符

```
Object.getOwnPropertyDescriptor(obj, "x")
```

删除指定属性

```jsx
delete obj.x;
```

更改对象属性名

```jsx
var ceshi = [
    {
        "Id":"3972679ef2c04151972b376dd88e6413",
        "T_CourseId":"7a4494aae1804d3e94094583249750fe",
        "CourseName":"英语",
        "Code":"english"
    },
    {
        "Id":"5665d803e7994b26a56c6287d12c2090",
        "T_CourseId":"75761ad2ce23498c9f9db134ab844aec",
        "CourseName":"药物化学",
        "Code":"ywhx"
    }
]
   var ceshix= JSON.parse(JSON.stringify(ceshi).replace(/CourseName/g,"title"));
```

```jsx
var ceshi = {
    ceshi6 : ‘pink’,
    ceshi666 : function () {
        console.log(‘’);
    }
};
//或者
var ceshi = new Object();
ceshi.ceshi6 = ‘pink’;
ceshi.ceshi666 = function () {
    console.log(‘’);
}
//调用
console.log(ceshi.ceshi6);
console.log(ceshi[‘ceshi6’]);
console.log(ceshi.ceshi666());
```

构造函数对象

```js
function ceshi(x,x6) {
    this.ceshi6 = x;
    this.ceshi66= function{
        console.log(x6);
    }
} 
var ceshi666 = ceshi(‘pink’,6);
console.log(ceshi666.ceshi6);
console.log(ceshi666[‘ceshi6’]);
console.log(ceshi666.ceshi66());
```

遍历对象

```js
for(var x in ceshi){
    console.log(x); //对象左侧
    console.log(ceshi[x]);
}
//删除对象的某个属性
for(var item in data){
      if (item == 'b') {
        delete data[item];
      } 
}
```

## 事件

鼠标

```css
onclick //左击
onmouseover //路过
onmouseout //离开
onfocus //来焦点
onblur //离焦点
onmousemove //移动
ommouseup //弹起
onmousedown //按下
```

键盘

```jsx
keypress
keyup
```

手指触屏

```jsx
touchstart //摸到
touchmove //滑动
touchend //移开
e.touches //摸到屏幕列表
e.targetTouches //摸到ceshi列表
e.changedTouches //摸到列表变化
e.targetTouches[6] //坐标信息
    var div = document.querySelector('div');
    div.addEventListener('touchmove',function(e){
        div.style.left = (e.targetTouches[0].pageX - this.scrollWidth) + 'px'; //坐标
        div.style.top = (e.targetTouches[0].pageY - this.scrollHeight) + 'px';
        e.preventDefault(); //阻止默认屏幕滚动等
    });
```

## 语句

```jsx
var x = (条件) ? 表达式1 : 表达式2先判断后赋值;
```

```jsx
label:语句break后跟label跳出位置label;
```

swith

```jsx
//case内break语句否会接下一个case
swith(){
    case ceshi:
    	break;
    case ceshi6:
    	break;
    default:
}
```

while

```jsx
while(条件)语句假时跳出;
continue跳出本轮break跳出终止;
```

## 日期

```js
var ceshi = new Date(); //构造函数先new
var ceshi6 = ceshi.getMonth() + 1;
var ceshi666 = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六'];
//方式
console.log(ceshi.getFullYear() + '年' + ceshi6 + '月' + ceshi.getDate() + '日' + ceshi666[ceshi.getDay()] + ceshi.getHours() + '点' + ceshi.getMinutes() + '分' + ceshi.getSeconds() + '秒');
Date.now //当前时间
Date.parse //解析日期字符
Date.UTC //接受年、月、日等变量作为参数;
```

## 其它

```jsx
continue; //跳出本次循环继续下一次循环
break; //跳出整个循环
```

# Hook

## 导包

```jsx
import React, { useState, useEffect } from 'react';
import ReactDOM from 'react-dom'
```

## useState状态

```jsx
function ExampleState() {
    // 声明一个新的叫做 “count” 的 state 变量
    const [count, setCount] = useState(0);
    // 声明多个 state 变量！
    const [age, setAge] = useState(42);
    const [fruit, setFruit] = useState('banana');
    const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
    //
    return (
        <div>
            //在函数中可以直接用count
            <p>You clicked {count} times</p>
            //在函数中已经有setCount和count变量不用this
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

#### class 有状态组件【类】

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

#### class 无状态组件【函数】

```jsx
const Example = (props) => {
  // 你可以在这使用 Hook
  return <div />;
}
```

```jsx
function Example(props) {
  // 你可以在这使用 Hook
  return <div />;
}
```

## 数组解构useState

使用 `useState` 定义 state 变量时候，它返回一个有两个值的数组。第一个值是当前的 state，第二个值是更新 state 的函数。

```jsx
const [fruit, setFruit] = useState('banana');
//数组解构同时创建fruit和setFruit两个变量，fruit值为useState返回的第一个值，setFruit是返回的第二个值
var fruitStateVariable = useState('banana'); // 返回一个有两个元素的数组
var fruit = fruitStateVariable[0]; // 数组里的第一个值
var setFruit = fruitStateVariable[1]; // 数组里的第二个值
```

## useEffect渲染函数

- 熟悉 React class 的生命周期函数，你可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合
- 想**在 React 更新 DOM 之后运行一些额外的代码。**比如发送网络请求，手动变更 DOM，记录日志，这些都是常见的无需清除的操作。

```jsx
function ExampleEffect() {
    const [count, setCount] = useState(0);

    // 相当于 挂载componentDidMount 和 更新componentDidUpdate:
    useEffect(() => {
        document.title = `You clicked ${count} times`;
    });
    //组件中多次使用 useEffect
    useEffect(() => {
        //设置定时器相当于componentDidMount组件渲染时函数
        ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
        return () => {
            //删除定时器相当于componentDidUpdate组件销毁时函数
            ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
        };
    }, [props.friend.id]); // 仅在 props.friend.id 发生变化时调用

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

#### 插件

推荐启用 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 中的 [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) 规则。此规则会在添加错误依赖时发出警告并给出修复建议。

#### class组件示例

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }
  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

## Hook间传递信息

```jsx
const friendList = [
  { id: 1, name: 'Phoebe' },
  { id: 2, name: 'Rachel' },
  { id: 3, name: 'Ross' },
];

function ChatRecipientPicker() {
  const [recipientID, setRecipientID] = useState(1);
  //调用组件信息
  const isRecipientOnline = useFriendStatus(recipientID);

  return (
    <>
      //调用组件信息
      <Circle color={isRecipientOnline ? 'green' : 'red'} />
      <select
        value={recipientID}
        onChange={e => setRecipientID(Number(e.target.value))}
      >
        {friendList.map(friend => (
          <option key={friend.id} value={friend.id}>
            {friend.name}
          </option>
        ))}
      </select>
    </>
  );
}
```

## useContext对象全局状态样式

- `useContext(MyContext)` 相当于 class 组件中的 `static contextType = MyContext` 或者 `<MyContext.Consumer>`
- `useContext(MyContext)` 只是让你能够*读取* context 的值以及订阅 context 的变化。你仍然需要在上层组件树中使用 `<MyContext.Provider>` 来为下层组件*提供* context

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

## useReducer状态管理

- 如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 `dispatch`

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  //reducer状态管理函数，initialState状态state初值
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
		//dispatch(action对象)
      <button onClick={() => dispatch({type: 'decrement'， ceshi: 'x'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

- 惰性初始化将 `init` 函数作为 `useReducer` 的第三个参数，这样初始 state 将被设置为 `init(initialArg)`

  ```jsx
  function init(initialCount) {
    return {count: initialCount};
  }
  
  function reducer(state, action) {
    switch (action.type) {
      case 'increment':
        return {count: state.count + 1};
      case 'decrement':
        return {count: state.count - 1};
      case 'reset':
        return init(action.payload);
      default:
        throw new Error();
    }
  }
  
  function Counter({initialCount}) {
    const [state, dispatch] = useReducer(reducer, initialCount, init);
    return (
      <>
        Count: {state.count}
        <button
          onClick={() => dispatch({type: 'reset', payload: initialCount})}>
          Reset
        </button>
        <button onClick={() => dispatch({type: 'decrement'})}>-</button>
        <button onClick={() => dispatch({type: 'increment'})}>+</button>
      </>
    );
  }
  ```

## useCallback函数返回方式

该回调函数仅在某个依赖项改变时才会更新

useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

## useMemo函数返回数据

如果没有提供依赖项数组，`useMemo` 在每次渲染时运作

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

## useRef可变对象绑定target.current

```jsx
const refContainer = useRef(initialValue);
```

- `useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内保持不变。
- 当 ref 对象内容发生变化时，`useRef` 并*不会*通知你。变更 `.current` 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://react.docschina.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

#### ref扩展useImperativeHandle

```jsx
useImperativeHandle(ref, createHandle, [deps])
```

`useImperativeHandle` 可以在使用 `ref` 时自定义暴露给上层组件

渲染 `<FancyInput ref={inputRef} />` 组件可以调用 `inputRef.current.focus()`

```jsx
function FancyInput(props, ref) {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }));
  return <input ref={inputRef} ... />;
}
FancyInput = forwardRef(FancyInput);
```

#### 上一轮的 props 或 state

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  const prevCountRef = useRef();
  useEffect(() => {
    prevCountRef.current = count;
  });
  const prevCount = prevCountRef.current;

  return <h1>Now: {count}, before: {prevCount}</h1>;
}
```

## useLayoutEffect渲染管理

- 其签名与 `useEffect` 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染
- 可以通过使用 `showChild && <Child />` 进行条件渲染，并使用 `useEffect(() => { setShowChild(true); }, [])` 延迟

## 开发者工具中显示useDebugValue

```jsx
useDebugValue(value)
```

- `useDebugValue` 可用于在 React 开发者工具中显示

```jsx
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  // 在开发者工具中的这个 Hook 旁边显示标签
  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? 'Online' : 'Offline');

  return isOnline;
}
```

- `useDebugValue` 接受一个格式化函数作为可选的第二个参数

```jsx
useDebugValue(date, date => date.toDateString());
```

# React Hook组件间传值的四种方式

### 一、父组件传值给子组件（props）

父组件想传递任何东西，都可以通过props来传递给子组件。
例如：变量，函数、jsx组件等等。

```jsx
import React, { useState } from 'react';

// 父组件
const PropsCom = () => {
    const [name, setName] = useState('winne');

    return (
        <div>
            <h2>父组件</h2>
            {/* 这里是重要代码，向子组件传递parentName这个prop，值为name变量 */}
            <ChildrenCom parentName={name} />
        </div>
    );
};

// 子组件
const ChildrenCom = (props) => (
    <div>
        <h4>子组件</h4>
        <p>获取父组件传过来的值：{props.parentName}</p>
    </div>
);

export default PropsCom;

```

### 二、父组件给后代组件传值（context）

```jsx
const value = useContext(MyContext);
```

useContext接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。

使用context 可以实现跨组件传值。

完整使用例子（举例的目录参考）：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706105612474.png)

##### 1、首先我们新建个createContext.js文件（方便扩展和引用）

```jsx
// createContext.js文件
import { createContext } from 'react';
const myContext = createContext(null);
export default myContext;
```

##### 2、在index.js文件写如下代码（我们的最顶层Test组件）

```jsx
import React, { useReducer } from 'react';
import { Button } from 'antd';

import myContext from './createContext';
import BrotherTest from './BrotherTest';

const reducer = (state, action) => {
    const [type, payload] = action;
    switch (type) {
        case 'set':
            return {
                ...state,
                ...payload,
            };
        default:
            return {
                ...state,
                ...payload,
            };
    }
};

const initData = {
    count: 0,
    text: 'Text-顶层组件',
};

const Test = () => {
    const [state, dispatch] = useReducer(reducer, initData);

    return (
        <div style={{ backgroundColor: '#f2f2f2' }}>
            <h1>
                Test最顶层组件----实现跨组件间传值。
            </h1>
            <Button
                onClick={() => {
                    dispatch(['set', { count: state.count + 1 }]);
                }}
            >
                点我修改count
            </Button>
            <Button
                onClick={() => {
                    dispatch([
                        'set',
                        { text: '最顶层组件Test修改了自己的text---' },
                    ]);
                }}
            >
                点我修改text
            </Button>
            <br />
            Test组件的最顶层组件----count：{state.count}
            <br />
            Test组件的最顶层组件----text：{state.text}
            <br />
            <myContext.Provider value={{
                count: state.count,
                text: state.text,
                // 把最顶层Test组件的dispatch传递下去给后代组件，这样后代组件就都能修改最顶层组件的数据了。
                proDispatch: dispatch,
            }}
            >
                {/* 子组件 */}
                <BrotherTest />
            </myContext.Provider>
        </div>
    );
};

export default Test;
```

#### 3、在BrotherTest.js 和 InTest.js文件中写入如下代码

从截图中可以看到，通过context上下文这种方式，我们能实现**跨组件的传值和操作**，不需要再一层一层通过props来传值。

![](C:\Program Files\Typora\20210706110444784.png)

### 三、父组件调用子组件的函数（useImperativeHandle & forwardRef）

如果想在父组件中调用子组件的某个函数，或者是使用子组件的某个值，可以使用这个方式（尽量少用）。

react官网的一段文字描述：
useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 ref 这样的命令式代码。useImperativeHandle 应当与 forwardRef 一起使用。

```jsx
import React, {
    useState,
    useRef,
    useImperativeHandle,
    forwardRef,
} from 'react';
import { Button } from 'antd';

// 父组件
const ParentCom = () => {
    // 获取子组件实例
    const childRef = useRef();

    // 调用子组件的onChange方法
    const onClickChange = () => {
        childRef.current.onChange();
    };

    return (
        <div>
            <h2>父组件</h2>
            <Button onClick={onClickChange}>点击调用子组件onChange函数</Button>
            <ChildrenCom ref={childRef} />
        </div>
    );
};

// 子组件
const ChildrenCom = forwardRef((props, ref) => {
    const [value, setValue] = useState(0);
    const [name, setName] = useState('winne');

    // 自定义暴露给父组件的实例值 (useImperativeHandle 要配合 forwardRef使用)
    useImperativeHandle(ref, () => ({
        // 暴露函数给父组件调用
        onChange,
        // 也可以暴露子组件的状态值给父组件使用
    }));

    const onChange = () => {
        setValue(value + 1);
        setName(name === 'winne' ? 'xf' : 'winne');
    };

    return (
        <div>
            <h4>子组件</h4>
            <p>子组件的value: {value}</p>
            <p>子组件的name: {name}</p>
            <Button onClick={onChange}>点击改变value和name</Button>
        </div>
    );
});

export default ParentCom;
```

### 四、子组件传值给父组件（父组件props传递回调函数）

如果子组件想向父组件传递某些值，或者是子组件在执行某一段逻辑后想执行父组件中的某一段逻辑，此时可以在父组件中写好对应的逻辑函数，通过props传递这个函数给子组件进行调用即可。

如果传递的函数需要进行昂贵的计算，需要优化的时候使用useCallback配合memo 。（使用方法可以参考：这里）

```jsx
import React, { useState } from 'react';
import { Button } from 'antd';

// 父组件
const CallbackCom = () => {
    const [count, setCount] = useState(0);

    // 获取子组件传过来的value值并设置到count，val参数就是子组件的value值
    const getChildrenValue = (val) => {
        setCount(val);
    };

    return (
        <div>
            <h2>父组件</h2>
            <p>获取子组件传过来的值：{count}</p>
            {/* 这里是重要代码，向子组件传递getValue这个prop，它的值是一个回调函数 */}
            <ChildrenCom getValue={getChildrenValue} />
        </div>
    );
};

// 子组件
const ChildrenCom = (props) => {
    const [value, setValue] = useState(0);

    const addValue = () => {
        setValue(value + 1);
        // 向父组件传递每次递增的value值
        props.getValue(value + 1);
    };

    return (
        <div>
            <h4>子组件</h4>
            <Button onClick={addValue}>点击改变子组件的value值：{value}</Button>
        </div>
    );
};

export default CallbackCom;
```

## 规则插件

linter 插件https://www.npmjs.com/package/eslint-plugin-react-hooks规则

推荐启用 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 中的 [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) 

- 只能在**函数最外层**调用 Hook。不能在循环、条件判断或者子函数中调用。

  ```jsx
    // 🔴 在条件语句中使用 Hook 违反第一条规则
    if (name !== '') {
      useEffect(function persistForm() {
        localStorage.setItem('formData', name);
      });
    }
    useEffect(function persistForm() {
      // 👍 将条件判断放置在 effect 中
      if (name !== '') {
        localStorage.setItem('formData', name);
      }
    });
  ```

  

- 只能在 **React 的函数组件**中调用 Hook。不能在其他 JavaScript 函数class组件中调用，在 React 函数的最顶层以及任何 return 之前调用

# TypeScript

- 安装

  ```
  npm install -g typescript
  ```

- `.ts`扩展名

- 声明类型

```jsx
function greeter(person: string) {
}
interface Person {
    firstName: string;
    lastName: string;
}
//类
//构造函数的参数上使用public等同于创建了同名的成员变量
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}
//无状态的功能组件
// 'HelloProps' describes the shape of props.
// State is never set so we use the '{}' type.
export class Hello extends React.Component<HelloProps, {}> {
    render() {
        return <div></div>;
    }
}
//在src下创建index.tsx文件
//将Hello组件导入index.tsx
import { Hello } from "./components/Hello";

ReactDOM.render(
    <Hello compiler="TypeScript" framework="React" />,
    document.getElementById("example")
);
//在根目录 proj创建一个名为index.html的文件

```



# AJAX

```jsx
function ProductPage({ productId }) {
    const [product, setProduct] = useState(null);

    useEffect(() => {
        // 把这个函数移动到 effect 内部后，我们可以清楚地看到它用到的值。    async function fetchProduct() {      const response = await fetch('http://myapi/product/' + productId);      const json = await response.json();      setProduct(json);    }
        fetchProduct();
    }, [productId]); // ✅ 有效，因为我们的 effect 只用到了 productId  // ...
}
```

# React Router

## 安装

```jsx
npm install --save react-router
```

## 导包使用

```jsx
import React from 'react'
import { render } from 'react-dom'

import { Router, Route, Link } from 'react-router'

const App = React.createClass({
  render() {
    return (
      <div>
        <h1>App</h1>
        <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/inbox">Inbox</Link></li>
        </ul>
        {this.props.children}
      </div>
    )
  }
})

const About = React.createClass({
  render() {
    return <h3>About</h3>
  }
})

const Inbox = React.createClass({
  render() {
    return (
      <div>
        <h2>Inbox</h2>
        {this.props.children || "Welcome to your Inbox"}
      </div>
    )
  }
})

const Message = React.createClass({
  render() {
    return <h3>Message {this.props.params.id}</h3>
  }
})

React.render((
  <Router>
    <Route path="/" component={App}>
        {/* 当 url 为/时渲染 Dashboard */}
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        {/* 使用 /messages/:id 替换 messages/:id */}
        <Route path="/messages/:id" component={Message} />
          {/* 跳转 /inbox/messages/:id 到 /messages/:id */}
        <Redirect from="messages/:id" to="/messages/:id" />
        <Route path="/hello/:name"> //匹配 /hello/michael 和 /hello/ryan
        <Route path="/hello(/:name)"> //匹配 /hello, /hello/michael 和 /hello/ryan
        <Route path="/files/*.*"> //匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
      </Route>
    </Route>
  </Router>
), document.body)
```

React Redux connect()

# API

## document.

```jsx
.title //主题标题
document.getElementById(‘’);
document.getElementByClassName(‘’);
document.getElementByTagName(‘’); //伪数组可length遍历
document.querySelector(‘#或.’); //指定第一个元素
document.querySelectorAll(‘.’);
document.createElement(‘’);
document.body
document.html
document.documentElement
console.dir(x); //查看属性方法
```

## node.

```jsx
nodeType //属性返回一个整数值，表示节点的类型
document.nodeType //等同Node.DOCUMENT_NODE
nodeName //属性返回节点的名称
nodeValue //属性返回一个字符，表示当前节点本身的文本值
textContent //属性返回当前节点和它的所有后代节点的文本内容
baseURI //属性返回一个字符，表示当前网页的绝对路径
Node.ownerDocument //属性返回当前节点所在的顶层文档对象
Node.nextSibling //属性返回紧跟在当前节点后面的第一个同级节点
previousSibling //属性返回当前节点前面的、距离最近的一个同级节点
parentNode //属性返回当前节点的父节点
parentElement //属性返回当前节点的父元素节点
firstChild //属性返回当前节点的第一个子节点
lastChild //属性返回当前节点的最后一个子节点
childNodes //属性返回数组对象（NodeList集合）包括当前节点的所有子节点
isConnected //返回布尔值，表示当前节点是否在文档之中
appendChild //方式接受一个节点对象作为参数，将其作为最后一个子节点，插入当前节点
hasChildNodes //方式返回一个布尔值，表示当前节点是否有子节点
cloneNode //方式用于克隆一个节点
insertBefore //方式用于将某个节点插入父节点内部的指定位置
removeChild //方式接受一个子节点作为参数，用于从当前节点移除该子节点
replaceChild //方式用于将一个新的节点，替换当前节点的某一个子节点
contains //方式返回一个布尔值
compareDocumentPosition //方式的用法，与contains方法完全一致，返回一个七个比特位的二进制值，表示参数节点与当前节点的关系
isEqualNode //方式返回一个布尔值，用于检查两个节点是否相等
isSameNode //方式返回一个布尔值，表示两个节点是否为同一个节点
normailize //方式用于清理当前节点内部的所有文本节点（text）
getRootNode //方式返回当前节点所在文档的根节点
getRootNode //方式返回当前节点所在文档的根节点
item //方式接受一个整数值作为参数，表示成员的位置
keys() //返回键名的遍历器
values() //返回键值的遍历器
entries() //返回的遍历器同时包含键名和键值的信息
HTMLCollection //是一个节点对象的集合，只能包含元素节点（element），不能包含其他类型的节点
ParentNode //接口表示当前节点是一个父节点，提供一些处理子节点的方法
ChildNode //接口表示当前节点是一个子节点
children //属性返回一个HTMLCollection实例，成员是当前节点的所有元素子节点
firstElementChild //属性返回当前节点的第一个元素子节点
lastElementChild //属性返回当前节点的最后一个元素子节点
childElementCount //属性返回当前节点的所有元素子节点的数目
append //方式为当前节点追加一个或多个子节点
prepend //方式为当前节点追加一个或多个子节点
remove //方式用于从父节点移除当前节点
replaceWith //方式使用参数节点，替换当前节点
```

## .内容

```jsx
.innerText //不识别标签
.innerHTML
.trim()
```

## .style

```jsx
.style.backgroundColor = ‘’; //驼峰type,value,checked,selected,disabled
.className = ‘’;
img.src = ‘.jpg’;
img.title = ‘’;
.getAttribute(‘class’);
.setAttribute(‘class’,’x’); //设置
.removeAttribute(‘class’); //删除
```

## div.

```jsx
<div data-ceshi = “6” data-ceshi6-x = “c”></div>
div.dataset //伪数组可length遍历
div.dataset.ceshi6X //驼峰
div.dataset[‘ceshi’]
div.parentNode
ul.childNodes
ul.childNodes[6].nodeType
ul.children //伪数组
ul.firstChild //或lastChild
ul.firstElementChild //或lastElementChild
div.nextSibling //或previousSibling
div.nextElementSibling //或previousElementSibling
ul.appendChild(li); //或
ul.insertBefore(li,ul.children[6]);
ul.removeChild(ul.children[6]);
var li = ul.children[6].cloneNode(true或flase); //深浅拷贝
ul.appendchild(li);
```

## .addEventListener与.attachEvent

```jsx
.addEventListener(‘click’,function);
.attachEvent(‘onclick’,function); //冒泡
    //兼容
    funtction ceshi(button,click,fn){
        if(button.addEventListener){
            button.addEventListener(‘click’,fn);
        }
        else if(button.attachEvent){
            button.attachEvent(‘on’ + click,fn);
        }
        else button[‘on’ + click] = fn;
    }
//删除
.onclick = null;
.removeEventListener(‘click’,fn);
.detachEvent(onlick,fn);
    //兼容
    function ceshi(div,click,fn){
        if(div.removeEventListener){
            div.removeEventListener(‘click’,fn);
        }
        else if(div.detachEvent){
            div.detachEvent(‘on’ + click,fn);
        }
        else div[‘on’ + click] = fn;
    }
```

## e.

```jsx
e.target //对象标准e.srcElement非标准
e.type //类型
e.clientX //可视坐标，或clientY
e.pageX //页面坐标，或pageY
e.screenX //屏幕坐标，或screenY
e.keyCode //键盘ASCII
e.stopPropagation() //阻止冒泡标准e.cancelBubble非标准
e.preventDefault() //阻止默认标准e.returnValue非标准
    //兼容
    if(e && e.stopPropagation){
        e.stopPropagation();
    }else{
        windows.event.cancelBubble = true;
    }
```

## console.

```jsx
console.log //将依次用后面的参数替换占位符
log //方法是写入标准输出，warn方法和error方法是写入标准错误
console.table //表格显示
count //用于计数
dir //对一个对象检查
dirxml //以目录树形式
console.assert //程序运行过程中进行条件判断time计时开始timeEnd计时结束
console.group //和console.groupEnd显示信息分组
console.trace //执行代码在堆栈中的调用路径
console.clear //清除控制台
debugger //除错设置断点;
```

## 禁菜单

```jsx
document.addEventListener(‘contextmenu’,function(e){ //禁右键菜单
    e.preventDefault();
});
document.addEventListener(‘selectstart’,function(e){ //禁框
    e.preventDefault();
});
```

## 页面加载windows.

```jsx
windows.onload = function; //页面加载后一次
windows.addEventListener(‘load’,function); //不限制
document.addEventListener(‘DOMContentLoaded’,function); //DOM加载后
window.onresize = function(){ //窗口大小
    console.log(window.innerWidth); //屏幕宽度
};
window.addEventListener(‘resize’,function(){});
```

## promise

```jsx
var p1 = new Promise(f1)
Promise构造函数接受一个回调函数f1作为参数，f1里面是异步操作的代码
然后，返回的p1就是一个 Promise 实例
then方法用来指定下一步的回调函数
```



## 定时器setTimeout与setInterval

```jsx
setTimeout(function,6); //设定时器
clearTimeout(定时器名); //删定时器
var ceshi = setInterval(function,6); //重复定时器
clearInterval(ceshi); //删掉
```

## this

```jsx
.bind(ceshi);将函数的this绑定到ceshi对象
.call(ceshi, arg1, arg2, ...);
.apply(ceshi, [arg1, arg2, ...]);
this指的window,方法指的调用者，构造指的实例
```

```jsx
//指的window
function ceshi(){
    console.log(this);
};
window.ceshi();
//指的ceshi方法对象
var ceshi = {
    x = function(){
        console.log(this);
    };
};
ceshi.x();
//指的ceshi6实例对象
function ceshi(){};
ceshi.prototype.x = function() {
    console.log(this);
};
var ceshi6 = new ceshi();
ceshi6.x();
//指的button事件对象
var button = document.querySelector(‘button’);
button.onclick = function() {
    console.log(this);
};
//指的window
window.setTimeout(function(){
    console.log(this);
},666);
//指的window
(function() {console.log(this);})();
//改变this指的x调用fn
function fn(ceshi,ceshi6) {
    console.log(ceshi);
    console.log(ceshi6);
};
var x = {
    ceshi666 = ‘pink’;
};
fn.call(x,ceshi,ceshi6);
```

## location.

```jsx
location.href
location.host //主机域名
location.port //端口
location.pathname
location.search //参数
location.hash //#
location.assign() //变页面
location.replace() //替换页面
location.reload() //刷新页面
```

## 访问设备

```jsx
if((navigator.userAgent.match(/(phone|pad|pod|iphone|ipod|ios|ipad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows phone)/I))){
    window.location.href = “”; //手机
    }else {
    window.location.href = “”/; //电脑
	}
}
```

## history

```jsx
history //对象
back() //上个页面
forward() //下个页面
go(6) //上第六个页面
```

## .浏览器

```jsx
.offsetParent
.offsetTop //position
.offsetLeft //没有单位
.offsetWidth //border盒
.offsetHeight
.clientTop //边框
.clientLeft
.clientWidth //padding盒
.clientHeight
.scrollTop //滚上
.scrollLeft
.scrollWidth //content
.scrollHeight
    //兼容
    function ceshi() {
        return{
            left: window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft || 0,
            top: window.pageYOffset || document.documentElement.scrollTop || document.body.scroolTop || 0
        };
    }
    ceshi().left
```

## classList

```jsx
div.classList.add('');
div.classList.remove('');
div.classList.toggle('');
div.classList[6]); //伪数组class
```

## 移动端click延时解决

```jsx
//fastclick插件解决延时https://github.com/ftlabs/fastclick
<meta name = “viewport” content = “user-scalable = no”> //禁双击缩放
    //或
    function ceshi(x,fn) {
        var timer = 0;
        var flag = false;
        x.addEventListener(‘touchsart’,function(e) {
            timer = Date.now();
        };
                           x.addEventListener(‘touchmove’,function(e) {
            flag = true;
        };
        x.addEventListener(‘touchend’,function(e) {
            if(!flag && (Date.now() - timer) < 150) {
                fn && fn();
            }
            timer = 0;
            flag = false;
        };
                           };
```

## sessionStorage.与localStorage.

```jsx
var ceshi = document.querySelector(‘div’);
ceshi.addEventListenter(‘click’,function(){
    sessionStorage.setItem(‘x’,6); //同窗口共享生命窗口
    sessionStorage.getItem(‘x’);
    sessionStorage.removeItem(‘x’);
    sessionStorage.clear();
    localStorage.setItem(‘x’); //同浏览器共享生命永久
    localStorage.getItem(‘x’);
    localStorage.removeItem(‘x’);
    localStorage.clear(‘x’);
}
```

## 构造函数

```jsx
class Cs { //命名规则
    constructor(ceshi,ceshi6) { //类自带constructor构造函数可忽略function
        this.ceshi = ceshi;
        this.ceshi6 = ceshi6;
    };
    zidingyi() { //自定义
        console.log(this.ceshi + this.ceshi6);
    };
};
class Cs6 extends Cs {
    constructor(ceshi,x) {
        super(ceshi); //调上面constructor(ceshi)
        this.x = x;
    };
};
//实例调用
var x6 = new Cs(‘pink’,6);
console.log(x6);
```

```jsx
//构造函数
function ceshi(x,x6) {
    this.x = x;
    this.x6 = x6;
};
//构造函数自带prototype原型对象作用共享自定义
ceshi.prototype.ceshi6 = function() {
    console.log(ceshi6);
};
var cs = new ceshi(‘pink’,6);
cs.ceshi6();
//对象自带_proto_查询对象原型
cs._proto_ 
ceshi.prototype.constructor
//构造函数对象原型自带constructor指的构造函数
cs._proto_.constructor
```

## Object.

```jsx
Object.keys(ceshi); //属性数组
Object.defineProperty(ceshi,’x’{
                      value: 666; //改属性值
                      writable: true或false; //可否改
                      enumerable: true或false; //可否枚举
                      configurable: true或false; //可否删除
                      });
Object.assign
```

## 严格模式

```js

//以下严格模式
<script>
    ‘use strict’;
//以内严格模式
function ceshi() {
    ‘use strict’;
};
</script>
```

## 正则表达式

```jsx
var ceshi = new RegExp(/666/); //或
var ceshi6 = /666666/; //包含666666
ceshi6.test(6)
/^666/; //开头666
/^666$/; //准666
/[666]/; //包含任意
/^[666]$/; //准6
/^[a-z]$/; //准某个
/^abc{6}$/; //准abcccccc
/^(abc){6}$/; //准abcabcabcabcabcabc
```

## 对象数组

```jsx
let x = {
    ‘0’: ‘ceshi’,
    ‘1’: ‘ceshi6’,
    ‘2’: ‘ceshi66’,
    length: 3
};
let x6 = Array.from(x); //[‘ceshi’,‘ceshi6’,‘ceshi66’]
let x66 = Array.from(x,item => item + ‘6’);
let ceshi = [{
    id: 6,
    x: ‘pink’
},{
    id: 66,
    x: ‘pink6’
}];
let ceshi6 = ceshi.find((item,index) => item.id == 6); //符合的成员
let ceshi = [6,66,666];
let ceshi6 = ceshi.findIndex((value,index) => value == 6); //符合的位置
```

## ES

```jsx
let ceshi = ‘pink’;
let ceshi6 = `feichangbucuo${ceshi}`;
let ceshi = {
    x: ‘pink’,
    x6: ‘pink6’
};
let html = `<div>
<span>${ceshi.x}</span>
<span>${ceshi.x6}</span>
</div>`;
const ceshi = function() {
    return ‘pink’;
};
let ceshi6 = `${ceshi()}`;
```

## Set

```jsx
const ceshi = new Set([6,66,666]); //结构唯一性
ceshi.size
…ceshi
ceshi.add(6);
ceshi.delete(6);
ceshi.has(6);
ceshi.clear();
ceshi.forEach(value => console.log(value)); //遍历结构
```

# 其它

框架bootstrap,vue,angular,react

插件swipes,superslide,iscroll

api查询网https://developer.mozilla.org/zh-CN/

api查询https://developer.mozilla.org/zh-CN/docs/Web/API

https://https://developer.Mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions
http://tool.oschina.net/regex

# React控制元素显示隐藏

变量控制

```jsx
<div>{this.state.showElem?(<div>显示的元素</div>):null}</div>
```

display属性

```jsx
<div style={{display:this.state.showElem}}>显示的元素</div>
```

className切换hide

```jsx
<div className={this.state.showElem?'word-style':'word-style hide'}>显示的元素</div>
<div className={`${this.state.showElem?'':'hide'} word-style`}>显示的元素</div>
```

# 爬网站api

```html
//html设置get不带referrer信息
<meta name="referrer" content="never">
//chrome://flags/浏览器打开设置Cookie deprecation messages关闭disabled跨域第三方Cookie
```

1. package.json配置

   ```jsx
   //主目录配置跨域
   "proxy": "http://geapi.5nd.com" //跨域网址
   ```

2. fetch网址

   ```jsx
   useEffect(() => {
       	//网址
           fetch("http://geapi.5nd.com/a/ar5bc.ashx?_c=mtest&_p=bXRlc3Q&nd=get2me&t=104&ids=2",
               {
               	//请求头
                   headers: {
                       'content-type': 'application/json',
                   },
               	//方式
                   method: 'GET',
               })
       		//文件类型或者res.json()
               .then(res => res.text())
       		//爬数据
               .then(
                   (result) => {
                       //变json
                       const resultx = JSON.parse(
                           //修改text
                           result.replace('get2me(', '').replace(');', '')
                       )
                       //成功
                       setData(resultx.data)
                       console.log(resultx.data)
                   },
               	//报错
                   (error) => {
                       console.log(error)
                       error
                   }
               )
       }, [])
   ```


# 网络

```jsx
//HTTPS
HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传 输、身份认证的网络协议，比 http 协议安全。
    tcp 三次握手，客户端和服务端都需要直到各自可收发。
    TCP 是面向连接的1对1，udp 是无连接的1对1,1对多即发送数据。
WebSocket 是 HTML5 中的协议，支持持久连续，http 协议不支持持久性连接，基于 Http 协议。
HTTP 请求的方式head类似于 get 请求，只不过返回的响应中没有具体的内容，用户获取报头。
//Bom 是浏览器对象
location.hash返回 URL#后面的内容
location.port返回 URL 中的端口部分
history.forward()前进一页。
//HTML5 drag api 
dragstart 开始拖放元素触发
darg 正在拖放元素触发
dragenter拖放元素进入某元素时触发
dragover拖放在某元素内移动时触发
dragleave拖放元素移出目标元素触发
drop目标元素完全接受被拖放元素触发
dragend拖放操作结束触发。
//HTTP2.0
请求资源时间少，访问速度快，会将所有的传输信息分割为更小的信息或者帧，并对他们进行二进制编码首部压缩服务器端推送。
400 状态码:请求无效，前端提交数据字段名称类型与后台不一致，后台数据是json，前端没通过 JSON.stringify 实现序列化
401 状态码:当前请求需要用户验证
403 状态码:服务器已经得到请求，但是拒绝执行
//fetch
发送 post 请求总是发送 2 次，第一次状态码是 204，第二次才成功? 因为用 fetch 的 post 请求，第一次发送了一个 Options 请求，询问服务器是否支持修改的请求头，如果服务器支持，则在第二次中发送真正的请求。
//Cookie、sessionStorage、localStorage 都保存在浏览器端，是同源的
cookie 数据始终在同源的 http 请求中携带(即使不需要)，在浏览器 和服务器间来回传递，在过期时间前一直有效，即使窗口和浏览器关闭
sessionStorage仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持
localStorage始终有效，窗口或浏览器关闭也一直保存
//web worker
运行在后台的 js，独立于其他脚本，不会影响页面性能。并且通过 postMessage 将结果回传到主线程。这样在进行复杂操作的时候，就不会阻塞主线程了
//iframe
创建包含另个文档的内联框架，会阻塞主页面的 onload，搜索引擎SEO不解读，和主页面共享连接池，而浏览器对相同区域有限制所以会影响性能
//Doctype
声明于文档最前面，告诉浏览器以严格或混杂模式渲染，严格模式排版和JS运作以浏览器最高标准运行。 混杂模式，向后兼容模拟老浏览器，防止浏览器不兼容
//XSS(跨站脚本攻击)
在返回的HTML中嵌入javascript 脚本，为了减轻这些攻击，需在HTTP头部配上set-cookie:httponly-属性防止XSS,禁止 javascript 脚本来访问cookie
secure - 属性仅在请求https时发送cookie
HTTP 是无状态协议，Cookie最大作用就是存储 sessionId用来唯一标识用户
RESTFUL是用 URL 定位资源，用 HTTP 描述操作
//click在 ios 上有 300ms 延迟，原因及解决，粗暴型
禁用缩放<meta name="viewport" content="width=device-width, user-scalable=no">
利用 FastClick，其原理是:检测到 touchend 事件后，立刻出发模拟 click 事件，并且把浏览器 300 毫秒之后真正发的事件给阻断掉
//鼠标点击onclick、页面滚动onscroll 等等
可添加事件侦听器预订阶段 addEventListener(event, function, useCapture)
    event//指定事件类型
    fn//指定要事件触发时执行的函数
    useCapture//指定事件是否在true捕获或false冒泡阶段调用处理程序。
//cookie数据放在客户浏览器上，session数据放在服务器上。
//get
参数只能通过 url 传递编码，有长度限制，暴露在 url 中不安全，保留在浏览历史记录里，用户获取数据，可以不用每次都与数据库连接，可以使用缓存，post 放在 request body 中，支持多种编码方式，参数不会被保留，一般是修改和删除的工作，所以必须与数据库交互，所以不能使用缓存，GET 产生一个 TCP 数据包（http header 和 data 一并发送）
//POST
产生两个 TCP 数据包(先发送 header，再发送 data）。
//画三角形原理:边框原理
div {
    width:0px;
    height:0px;
    border-top:10px solid red; 
    border-right:10px solid transparent; 
    border-bottom:10px solid transparent; 
    border-left:10px solid transparent;
}
//状态
状态码 200:请求已成功
状态码 304:发送的 GET 请求已被允许，而文档的内容(自上次访问以来或者根据请求的条件)并没有改变，则服务器应当返回这个状态码
//header
缓存分强缓存和协商缓存，根据响应的 header 内容来决定。 强缓存相关字段有 expires，cache-control。如果 cache-control 与 expires 同时存在的话， cache-control 的优先级高于 expires。
协商缓存相关字段有 Last-Modified/If-Modified-Since，Etag/If-None-Match。
cache-control 是通用消息头字段被用于 HTTP 请求和响应中，通过指定指令来实现 缓存机制，这个缓存指令是单向的，常见的取值有 private、no-cache、max-age、 must-revalidate 等，默认为 private。
//浏览器生成页面
生成两棵树，DOM树和CSSOM树，当浏览器接收到服务器相应来的HTML文档后，会遍历文档节点，生成DOM树，CSSOM规则树由浏览器解析CSS文件生成
//CSRF:(跨站请求）伪造
用户登录网站在另页发请求，这时CSRF就产生了，防御使用验证码，检查https头部的refer，使用token
//XSS:(跨站脚本）攻击
通过注入脚本，在浏览网页时攻击，比如获取cookie，或其他信息，可以分为存储型和反射型，存储型是攻击者输入一些数据并且存储到了数据库中，其他浏览者看到的时候进行攻击，反射型的话不存储在数据库中，往往表现为将攻击代码放在 url 地址的请求参数中，防御的话为 cookie 设置 httpOnly 属性，对用户的输入进行检查，进行特殊字符过滤。
//检测页面加载
在页面置入脚本或探针采集用户访问数据分析
主动监测，模拟用户访问采集数据分析，比如说性能极客。
URL 到页面加载显示完成，DNS 解析，TCP 连接，发送 HTTP 请求，服务器处理请求并返回 HTTP 报文，浏览器解析渲染页面，连接结束
//cookie 设置
name名称value值
domain 可以访问此 cookie 的域名，只能获取到 domain 设置为顶级域名的 cookie
path 可以访问此 cookie 的页面路径页面可以读取此 cookie
expires/Max-Age 超时时间，不设置默认值是和 session 一起失效。 当浏览器关闭(不是浏览器标签页，而是整个浏览器) 后，此 cookie 失效
Size 大小
httponly 若此属性为 true，则只在 http 请求头中会带有此 cookie ，不能通过 document.cookie 来访问此 cookie
secure 设置是否只能通过 https 来传递此cookie
cookie 编码方式encodeURI()。
//box-sizing
box-sizing:content-box默认标准盒子
box-sizing:border-boxIE盒子模型
box-sizing:padding-box
//画一条 0.5px 的线，采用 meta viewport 的方式
<meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />采用 border-image 的方式，采用 transform: scale()的方式
//加载
link 属于html标签，页面加载link会同时加载,@import是css提供引用的css会等到页面加载结束后加载。
//Animation (动画) 和 transition(过渡）
方式是随时间改变元素的属性值，区别是 transition 需要触发一个事件才能改变属性，而 animation 不需要触发任何 事件的情况下才会随时间改变属性值，并且 transition 为 2 帧，从 from .... to，而 animation 可以一帧一帧的
//Flex
意为"弹性布局"，用来为盒状模型提供灵活性
//BFC
块级格式化上下文，用于清楚浮动，防止 margin 重叠等
//隐藏
opacity=0，元素隐藏不改变页面能触发
visibility=hidden，元素隐藏不改变页面不触发
display=none，把该元素删除掉一样
//position 属性
固定定位 fixed:窗口定位。
相对定位 relative:起点定位。
绝对定位 absolute:上级定位。
粘性定位 sticky:元素在流中的 flow root(BFC)和 containing block(最近的块级祖先元素)定位。
默认定位 Static:默认值。
inherit:规定应该从父元素继承 position 属性的值。
//line-height
上下之间的高度，是针对字体来设置的，height 一般是指容器的整体高度
//background-color
设置的背景颜色会填充元素的 content、padding、border 区域
//overflow
(溢出元素框) 的原理
//闭包
能够读取其他函数内部变量的函数
//解决异步回调地狱promise、generator、async/await
//加载
预加载:(提前加载）增加服务器前端压力，需要查看时可直接从本地缓存中渲染
懒加载:(迟缓甚至不加载）作为服务器前端的优化，减少请求数或延迟请求数，有缓解压力作用。
//判断类型
typeof()，instanceof，Object.prototype.toString.call()等
//数组
push()，pop()，shift(),unshift()，splice()，sort()，reverse()，map()等
//Ajax 解决浏览器缓存问题
在 ajax 发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
在 ajax 发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
在 URL 后面加上一个随机数: "fresh=" + Math.random()。
在 URL 后面加上时间搓:"nowtime=" + new Date().getTime()。
如果是使用 jQuery，直接$.ajaxSetup({cache:false})就可以了
//数组去重
indexOf 循环去重
ES6 Set 去重;Array.from(new Set(array))
Object 键值对去重;把数组的值存成 Object 的 key 值，比如 Object[value1] = true， 在判断另一个值的时候，如果 Object[value2]存在的话，就说明该值是重复的
//ajax
状态0 (未初始化) - 1 (载入) - 2 (载入完成) - 3 (交互) - 4 (完成)
AJAX 创建异步对象 XMLHttpRequest
操作 XMLHttpRequest 对象
(1)设置请求参数(请求方式，请求页面的相对路径，是否异步
(2)设置回调函数，一个处理服务器响应的函数，使用 onreadystatechange ，类似函数 指针
(3)获取异步对象的 readyState 属性:该属性存有服务器响应的状态信息。每当 readyState 改变时，onreadystatechange 函数就会被执行
(4)判断响应报文的状态，若为 200 说明服务器正常运行并返回响应数据
(5)读取响应数据，可以通过 responseText 属性来取回由服务器返回的数据
var xhr = new XMLHttpRequest();
xhr.open('get', 'aabb.php', true);
xhr.send(null);
xhr.onreadystatechange = function() {
    if(xhr.readyState==4) {
        if(xhr.status==200) {
			console.log(xhr.responseText)
        }
    }
}
```

# VsCode开发Vue

安装

```jsx
npm install vue@next
npm install -g @vue/cli
npm init vite@latest <project-name> -- --template vue
cd <project-name>
npm install
npm run dev
//如果报错esbuild
node node_modules/esbuild/install.js
```

