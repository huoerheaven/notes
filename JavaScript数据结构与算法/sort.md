##### 注:各种排序动画可参考 [VISUALGO.NET](https://visualgo.net/zh/sorting)

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
- 时间复杂度：**O(nlogN)**
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

#### 3. 归并排序
 > 思路 
- 分：把数组劈成两半，再递归地对子数组进行“分”操作，直到分成一个个单独的数
- 合：把两个数合并为有序数组，再对有序数组进行合并，直到全部子数组合并为一个完整数组
> 合并两个有序数组   
- 新建一个空数组res,用于存放最终排序后的数组；
- 比较两个有序数组的头部，较小者出队并推入res中；
- 如果两个数组还有值，就重复第二步；
> 时间复杂度   
- 分的时间复杂度是O(logN)
- 合的时间复杂度是O(n)
- 时间复杂度：**O(nlogN)**
```js
Array.prototype.mergeSort = function(){
    const rec=function(arr){
        if(arr.length===1) return arr;
        const midIndex = Math.floor(arr.length/2);
        const left = arr.slice(0,midIndex);
        const right = arr.slice(midIndex,arr.length);
        const orderLeft = rec(left);
        const orderRight = rec(right);
        let res=[];
        while(orderLeft.length || orderRight.length){
            if(orderLeft.length&&orderRight.length){
                res.push(orderLeft[0]>orderRight[0]?orderRight.shift():orderLeft.shift())
            }else if(orderLeft.length){
                res.push(orderLeft.shift())
            }else if(orderRight.length){
                res.push(orderRight.shift())
            }
        }
        return res;
    };
    let arrRes = rec(this);
    arrRes.forEach((item,index)=>this[index]=item);
    
}
let arr=[3,2,1,5,4];
arr.mergeSort();
console.log(arr);//[1,2,3,4,5]
```

#### 4. 冒泡排序
> 思路  
- 比较所有相邻元素，如果第一个比第二个大，则交换他们。
- 一轮下来，可以保证最后一个数是最大的
- 执行n-1轮，就可以完成排序
> 时间复杂度：**O(N^2) **
```js
Array.prototype.BubbleSort=function(){
    for(let i=0;i<this.length-1;i++){
        for(let j=0;j<this.length-1-i;j++){
            if(this[j]>this[j+1]){
                let temp = this[j];
                this[j]=this[j+1];
                this[j+1]=temp;
            }
        }
    }
}

let arr=[2,4,1,5,3];
arr.BubbleSort()
console.log(arr);//[1,2,3,4,5]
```

#### 5. 选择排序
> 思路  
- 找到数组中的最小值，选中它并将其放置在第一位
- 接着找到第二小的值，选中它并将其放置在第二位
- 以此类推，执行n-1轮
> 时间复杂度：**O(n^2)**
```js
Array.prototype.selectionSort=function(){
    for(let i=0;i<this.length-1;i++){
        let minIndex=i;
        for(let j=i+1;j<this.length;j++){
            if(this[minIndex]>this[j]){
                minIndex=j;
            }
        }
        let temp=this[i];
        this[i]=this[minIndex];
        this[minIndex]=temp;
    }
}
let arr=[2,1,4,5,3];
arr.selectionSort();
console.log(arr);//[1,2,3,4,5]
```
