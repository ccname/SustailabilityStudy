### computed vs methods
我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

### vue 其他指令
```
  v-pre 原样显示（<p v-pre>{{message}}</p>）即使与js绑定了数据的交互。浏览器也是显示{{message}}
  
  v-clock 渲染完成后，才显示！
  v-once  只渲染一次。eg:与v-model 双向数据绑定时，只改变一次。
```
### 使用淘宝的镜像，安装cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
### style作用域
```
# scoped 局部作用域
<style scoped>
</style>
```

### v-model属性
```
<input v-model.lazy = 'name'> 
懒惰加载，光标离开输入框才进行双向绑定。
v-model.trim  去掉输入两端的空格
v-model.number  自动把字符串数字转换成数字
```
### vue-route 路由

安装：bootcdn.cn  搜索vue-router 复制标签  <script src="https://cdn.bootcss.com/vue-router/3.1.3/vue-router.js"></script>
```
#定义路由组件
var routes = [
	{
		path: '/',
		component: {
			template:`<div><h1>首页</h1></div>`
		}
	},
]
#注册路由及使用
var router = new VueRouter({
	routes:routes
})
new Vue({
	el:"#app",
	router: router
})
#html 写法
<div id="app">
	<div>
		<router-link to="/">首页</router-link>
	</div>
	<div>
		<router-view></router-view>
	</div>
</div>


#路由中可以有子路由
var routes = [
	{
		path: '/',
		component: {
			template:`<div><h1>首页</h1></div>`
		}
		children: [
			{
				path: 'more',
				component: {
					template:`<div>生
					用户{{$route.params.name}}的详细信息
					</div>`
				},
			}
		]
	},
]

# 子路由的写法 可以用追加可以用拼接：
<router-link to="/more" append>更多...</router-link>
<router-link :to="'/user/' + $route.params.name + '/more'">更多...</router-link>

```

- v-show="false" （相当于display=none） 和 v-if="false"(页面中不会出现这个标签) 区别 
