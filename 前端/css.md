style { }
-  white-space: nowrap  让文字在标签内不换行。
-  box-shadow: 10px 5px 5px red;  边框设置阴影效果
-  background-repeat 背景图平铺方式
-  box-sizing: border-box; 设置了这个属性，padding设置的宽度，将不会起作用
-  a[href^="https://abc.com"] {color:blue} 属性选择器，所有a标签的href属性以"https://abc.com"开头的颜色为红色。
-  [title*="点击"]{border: 1px dashed red} 属性为title并且title的值中包含点击，样式为虚线红色边框。
-  后代选择器 .a .b{border: 1px solid green}  class名为a的子标签class为b的标签作用样式。
-  **.a + div{color:red}**   '+'代表相邻选择器。就是与class="a"平级的下一个div
-  **.a ~ div{color:green}** 代表 '~'代表选中与class="a"平级的下面所有div
-  p{font-family: sans-serif} 字体样式
-  p{text-align:left} 文字样式，默认为left,可选right
，center, justify 两端对齐。
- p{text-decoration: none}默认为none，underline(下划线) line-through(删除线)...
- display: [none(不显示),block(可以将行级元素设置为块级元素)， inline(块级元素设置为行级元素)，inline-block(行级元素中的块级元素)] 行级元素不能设置上下的边距。

**Font Awesomes**
- css小图标：http://fontawesome.dashgame.com/
- 菜鸟教程使用用例 https://www.runoob.com/font-awesome/fontawesome-tutorial.html
```
<link href="//netdna.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">
<i class="fa fa-question-circle" style="font-size:48px;color:red"></i>
```

**设置div边框的阴影效果**
```
box-shadow:
-2px 0 3px -1px green, //左边阴影
0 -2px 3px -1px blue, //顶部阴影
0 2px 3px -1px red, //底部阴影
2px 0 3px -1px yellow; //右边阴影
3px 为阴影扩展半径 不难看出box-shadow: 第1像素 第2像素 第3像素 第4像素 颜色；
第1像素 的正负值确定阴影的左右；第2像素的正负值确定阴影的上下；
```

**设置表格中奇数行和偶数行的颜色不一样**
```
简单做法：
/* 偶数行背景使用样式： */
table tr:nth-child(even){
background:#D9E8F8;
}
/* 奇数行背景使用样式： */
table tr:nth-child(odd){
background:darkslategray;
}

// 通过js来控制
window.onload = function () {
    var table=document.getElementById("mytable");
    alert(table);
    for(var i=0;i<table.rows.length;i++){
        if(i%2!=0){//判断的为偶数行，从第2行开始起，偶数行设定那个颜色是蓝色
            table.rows[i].style.background='blue';
            }
        }
    for(var j=2;j<table.rows.length;j++){
        if(j%2==0){//判断的为偶数行，偶数行设定那个颜色是蓝色(从第三行开始)
        table.rows[j].setAttribute('style','backgroung-color:red');
            }
        }
};
```