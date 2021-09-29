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
1. push
> 从数组尾部添加元素，返回新数组的长度，会改变原数组；
2. pop
> 从数组尾部删除元素，返回被删除的元素，会改变原数组；
3. unshift
> 从数组头部添加元素，返回新数组的长度，会改变原数组；
4. shift
> 从数组头部删除元素，返回被删除的元素，会改变原数组；
5. splice
> 向/从数组中添加/删除项目，返回空数组/被删除的项目,会改变原始数组；
7. slice
> 如果不传参，则返回原数组，如果传参，参数是startIndex(可选填)，endIndex(可选填)，返回一个新的数组，包含从 startIndex 到 endIndex （不包括该元素）的数组中的元素，不会改变原数组；
9. concat
> 连接2个或更多数组，返回合并后的新数组（不去重），不会改变原数组；
11. every
> 对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true,不会改变原数组;
13. some
> 对数组中的每一项运行给定函数，如果任一项返回true，则返回true,不会改变原数组；
15. forEach
> 对数组中的每一项运行给定函数。这个方法没有返回值,不会改变原数组;
17. filter
> 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组,不会改变原数组；
19. map
> 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组,不会改变原数组；
21. reduce
> reduce方法接收两个参数，一个是callback,一个initValue,initValue是累加器的初始值;callback接收四个参数：total、currentValue、currentIndex和array;total是累加器，值为initValue || 0，callback每次循环执行会将累加值赋值给第一个参数total，reduce方法停止执行后会返回这个累加器total。
23. join
> 将所有的数组元素连接成一个字符串;
24. indexOf
> 返回第一个与给定参数相等的数组元素的索引，没有找到则返回-1;
26. lastIndexOf
> 返回在数组中搜索到的与给定参数相等的元素的索引里最大的值;
28. reverse 
> 颠倒数组中元素的顺序，原先第一个元素现在变成最后一个，同样原先的最后一个元素变成了现在的第一个;
30. sort
> 按照字母顺序对数组排序，支持传入指定排序方法的函数作为参数;
32. toString
> 将数组作为字符串返回;
34. valueOf
> 和toString类似，将数组作为字符串返回;
#### 栈
> 栈是一种遵从后进先出（LIFO）原则的有序集合。新添加的或待删除的元素都保存在栈的
末尾，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。
1. 创建一个类来表示栈
```
function Stack() { 
   var items = []; //用数组来保存栈里的元素
   //添加一个（或几个）新元素到栈顶。
   this.push = function(element){ 
     items.push(element); 
   }; 
   //移除栈顶的元素，同时返回被移除的元素。
   this.pop = function(){ 
     return items.pop(); 
   }; 
   //返回栈顶的元素，不对栈做任何修改（这个方法不会移除栈顶的元素，仅仅返回它）。
   this.peek = function(){ 
     return items[items.length-1]; 
   }; 
   //如果栈里没有任何元素就返回true，否则返回false。
   this.isEmpty = function(){ 
     return items.length == 0; 
   }; 
   //返回栈里的元素个数。这个方法和数组的length属性很类似。
   this.size = function(){ 
     return items.length; 
   }; 
   //移除栈里的所有元素。
   this.clear = function(){ 
     items = []; 
   }; 
   //把栈里的元素都输出到控制台
   this.print = function(){ 
     console.log(items.toString()); 
   }; 
}
```
2. 十进制转成二进制
> 1.首先用2整除一个十进制整数，得到一个商和余数  
> 2.然后再用2去除得到的商，又会得到一个商和余数  
> 3.重复操作，一直到商为小于1时为止  
> 4.然后将得到的所有余数全部排列起来，再将它反过来（逆序排列），切记一定要反过来！  
```
function divideBy2(decNumber){ 
   var remStack = new Stack(), 
       rem, 
       binaryString = ''; 
   while (decNumber > 0){ 
       rem = Math.floor(decNumber % 2); 
       remStack.push(rem); 
       decNumber = Math.floor(decNumber / 2); 
   } 
   while (!remStack.isEmpty()){ 
       binaryString += remStack.pop().toString(); 
   } 
   return binaryString; 
}
```
3. 十进制和任何进制的转化
```
function baseConverter(decNumber, base){ 
   var remStack = new Stack(), 
       rem, 
       baseString = '', 
   digits = '0123456789ABCDEF'; 
   while (decNumber > 0){ 
       rem = Math.floor(decNumber % base); 
       remStack.push(rem); 
       decNumber = Math.floor(decNumber / base); 
   } 
   while (!remStack.isEmpty()){ 
       baseString += digits[remStack.pop()]; 
   } 
   return baseString; 
}
```
#### 队列
> 队列是遵循FIFO（First In First Out，先进先出，也称为先来先服务）原则的一组有序的项。
队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。
1. 创建队列类
```
function Queue() { 
 var items = []; 
 //向队列尾部添加一个（或多个）新的项。
 this.enqueue = function(element){ 
   items.push(element); 
 }; 
 //移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。
 this.dequeue = function(){ 
   return items.shift(); 
 }; 
 //返回队列中第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息——与Stack类的peek方法非常类似）。
 this.front = function(){ 
   return items[0]; 
 };
 //如果队列中不包含任何元素，返回true，否则返回false。
 this.isEmpty = function(){ 
   return items.length == 0; 
 }; 
 this.clear = function(){ 
   items = []; 
 }; 
 //返回队列包含的元素个数，与数组的length属性类似。
 this.size = function(){ 
   return items.length; 
 }; 
}
```
2. 优先队列 （元素的添加和移除是基于优先级的）
> 最小优先队列：优先级的值较小的元素被放置在队列最前面（1代表更高的优先级）。
```
function PriorityQueue(element,priority){
    let items=[];
    function QueueElement(element,priority){
        this.element = elememt;
        this.priority = priority;
    }
    this.enqueue = function(element,priority){
        let queueElement = new QueueElement(element,priority);
        if(this.isEmpty){
            items.push(queueElement);
        }else{
            let added = false;
            for(let i=0;i<items.length;i++){
                if(queueElement.priority<items[i]["priority"]){
                    items.splice(i,0,queueElement);
                    added = true;
                    break;
                }
            }
            if(!added){
                items.push(queueElement);
            }
        }
    }
    //移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。
     this.dequeue = function(){ 
       return items.shift(); 
     }; 
     //返回队列中第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息——与Stack类的peek方法非常类似）。
     this.front = function(){ 
       return items[0]; 
     };
     //如果队列中不包含任何元素，返回true，否则返回false。
     this.isEmpty = function(){ 
       return items.length == 0; 
     }; 
     this.clear = function(){ 
       items = []; 
     }; 
     //返回队列包含的元素个数，与数组的length属性类似。
     this.size = function(){ 
       return items.length; 
     }; 
}
```
> 最大优先队列：把优先级的值较大的元素放置在队列最前面。
3. 循环队列---击鼓传花游戏的实现(Hot Potato)
> 什么是击鼓传花游戏：这个游戏中，孩子们围成一个圆圈，把花尽快地传递给旁边的人。某一时刻传花停止，
这个时候花在谁手里，谁就退出圆圈结束游戏。重复这个过程，直到只剩一个孩子（胜者）。
```
function HotPotato(nameList,num){
  let queue = new Queue(),eliminated="";//eliminated记录每轮游戏出局的那个人
  for(let i=0;i<nameList.length;i++){
      queue.enqueue(nameList[i])
  }
  while(queue.size>1){
      for(let i=0;i<num;i++){
          queue.enqueue(queue.dequeue());
      }
      eliminated = queue.dequeue();
      console.log(eliminated+"出局");
  }
  return queue.dequeue();
}
let names = ['John','Jack','Camila','Ingrid','Carl']; 
let winner = hotPotato(names, 7); 
console.log('胜利者：' + winner);

//
//以上算法的输出如下：
//Camila在击鼓传花游戏中被淘汰。
//Jack在击鼓传花游戏中被淘汰。
//Carl在击鼓传花游戏中被淘汰。
//Ingrid在击鼓传花游戏中被淘汰。
//胜利者：John
```
#### 链表
> 链表存储有序的元素集合，但不同于数组，链表中的元素在内存中并不是连续放置的。每个元素由一个存储本身的节点和一个指向下一个元素的引用（指针）组成。  
> 相对于传统的数组，链表的一个好处在于，添加或移除元素的时候不需要移动其他元素。然
而，链表需要使用指针，因此实现链表时需要额外注意。数组的另一个细节是可以直接访问任何
位置的任何元素，而要想访问链表中间的一个元素，需要从起点（表头）开始迭代列表直到找到
所需的元素。
1. 单项链表  
