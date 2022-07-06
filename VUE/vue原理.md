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

##### 5. vue 编译模板
- 模板不是html,有指令、插值、JS表达式，能实现判断、循环
- html 是标签语言，只有JS 才能实现判断、循环（图灵完备的）
- 因此，模板一定是转换为某种 JS 代码，即编译模板

##### 66. 编译模板 2
- 模板编译为 render 函数，执行 render 函数返回 vnode
- 基于 vnode 再执行 patch 和 diff
- 使用webpack vue-loader，会在开发环境下编译模板（重要）
