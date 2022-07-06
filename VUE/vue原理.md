##### 1. 通过 snabbdom 学习 vdom
- [snabbdom](https://github.com/snabbdom/snabbdom)
- 两个js对象做diff   [jiff](https://github.com/cujojs/jiff)

##### 2. diff 算法优化时间复杂度到 O(n)
- 只比较同一层级，不跨级比较
- tag 不相同，则直接删掉重建，不再深度比较
- tag 和 key,两者都相同，则认为是相同节点，不再深度比较

##### 3. snabbdom 中的 diff 算法总结
- patchVnode (新旧vnode children text的比较 )
- addVnodes removeVnodes
- updateChildren (patchVnode 方法中，当新旧vnode 的 children 都存在时，更新children)
- - oldStart vs newStart
- - oldEnd vs newEnd
- - oldStart vs newEnd
- - oldEnd vs newStart
- - else newVnode 逐个和 oldVnode去比较，有相同的就进行替换，没相同的就直接插入 （**体现 key 的重要性**）

##### 4. vodm 和 diff
- 细节不重要，updateChildren 的过程也不重要，不要深究
- vdom 核心概念很重要：h vnode patch diff key 等
- vdom 存在的价值更加重要：数据驱动视图，控制 DOM 操作
