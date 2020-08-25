# **git地址**

http://gitlab.lookpoint.cn/yangyimei/vue-demos/tree/master/src

# **npm**

淘宝镜像切换：

- npm config set registry https://registry.npm.taobao.org

- npm config set registry http://registry.npmjs.org

使用nrm管理registry地址：

- 下载nrm：npm install nrm
- 添加registry地址
   - nrm add npm http://registry.npmjs.org
   - nrm add taobao https://registry.npm.taobao.org
- 切换npm registry地址
   - nrm use taobao
   - nrm use npm

> PhoneGap：移动端跨平台应用的开发平台（采用HTML CSS JAVASCRIPT）,能够在网页中调用智能手机的核心功能（地理定位  加速器 震动 联系人）

# **react**

**react中的事件分为合成事件和原生事件**

合成事件：

```
<a ref="aaa" onClick={
	(e)=>this.handleClick(e)
}>更新</a>
```
原生事件：

js原生事件: `document.body.addEventListener('click', e =>console.log('xxx'))`

**阻止冒泡事件的三种方式**

阻止合成事件间的冒泡

`e.stopPropagation()`;

阻止合成事件与最外层document上的原生事件冒泡

`e.nativeEvent.stopImmediatePropagation()`;


阻止合成事件与除最外层document上的原生事件的冒泡，通过判断e.target


```
render(){
	return(
		<div ref="test">
			<p>{this.state.count}</p>
			<a ref="update" onClick={
				(e)=>this.handleClick(e)
			}>更新</a>
		</div>
	)
}
```

```
componentDidMount() {
	document.body.addEventListener('click',e=>{
		// 通过e.target判断阻止冒泡
		if(e.target&&e.target.matches('a')){
			return;
		}
		console.log('body');
	})
}
```

**react-router页面传值的四种方法**

props.params两种形式

第一种

`<Route path='/user/:name' component={UserPage}></Route>`

跳转：

```
<Link to="/user/sam">用户</Link>
或者
this.props.history.push('/user/sam')
```

取值：`this.props.params.name` (sam)

第二种

`<Route path='/user/:data' component={UserPage}></Route>`

`var data = {id:3,name:sam,age:36}`

`data = JSON.stringify(data)`

```
var path = `/user/${data}`
```

跳转：
```
<Link to={path}>用户</Link>
或者
this.props.history.push(path)
```

取值：`var data = JSON.parse(this.props.params.data)`


query两种形式

第三种 query

`<Route path='/user' component={UserPage}></Route>`

`var data = {id:3,name:sam,age:36}`

```
var path = {
	pathname:'/user',
	query:data,
}
```

跳转：
```
<Link to={path}>用户</Link>
或者
this.props.history.push(path)
```

取值：`var data = this.props.location.query`

第四种 state

state：`<Route path='/user' component={UserPage}></Route>`

`var data = {id:3,name:sam,age:36}`

```
var path = {
	pathname:'/user',
	state:data,
}
```
跳转：
```
<Link to={path}>用户</Link>
或者
this.props.history.push(path)
```

取值：`var data = this.props.location.state`

## **redux**

- @connect: connect装饰器，组件之间共享状态，简写mapStateToProps mapDispatchToProps

- 当涉及到应用程序状态时，redux store是唯一的真实来源

- Redux 存储是一个用于保存和管理应用程序的对象

- 使用createStore() 创建Redux Store  参数为reducer函数，且必须；reducer函数传入state作为参数，并返回state  例如：

	```
	const reudcer = (state = 5) => state
	const store = Redux.createStore(reducer)
	```

- store.getState()检索store中的状态  例如：

	`const currentState = store.getState()`

- 在Redux中所有状态更新都是由dispatching actions 来触发

- 一个action 仅是一个包含已经发生的action事件信息的js对象

- Redux store 接收这些action对象，相应的更新state

- action 必须携带一个type属性指定已发生的action type

- action 可以看做一个信使 将事件信息传递给 Redux store,然后store根据发生的action执行更新state

- 通过函数创建一个action

	`const loginAction = () => {return { type: 'LOGIN'}}` 

- dispatch 方法用于将action发送到Redux store

	`store.dispatch(loginAction())`

- dispatch 一个action后，Redux store需要reducer来响应这个action

- reducer将state和action作为参数，并始终返回一个新的state对象，而不是直接修改state，这是reducer的唯一作用；reducer是一个纯函数； 例如：

	```
	const reducer = (state=defaultState,action)=> {
		if(action.type === 'LOGIN'){
			return {login:true}
		} 
		return state
	}
	```

- Redux中的state是只读的

- 所有应用程序的状态都保存在store的单个state对象中

- 使用store.subscribe()将监听函数订阅到Stroe，每次dispatch一个action就会调用这个方法中的回调函数 例如：

	`store.subscribe((count=0)=>count++)`

- Redux.combineReducers() 方法组合多个reducer，参数为一个对象，value为与之对应的reducer 例如：
	
	```
	Redux.combineReducers({
		count: counterReducer,
		auth:authReducer
	})
	```

- 使用Redux.applyMiddleWare(ReduxThunk.default) 中间件处理异步action 将之作为第二个参数传入store，创建一个处理异步的action creator；返回一个dispatch为参数的函数；函数分别dispatch其他action；其余交给中间件处理：例如
	```
	const handleAsync = () => {
		return function(dispatch){
			dispatch(action1());
			setTimeout(function(){
				dispatch(action2(data))
			},2000)
		}
	}
	```

## **react-redux**

- 连接react和redux的两个关键api是Provider和Connect， Provider是用来包装react组件的weapper组件  它允许访问整个组件树的redux store 和 dispatch方法；使用需要两个props ：redux store和App应用子组件  例如： 
	`<Provider store={store}> <App /> </Provider>`

- Provider向react提供state和dispatch，mapStateToProps函数将state作为参数，将你所需要的state数据映射到当前组件的特定属性名上：例如： 

	```
	const mapStateToProps = (state) => {
		return {message: state //或 state.xxx}
	}
	```

- 同上，mapDispatchToProps函数将dispatch映射到props上，将dispatch作为参数，返回一个对象，将dispatch action的操作放入放入一个函数，映射到属性名上；例如：

	```
	action1 = (data) ={return {type: 'ADD',data:data}} 
	const mapDispatchToProps = (dispatch) => {
		return {
			dispatchAction1: (data) => {dispatch(actoin1(data))}
		}
	}
	```
- 以上定义了两个映射函数，使用connect真正将state和dispatch映射到组件的props（或者说将两个函数作为参数放到connect函数里执行），connect（mapStateToProps,mapDispatchToProps）;执行结果仍然是一个函数，再次调用传入当前组件；完整使用如下： 

	`connect(mapStateToProps,mapDispatchToProps)(当前组件)`

- 如果要省略connect中的某个参数，应当使用null替换这个参数

- reduxjs/toolkit工具进行状态管理

- 如果使用createReducer({initialState,{[actionType]:(state,action)=>state++} or builderCallback})则在组件中引入action，通过mapDispatchToProps函数将action creator映射到组件的props 例如：

	```
	mapDispatchToProps = (dispatch) => {
		return {a: (payloadParams)=>dispatch(action1())}
	}
	```
   - a: 映射到组件props中的函数

   - payloadParams：传给action的payload值
   - action1：通过createAction()函数创建的action creator

- 如果使用
	```
	const AA=createSlice({
		initialState,
		name:'string',
		reducers:{
			increment:(state,action)=> state++
		}
	})
	```
	- 省略了createReducer() 和 createAction()  在组件中直接引入，通过const {increment} = AA.actions结构的increment（类似action creator）最后dispatch(increment()) 例如：

	```
	mapDispatchToProps = (dispatch) => {
		return {
			a: (payloadParams) => dispatch(increment())
		}
	}
	```


# **vim编辑器**

- i键进入插入模式，开始编辑内容;a和s也可进入插入模式，略有不同

- esc 退出插入模式

- w filename 命令将文件内容保存到另外一个文件

- 输入冒号进入命令模式

- 在命令模式中G移动到最后一行

- 在命令模式中5G移动到第5行；gg移动到第一行；G移动到最后一行；ctrl+B ctrl+F上翻页下翻页

- 普通模式下编辑数据

   - x删除当前光标所在位置的字符

   - dd删除当前光标所在行

   - dw删除当前光标所在位置的单词

   - u撤销

   - a在当前光标后追加数据

   - 在当前光标行行尾追加数据

- 复制粘贴

   - 在删除数据时，实际会将数据保存单独的一个寄存器中，可以使用p命令取回，例如使用dd命令删除一行文本，然后将光标放到缓冲区要放置该文本的地方，使用p命令，文本会被插入到当前光标所在行之后

   - 复制命令y，可以在y命令后使用第二字符，如yw复制一个单词，y$复制到行尾，复制后，把光标移动到想要防止文本的地方，键入p命令，复制的文本就会出现在该位置
   
   - 复制命令一般在可视模式下使用，可视模式会在移动光标的同时高亮显示文本，键入v键进入可视模式

   - 复制快捷命令   : 1, 2 co 3将第一行和第二行复制到第三行后面: 1, 2 m 3将第一行和第二行移动到第三行后面



# **Webpack**

- loader用来处理非JavaScript模块，webpack本身只识别JavaScript

- package.json的name名称不能和安装的包名相同

- 单独使用webpack 需要设置webpack.config.js

- webpack打包时，使用webpack命令

- 如果要使用npm run dev/build 在package.json中的scripts中添加dev和build命令，并且需要安装webpack-cli

- webpack 读取loader时是从右到左，loader执行顺序是从右到左以管道方式链式调用
css-loader 解析css文件，style-loader放到html文件中: ['style-loader','css-loader']  顺序错误执行时会报错
- html 打包时使用的模板文件：template: './src/list.html'；里面部门包含script标签和引入css文件
- css文件的打包：在入口文件用es6或commonJs语法引入，将入口文件打包压缩
- 运行webpack命令直接将文件打包到指定的文件夹下
- webpack-dev-server： 在本地启一个服务，默认8080端口


# **MongoDB**
	
- mongoDB是基于分布式 文件存储的NoSQL数据库；为web应用提供数据存储解决方案

- bson是json的二进制格式，但占用空间不一定比json少

- bson处理数据速度快，易于将bson数据快速转化为变成语言的原生数据格式

## mongoDB三要素

- 数据库：集合的物理容器，一个数据库可以包含多个文档，一个服务器通常有多个数据库
- 集合：类似于关系数据库中的表，存储多个文档，结构不固定，如可以存储如下文档在一	个集合中：
	```
	{'name':'zhangsan','gender':'男'}
	{'name':'lisi','age':18}
	{'book':'qunshuzhiyao','effect':'jiuguojiumin'}
	```

- 文档：就是一个键值对 对象，是JSON的扩展BSON格式

	`{'name':'zhangsan','gender':'nan'}`

**在启动mongoDB的命令行窗口中 输入help 查看 命令行方法**

常用命令： 

- db.help() 查看当前数据库方法

- db.mycoll.help() 查看当前数据库的集合方法

- show dbs 显示数据库列表

- db 显示当前数据库

- use <db_name> 切换数据库

创建数据库如果没有插入数据，不会显示在数据库列表中

[mongoDB使用总结](https://segmentfault.com/a/1190000016977614)
[mongoDB使用总结](https://www.cnblogs.com/djcoder/p/12292952.html)

# Class 

- 声明式class和语句式 class 访问name属性时有区别：
   - 声明式:  let A = class AS {}; 通过声明的变量来访问name属性：A.name  = AS;如果是匿名A.name = A;
   - 语句式：class A {}; 则直接访问：A.name = a

- class super 继承父类的构造函数：相当于执行父类的constructor，并传值，此时子类就有了父类的实例属性，如果不传值，子类实例属性为undefined：
	```
	class Polygon {
		constructor(height, width) {
			this.name = 'Polygon';
			this.height = height;
			this.width = width;
		}
	}
	```

	```
	class Square extends Polygon {
		constructor(length) {
			super(length, length); // 将父类的name、height、width属性继承过来
			this.name = 'Square'; // 修改name属性值
		}
	}
	```

- 通过 static 添加的静态方法只能通过类本身访问，实例无法访问;相当于传统构造函数中的公有方法

- 原型方法和静态方法中的this，指向调用者，如果赋值给其他变量，this指向undefined，不会指向window；因为类体内的代码默认是在严格模式下执行（"use strict"）
	```
	class Animal {
		speak(){return this}
		static eat() {return this}
	}
	
	let obj = new Animal();
	obj.speak(); // Animal {}
	let speak = obj.speak;
	speak(); // undefined

	Animal.eat() // class Animal
	let eat = Animal.eat;
	eat(); // undefined
	```

- 如果在传统的构造函数语法中，不使用严格模式，this会被设为全局对象

- 类的静态属性和原型属性只能在类体外定义 class Reatangle {};
	`Reatangle.width = 20 // 相当于构造函数中的公有属性`
	`Reatangle.prototype.width = 30;`

# Git
git本地仓库和远程仓库之间的传输如果使用的是SSH协议，需要创建SSH key；使用SSH key  pull和push的时候不需要验证用户名和密码（如果配置时设置了密码，则需要验证密码）如果使用https协议，则不需要创建SSH key；但是pull和push的时候需要验证用户名密码

修改内网gitlab：
`git remote set-url origin http://192.168.9.30:3080/dt/webui.git`
外网gitlab：
`http://xiants.qicp.vip:3080/dt/webui/-/branches`

git修改仓库地址：（先删后加）
`git remote rm origin`
`git remote add origin [url]`
直接修改：
`git remote set-url origin 【新地址】`

# Vue

- 对子组件使用v-if切换显示隐藏后，子组件中使用watch无法监听到父组件传值。

- watch中使用 immediate: true : 子组件首次赋值也会触发监听：newVal: xxx; oldVal: undefined;

# 其它
## Ubuntu桌面
**gnome、unity**
