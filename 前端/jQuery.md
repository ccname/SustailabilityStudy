- jQueryui小组件 https://www.jqueryui.org.cn/

#### 过滤器
- $('.pa').find('#child1').css('color', 'red')
- find()
- filter()
- parent()
- parents()

- $('.a').addClass('black')  给标签增加类名
- removeClass('foo')  删除类名
- hasClass('aaa')  判断类名是否存在
- text() 获取元素和修改元素
- html() 获取html元素和 修改html元素
- append()  在获取的节点末尾追加html代码
- prepend()  在节点开头追加html代码
- remove()  删除元素，和本身标签一起删除
- empty()  删除标签下的元素
- attr('href', 'http://www.baidu.com') 修改属性(显性)
- prop('href', 'http://a.com') 修改属性(隐性)
- prop('asdf', '1234') 修改不了
- attr('asdf', '1234')  attr 可以修改自定义的属性
prop attr ('text')   传一个值的话就是获取属性的值
- removeAttr('asdf')  删除标签的属性
- $('.name')datepicker()  绑定一个input框，鼠标点上去出现一个日历可供选择
- $(this).serialize() 打印序列化form表单的值


### jQuery ajax
```javascript
// 
$.ajax('http://api.github.com/users/'+username)
	.done(function(data){
		var html = '<div>我的姓名：' + data.login + '</div>' +
		'<div>我的简介：' + (data.bio || '无') + '</div>'
		gittext.append(html)
	})

$.ajax('http://api.github.com/users/ccname', {
	// url: 'http://api.github.com/users/ccname',
	type: 'get', // get,delete,put
	data: {
		username: 'whh',
		password: '123456'
	},
	dataType: 'json',
	success: function(data){
		console.log('成功', data)
	},
	error: function(){
		alert('网络繁忙！')
	}
});
// jQuery ajax封装的快捷方法
$.post('/path/to/file', {param1: 'value1'}, function(data, textStatus, xhr) {
	/*optional stuff to do after success */
});
$.get('/path/to/file', function(data) {
	/*optional stuff to do after success */
});
$.getJSON('/path/to/file', {param1: 'value1'}, function(json, textStatus) {
		/*optional stuff to do after success */
});
$.getScript('path/to/file', function(data, textStatus) {
	/*optional stuff to do after getScript */ 
});
```