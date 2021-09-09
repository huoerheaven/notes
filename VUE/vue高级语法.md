#### mixin混入
1. 组件data,methods 优先级高于mixin的data,methods 优先级
2. 生命周期函数，先执行mixin里面的，再执行组件里面的
3. 自定义的属性，组件中的属性优先级高于mixin属性的优先级（this.$options.XX获取组件的自定义属性）
