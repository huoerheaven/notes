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

##### 6. 编译模板 2
- 模板编译为 render 函数，执行 render 函数返回 vnode
- 基于 vnode 再执行 patch 和 diff
- 使用webpack vue-loader，会在开发环境下编译模板（重要）
- 模板到 render 函数，再到 vnode, 再到渲染和更新
- vue 组件中使用 render 代替 template

##### 7. 组件 渲染/更新 过程
==== 一个组件渲染到页面，修改 data 触发更新（数据驱动视图）
- 初次渲染过程
- - 1. 解析模板为 render 函数（或在开发环境已完成，vue-loader）
- - 2. 触发响应式，监听 data 属性 getter setter (执行render过程中 触发data的getter)
- - 3. 执行 render 函数，生成 vnode,patch(elem,vnode)
- 更新过程
- - 1. 修改 data,触发 setter (此前在getter中已被监听  未在getter中监听的数据修改，不会触发setter)
- - 2. 重新执行 render 函数，生曾 newVnode
- - 3. patch(vnode,newVode)
- 异步渲染
- - 1. $nextTick 
- - - 异步渲染，$nextTick 待 DOM 渲染完再回调
- - - 页面渲染时会将 data 的修改做整合，多次 data 修改只会渲染一次
- - 2. 汇总 data 的修改，一次性更新视图
- - 3. 减少 DOM 操作次数，提高性能

##### 8. 回顾
- 响应式：监听 data 属性 getter setter (包括数组)
- 模板编译：模板到 render 函数，再到vnode
- vdom: patch(elem,vnode) 和 patch(vnode,newVnode)

##### 9. 总结1
- 渲染和响应式的关系
- 渲染和模板编译的关系
- 渲染和 vdom 的关系
