#### 1. 顺序搜索
> 时间复杂度：**O(n)**   遍历数组是一个循环操作 
```js
Array.prototype.sequentialSearch = function(item){
    for(let i=0;i<this.length;i++){
        if(this[i]===item){
            return i;
        }
    }
    return -1;
}

let arr=[2,1,3,5,4];
console.log(arr.sequentialSearch(5));//3
```

#### 2. 二分搜索
> 前提：已经排好序的数组  
> 思路  
- 从数组元素的中间元素开始，如果中间元素正好是目标值，则搜索结束；
- 如果目标值大于或小于中间元素，则在大于或小于中间元素的那一半数组中搜索；
> 时间复杂度：**O(logN)**
```js
Array.prototype.binarySearch=function(item){
    let min=0;
    let max=this.length-1;
    while(min<=max){
        let mid = Math.floor((min+max)/2);
        if(item>this[mid]){
            min = mid+1;
        }else if(item<this[mid]){
            max=mid-1;
        }else{
            return mid;
        }
    }
    return -1;
}

let arr=[1,2,3,4,5];//已经排好序的数组（也可以送排序算法将没有排好序的数组进行排序）
console.log(arr.binarySearch(4));//3
```
