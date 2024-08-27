# Vue2
目前对Vue2的原理并不是很了解，只能直观的写出Vue2和react的不同之处和一些基本的使用方法

## 坑
* Vue.js devtools对于纯中文数据的兼容性不是很好，可能会出现点击无任何反应或不出现Root的情况。如果出现可以在纯中文数据中加入一点数字或字母。


### 缓存token
当输入用户名和密码登录后，接口返回的数据中有一个`token`字段，用于唯一标识这个用户。在就收后，应该使用`sessionStorage.setItem('token',"")`将`token`存在浏览器本地，并且设置在以后的请求头中。可以通过在axios中传入配置对象或者在全局的axios中直接设置。

## 初始化Vue2项目
Vue2项目的初始化和React项目的初始化不太一样，在React中，我们想以最简单的方式完成一个react项目时至少需要下载三个包react、react-dom、babel包，分别用来创建react组件、渲染react组件和处理jsx语法。而Vue2中不需要这么多包，只需要引入Vue包，利用Vue构造函数创建Vue实例即可。

想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象。当new一个Vue构造函数时，会创建一个Vue实例，其中传入Vue构造函数的配置对象中的`data`property会被写入生成的Vue实例身上。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--引入vue.js后，会在全局暴露一个Vue构造函数  -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
</head>
<body>
    <h1 id="root">{{message}}</h1>
    <script>
        new Vue({
            el:"#root",
            data:{
                message:"Hello world!"
            }
        })
    </script>
</body>
</html>
```

## Vue基础语法——模板
简而言之Vue模板就是HTML代码并加入一些特殊的Vue语法，仍然符合html规范。这个模板是直接写在HTML文件标签内部的，是合法的HTML元素。（在react中，在js中写html代码而形成jsx文件，再把jsx渲染为虚拟DOM，**这也是为什么Vue不依赖jsx语法**）

Vue实例和模板是一一对应的，当`new`一个Vue实例时，会将Vue实例与配置对象中`el`property指定的Vue模板进行绑定。模板可以按照一定的语法规范（插值和声明）来访问与之绑定的Vue实例中的数据（Vue配置对象中的`data`值），当实例化Vue对象时，会将与之绑定的Vue模板编译为虚拟DOM对象。

### 模板语法——插值
在HTML代码中使用“Mustache”语法 (双大括号) 进行插值，`{{ }}`中可以写入任意的js表达式，同时可以直接访问到与之绑定的Vue实例的data对象身上的所有property值。
```html
<body>
    <div id="root">
        <h1>{{message1}}</h1>
        <h2>{{message2}}</h2>
    </div>
    <script>
        new Vue({
            el:"#root",
            data:{
                message1:"Hello world!",
                message2:"i'm zxr."
            }
        })
    </script>
</body>
```

`{{ }}`语法一般用于标签体插值，并不适用于标签内部属性的插值，下述示例为错误写法。在标签内部使用`{{ }}`会被原封不动的解析为字符串。
```html
<body>
    <div id="root">
        <a href={{url}}>百度</a>
    </div>
    <script>
        new Vue({
            el:"#root",
            data:{
                url:"www.baidu.com"
            }
        })
    </script>
</body>
```

### 模板语法——指令
指令 (Directives) 是HTMl标签中带有`v-`前缀的特殊 attribute，用于解析标签属性、标签体内容、绑定事件等。指令 attribute 的值预期是单个 JavaScript 表达式

#### v-bind
用于将标签属性与表达式进行绑定，同时响应式地更新标签属性。`{{ }}`插值无法用于指定标签内部属性，v-bind用于指定标签属性。
```html
<body>
<div id="root">
    <a v-bind:href="url">{{name}}</a>
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            url:"https://www.baidu.com",
            name:"zxr"
        }
    })
</script>
</body>
```

`data`对象中的数据会写入Vue实例上，`data`中的property还可以是对象。
```html
<body>
<div id="root">
    <a v-bind:href="url">{{student.name}} {{student.age}}</a>
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            url:"https://www.baidu.com",
            student:{
                name:"zxr",
                age:22
            }
        }
    })
</script>
</body>
```

#### v-show
用于控制是否显示某个标签元素，`v-show:xxx`接收一个布尔表达式。相当于给标签加上了`display:hidden`

#### v-if
用于控制是否渲染某个标签元素，`v-if:xxx`接收一个布尔表达式，当值为`flase`时，此标签其其内部标签将不再渲染。

`v-if`还可以配合`v-else-if`和`v-else`使用，但这些标签必须紧挨在一起。

#### v-for
用于渲染具有相同结构的多个标签，一般用于渲染列表。

使用`v-for`语法必须搭配`:key`使用，不同于react中不设定key值直接报错，Vue会自动将每个标签的索引作为key值。

由数组渲染一个列表,`v-for:xxx`中的表达式为`item in arr`形式或`(item,index) in arr`，可以接收两个参数分别表示数组元素和对应索引。

由对象渲染一个列表时，为`v-for="(value,k) in obj`形式，其中`value`为属性值，`k`为键。

标签内的`{{ }}`插值语法中的数据可能来自于`vm`和标签中的形参，这里指`item`和`index`

```html
<body>
    <div id="root">
        <ul>
            <li v-for="(item,index) in persons" :key="index">{{item.name}}-{{item.age}}</li>
        </ul>
        <ul>
            <li v-for="(value,k) in cars">{{k}}-{{value}} </li>
        </ul>
    </div>
</body>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            persons:[
                {name:"zxr",age:20},
                {name:"mfl",age:21},
                {name:"ljy",age:22}
            ],
            cars:{
                name:'奥迪A8',
                price:'70万',
                color:'黑色'
            }
        }
    })
</script>
```



#### 数据绑定
v-bind可以用于将标签属性与Vue实例中property进行绑定，但是这种绑定是单向，Vue实例中property值改变会影响标签值的改变，但是标签值的改变不会影响Vue实例中property的值。
v-model可以实现**表单类标签属性**的双向绑定，即用户对表单元素的操作也会影响Vue实例中property的值。
```html
<body>
<div id="root">
    <input type="text" v-bind:value="name">
    <br>
    <input type="text" v-model:value="password">
    <!--简写v-model默认收集表单标签的value值 <input type="text" v-model="password">    -->
</div>
<script>
    devtools: 1

    var vm=new Vue({
        el:"#root",
        data:{
            url:"https://www.baidu.com",
            // 这里如果你的数据是纯中文的话，打开vue devtools不会显示root，可能是因为中文兼容性差。
            name:"用户名1",
            password:""
        }
    })
</script>
</body>
```


1. 哪些是值引用，哪些是地址引用
2. getter和setter

## 数据代理

### Object.defineproperty方法
方法接受三个必填的参数，分别为obj（要添加属性的对象）,"property"（添加的属性名）和{}（配置对象），为目标对象添加一个可定义的属性

```javascript
var stu= {}
Object.defineProperty(stu,"name",{
    value:"zxr"
})
console.log(stu.name);
//zxr
```

`Object.defineProperty`中传入的配置对象中可以设置很多属性，仅设置`value`时，会默认帮我们把`writable`，`configurable`，`enumerable`设置为false，表示不可修改、不可删除和不可遍历。
```javascript
var stu= {}
Object.defineProperty(stu,"name",{
    value:"zxr", 
    // 实际也设置了下方三个
    // writable:false,
    // enumerable:false,
    // configurable:false
})
console.log(stu.name);
//zxr
```

`Object.defineProperty`中传入的配置对象中还可以设置`get`和`set`访问器，但访问器和 `wriable` 或 `value`不能同时设置，否则会错。

下段代码中stu1代理了stu2中的`name`属性，在开发者工具中输出stu1可以发现有一个默认隐藏的name属性（显示为name:(...)），修改stu1中的`name`属性会同时修改stu2中的`name`属性。

```javascript
var stu1 = {}
var stu2 = {name:"zxr"}
Object.defineProperty(stu1,"name",{
    get(){
        return stu2.name
    },
    set(newname){
        stu2.name=newname
    },
})
stu1.name="mfl"
console.log(stu1.name)
// mfl
console.log(stu2.name)
//mfl     
```
通过数据代理，当我们访问或者修改对象的某个属性时，通过`get`和`set`中的代码代理直接对属性的读取和修改过程，可以进行额外的操作或者修改返回结果。


### Vue中的数据代理
当`new`一个Vue实例时，会将传入的配置对象中的`data`property赋值给Vue实例对象`vm`的`_data`property。和Vue实例`vm`绑定的Vue模板可以访问到`vm`及其原型中的所有属性和方法，因此可以使用`_data.`的方式取得需要的数据。

每次都通过`_data.`的方法获取数据有些繁琐，使用数据代理可以方便这个取值过程，将`vm`的`_data`property中的所有属性通过Object.defineproperty方法直接代理到`vm`上，使我们可以直接在`vm`身上访问`_data`中的属性值。


## Vue事件
### 事件绑定
Vue中通过指令`v-on:`来为模板中的标签添加事件（可以简写为`@`），同时添加的事件会通过Vue实例调用，即`vm.`的形式调用。所以事件处理函数中的this不再指向目标DOM元素，而是Vue实例`vm`。

一般将事件处理函数写在配置对象的`methods`中，而不写在`data`。**虽然写在`data`中也可以正常使用，但是`data`中的数据会进行数据劫持来实现响应式，但事件处理函数无需变化，所以不需要进行数据劫持**

`methods`中的方法都会直接挂载在Vue实例`vm`上，所以Vue模板可以直接访问`methods`中的所有方法。

由Vue实例管理的函数都尽量不写为箭头函数，因为箭头函数没有自己的`this`，在定义时就会绑定其外部最近的`this`，这里会绑定为`window`

注意：在绑定事件处理函数时`@click="xxxxx"`，其中`xxxxx`处可以写一些简单的表达式，但是不能写 **`alert()`、`console.log()`这种在`window`下的方法，模板可以访问到的数据只有Vue实例`vm`身上的属性**
```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--最简单的vue项目只需要引入vue.js即可，全局会多一个Vue构造函数    -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <button v-on:click="handelclick">点击</button>
<!--<button @click="handelclick">点击</button>    -->
<!--    <button v-on:click="console.log(this)">点击</button> 这里没法这样写，vm身上并没有console.log()方法-->
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            name:"zxr",
        },
        methods:{
            handelclick(){
                console.log(this)
            }
        }
    })
</script>
<!--点击后输出vm-->
</body>
```

#### 传递event参数
默认情况下，Vue实例在调用事件处理函数时会直接传入`event`。这两种方式效果相同
```html
<body>
<div id="root">
    <button v-on:click="handelclick">点击</button>
    <!--    <button @click="handelclick($event)">点击</button>-->
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            name:"zxr",
        },
        methods:{
            handelclick(event){
                console.log(event)
            }
        }
    })
</script>
</body>
```

#### 传递自定义参数
如果在直接传递了自定义参数，而不传入`$event`实参，Vue不会传入`event`参数。
```html
<body>
<div id="root">
<!--    <button v-on:click="handelclick">点击</button>-->
    <button @click="handelclick(1)">点击</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            name:"zxr",
        },
        methods:{
            handelclick(num,event){
                console.log(num,event)
            }
        }
    })
</script>
</body>
<!--
输出
1 undefined
-->
```

想传递自定义参数的同时保留`event`参数需要在调用时直接传入`$event`参数来通知Vue实例传入`event`参数。
```html
<body>
<div id="root">
<!--    <button v-on:click="handelclick">点击</button>-->
    <button @click="handelclick(1,$event)">点击</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            name:"zxr",
        },
        methods:{
            handelclick(num,event){
                console.log(num,event)
            }
        }
    })
</script>
</body>
<!--
输出
1
PointerEvent
-->
```

### Vue中的事件修饰符：
事件修饰符可以改变事件的特性，在原生JS中可以通过设置`event`事件对象实现，Vue对其进行了封装，简化代码。
#### prevent：阻止默认事件（常用）
```html
<button @click.prevent="handelclick(1,$event)">点击</button>
```
#### stop
阻止事件冒泡（常用）
```html
<button @click.stop="handelclick(1,$event)">点击</button>
```
#### once
事件只触发一次（常用）
```html
<button @click.once="handelclick(1,$event)">点击</button>
```
#### capture
使用事件的捕获模式
```html
<button @click.capture="handelclick(1,$event)">点击</button>
```
#### self
只有event.target是当前操作的元素时才触发事件
```html
<button @click.self="handelclick(1,$event)">点击</button>
```
#### passive：
事件的默认行为立即执行，无需等待事件回调执行完毕。

很多具有默认行为的标签事件会在事件处理函数全部执行结束后才进行默认行为，passive可以不等待事件处理函数执行结束就直接加载默认行为。
```html
<button @click.passive="handelclick(1,$event)">点击</button>
```

**事件修饰符可以连写，表示多个修饰应用在同一事件上，修饰符连写的顺序不同对应不同的效果**
```html
<button @click.passive.once="handelclick(1,$event)">点击</button>
```

### 键盘事件
Vue中提供了一种方便的写法来识别某些特定键所触发的键盘事件，无需自己在事件处理函数中进行判断

Vue中常用的按键别名：
* 回车 => enter
* 删除 => delete (捕获“删除”和“退格”键)
* 退出 => esc
* 空格 => space
* 换行 => tab (特殊，必须配合keydown去使用)
* 上 => up
* 下 => down
* 左 => left
* 右 => right

Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case（短横线命名）

系统修饰键ctrl、alt、shift、meta 键配合keyup使用时无法直接识别，需要按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。 配合keydown使用正常触发事件。
```html
<body>
<div id="root">
    <input type="text" v-bind:placeholder="msg" @keydown.enter="handelkeydown">
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            msg:"nihao!"
        },
        methods:{
            handelkeydown(event){
                console.log(event.target.value)
            }
        }
    })
</script>
</body>
```

别名也可以连续使用
```html
<input type="text" v-bind:placeholder="msg" @keydown.crtl.k="handelkeydown">
```

## 计算属性
使用一个简单的姓名案例来实现对于计算属性的理解，输入姓和名，呈现出全名

### 原始实现
使用v-model的数据双向绑定机制，用户在input框输入改变`value`值并且改变Vue实例`vm`身上的属性值，`vm`中的`_data`中的响应式属性发生变化，导致模板从新解析并渲染，从而影响`span`中的值。
```html
<body>
    <div id="root">
        <input type="text" v-model:value="xing">
        <br>
        <input type="text"  v-model:value="ming">
        <br>
        <span>{{xing}}{{ming}}</span>
    </div>
    <script>
        var vm=new Vue({
            el:"#root",
            data:{
                xing:"",
                ming:""
            })
    </script>
</body>
```

### methods实现
使用插值语法键入一个函数调用表达式来返回一个由已有属性计算得到的值。**`_data`中的响应式属性一旦变化就会重新解析模板，模板中所有的`methods`函数调用都会重新执行，即使函数不依赖`_data`中的属性值**

methods实现的一个缺点是即使是相同的值在不同位置多次使用也需要计算多次，开支较大
```html
<body>
    <div id="root">
        <input type="text" v-model:value="xing">
        <br>
        <input type="text"  v-model:value="ming">
        <br>
        <span>{{fullname()}}</span>
    </div>
    <script>
        var vm=new Vue({
            el:"#root",
            data:{
                xing:"",
                ming:""
            },
            methods:{
                fullname(){
                    console.log("11")
                }
            }
        })
    </script>
</body>
```

### 计算属性实现
Vue中的计算属性定义在Vue配置对象的`computed`property中，每个计算属性都需要设置一个`get`访问器，当模板中访问计算属性时，由`get`给出计算属性的值

计算属性基于Object.defineProperty实现，在Vue实例`vm`上代理了一个**完全依赖`_data`中属性计算而来的虚拟属性**。

与methods实现相比，计算属性内部有缓存机制，复用时无需二次计算，效率更高。

只要在配置对象中加入了计算属性，生成的Vue实例`vm`上就会有对应的属性，但是当模板中未使用计算属性时，`vm`不会调用`get`来计算值。

get函数会在模板中初次读取计算属性时会执行一次，并将结果缓存。 当依赖的数据发生改变时，由于计算属性依赖的属性都是响应式属性，模板会重新渲染，同时会在下次访问计算属性时调用`get`并缓存值。

如果想主动修改计算属性则需要配置`set`，并且可以在`set`中修改依赖的响应式属性

**Vue对计算属性中的`get`和`set`做了处理，其中的`this`都指向Vue实例`vm`，这使得我们可以方便的访问`vm`上的响应式属性**
```html
<body>
<div id="root">
    <input type="text" v-model:value="xing" v-on:keydown="handlekey">
    <br>
    <input type="text"  v-model:value="ming">
    <br>
<!--    <span>{{fullname}}</span>-->
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            xing:"",
            ming:""
        },
        computed:{
            fullname:{
                get(){
                    console.log("fullname")
                    return this.xing+this.ming
                },
                set(value){
                    this.xing=value[0]
                    this.ming=value[1]
                }
            }
        },
        methods:{
            handlekey(event) {
                console.log(this)
            }
        }
    })
</script>
</body>
```

### 计算属性简写
当计算属性只需要读而不需要修改时，可以不写`set`，且简写为函数。
```html
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            xing:"",
            ming:""
        },
        computed:{
            fullname(){
                    console.log("fullname")
                    return this.xing+this.ming
                }
        })
</script>
```

## 属性监视
属性监视写在配置对象的`watch`property中，用来监视Vue实例配置对象中`data`property中属性的变化，当`data`中的属性变化时，自动调用我们设置好的`handler`回调函数。

`watch`中以要监视的属性名为键，以一个配置对象为值。配置对象中至少要实现一个`handler`方法，每当监视属性发生变化时，`handler`方法被自动调用，同时传入两个参数值`newvalue,oldvalue`,分别表示变化后的新值和变化前的旧值。

**Vue底层使`handler`内部的`this`指向了Vue实例`vm`，可以在`handler`内部访问`vm`身上的所有属性。**

使用天气案例来进行演示，实现一个按钮切换天气情况，并且监视`ishot`属性的变化，输出变化前后的值。
```html
<body>
<div id="root">
    <span>今天天气很{{weather()}}</span>
    <br>
    <button @click="ishot=!ishot">切换</button>
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            ishot:true
        },
        methods:{
           weather(){
               return this.ishot? "很热" :"凉快"
            }
        },
        watch:{
            ishot: {
                handler(newvalue,oldvalue,){
                    console.log(newvalue,oldvalue)
                }
            }
        }
    })
</script>
</body>
```

监视属性配置对象中也可以设置其他的选项，如`immediate:true`，会在页面刚渲染完（即使模板中没有使用监视的属性）就执行一次`handler`方法

**监视属性可以监视计算属性**

可以不再配置对象中进行监视，可以在生成Vue实例`vm`后，使用`vm.watch`来监视属性。
```html
<script >
    vm.$watch("ishot",{
        immediate:true,
        handler(newvalue,oldvalue,){
            console.log(newvalue,oldvalue)
        }
    })
</script>
```

### 深度监视
默认形况下，`watch`中监听的属性只监听属性对应的存储单元中的值是否发生改变，当属性值是一个对象时，属性对应的存储单元中存放的是一个地址，属性内部发生变化并不会引起存储单元中的地址改变，所以`watch`无法监视到，需要添加`deep:true`选项来进行深度监视。
```html
<script >
    watch:{
        immediate:true, 
        deep:true,
        ishot(newvalue,oldvalue){
            console.log(this)
            console.log(newvalue,oldvalue)
        }
    }
</script>
```

### watch简写
当监视属性不需要配置除`handler`以外的所有配置项时，可以进行简写。在`watch`中定义以监视属性命名的函数。
```html
<body>
<div id="root">
    <span>今天天气很{{weather()}}</span>
    <br>
    <button @click="ishot=!ishot">切换</button>
</div>
<script>
    var vm=new Vue({
        el:"#root",
        data:{
            ishot:true
        },
        methods:{
           weather(){
               return this.ishot? "很热" :"凉快"
            }
        },
        watch:{
            ishot(newvalue,oldvalue,){
                console.log(this)
                console.log(newvalue,oldvalue)
            }
        }
    })
</script>
</body>
```

## 计算属性和监听属性的使用
计算属性中，依赖`get`访问器的返回值来得到计算属性的值，当我们在计算属性的`get`访问器中进行异步任务并想将异步任务得到的结果作为返回值时会出现问题。

当`get`中执行到异步任务后，异步任务的回调函数并不会立即执行，而是放入任务队列，JS引擎继续执行后续任务，可能返回的就是`undefined`。

而监听属性并不通过`Object.defineproperty`代理至Vue实例`vm`上，而是直接做出某种行为。并且监听属性的`handler`方法中的`this`指向vm,所以可以在监听属性中进行异步任务并修改Vue实例`vm`身上的响应式属性，让模板从新解析

**被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。**

但此处`watch`中的监听属性`firstName`中的定时器中的回调函数为箭头函数，这里必须这样实现。因为定时器的回调函数会由JS引擎调用，而我们还想在回调函数中访问`vm`身上的属性，所以应该使用箭头函数，使其中的`this`绑定为外部函数`firstName`的`this`。同时`firstName`又是Vue管理的函数，所以`this`指向`vm`。

**所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数， 这样this的指向才是vm 或 组件实例对象。**
```html
	<body>
		<!-- 
				computed和watch之间的区别：
						1.computed能完成的功能，watch都可以完成。
						2.watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作。
				两个重要的小原则：
							1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。
							2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，
								这样this的指向才是vm 或 组件实例对象。
		-->
		<!-- 准备好一个容器-->
		<div id="root">
			姓：<input type="text" v-model="firstName"> <br/><br/>
			名：<input type="text" v-model="lastName"> <br/><br/>
			全名：<span>{{fullName}}</span> <br/><br/>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				firstName:'张',
				lastName:'三',
				fullName:'张-三'
			},
			watch:{
				firstName(val){
					setTimeout(()=>{
						console.log(this)
						this.fullName = val + '-' + this.lastName
					},1000);
				},
				lastName(val){
					this.fullName = this.firstName + '-' + val
				}
			}
		})
	</script>
```

## Vue绑定类
Vue中标签固定的类直接写在标签的`class`中，而需要动态调整的类可以使用`v-bind:class="xxxx"`来进行添加。其中`xxxx`可以是已定义的类名、数组、对象。
* 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定。一般将类名维护成`data`中的属性值，通过改变`data`中的属性来从新解析模板，这样可以避免操作真实DOM。
* 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定。**数组中是类名字符串（不能是类名，否则Vue会去`vm`上找同名属性）**
* 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用。对象中是键值对，类名作为键，值为真假，表示是否应用此样式。Vue会自动解析对象，添加对应的样式。

绑定的类一定由对应的类样式存在
```html
    <style>
        .d{
            height: 100px;
            width: 100px;
        }
        .bgc{
            background-color: red;
        }
        .fs{
            font-size: 40px;
        }
    </style>
</head>
<body>
<div id="root">
    <div class="d" :class="classname">div1</div>
    <br>
    <div class="d" :class="classarr">div2</div>
    <br>
    <div class="d" :class="classobj">div3</div>
</div>
<script>
   new Vue({
       el: "#root",
       data:{
            classname:"bgc",
            classarr:["bgc","fs"],
            classobj:{
                bgc:true,
                fs:true
           }
       }
   })
</script>
</body>
```

## Vue绑定style
Vue中标签固定的样式直接写在标签的`style`中，而需要动态调整的类可以使用`v-bind:style="xxxx"`来进行添加。其中`xxxx`可以是规定格式的的样式表达式、数组、对象。

无论使用哪种写法，style中CSS样式的名称都有一样的规定，一个单词的样式名称不变，多个单词的样式名称中`-`去掉，使用驼峰命名法。

* 绑定style样式--字符串写法。要实现字符串写法，我们最终应在`xxxx`中写入形如`fontSize:40px`的字符串，但模板中要求`xxxx`应该是一个表达式，而`fontSize:40px`并不符合表达式的形式，我们需要使用一个计算属性对其进行封装。
* 绑定style样式--对象写法。对象中使用别名的style样式作为键，style值作为值。
* 绑定style样式--数组写法。数组中实际上是一个个样式对象。
```html
<body>
<div id="root">
    <div class="d" :style="fontsize">div1</div>
    <br>
    <div class="d" :style="styleobj">div2</div>
    <br>
    <div class="d" :style="stylearr">div3</div>
</div>
<script>
    new Vue({
        el: "#root",
        data:{
            font_size:40,
            styleobj:{
                fontSize: "40px",
                backgroundColor:"red"
            },
            stylearr:[
                {fontSize: "40px"},
                {backgroundColor:"red"}
            ]
        },
        computed:{
            fontsize:{
                get(){
                    return "fontSize:"+this.font_size+"px"
                }
            }
        }
    })
</script>
</body>
```


## Vue响应式数据的原理

### 实现监视对象属性变化
一般可以利用`Object.defineProperty`方法来监视对象中某一属性是否被修改或者读取。

下述代码无法正常工作，因为直接在对象自身实现`Object.defineProperty`，当访问`name`时会引起循环调用，直至栈溢出。
```javascript
  let obj={
    name:"zxr"
  }
  Object.defineProperty(obj,"name",{
    get(){
      console.log("name被访问")
      return obj.name
    },
    set(value){
      console.log("name被修改")
      obj.name=value
    }
  })
```

下述代码可以正常工作，且实现了监视对象属性。

**这里利用了闭包，`obj`作为匿名对象传入构造函数`observe`中，而`observe`内部的`get`和`set`方法依赖外部函数中的局部变量，从而形成闭包，`obj`作为闭包环境中的变量持续存在在内存中。**

`obs`对象代理了`obj`对象中数据的查询和修改，是`obj`的监视者，实现了数据监视。
```javascript
  function observe(obj) {
    let keys=Object.keys(obj)
    console.log(keys)
    for(let key of keys){
      console.log(key)
      Object.defineProperty(this,key,{
        get(){
          return obj[key]
        },
        set(value){
          obj[key]=value
        }
      })
    }
  }
  let obs=new observe({
    name:"zxr",
    age:24
  })
  console.log(obs)
```

**Vue中正是利用了上述机制在`vm`身上实现了`vm._data`这样的监视对象来监视传入的配置对象中的`data`中的数据改变。**

此处还有一个细节，在Vue中，`vm._data`和`data`是相等的，但是在上述代码中没有实现这个功能。下述代码可以验证
```html
<body>
<div id="root"></div>
    <script>
        let obj={
            el:"#root",
            data:{
                name:"zxr"
            }
        }
        var vm=new Vue(obj)
        console.log(vm._data===obj.data)
        //true
    </script>
</body>
```

上述代码中也可以实现这个细节。

在上个自定义实现中，并没有把`obj`单独定义出来，而是作为匿名函数直接传入构造函数中。此处将`obj`单独定义出来，再传入构造函数。这时并没有形成闭包，构造函数中的`get`和`set`引用的是全局变量`obj`。

最后一句将`obj`赋值为`vm`，此处可能会产生误解，这样`obj`和`obs`不就完全相等了，难道不会再形成循环访问吗？

**这里确实是不会的，在JS中，对象类型中保存的是堆中真实数据结构的地址（引用）。这里将`obs`赋值为`obj`实际上只改变了`obj`的引用，而原来`obj`指向的数据并没有消失，因为构造函数内部的`get`和`set`还在引用，这时再次形成了闭包。**
```javascript
let obj={
    name:"zxr",
    age:20
}

function observe(obj) {
    let keys=Object.keys(obj)
    console.log(keys)
    for(let key of keys){
        console.log(key)
        Object.defineProperty(this,key,{
            get(){
                return obj[key]
            },
            set(value){
                obj[key]=value
            }
        })
    }
}
let obs=new observe(obj)
// 只需要增加这一句赋值代码
obj=obs
```

### 监视数组属性时的问题
如果属性是一个数组，Vue并不会监视数组内部元素的变化。输出`vm`会发现，数组属性内部元素并没有对应的`set`访问器。

下述代码中，点击按钮姓名窗口中的姓名不会改变，但Vue实例`vm`中的数据发生了改变，但是Vue并没有重新编译模板并渲染
```html
<body>
<div id="root">
    <ul>
        <li v-for="stu in students">{{stu}}</li>
    </ul>
    <button @click="handleclick">改名</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            students:["zxr","mfl","ljy"]
        },
        methods:{
            handleclick() {
                console.log("点击")
                this.students[0]="xxx"
            }
        }
    })
</script>
</body>
```

但如果数组属性中存储的是对象，改变数组中对象的属性可以引起Vue对模板重新编译和渲染。同时这些对象的属性也有`set`方法。
```html
<div id="root">
    <ul>
        <li >{{students[0].name}}</li>
    </ul>
    <button @click="handleclick">改名</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            students:[
                {name:"zxr",age:"25"},
        },
        methods:{
            handleclick() {
                console.log("点击")
                this.students[0].name="xxx"
                console.log( this.students[0].name)
            }
        }
    })
</script>
</body>
```

即使数组属性中存储的是对象，但是直接对数组中数字索引的赋值操作都不会被Vue监视。下述代码将一个新的对象`stu`赋值给数组属性的数字索引并不会被Vue察觉。
```html
<body>
<div id="root">
    <ul>
        <li >{{students[0].name}}</li>
    </ul>
    <button @click="handleclick">改名</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            students:[
                {name:"zxr",age:"25"},
        },
        methods:{
            handleclick() {
                console.log("点击")
                let stu={name:"mfl",age:"20"}
                this.students[0]=stu
            }
        }
    })
</script>
</body>
```
**综上，只要是对`data`中数组属性中的数字索引直接赋值的操作都不会被Vue所监视到**

Vue提供了7个数组方法，使用这些方法来改变`data`中的数组属性可以被Vue监视到。
来改变`data`中的数组属性可以被Vue监视到。
* push()
* pop()
* shift()
* unshift()
* splice()
* sort()
* reverse()


## Vue.set()
直接在`vm`身上或者`vm._data`中添加属性是非响应式，需要使用`Vue.set()`来添加响应式属性。

`Vue.set( target, propertyName/index, value)`，**其中`target`不能是不能是 Vue 实例`vm`，或者 Vue 实例的根数据对象`vm._data`**

**`Vue.set()`方法还可以用于对`data`中数组属性中的数字索引直接赋值的操作**
```html
<body>
<div id="root">
  <ul>
    <li v-for="stu in students">{{stu}}</li>
  </ul>
  <button @click="handleclick">查看</button>
</div>
<script>
  var vm=new Vue({
    el:"#root",
    data:{
      students:["zxr","mfl","ljy"]
    },
    methods:{
      handleclick() {
        this.students[0]="xxx"
      }
    }
  })
  Vue.set(vm.students,3,"hcz")
  // 页面上展示第四条学生的姓名
  vm.students[3]="hcz"
  //只有`vm`身上有，但是页面不展示，同时也不是响应式的。     
</script>
</body>
```

## Vue中使用表单

label标签可以用来匹配某个input框,可以实现点击label中的内容就聚焦到对应的input框

对于`type="text"`的文本框,使用`v-model:`双向绑定响应式元素

对于`type="radio"`的单选框,当单选框没有`value`属性时,即使勾选了某个单选框,实际得到的值也为`null`
```html
    <label for="r1">男</label>
    <input type="radio" id="r1" name="sex"  v-model="sexy">
    <label for="r1">女</label>
    <input type="radio" id="r2" name="sex"  v-model="sexy">
```

对于`type="checkbox"`的复选框,当没有配置`value`属性时,`v-model`默认收集的是`checked`数据.

当复选框配置了`value`属性时,且`v-model="xxxx"`中的`xxxx`为数组时.选中某个复选框时,会自动向`xxxx`数组中添加勾选的复选框的`value`值.当数组中的某一项被删除时,复选框也会自动取消勾选.
```html
<div>
    跑步<input type="checkbox" name="hobby" value="run" v-model="hobby">
    跳高<input type="checkbox" name="hobby" value="jump" v-model="hobby">
    游泳<input type="checkbox" name="hobby" value="swim" v-model="hobby">
    健身<input type="checkbox" name="hobby" value="workout" v-model="hobby">
</div>
```


## Vue路由

### 基本使用
1. vue2.x使用vue-router@3路由插件，下载插件`npm install vue-router@3`
2. main.js中引入`VueRouter`路由器。
3. 






## ref
在一个组件内部，给一个html标签或者自定义子组件标签添加`ref='xxx'`属性，可以在组件实例对象访问到对应元素的真实DOM或者子组件的实例对象。

## props
接收父组件传入的参数，可以提高组件复用。

传入的props除了字符串形式，其他的类型都需要进行数据绑定`v-bind:`。

props传入的数据会挂载在组件的实例化对象身上，禁止直接修改（直接修改可以修改，但是会报错，且可能造成其他错误）。

如果需要修改props的值，一般将props作为初值赋值给一个新的内部属性，或者顶一个依赖props的计算属性。

## mixin混入
用来进行组件配置对象的部分复用，在一个js文件中定义一个配置对象并暴露，可以选择在全局混入，也可以选择在需要的组件中混入。

组件加入mixin后，会和之前的配置对象合并，原来有的不覆盖，只添加原来没有的。

使用`Vue.mixin(混入器)`会在Vue实例和所有的组件实例化对象都会被混入。

## 插件
在一个js文件中暴露的一个对象，对象至少实现了一个`install`方法，`install`方法会传入`Vue`构造函数，可以向构造函数添加混入，过滤器，原型方法，自定义指令等。

## scope
组件样式上加上`scope`属性，可以使样式模块化，并且可以通过`lang=""`来指定CSS的语言。不过需要使用对应的loader。

## 过滤器
过滤器简单来说就是一个函数，对传入的数据进行处理并返回。局部过滤器写在组件配置对象中的`fliters`中，全局过滤器使用`Vue.fliters()`进行添加，会被放在所有组件实例化对象和Vue实例身上。


## Vuex

### 引入
Vue2引入的应该是Vuex3。在main.js中引入Vuex后，使用Vue.use(Vuex)，并且在创建`vm`时传入的配置对象中添加`store`属性，就可以让所有的组件实例对象和`vm`身上有`$store`属性。








