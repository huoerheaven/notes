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
  while(queue.size()>1){
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
1. 单向链表  
```
function LinkedList(){
  let Node = function(element){
      this.element = element;
      this.next = null;
  }
  let head=null;
  let length=0;
  //向链表尾部添加一个元素
  this.append=function(element){
      let node = new Node(element),
      current;
      if(head===null){//列表为空
        head = node;
      }else{
        current = head;
        //循环列表，直到找到最后一项
        while(current.next){
          current = current.next;
        }
        //找到最后一项，将其next赋为node,建立链接
        current.next = node;
      }
      //更新列表的长度
      length++;
  }
  //***注***：列表最后一个节点的下一个元素始终是null。
  
  //从链表中移除元素
  //从特定位置移除一个元素
  this.removeAt = function(position){
    let current=head,index=0,previous;
    //检查越界值
    if(position>-1&&position<length){
      //移除第一项
      if(position===0){
        head = current.next;
      }else{
        while(index++<position){
          previous = current;
          current = current.next;
        } 
        //将previous与current的下一项链接起来，跳过current,从而移除它
        previous.next = current.next;
      }
      length--;
      return current.element;
    }else{
      return null;
    }
  }
  //在任意一个位置插入一个元素
  this.insert=function(position,element){
      //检查越界值
      if(position>=0&&position<=length){
         let node = new Node(element),current=head,previous=null,index=0;
         //在第一个位置添加
         if(position===0) {
              node.next=current;
              head = node;
         }else{
              while(index++<position){
                  previous=current;
                  current=current.next;
              }
              previous.next=node;
              node.next=current;
         }
         //更新列表长度
         length++;
         return true;
      }else{
          return false;
      }
  }
  //***注***：使用变量引用我们需要控制的节点非常重要，这样就不会丢失节点之间的链接。我们可以只使用一个变量（previous），但那样会很难控制节点之间的链接。由于这个原因，最好是声明一个额外的变量来帮助我们处理这些引用。

  //把LinkedList对象转换成一个字符串
  this.toString=function(){
      let current=head,str="";
      while(current){
          str+=current.element;
          current=current.next;
      }
      return str;
  }
  //indexOf方法接收一个元素的值，如果在列表中找到它，就返回元素的位置，否则返回-1。
  this.indexOf=function(element){
      let current=head,index=0;
      while(current){
          if(current.element===element){
              return index;
          }
          index++;
          current=current.next;
      }
      return -1;
  }
  //通过indexOf和removeAt方法来实现
  this.remove=function(element){
      let index = this.indexOf(element);
      return this.removeAt(index);
  }
  /**
   * 我们已经有一个移除给定位置的一个元素的removeAt方法了。现在有了indexOf方法，如果传入元素的值，就能找到它的位置，然后调用removeAt方法并传入找到的位置。这样非常简单，如果需要更改removeAt方法的代码，这样也更容易——两个方法都会被更改（这就是重用代码的妙处）。这样，我们就不需要维护两个从列表中移除一项的方法，只需要一个！同时，removeAt方法将会检查边界约束。
   */
  this.isEmpty=function(){
      return length===0;
  }
  this.size=function(){
      return length;
  }
  this.getHead=function(){
      return head;
  }
}
```
2. 双向链表  
> 双向链表和普通链表的区别在于，在链表中，一个节点只有链向下一个节点的链接，而在双向链表中，链接是双向的：一个链向下一个元素，另一个链向前一个元素。
> 双向链表提供了两种迭代列表的方法：从头到尾，或者反过来。我们也可以访问一个特定节点的下一个或前一个元素。在单向链表中，如果迭代列表时错过了要找的元素，就需要回到列表起点，重新开始迭代。这是双向链表的一个优点。
```
//双向链表
function DoublyLinkedList(){
    let Node=function(element){
        this.element = element;
        this.prev=null;
        this.next=null;//新增的
    }
    let length=0,head=null,tail=null;//tail新增的
    /**
     * 在代码中可以看到，LinkedList类和DoublyLinkedList类之间的区别标为新增的。在Node类里有prev属性（一个新指针），在DoublyLinkedList类里也有用来保存列表最后一项的引用的tail属性。
     */
    this.insert=function(position,element){
        let node = new Node(element),current=head,previous=null,index=0;
        if(position>=0&&position<=length){
            if(position===0){
                if(!head){//新增
                   head=node;
                   tail=node; 
                }else{
                    node.next=current;
                    current.prev=node;
                    head=node;
                }
            }else if(position===length){//最后一项 新增
                current=tail;
                current.next=node;
                node.prev=current;
                tail=node;
            }else{
                while(index++<position){
                    previous=current;
                    current=current.next;
                }
                previous.next = node;
                node.prev=previous;//
                node.next = current;
                current.prev = node;
            }
        }else{
            return false;
        }
    }
    /**
     * 我们可以对insert和remove这两个方法的实现做一些改进。在结果为否的情况下，我们可以把元素插入到列表的尾部。性能也可以有所改进，比如，如果position大于length/2，就最好从尾部开始迭代，而不是从头开始（这样就能迭代更少列表中的元素）。
     */
    this.removeAt=function(position){
        if(position>-1&&position<length){
            let current=head,previous=null,index=0;
            if(position===0){
                head=current.next;
                //如果只有一项 更新tail 新增
                if(length===1){
                    tail=null;
                }else{
                    head.prev=null;
                }
            }else if(position===length-1){//最后一项 新增
                current=tail;
                tail=current.prev;
                tail.next=null;
            }else{
                while(index++<position){
                    previous=current;
                    current=current.next;
                }
                previous.next=current.next;
                current.next.prev = previous;
            }
            length--;
            return current.element;
        }else{
            return null;
        }
    }
}
```
3. 循环链表
> 循环链表可以像链表一样只有单向引用，也可以像双向链表一样有双向引用。循环链表和链表之间唯一的区别在于，最后一个元素指向下一个元素的指针（tail.next）不是引用null,而是指向第一个元素（head）。双向循环链表有指向head元素的tail.next，和指向tail元素的head.prev。
4. 小结
> 链表相比数组最重要的优点，那就是无需移动链表中的元素，就能轻松地添加和移除元素.因此，当你需要添加和移除很多元素时，最好的选择就是链表，而非数组。

#### 集合
> 集合是由一组无序且唯一（即不能重复）的项组成的。
> 我们要实现的集合类就是以ECMAScript 6中Set类的实现为基础的。
>> add(value)：向集合添加一个新的项。  
>> remove(value)：从集合移除一个值。  
>> has(value)：如果值在集合中，返回true，否则返回false。   
>> clear()：移除集合中的所有项。  
>> size()：返回集合所包含元素的数量。与数组的length属性类似。  
>> values()：返回一个包含集合中所有值的数组。  
```
function Set(){
    //我们使用对象而不是数组来表示集合（items）
    let items={};
    //如果值在集合中，返回true，否则返回false。
    //方法一
    // this.has=function(value){
    //     //也可以用 in操作符来验证给定的值是否是items对象的属性(包括继承属性，如toString)
    //     return value in items;
    // }
    //方法二（好！）
    this.has=function(value){
        //对象的hasOwnProperty方法返回一个表明对象是否具有特定属性的布尔值
        return items.hasOwnProperty(value);
        
    }
    this.add=function(value){
        if(!this.has(value)){
            items[value]=value;
            return true;
        }
        return false;
    }
    /**注：添加一个值的时候，把它同时作为键和值保存，因为这样有利于查找这个值。 */
    //从集合移除一个值。
    this.remove=function(value){
        if(this.has(value)){
            delete items[value];
            return true;
        }
        return false;
    }
    //移除集合中的所有项。
    this.clear=function(){
        items={};
    }
    this.size=function(){
        //方法一
        //使用一个length变量，每当使用add或remove方法时控制它，就像在LinkedList类一样。
        //方法二（只能在现代浏览器中运行，（比如IE9以上版本、Firefox 4以上版本、Chrome 5以上版本、Opera 12以上版本、Safari 5以上版本，等等）
        // return Object.keys(items).length;
        //方法三
        let count=0;
        for(let prop in items){
            if(items.hasOwnProperty(prop)) ++count;
        }
        return count;
        /**
         * 不能简单地使用for-in语句遍历items对象的属性，递增count变量的值。还需要使用has方法（以验证items对象具有该属性），因为对象的原型包含了额外的属性（属性既有继承自JavaScript的Object类的，也有属于对象自身，未用于数据结构的）。
         * Object的hasOwnProperty方法，如果是对象自身属性，返回true;如果是继承属性，返回false
         */
    }
    //返回一个包含集合中所有值的数组。
    this.values=function(){
        //方法一
        // return Object.keys(items);
        //方法二
        let values=[];
        for(let prop in items){
            if(items.hasOwnProperty(prop)) {
                values.push(prop);
            }
        }
        return values;
    }
    //集合操作
    /**
     *  并集：对于给定的两个集合，返回一个包含两个集合中所有元素的新集合。
        交集：对于给定的两个集合，返回一个包含两个集合中共有元素的新集合。
        差集：对于给定的两个集合，返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合。
        子集：验证一个给定集合是否是另一集合的子集。
     */
    //并集
    this.union=function(otherSet){
        let unionSet = new Set();
        let values = this.values();
        for(let i=0;i<values.length;i++){
            unionSet.add(values[i])
        }
        values=otherSet.values();
        for(let j=0;j<values.length;j++){
            unionSet.add(values[j])
        }
        return unionSet;
    }
    //交集
    this.interaction=function(otherSet){
        let interaction=new Set();
        let values=this.values();
        for(let i=0;i<values.length;i++){
            if(otherSet.has(values[i])){
                interaction.add(values[i])
            }
        }
        return interaction;
    }
    //差集
    this.difference=function(otherSet){
        let difference = new Set();
        let values=this.values();
        for(let i=0;i<values.length;i++){
            if(!otherSet.has(values[i])){
                difference.add(values[i])
            }
        }
        return difference;
    }
    //子集
    this.subset=function(otherSet){
        let subset = new Set();
        if(this.size>otherSet.size) return false;
        else{
            let values=this.values();
            for(let i=0;i<values.length;i++){
                if(!otherSet.has(values[i])){
                    return false;
                }
            }
            return true;
        }
        
    }

}
//测试
let set = new Set();
set.add(1);
console.log(set.values());//输出["1"]
console.log(set.has(1));//输出true
console.log(set.has("toString"));//输出false
console.log(set.size()); //输出1

set.add(2); 
console.log(set.values()); //输出["1", "2"] 
console.log(set.has(2)); //true 
console.log(set.size()); //2

set.remove(1); 
console.log(set.values()); //输出["2"]

set.remove(2); 
console.log(set.values()); //输出[]

let setA = new Set();
setA.add("a");
setA.add("b");
setA.add("c");
let setB = new Set();
setB.add("c");
setB.add("e");
setB.add("f");
let unionAB=setA.union(setB);
console.log(unionAB.values());//['a', 'b', 'c', 'e', 'f']
let interactionAB=setA.interaction(setB);
console.log(interactionAB.values());//['c']
let differenceAB=setA.difference(setB);
console.log(differenceAB.values());//['a', 'b']
let subset=setA.subset(setB);
console.log(subset);//false
```
