### 第 8 题：setTimeout、Promise、Async/Await 的区别

解析：[第 8 题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/33)

解答：

setTimeout回调中的函数任务为宏任务，会在到期时间之后，推入宏任务执行栈中，等待执行；

（x）Promise是可以生成异步执行函数的构造器，Promise创建过程中的代码会立即执行，但resolve回调会推入到微任务队列中等待执行；

Async/Await的区别：Async是用来修饰函数，将函数变成异步函数，只有用async修饰的函数里面才可以用await控制其他异步过程调用

```javascript

```

#### 总结：

这题怎么没人答，我说下我粗浅的认识，抛砖引玉，欢迎指正和补充。 我觉得这题主要是考察这三者在事件循环中的区别，事件循环中分为宏任务队列和微任务队列。 其中settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行； promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；async函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。

👍 93😄 7🎉 17😕 5❤️ 13🚀 17👀 21

#### 扩展：

- 关于Promise的正确认识：Promise本身是**同步的立即执行函数**， 当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法执行。

```js
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
// 输出顺序：script start->promise1->promise1 end->script end->promise2->settimeout
```

- async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。

```js
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}

console.log('script start');
async1();
console.log('script end')

// 输出顺序：script start->async1 start->async2->script end->async1 end
```

- await的含义为等待，也就是 async 函数需要等待await后的函数执行完成并且有了返回结果（Promise对象）之后，才能继续执行下面的代码。await通过返回一个Promise对象来实现同步的效果。

- 更多可见[setTimeout、Promise、Async/Await](https://github.com/sisterAn/blog/issues/21)

  👍 140👎 1😄 3🎉 13😕 1🚀 2👀 11

- 关于执行栈的总结
  - 在执行上下文栈的同步任务执行完后；
  - 首先执行Microtask队列，按照队列`先进先出`的原则，一次执行完所有Microtask队列任务；
  - 然后执行Macrotask/Task队列，一次执行一个，一个执行完后，检测 Microtask是否为空；
  - 为空则执行下一个Macrotask/Task；
  - 不为空则执行Microtask



![img](https://user-images.githubusercontent.com/18441915/68822044-d088ad00-06ca-11ea-8570-54a683dfef5d.jpg)