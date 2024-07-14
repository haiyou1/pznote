## D7.14 简单算法

### 数组元素去重，并且是直接修改原数组然后返回数组长度。

我一开始是用的 map 映射，因为将数据为键，重复的可以覆盖掉。但是似乎提交这种的有问题就换一种思路了。一开始都是用的 hash 来做的。但是这个数组++的部分 js 不清楚如何++。

```js
//可以用对象来进行存储，再push到数组中。因为对象中没有的话会是undefined的。但是我进行赋值的话，对应的值就不是undefined了。
//上述有问题，push用result再赋值给nums，会报错。覆盖不过去
let count = {};
let result = [];
let writeIndex = 0;
for (let i = 0; i < nums.length; ++i) {
	if (count[nums[i]] === undefined) {
		count[nums[i]] = 1;
		nums[writeIndex] = nums[i];
		writeIndex++;
	}
}
return writeIndex;
```

第二种感觉是对的但是提交为空数组的：

```js
const map = new Map();
let nums = [0, 0, 1, 1, 1, 1, 2, 2, 3, 3, 4, 5, 5, 6, 6, 6];
for (let i = 0; i < nums.length; ++i) {
	map.set(nums[i], nums[i]);
}
const key = map.keys();
// console.log(map);
const arrkey = Array.from(key);
nums = [];
for (let j = 0; j < arrkey.length; ++j) {
	nums[j] = arrkey[j];
}
console.log(nums);
console.log(Array.from(map.keys()).length);
//不知道为什么这样子传参会为空。
```

然后第三种想法是用的暴力枚举，结果时间超时了。双重循环，用的向前移动覆盖的想法。

```js
var removeDuplicates = function (nums) {
	let sign = nums[0];
	for (let i = 0; i < nums.length - 1; ++i) {
		sign = nums.find((num) => num > sign); //找到比他大的数据后将后面相同的当前数据全部修改为比他大的。如此循环超时了。
		for (let j = i + 1; j < nums.length; ++j) {
			if (nums[j] == nums[i]) {
				nums[j] = sign;
			}
		}
	}
	console.log(nums);
	let res = 0;
	for (let n = 0; n < nums.length; ++n) {
		if (nums[n] != undefined) {
			res++;
		}
	}
	return res;
};
```

后面发现，我都不用向前移动每项数据。因为这个题目是单向递增的。所以直接可以用单循环来找并赋值。只需要我保留一下当前的数据的下标就行了。我用下标向前移动是真的纯啊。这样记录一个下标直接用来赋值不就行了？

```js
let writeIndex = 1; // 指向下一个应该写入的位置

for (let readIndex = 1; readIndex < nums.length; readIndex++) {
	if (nums[readIndex] !== nums[readIndex - 1]) {
		//找到不一样的后，对记录的下标进行赋值就行了。然后对记录的下标向后移动一位。
		nums[writeIndex] = nums[readIndex];
		writeIndex++;
	}
}
// 最后移除多余的元素 这里也可直接返回writeIndex就是长度。
nums.splice(writeIndex);
return nums.length;
```

### 题 35 搜索插入位置(简单题)

描述：给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

```js
//简单到我都能一次过。。。。
let count = 0; //在一次循环中边查找，边比对就行了。
while (count < nums.length) {
	if (target < nums[count]) {
		break;
	} else if (target >= nums[count]) {
		if (target == nums[count]) {
			return count;
		}
		count++;
	}
}
return count;
//还有更快的方法吗？
// 尝试了另一种却更慢了：
let count = nums.indexOf(target);
if (count != -1) {
	return count;
} else if (nums[nums.length - 1] < target) {
	return nums.length;
} else {
	return nums.indexOf(nums.find((num) => num > target));
}
```

更快的二分查找

```js
let left = 0,
	length = nums.length,
	right = length - 1;
while (left <= right) {
	//注意要把这个mid放到循环里面来每次更新才对。
	let mid = Math.floor((left + right) / 2);
	if (nums[mid] == target) {
		return mid;
	} else if (nums[mid] < target) {
		// 就死如果目标值比中间值要大的话，就把左放到中间值右边来。
		left = mid + 1;
	} else {
		right = mid - 1; // 如果和中间值比较，目标值较小的话，说明需要把右边调到中间值的左边去。
	}
}
return left; //最后会指向一个的，左边的直接写。 返回right的话需要 +1位置才对应上。
```
