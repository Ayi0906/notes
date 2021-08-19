```
$('btn').click(function(){
	$('btn-1').trigger('click')
	// 模拟事件会冒泡，需要在$('btn-1').click(function(){})中禁止冒泡
})
```

