js实现点击显示和隐藏
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>P&L</title>
</head>
<body>
<div id="mySidenav" class="sidenav">

</div>
  <a id="main" style="font-size:30px;" onclick="switchNav()">打开/关闭</a>
<script>
    var btn = false;
	mySidenav = document.getElementById("mySidenav");
	main = document.getElementById("main");
    function switchNav() {
        btn = !btn
        if (btn === true) {
            document.getElementById("mySidenav").style.height = "250px";
            document.getElementById("main").style.bottom = "250px";
        } else {
            document.getElementById("mySidenav").style.height = "0";
            document.getElementById("main").style.bottom = "0px";
        }
}
</script>
<style>
body {
    background: azure;
}

.sidenav {
    height: 0;
    width: 100%;
    position: fixed;
    bottom: 0;
    background-color: #111;
    transition: 0.5s;
}
#main {
    transition: bottom .5s;
    position: absolute;
    bottom: 0px;
    text-align: center;
}
</style>
</body>
</html>
```