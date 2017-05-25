## 我自己的答案
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
var flatten = function(args){
    return args.reduce((prev, current, index, arr) => {
    	return prev.concat(Array.isArray(current) ? flatten(current) : current);
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
