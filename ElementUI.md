# ElementUI
Element中的组件Attributes大部分是作为组件props传入的，部分Attribute允许用户指定值，也有一部分不支持传入值。

Attributes值的指定也符合Vue2中指定props的语法，如果值为字符串时，可以不在Attribute前写`:`，其余类型的值都需要在Attribute前加`:`

Attributes的值可以为任何基础数据类型或者是Vue实例、组件实例对象身上的属性。
## Form的使用

### <el-form>的常用属性
<el-form>标签会被转换为form标签，可以传入一些规定好的`props`

* `:model=""`用于指定整个表单绑定的对象。（但实际上并没有与表单中的数据绑定，更多是用于语义化的显示和表单验证）
* `:rules=""`用于进行表单校验，值为对象。
* `:inline=""`值为真假。

### <el-form-item>的常用属性
<el-form-item>会被转换为一个div，一个div表示表单中的一项。

<el-form-item>主要用来进行表单验证和显示表单元素的标签。

常用属性：
* `label=""`用于定义表单项的标签，值一般为字符串
* `:props=""`用于指定整个表单绑定的数据对象中的属性，绑定后会按照表单的`rules`对此表单项进行验证。
也可以自定义的传入`rules`配置对象。

### 
