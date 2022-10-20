```js
<script>
    const dom = new Proxy({}, {
    get(target, property) {
        return function(attrs = {}, ...children) {
        const el = document.createElement(property);
        for (let prop of Object.keys(attrs)) {
            el.setAttribute(prop, attrs[prop]);
        }
        for (let child of children) {
            if (typeof child === 'string') {
            child = document.createTextNode(child);
            }
            el.appendChild(child);
        }
        return el;
        }
    }
    });

    // const el = dom.div({},
    // 'Hello, my name is ',
    // dom.a({href: '//example.com'}, 'Mark'),
    // '. I like:',
    // dom.ul({},
    //     dom.li({}, 'The web'),
    //     dom.li({}, 'Food'),
    //     dom.li({}, '…actually that\'s it')
    // )
    // );

    // document.body.appendChild(el);


    var twice = {
        apply (target, ctx, args) {
            console.log(arguments)
            return Reflect.apply(...arguments) * 2;
        }
    };
    function sum (left, right) {
        return left + right;
    };
    var proxy = new Proxy(sum, twice);
    proxy(1, 2) // 6
    proxy.call(null, 5, 6) // 22
    proxy.apply(null, [7, 8]) // 30


</script>
``
