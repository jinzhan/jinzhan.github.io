## 自己写的参考答案
11.

```
// 1
$('ul').click('li', function(){
    console.log($(this).index());
});

// 2
var li = document.getElementsByTagName('li');
var i = 0;
var len = li.length;
for(;i<len;i++){
  !function(i){
    li[i].onclick(function(){alert(i)});
  }(i);
}

```

14.
```
var flatten = function(arg){
    return arg.reduce((prev, current, index, arr) => {
    	Array.isArray(current) ? flatten(current) :  prev.push(current);
    }, []);
};

```


15.
```
var bubbling = function(arr){
	var len = arr.length;
	var i;
	while(len--) {
		for(i = 0;i<len;i++) {
			if(arr[i] > arr[i+1]) {
				arr[i] ^= arr[i+1];
				arr[i+1] ^= arr[i];
				arr[i] ^= arr[i+1];
			}
		}
	}
	return arr;
};
```
