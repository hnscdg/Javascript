# Array 的相关属性和方法

## Array 对象属性

- constructor 返回对创建此对象的数组函数的引用。

    ``` javascript
    var test=new Array();

    if (test.constructor==Array)
    {
        document.write("This is an Array");
    }

    if (test.constructor==Boolean)
    {
        document.write("This is a Boolean");
    }

    if (test.constructor==Date)
    {
        document.write("This is a Date");
    }
    
    if (test.constructor==String)
    {
        document.write("This is a String");
    }
    ```
- length 设置或返回数组中元素的数目。

- prototype 使您有能力向对象添加属性和方法。

## 对象方法

- concat() 连接两个或更多的数组，并返回结果

    ``` javascript
    var arr = [1,2,3,4];
    var arr2 = [5,6,7,8];
    var arr3 = arr.concat(arr2);
    console.log(arr3); // 连接之后返回的数组为：[1, 2, 3, 4, 5, 6, 7, 8]
    ```

- join() 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。

    ``` javascript
    var arr = ['xiao','lin','qiqi','mingtian'];
    var arr2 = arr.join(',');
    console.log(arr2); // 根据','隔开返回的字符串为："xiao,lin,qiqi,mingtian"
    ```

- pop() 删除并返回数组的最后一个元素。

    ``` javascript
    var arr = [2,3,4,5];
    var arr2 = arr.pop();
    console.log(arr2); // 删除的数组的最后一个元素为：5
    console.log(arr);  // 删除元素之后的数组为：[2, 3, 4]
    ```
- shift() 删除并返回数组的第一个元素

    ``` javascript
    var arr = [2,3,4,5];
    var arr2 = arr.shift();
    console.log(arr2); // 删除的数组的第一个元素为：2
    console.log(arr);  // 删除元素之后的数组为：[3, 4，5]
    ```

- push() 向数组的末尾添加一个或更多元素，并返回新的长度。

    ``` javascript
    var arr = [2,3,4,5];
    var arr2 = arr.push(6);
    console.log(arr2);  // 返回的数组长度：5 
    console.log(arr);  // [2, 3, 4, 5, 6]
    ```
- unshift() 向数组的开头添加一个或更多元素，并返回新的长度。

    ``` javascript 
    var arr = ['xiao','ming','qiqi','aiming'];
    var arr1 = arr.unshift('lang');
    console.log(arr1);  // 返回的数组的长度：  5
    console.log(arr);  //向数组开头添加元素返回的结果：["lang", "xiao", "ming", "qiqi", "aiming"]
    ```

- reverse() 颠倒数组中元素的顺序。

    ``` javascript
    var arr = [2,3,4,5];
    arr.reverse();
    console.log(arr);   //  [5, 4, 3, 2]
    ```

- slice() 从某个已有的数组返回选定的元素

    ``` javascript
    var arr = [2,3,4,5];
    var arr2 = arr.slice(1,3);
    console.log(arr2);  // 截取区间返回的数组为：[3, 4]
    console.log(arr);  // [2, 3, 4, 5]
    ```

- sort() 对数组的元素进行排序

    ``` javascript

    借助排序函数，实现数值由小到大排序
    function sortNumber(a,b){
        return a - b
    }
    var arr = [23,30,42,5];
    var arr2 = arr.sort(sortNumber);
    console.log(arr2);  // [5, 23, 30, 42]
    console.log(arr);   // [5, 23, 30, 42]

    借助排序函数，实现数值由大到小排序
    function sortNumber(a,b){
        return b - a
    }
    var arr = [23,30,42,5];
    var arr2 = arr.sort(sortNumber);
    console.log(arr2);  // [42, 30, 23, 5]
    console.log(arr);  // [42, 30, 23, 5]
    ```

- splice() 删除元素，并向数组添加新元素。

    ``` javascript
    语法:arrayObject.splice(index,howmany,item1,.....,itemX)
    index:必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
    howmany:必需。要删除的项目数量。如果设置为 0，则不会删除项目。
    item1, ..., itemX:可选。向数组添加的新项目。

    // 创建一个新数组，并向其添加一个元素
    var arr = [1,2,3,4];
    arr.splice(2,0,5);
    console.log(arr);  // [1, 2, 5, 3, 4]

    // 删除位于 index 2 的元素，并添加一个新元素来替代被删除的元素：
    var arr = [1,2,3,4];
    arr.splice(2,1,5);
    console.log(arr);  // [1, 2, 5, 4]
    ```

- toSource() 返回该对象的源代码。

    ``` javascript 
    浏览器支持
    只有 Gecko 核心的浏览器（比如 Firefox）支持该方法，也就是说 IE、Safari、Chrome、Opera 等浏览器均不支持该方法。
    <script type="text/javascript">
    function employee(name,job,born){
        this.name=name;
        this.job=job;
        this.born=born;
    }
    var bill = new employee("Bill Gates","Engineer",1985);
    document.write(bill.toSource());
    </script>
    输出：({name:"Bill Gates", job:"Engineer", born:1985}) 
    ```

- toString() 把数组转换为字符串，并返回结果。

    ``` javascript
    var arr = ['xiao','ming','qiqi','aiming'];
    arr.toString();
    console.log(arr);  // ["xiao", "ming", "qiqi", "aiming"]
    ```

- toLocaleString() 把数组转换为本地数组，并返回结果。

    ``` javascript
    var arr = ['xiao','ming','qiqi','aiming'];
    arr.toLocaleString();
    console.log(arr);  // ["xiao", "ming", "qiqi", "aiming"]
    ```

- valueOf() 返回数组对象的原始值

    ``` javascript
    var arr = ['xiao','ming','qiqi','aiming'];
    arr.valueOf('lang');
    console.log(arr); // ["xiao", "ming", "qiqi", "aiming"]
    ```

## 编写一个方法 去掉一个数组的重复元素

### 方法一

``` javascript
var arr = [0,2,3,4,4,0,2];
var obj = {};
var tmp = [];
for(var i = 0 ;i< arr.length;i++){
if( !obj[arr[i]] ){
    obj[arr[i]] = 1;
    tmp.push(arr[i]);
}
}
console.log(tmp); //  [0, 2, 3, 4]
```

### 方法二

``` javascript 
var arr = [2,3,4,4,5,2,3,6],
arr2 = [];
for(var i = 0;i< arr.length;i++){
    if(arr2.indexOf(arr[i]) < 0){
        arr2.push(arr[i]);
    }
}
console.log(arr2); // [2, 3, 4, 5, 6]
```

### 方法三

``` javascript 
var arr = [2,3,4,4,5,2,3,6];
var arr2 = arr.filter(function(element,index,self){
return self.indexOf(element) === index;
});
console.log(arr2); //[2, 3, 4, 5, 6]
```
## 求数组的最值

### 方法一

- 求数组最大值：Math.max.apply(null,arr);

    ``` javascript
    var arr = [3,43,23,45,65,90];
    var max = Math.max.apply(null,arr);
    console.log(max);
    // 90
    ```
- 求数组最小值：Math.min.apply(null,arr)

    ``` javascript
    var arr = [3,43,23,45,65,90];
    var min = Math.min.apply(null,arr);
    console.log(min);
    // 3
    ```

### 方法二 Array.max = function(arr){} / Array.min = function(arr){}

``` javascript 
var array = [3,43,23,45,65,90];

Array.max = function( array ){ 
return Math.max.apply( Math, array );
};
Array.min = function( array ){ 
return Math.min.apply( Math, array );
};

var max = Array.max(array);
console.log(max);  // 90

var min = Array.min(array);
console.log(min);  // 3
```

### 方法三 Array.prototype.max = function(){};Array.prototype.min = function(){}

- 求数组最大值(基本思路：将数组中的第一个值赋值给变量max ,将数组进行循环与max进行比较，将数组中的大值赋给max,最后返回max;)

    ``` javascript 
    var arr = [3,43,23,45,65,90];
    Array.prototype.max = function() { 
        var max = this[0];
        var len = this.length; 
        for (var i = 0; i < len; i++){ 
            if (this[i] > max) { 
            max = this[i]; 
            } 
        } 
        return max;
    }
    var max = arr.max();
    console.log(max);  // 90
    ```
- 求数组最小值

    ``` javascript
    var arr = [3,43,23,45,65,90];
    Array.prototype.min = function() { 
        var min = this[0];
        var len = this.length;
        for(var i = 0;i< len;i++){
            if(this[i] < min){
                min = this[i];
            }
        }
        return min;
    }
    var min = arr.min();
    console.log(min);  // 3
    ```

## 数组排序相关

### 结合sort和函数排序

- 数组由小到大进行排序：sort,sortnum

    ``` javascript
    var arr = [3,43,23,45,65,90];
    function sortnum(a,b){
    　　return a-b;
    }
    arr = arr.sort(sortnum);
    console.log(arr);
    // [3, 23, 43, 45, 65, 90]
    ```
- 数组由大到小进行排序：sort,sortnum

    ``` javascript
    var arr = [3,43,23,45,65,90];
    function sortnum(a,b){
    　　return a+b;
    }
    arr = arr.sort(sortnum);
    console.log(arr);
    // [90, 65, 45, 23, 43, 3]
    ```

### 冒泡排序：即实现数组由小到大进行排序；思路为：每次比较相邻的两个数，如果后一个比前一个小，换位置。如果要实现由大到小排序，使用reverse()即可

``` javascript
var arr = [3, 1, 4, 6, 5, 7, 2];

function bubbleSort(arr) {
    var len = arr.length;
    for (var i = len; i >= 2; --i) {
        for (var j = 0; j < i - 1; j++) {
            if (arr[j + 1] < arr[j]) {
                var temp;
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

var arr2 = bubbleSort(arr); 
console.log(arr2);  // [1, 2, 3, 4, 5, 6, 7]
var arr3 = arr2.reverse();
console.log(arr3);  //  [7, 6, 5, 4, 3, 2, 1]
```

### 快速排序：思路：采用二分法，取出中间数，数组每次和中间数比较，小的放到左边，大的放到右边。

``` javascript
var arr = [3, 1, 4, 6, 5, 7, 2];
function quickSort(arr) {
    if(arr.length == 0) {
        return [];  // 返回空数组
    }
    var cIndex = Math.floor(arr.length / 2);
    var c = arr.splice(cIndex, 1);
    var l = [];
    var r = [];

    for (var i = 0; i < arr.length; i++) {
        if(arr[i] < c) {
            l.push(arr[i]);
        } else {
            r.push(arr[i]);
        }
    }
    return quickSort(l).concat(c, quickSort(r));
}
console.log(quickSort(arr));
//[1, 2, 3, 4, 5, 6, 7]
```

## 数组的翻转（非reverse()）

### 方法一

``` javascript
var arr = [1,2,3,4];
var arr2 = [];
while(arr.length) {
    var num = arr.pop(); //删除数组最后一个元素并返回被删除的元素
    arr2.push(num);
}
console.log(arr2);
// [4, 3, 2, 1]
```

### 方法二

``` javascript
var arr = [1,2,3,4];
var arr2 = [];
while(arr.length){
    var num = arr.shift(); //删除数组第一个元素并返回被删除的元素
    arr2.unshift(num);
}
console.log(arr2);
```



