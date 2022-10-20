```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>line-height继承问题</title>
</head>
<style>
    .container{
        font-size: 20px;

        /* 如果line-height为40px，则p元素会继承line-height等于40 */
        /* line-height: 40px; */

        /* 如果line-height为比例，如1.5，则p元素会继承line-height=1.5，然后再乘以16 */
        /* line-height: 2; */

        /* ！！！重要！！！ */
        /* 如果line-height为百分比，则先计算父元素的line-height=20*200%=40 ,再继承40*/
        line-height: 200%;
    }
    .item{
        background-color: aquamarine;
        font-size: 16px;
    }
</style>
<body>
    <div class="container">
        <p class="item">这是一行文字</p>
    </div>
</body>
</html>
```
