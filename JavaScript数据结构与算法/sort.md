#### 1. 插入排序
> 思路  
- 从第二个数开始**往前比**
- 比它大就**往后排**
- 以此类推进行到最后一个数
> 时间复杂度：**O(n^2)**  
```js
Array.prototype.insertionSort = function(){
    for(let i=1;i<this.length;i++){
        let temp = this[i];
        let j=i;
        while(j>0){
            if(this[j-1]>temp){
                this[j]=this[j-1];
            }else{
                break;
            }
            j--;
        }
        this[j]=temp;
    }
};
let arr=[3,2,5,1,4];
arr.insertionSort();
```
