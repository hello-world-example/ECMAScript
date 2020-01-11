# Promise

> - [JavaScript 参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference) > [JavaScript 标准内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects) > [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
> - ECMAScript 6 入门 [Promise 对象](http://es6.ruanyifeng.com/#docs/promise)



Promise 简单说就是一个容器，里面保存着某个未来才会结束的事件，是**异步编程的一种解决方案**，比传统的 "回调函数和事件" 解决方案更合理和更强大。优点在于**将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数**。



- 有三种状态：`pending`（进行中）、`fulfilled`（已成功 `resolve`）、`rejected`（已失败）
- 状态变化只能从 `pending` 变为 `fulfilled `( `resolve`) 、从 `pending `变为 `rejected`
- 一旦状态改变，就不会再变，再对 `Promise` 对象添加回调函数，也会立即得到这个结果



## then / catch / finally

```js
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        // 把正确的结果通知出去
        resolve(this.response);
      } else {
        // 把错误的结果通知出去
        reject(new Error(this.statusText));
      }
    };
    // AJAX 请求
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });
	// 返回 Promise 实例
  return promise;
};

// bad
getJSON("/posts.json").then(
  // 正确执行的方法
  function(json) { console.log('Contents: ', json); }, 
  // 错误执行的方法  
  function(error) { console.error('出错了', error); }
);

// good
getJSON("/posts.json")
  // 正确执行的方法
  .then((json) => console.log('Contents:', json))
  // 错误执行的方法  
  .catch((error) => console.log('出错了', error))
  //  ES2018 引入
  .finally(() => console.log('无论如何都执行') );
;
```



## Promise.all() / Promise.race()

```js
const p = Promise.all([p1, p2, p3]);
```

- all()
  - 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
  - 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数
- race()
  - 只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变
  - 率先改变的 Promise 实例的返回值，就传递给`p`的回调函数

## Promise.resolve() 

将现有对象转为 Promise 对象，不带参数时，直接返回一个`resolved` 状态的 Promise 对象

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```

- `setTimeout(fn, 0)`在下一轮“事件循环”开始时执行
- `Promise.resolve()`在本轮“事件循环”结束时执行
- `console.log('one')`则是立即执行，因此最先输出

