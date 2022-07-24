# Vue

## 指令

```javascript
var vm = new Vue({
  el: '',
  data: {
    
  },
  methods: {
    fun:function(event){
      
    }
  },
  computed: {
    计算属性
  }
  
})
```

- if-else

```html
<h1 v-if="判断条件"></h1>
<h1 v-else-if=""></h1>
<h1 v-else=""></h1>
```

- for

```html
<h1 v-for="x in date">
  {{x}}
</h1>
```

- v-on绑定事件

```html
<button v-on:onclick="fun()"></button>
```



## 自定义指令

``` JavaScript
directives: {
//自定义指令名
	//element是指令所在的标签，binding指令的基本信息----简写
    big(element, binding){
		
    },
        
    
    big{
        //指令与元素绑定时调用---简写默认
        bind(element, bindingelement, binding){
            
        },
        //指令所在元素被插入页面时
        inserted(element, binding){
            
        },
        //指令所在模板被重新解析时
        update(element, binding){
            
        }
    }

}
```

> 指令名称多个单词用 - 分割，不适用驼峰命名；；；
>
> 自定义指令中的this是window对象





## vue生命周期函数

|                         生命周期函数                         |                        vue初始化过程                         |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                                                              |              ***初始化数据代理，数据监测流程***              |
|                                                              |                   ==初始化生命周期和事件==                   |
| **beforeCreate**<br />（初始化数据代理和数据监测，无法访问到data和methods中的数据） |                                                              |
|                                                              |                 ==初始化数据监测和数据代理==                 |
| **created**<br />（初始化完成，可以访问到data和methods中的数据） |                                                              |
|                                                              | ==vue开始解析模板，生成虚拟dom，在内存中，但是页面中未显示== |
| **beforeMount** <br />（此时页面中显示的是未编译的dom，对dom操作最终都不生效） |                                                              |
|                                                              |          ==将内存中的虚拟dom转换为真实dom插入页面==          |
| **mounted**<br />(都是经过编译的dom，但是避免此过程操作，此过程一般进行开启定时器，发送网络请求，订阅消息) |                                                              |
|                                                              |                      ***数据更新流程***                      |
|    **beforeUpdate**<br />此时数据是最新的但是为渲染到页面    |                                                              |
|                                                              |                       ==开始更新页面==                       |
|           **updated**<br />此时页面和数据保持同步            |                                                              |
|                                                              |         ***销毁vm流程，此阶段数据可访问但修改无效***         |
| **beforeDestroy**<br />此时还未销毁，所有自定义的数据都可访问，但是修改不生效，此阶段一般关闭定时器，取消订阅消息，解绑自定义事件 |                                                              |
|                                                              | ==完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。== |
|            ***destory***<br />销毁完成调用的钩子             |                                                              |
|                                                              |                      ***生命周期结束***                      |

### 







## 组件



- 关键字：components

``` html
//组件定义方式
const school = Vue.extend({
			template:`<div class="demo">
					<h2>学校名称：{{schoolName}}</h2>
					<h2>学校地址：{{address}}</h2>
					<button @click="showName">点我提示学校名</button>	
				</div>`,
			data(){
				return {
					schoolName:'尚硅谷',
					address:'北京昌平'
				}
			},
			methods: {
				showName(){
					alert(this.schoolName)
				}
			},
			//子组件
			components{
			
			}
		})
//简写
const school = Vue.extend(options) 可简写为：const school = options


//使用引入
components:{
	//组件名
}

//组件使用
<组件名称></组件名称>
```

> 组件命名
>
> 1.如果一个单词那么首字母推荐大写
>
> 2.大驼峰（脚手架）



- ***单文件组件***

``` vue
<template>
	定义模板
<template/>

<script>
	定义脚本
    ecport default {
    	//指定组件名
    	name: '',
    	data(){
            return{
                
            }
        },
        methods{
            
        }
    }
</script>

<style>
	定义样式    
</style>
```

