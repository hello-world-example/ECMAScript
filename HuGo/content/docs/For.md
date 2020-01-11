# 循环语句

## for...in

>  遍历数组**下标**，obj 的 **key**

```js
// 对象遍历
let obj = {a: 111, b: 222, c: 333};
for(let key in obj) {
　　console.log(key, obj[key]);
}
> "a" 111
> "b" 222
> "c" 333

// 数组遍历
let array = ['a', 'b', 'c'];
for(let index in array) {
　　console.log(index, array[index]);
}
> "0" "a"
> "1" "b"
> "2" "c"
```

## for...of

> 遍历数组元素，**无法直接遍历 obj**，需要搭配 ` Object.keys(obj)`

```js
// 遍历数组
let array = ['a', 'b', 'c'];
for (let item of array) {
  console.log(item);
}

// 遍历 Obj
let obj = {'a':111,'b':222,'c':322};
for (let key of Object.keys(obj)) {
  console.log(key, obj[key]);
}
```

## map() / filter() / forEach()

> - map()
> - filter()
> - forEach()
>
> 不能遍历对象

```js
// 数组遍历
let array = ['a', 'b', 'c'];
array.map(function(item,i,arr){
  console.log(item,i,arr);
})

// 配合使用
var obj = [1, 2, 3, 4, 5, 6]
obj.filter(function(item, index, arr) {
　　return item % 2 === 0;
}).map(function(item, index, arr) {
　  return item * 2;
}).forEach(function(item, index, arr) {
　　console.info("print",item, index, arr)
})
> "print" 4  0 [4,8,12]
> "print" 8  1 [4,8,12]
> "print" 12 2 [4,8,12]
```

## for / while / do...while

```js
// for
for (let i = 0; i < 9; i++) {
   console.log(i);
}

// while
let n = 0;
while (n < 3) {
  console.log(n++);
}

// do..while
let n = 0;
do{
  console.log(n++);
}while (n < 3);

```

