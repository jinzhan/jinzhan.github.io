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
