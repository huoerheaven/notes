#### 一、斐波那契数列
> 1,2,3,5,8,13...(斐波那契数列第一个数字是1，第二个是2，从第三项开始，每一项都等于前两项之和)
```
let fibonacci = [];
//省略fibonacci[0]
fibonacci[1]=1;
fibonacci[2]=2;
for(let i=3;i<=20;i++){
  fibonacci[i] = fibonacci[i-1]+fibonacci[i-2]
}
```
#### 二、数组的方法
> push
>> 从数组尾部添加元素，返回新数组的长度，会改变原数组；
> pop
>> 从数组尾部删除元素，返回被删除的元素，会改变原数组；
> unshift
>> 从数组头部添加元素，返回新数组的长度，会改变原数组；
> shift
>> 从数组头部删除元素，返回被删除的元素，会改变原数组；
> splice
> slice
> concat
> every
> some
> forEach
> filter
> map
> reduce
> join
> indexOf
> lastIndexOf
> reverse 
> sort
> toString
> valueOf
>
