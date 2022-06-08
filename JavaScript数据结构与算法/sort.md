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

#### 2. 快速排序
> 思路  
 - 区分：从数组中任意选择一个“基准”，所有比基准小的元素放在基准前面，比基准大的元素放在基准后面；
 - 递归：递归地对基准前后地子数组进行区分
> 时间复杂度  
- 递归地时间复杂度是O(logN)
- 分区操作的时间复杂度是O(n)
- 时间复杂度：O(n*logN)
```js
Array.prototype.quickSort=function(){
    const rec=(arr)=>{
        if(arr.length<=1) return arr;
        let mid = arr[0];
        let left=[];
        let right=[];
        for(let i=1;i<arr.length;i++){
            if(arr[i]<mid){
                left.push(arr[i])
            }else{
                right.push(arr[i])
            }
        }
        return [...rec(left),mid,...rec(right)];
    }
    let arrRes = rec(this);
    arrRes.forEach((item,index)=>this[index]=item)
}

let arr=[2,1,3,5,4];
arr.quickSort();
console.log(arr);//[1,2,3,4,5]

```
