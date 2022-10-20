```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>flex布局实现三点色子</title>
</head>
<style>
    .container{
        width: 200px;
        height: 200px;
        border: 1px solid #ccc;
        border-radius: 4px;
        padding: 20px;
        display: flex;
        justify-content: space-between;
    }
    .item{
        width: 20px;
        height: 20px;
        background-color: aquamarine;
        border-radius: 50%;
    }
    .item:nth-of-type(2){
        align-self: center;
    }
    .item:nth-of-type(3){
        align-self: flex-end;
    }
</style>
<body>
    <div class="container">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
</body>
</html>
```
