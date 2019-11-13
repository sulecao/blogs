
```vue
<!-- v-model指令  -->
<input type="text" v-model="value1">
<p>{{value1}}</p>
<!-- v-model是一个语法糖，通过监听input事件获取输入值赋值给value  -->
<input type="text" :value="value2" @input='value2 = $event.target.value'>
<p>{{value2}}</p>
<!-- 自己引用input组件  -->
<my-input v-model="value3"></my-input>
<p>{{value3}}</p>
```
  ```vue
  <!-- input组件  --> 
<template>
      <input type="text" :value="value" @input='changeInput($event)'>
</template>
<script>
export default {
  name: '',
  props:['value'],
  data() { 
    return {
    }
  },
  methods:{
      changeInput(event){
          this.$emit('input',event.target.value)
      }
  }
 }
</script>
  ```

#### v-model的修饰符

.lazy：可以移除实时双向绑定转：输入框失去焦点后会更新数据。而不会在输入时一直更新数据。
.number：自动将用户的输入值转为数值类型，如果这个值无法被 parseFloat() 解析，则会返回原始的值。
.trim：自动过滤用户输入的首尾空白字符。

