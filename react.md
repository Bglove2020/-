# React

## 初始化React项目

1. 全局安装 create-react-app包，在要创建项目的文件夹下打开cmd，键入create-react-app 项目名，即可初始化React脚手架
2. 使用webstorm可以直接一键初始化一个新的React脚手架项目。
3. 不使用脚手架，通过自己配置相关包和webpack的webpack.config.js来自己搭建一个React项目。
**注意：react脚手架也是基于webpack构建的项目，但是脚手架中隐藏了webpack的配置文件webpack.config.js，我们可以手动打开。同时脚手架的package.json中配置了相关命令，热更新。如果我们自己创建React项目，我们需要自己配置相关命令，热更新。再键入webpack命令从入口文件开始打包，再键入webpack-dev-server启动一个热更新服务器。**

## React脚手架文件介绍

1. webpack.config.js文件隐藏了，也可以打开。
2. 两个主目录public和src。public中存放的是静态资源，图片，页面等。src中存放的是组件和其他脚本。
3. src目录下的index.js为整个项目打包的入口文件，主要作用是将App组件渲染到页面上。src目录下的App.js是所有组件的父组件，脚手架项目的思想是，只有一个index.js，而index.js也只渲染一个组件，其余所有组件都在App组件内部。
4. public下的index.html也是脚手架启动后展示的初始界面。
5. 脚手架项目也是一个git仓库。

## React脚手架项目瘦身
**脚手架初始化后有部分用不到的功能我们可以进行删除**
1. public内部只留下index.html和favicon.ico（用来设置网站的小图标）
2. src目录下只保留App.js,App.css,index.js,index.css。

## 在js文件中导入包或者文件

1. js文件中当你要导入一个npm包时，不需要指定文件路径，只需用字符串指定包的名字。
2. js文件中导入与当前文件在同一目录或子目录中的文件时，用字符串指出文件的相对路径或绝对路径来指定文件的位置。
3. **webpack构建的项目于允许我们可以在js文件中导入css文件，这样在js中导入css文件，webpack会把导入的css中的代码写入style标签中，并渲染到页面上**

## React中的jsx

1. jsx文件相当于在js中加入了html标签，在js文件中可以直接写html标签。
2. 在jsx文件的html标签中还可以嵌套**js表达式**，将js表达式用{}括起，可以放在标签任何需要的地方。
3. **在普通的js文件中，数组会直接原封不动的写入html标签内部，同时数组中的逗号也会保留。但是React中的标签内容中的数组会被处理，将逗号去除**

---
# React
## 细节问题

### react开发者工具
当我们使用一个文件夹对应一个组件时，文件夹内部组件JS文件名一般为index，但此JS文件内部组件名不能设置为Index。因为react开发者工具并不是根据你导入APP.js中自定义的组件名来显示组件的，而是根据你每个组件文件夹中JS文件中的组件名来显示组件的。

### 类式组件
类式组件区别于函数式组件的地方为类式组件内部可以使用this，类式组件中的所有this都指向类式组件的实例化对象。可以在类式组件内部任意位置使用this来指示实例化后的组件对象。

类式组件的实例化对象 **(函数式组件没有state)** 身上有三个核心属性，state、props和refs。state指示了当前组件的状态，页面上任意组件的state更新后会导致页面的重新渲染。props用于父子组件之间传递信息。refs用于标识一个页面dom对象。

### 消息订阅
一个组件既可以发布某个消息，也可以订阅某个消息 (本网站的侧边栏按钮即使用这个技术)

### 组件路由
当某个url跳转到指定链接后，对应的navLink会被直接激活。

路由组件从link接收的参数会直接保存在路由组件的props中。





## 组件间的消息传递

1. 未引入消息订阅机制，除父子组件，其余组件均无法进行直接通信。此时通信方法主要是通过将所有所需信息全部保存到根组件App的state中，且需要给要改变状态的组件传递状态改变函数，使其可以改变App组件的state。传递数据和函数都是通过props传递，函数一般写为箭头函数，这样可以让其中的this初始时就绑定为App。
2. 引入消息订阅机制可以让处于任何级别的两个组件进行通信。使用pubsub-js库实现消息发布订阅机制。

```javascript
//接收组件内，在组件挂在完成时就开始订阅消息
componentDidMount(){
    pubsub.subscribe("change_state",(mes,data)=>{
        this.setState(data)
    })
}
//在发布消息组件内的某个回调函数中进行发布消息
PubSub.publish('testEvent', data);

```

## 组件实例化对象的state

1. 组件类并不具有state属性，而是组件的实例化对象具有stete属性。
2. state不能直接使用state.属性名=xxx的方式进行属性修改，而是要调用对象身上的setState方法，传入一个新对象进行更新

```javascript
//在任何一处回调函数中写入
this.setState({})
```

3. 传入setState中的对象不需要具有原State中的所有键值对，只需要传入修改的属性，setState会对比传入的对象进行修改。
4. **setState是异步函数，并不会在调用后直接执行，而是会在当前事件结束后再进行更新，所以State更新后直接对state进行访问得到的结果不对，如果需要更新后直接访问state，可以在setState中传入一个回调函数，会在状态更新后进行调用,此回调函数无参数**

```javascript
this.setState(data,()=>{
    console.log(this.state)
})
```

## Route

最初在web应用开发中前端并不太关注路由，采用的是后端模板渲染的方式，由后端根据url请求信息来决定响应某个页面，此时路由是在服务端配置的。这时候的路由就是url和后端服务器的交互，根据不同的路径显示不同的资源，页面也是一种资源。
这种开发方式有明显的不足，每切换一个页面都要重新加载一次，即使两个页面有很多相同的地方,前端迫切的需要一种革新来改变这种开发方式。
随着前后端分离，出现了一种新的开发方式，单页应用（SPA）。单页应用的意思是只有一个页面，是无刷新的，看到的页面之间的跳转其实只是组件的切换，同时URL也要相应的变化，为了实现这种单页应用，出现了前端路由。

### BOM中的history配合history.js实现url改变但不跳转

前端路由的整体过程是先通过history库来向BOM.history中压入新路径，通过history改变url并不会跳转。然后浏览器检测url发生了变化，渲染对应的组件。 前端中的路由就相当于一个个键值对，一个path对应一个组件。
**浏览器的history是栈结构**

### react-router-dom的基本使用

是一个前端路由库，由JavaScript实现。定义了路由规则，并根据这些规则来显示不同的组件或页面。例如，当用户点击一个链接时，前端路由会将URL解析为一个路由路径，然后根据路径匹配相应的组件或页面并显示在页面上，而不需要向服务器发起请求。 其中包括了许多内置组件，BrowserRouter，HashRouter，Route，Link等

1. 首先需要选择一种路由器（路由器即实现路由的容器）BrowserRouter或者HashRouter，并将所有的内置路由组件全部放进路由器，用路由器组件包裹整个应用程序，使得我们可以在浏览器中使用路由。**一般直接在index.js中用路由器包住整个app组件。**
2. 然后将页面划分为导航区和展示区。
3. 在导航区使用Link、NavLink组件实现路由链接。
4. 在展示区使用Route标签注册路由

```javascript
import './App.css';
import Home from "./pages/home";
import About from "./pages/about";
import {Link,Route} from "react-router-dom"
function App() {
  return (
    <div className="App">
        <div className="title">
            <h1>路由切换演示</h1>
        </div>
        <div className="nav">{/*导航区*/}
            <h1>
                <Link to="/about">about</Link>
            </h1>
            <h1>
                <Link to='/home'>home</Link>
            </h1>
        </div>
        <div className="show_area">{/*展示区*/}
            <Route path="/about" component={About}></Route>
            <Route path="/home" component={Home}></Route>
        </div>
    </div>
  );
}
```

### Link与NavLink

Link与NavLink是react-router-dom中的默认组件，在进行组件渲染时实际渲染为a标签。他们的原理是a标签+监听事件。事件内部通过history操作url并阻止a标签默认事件，来达到改变url但不跳转。

* 区别：Link路由链接不带有高亮效果，而NavLink路由链接可以通过acitveClassName来动态的添加一个类样式来实现点击高亮效果。
* 区别：NavLink相较于Link会在被点击时多添加一个aria-current="page"属性，来标识自己。

### 封装NavLink

原生的NavLink可能有大量的重复代码，为了避免重复代码我们可以自己对NavLink进行二次封装。**二次封装的组件为一般组件**

```javascript
import React, {Component} from 'react';
import {NavLink} from "react-router-dom";

class mynavlink extends Component {
    render() {
        return (
            <div>
                {/*在自己封装的NavLink链接中可以直接写死固定一些相同的属性，如className="" activeClassName="",剩余的属性可以用props解构赋值得到，这是react实现的功能
                    注意：标签中的内容会作为props中的children的值，当解构赋值中拿到children属性的值时，会自动渲染到NavLink的内容中。
                */}
                <NavLink activeClassName="hightlight" {...this.props}></NavLink>
            </div>
        );
    }
}

export default mynavlink;
```

### react中导入的css表由于二级路由链接导致刷新丢失

**React脚手架使用npm start启动项目后，会在本地3000端口开启一个服务器，这是通过webpack的dev-server实现的，并且把public文件作为localhost3000上运行的服务器的根路径。当访问localhost3000里的内容时，有则正常返回，没有则返回index.html**

1. 如果路由为二级路由形如：/a/b/c,则刷新后，页面会把根路径中的./当作localhost3000/a/，从而请求错误路径，但是浏览器插件的Network会显示请求成功，这是因为请求回来了index.html。
2. 解决办法1 不解析./就不会出错，在link的href中不写./
3. 解决办法2 使用hashRouter，因为会在根路径后面加#，所以会明确知道#后面都是路由。
4. 解决办法3 使用react中定义的public文件夹的绝对路径%PUBLIC_URL%来指出文件的绝对路径。

### 注册路由

通过Route标签将对应的路径与组件进行绑定

### 一般组件和路由组件

组件分为一般组件和路由组件：一般组件就是自己写的组件通过组件名标签的形式引入的组件，路由组件就是通过Route标签引入的组件(Link组件也算路由组件)，这样的组件一般放在pages目录下，而一般组件一般放在components目录下 一般组件如果自己不主动传入参数，则组件props中无参数。路由组件中由Route标签引入，即使不主动传递参数也会默认传入一些参数如match、location和history对象。但在在React开发者工具（例如Chrome的React DevTools）中看不到组件的props中有这些属性。

### Link与Route的匹配规则

当点击组件树中任意位置的Link标签后，浏览器url被修改，浏览器开始从新渲染组件，渲染过程沿着组件树，自顶向下，将所有组件中可以匹配成功的Route进行渲染。

1. 如果一个组件内部有很多Route组件，且其中只有一个组件会与当前url匹配，可以使用Switch标签将所有Route组件包括，当出现第一个匹配成功的标签后，当前组件的匹配结束，继续下一级组件的渲染，可以增加匹配速度。
2. 几级嵌套组件最好就用几级路径，如果使用父级组件的路径，则父级组件渲染的同时自己也会进行渲染。
3. 默认采用模糊匹配，即Route中的path要是Link中的to的值的从首个路径开始的任意长子串
4. 可以主动开启精准匹配，只需要在Route标签中加入exact关键字即可
5. 能不用exact就不用exact，

### Redirect

当所有的路由都没匹配上时，就会触发Redirect标签。**初次进行页面渲染时，实际上也是有路由的，不过是空字符串，所以没有和任何路由匹配上，这就可以使用Redirect来选择默认路由**

```javascript
<Redirect to="" />
```

### 嵌套路由匹配

路由组件内部再添加路由组件

1. 嵌套的路由组件代码可以放在父组件内部也可以放在pages下
2. 嵌套路由最主要的是嵌套的路由组件的to和path是多级路由，这样每次点击Link修改url后，整个页面开始从头渲染。由于他是多级路由，且路由中每一级和组件的从属关系也对应，父组件采用模糊匹配就可以一级一级的进行渲染。
3. 路由匹配规则是按照路由注册顺序匹配的。

---

### 向路由组件传递参数

有时候不同的链接对应的组件相同，仅其中内容不同，这时候就可以动态的向路由组件传递一些参数用于展示信息。

#### params方式传递参数

1. 在Link标签的to属性中指定的路径后面拼上参数(可以传递多个参数)
2. 在Route组件的path属性中声明Link中传入的参数(如果不声明，Route直接进行模糊匹配，会忽略传入的参数,但是仍然可以从this.props.loaction.pathname中获得)
   **通过params传递过来的参数都是字符串，匹配时需要注意**

```javascript
    render() {
    return (
        <div>
            <div>
                <h4><Link to="/home/message/1/zxr">1号</Link></h4>
                <h4><Link to="/home/message/2/mfl">2号</Link></h4>
                <h4><Link to="/home/message/3/ljy">3号</Link></h4>
            </div>
            <hr/>
            <div>
                <Route path="/home/message/:id/:name" component={Detail}></Route>
            </div>
        </div>
    );
}
```

#### search方式传递参数

search参数传递不需要另外声明接收，search传递的参数在this.props.location.search中。

```javascript
{/*search参数传递Link组件写法*/}
<h4><Link to="/home/message/detail/?id=2&name=mfl">2号</Link></h4>

{/*search参数传递不需要另外声明接收*/}
<Route path="/home/message/detail" component={Detail}></Route>

{/*search传递的参数在this.props.location.search中，以字符串?id=2&name=mfl形式存在，通过.slice(1)去掉第一个问号，再通过querystringify库将urlencode形式转换为对象*/}
import qs from "querystringify";
const {id,name} =qs.parse(this.props.location.search.slice(1))
```

#### state方式传递参数

state参数传递不会出现在url上，具有隐蔽性。同时state传递的参数不仅仅是字符串形式，数字仍然会保持数字格式。state传递的参数在this.props.location.state中。
**使用HashRouter会导致页面刷新后，传递的state参数丢失，因为HashRouter并不使用history api，所以页面刷新后由于state参数不在url中体现，所以传递的参数丢失**

```javascript

{/*state参数传递Link组件写法*/}
<h4><Link to={{pathname:"/home/message/detail/",state:{id:3,name:"ljy"}}} >3号</Link></h4>

{/*state参数传递不需要另外声明接收*/}
<Route path="/home/message/detail" component={Detail}></Route>

{/*state传递的参数在this.props.location.state中*/}
const {id,name} =this.props.location.state
```

### 路由改变方式push和replace

默认的Link标签都是通过push来向Bom.history压入新的路径，可以在Link标签中加入replace属性，即可通过replace来替换Bom.history顶部最新的路径

### 编程式路由导航

Link标签和NavLink标签的本质都是对Bom.history的操作，采用标签的形式简化了开发。我们可以不借助Link/NavLink标签来实现路由改变。
路由组件的props中有history属性，通过对这个属性进行操作就可以push/replace压入路径，改变url而不跳转。由于这是js代码实现的url改变，所以可以写在回调函数中，生命周期中等等。
this.props.history.push()也有三种传递路由参数的方式。params、search、state。

### 编程式路由导航的限制

由于仅有路由组件的props具有history、match、location这三个属性，所以如果想实现编程式路由导航只能在路由组件内实现，这就是很大的限制。
但是可以通过react-route-dom中的withRoute函数来将一般组件转换为路由组件。

```javascript
import React, {Component} from 'react';
import {withRouter} from "react-router-dom";
class Header extends Component {
    render() {
        return (
            <div className="title">
                <h1>嵌套路由练习</h1>
            </div>
        );
    }
}

export default withRouter(Header);
```

## 函数式组件





## 知识扩展

### 生命周期函数小坑
初始化的react脚手架会在index.js使用<StrictMode>标签框住<App>标签，这就是开启了严格模式。当 严格模式 开启时，在开发中 React 会在调用 componentDidMount 后立即调用 componentWillUnmount，接着再次调用 componentDidMount。

### setstate

1. 调用setstate()后会立刻执行此函数，但是状态改变是异步的，等任务队列中的任务执行结束才会改变state状态
2. 对象方式setstate({},()=>{}),有两个参数，第一个参数为一个对象，会对原始state进行增量替换。第二个参数是一个回调函数，在state真的改变后再调用。
3. 函数方式setstate((state,props)=>{},()=>{}),两个回调函数。第一回调返回一个对象，且有两个参数state,props。
4. **对象方式的setstate()实际上是函数方式的语法糖，从函数方式的setstate()我们可以看出来确实改变状态是异步的，因为改变状态是通过回调完成的。**

### lazyLoad

在请求页面时，会一次性加载所有的组件，可能较慢。对于暂时用不到的组件和嵌套组件可以选择使用懒加载，即使用的时候才加载。
1. 引入Lazy函数`import {lazy} from 'react'`
2. 使用懒加载导入模块`const 组件名 =lazy(()=>{import ("组件路径")})`
3. 引入Suspense组件`import {lazy,Suspense} from 'react'`
4. 将所有的懒加载组件用Suspense标签框住，同时设置fallback属性，用于在还未成功加载之前显示的内容。**注意加载组件不能再使用懒加载**
```javascript
<Suspense fallback={虚拟DOM/自定义的加载组件}>
    <Route path="" component={}></Route>
</Suspense>
```
### react-router-dom 6

* <Switch></Switch>取消，变成<Routes></Routes>。同时现在的<Route/>标签必须用Routes标签包裹住，否则报错。
* <Route/>里的部分属性变化，path属性无变化，component属性变为element属性，且直接传入一个组件，而不是组件名。`<Route path="path" element={<组件名>} />`
* 重定向标签<Redirect></Redirect>被移除，引入了一个组件<navigate to="path"></navigate>。当<navigate to="path"></navigate>被渲染时，网页url会被重定向到root+path路径。匹配"/"路由的写法`<Route path="/" element={<navigate to="path"/>}>`。<navigate to="path"/>组件可以用在<Routes></Routes>外部。
* <NavLink>取消了activeClassname，而是className属性传入一个回调函数，回调函数接收一个bool参数，表示此链接是否激活，回调函数需要return一个字符串类名。
* 可以利用useRoutes传入一个对象数组来生成路由表。


