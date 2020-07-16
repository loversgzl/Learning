# EL 组件学习
[el 组件学习官网](https://element.eleme.cn/#/zh-CN/component/form)

## Form 表单
**注意：属性绑定变量需要使用 v-bind，如 :key，:value，:disabled 等等，不绑定直接写关键字即可**  
1、在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input（输入框常与v-model一起使用）、Select（下拉选择器）、Checkbox、Radio（单选按钮）、Switch、DatePicker、TimePicker

###  <el-Form> 属性
（:inline="true"）：当垂直方向空间受限且表单较简单时，可以在一行内放置表单。
（:model="formData"）：绑定数据对象
（size="small"）：用于控制该表单内组件的尺寸
（:disable="true"）：不可修改

### <el-Form> 方法
**resetFields()**  
对整个表单进行重置，将所有字段值重置为初始值并移除校验结果，但是此表单需要将所有属性都设置到 prop="attr" 中，它会根据这些属性，清空 formData 中的对应字段。    
（注意：如果使用了 vuex 传递 formData，那么此方法无效，需要强制传入一个空对象到 vuex 清空，然后再用 vuex 赋值 formData）  
**label-position="right"**  
form表单文字的对齐方式，如果要填充空格，只能采用下面的方式。
```js
<el-form-item label="负责人：" prop="linkPeople">  
<label slot="label">负&nbsp;责&nbsp;人&nbsp;：</label>
```

### <el-Form-Item> 属性
（prop="name"）Vue 组件库 element-ui 中的 Form 表单组件提供了表单验证功能，prop 属性设置需要校验的字段名，表单验证时，就会验证 el-input 元素绑定的变量的值是否符合验证规则
表单域 model 字段，在使用 validate、resetFields 方法的情况下，该属性是必填的

### <el-radio-group v-model=""> 属性
要使用 Radio 组件，只需要设置 v-model 绑定变量，选中意味着变量的值为相应 Radio label 属性的值，label 可以是 String、Number 或 Boolean。  
注意：Radio 显示的值外置（写在括号外面的）的优先级大于 label 中的值。  
v-model：绑定的数据与哪个 label 相同就选中哪个 radio，都不相同就都不选中。  
disabled：添加了默认为 true  
border：添加边框（把按钮和文字都括起来）  
size：不写这个关键字，默认最大，写了会小一点  

### <el-select v-model=""> 属性
v-model：绑定的变量值与哪个 <el-option> 的 value 值相同， 就选中哪个，都不相同就都不选中；  
value：用来匹配 v-model  的值，进行选中；  
label：用于显示；  
key：用于向后端传递参数；  
（提示：key，value 一般相同，传递什么到后端，取回来的时候也刚好匹配上，所以需要相同）  
（且如果 value 绑定了变量，一定要添加 key 属性）




