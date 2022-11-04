##### 千分位
```js
function format_with_mod(number){
    let n=number;
    let r="";
    let temp;
    do{
        //求模的值 用于获取高三位 这里可能有小数
        let mod=n%1000;
        //值是不是大于1 是继续的条件
        n=n/1000;
        //高三位
        temp=~~mod;
        //1. 填充：n>=1循环未结束，就要填充为比如，1=>001
        //2. 拼接“，”
        r=(n>=1?`${temp}`.padStart(3,"0"):temp)+(r?","+r:r);
    }while(n>=1)
    let strNumber = number+"";
    let index = strNumber.indexOf(".");
    //拼接小数部分
    if(index>-1){
        r+=strNumber.substring(index);
    }
    return r;
}

// console.log(format_with_mod(2345678.234));//2,345,678.234

function format_with_array(number){
    //数字转字符串，并按照.分割
    let arr = (number + "").split(".");
    //整数部分转为数组
    let int = arr[0].split("");
    //小数部分
    let fraction = arr[1]||"";
    let r="";
    //倒叙并遍历
    let reverseInt = int.reverse();
    reverseInt.forEach((n,i)=>{
        //非第一位并且是位值是3的倍数，添加“，”
        if(i!==0&&i%3===0){
            r=n+","+r;
        }else{
            //正常添加字符
            r=n+r;
        }
    });
    //整数部分和小数部分拼接
    return r+(!!fraction?"."+fraction:"");
}
// console.log(format_with_array(2345678.234));//2,345,678.234
function format_with_substring(number){
    //数字转字符串，并按照.分割
    let arr = (number + "").split(".");
    let int = arr[0]+"";
    let fraction = arr[1]||"";

    //多余的位数
    let f = int.length%3;
    //获取多余的位数 f可能是0 即r可能是空字符串
    let r=int.substring(0,f);
    //每三位添加“，”和对应的字符
    for(let i=0;i<Math.floor(int.length/3);i++){
        r+=","+int.substring(f+i*3,f+(i+1)*3);
    }
    //如果没有多余的位数
    if(f===0){
        r=r.substring(1)
    }
    //整数部分和小数部分拼接
    return r+(!!fraction?"."+fraction:"");
}
console.log(format_with_substring(2345678.234));//2,345,678.234

```
```js
/**
 * 千分位格式化（使用数组）
 * @param {*} n number
 */
function format1(n){
    n=Math.floor(n);//只考虑整数
    const s=n.toString();
    const arr= s.split("").reverse();
    return arr.reduce((str,val,index)=>{
        if(index%3===0){
            if(str){
                return val+","+str;
            }else{
                return val+str;
            }
        }else{
            return val+str;
        }
    },"");
}

/**
 * 千分位格式化（字符串）
 * @param {*} n 
 */
function format2(n){
    n=Math.floor(n);//只考虑整数
    const s=n.toString();
    const length = s.length;
    let res="";
    for(let i=length-1;i>=0;i--){
        let j=length-i;
        if(j%3===0){
            if(i===0){
                res=s[i]+res;
            }else{
                res=","+s[i]+res;
            }
        }else{
            res=s[i]+res;
        }
    }
    return res;
}

console.log(format1(1234567800));//1,234,567,800
console.log(format2(1234567800));//1,234,567,800
```
