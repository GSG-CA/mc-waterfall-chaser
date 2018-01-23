# Morning challenge: implement a waterfall function
This workshop has been inspired by [Eoin McCarthy](https://github.com/des-des) and his legendary waterfall function workshop given to FAC9 by [Besart Hoxhaj](https://github.com/besarthoxhaj).

You may have seen the compose function before, it allows you to take some functions and compose them into a single function! 
What do you do if the functions are asynchronous? You implement the waterfall function. 

Like the compose function, the waterfall function runs an array of functions in series, each passing their result to the next function in the array. Crucially, the waterfall function will not invoke the next function in the array until the current function being run has been returned. 

### Waterfall function:
The waterfall function takes three arguments:

1) An initial argument, this is passed into the first async function in the waterfall.

2) The second argument is an array of functions, each function takes two arguments. The first argument it uses to find a result and the second is a callback function.

3) A final callback is given as the third argument. This is called after the waterfall has invoked all of the functions in the array.

### Workshop: 

1) Copy the code below and paste it into [Repl](https://repl.it/languages/javascript).

2) Create the waterfall function, pass the tests. 

3) For bonus points, create some additional tests for the waterfall function.

```
function asyncAddOne(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number'){ return callBack(new Error('need a number!')) }
    else { return callBack(null, x + 1) }
  }, 200)
}

function asyncDouble(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number') { return callBack(new Error('need a number!')) }
    else { return callBack(null, x * 2) }
  }, 200)
}

function asyncTimesTen(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number') { return callBack(new Error('need a number!')) }
    else { return callBack(null, x * 10) }
  }, 200)
}

function waterfall(arg, tasks, cb) {
    if(tasks.length === 0){
      return cb(null, arg)
    }
    tasks[0](arg, function(err, arg){
        if (err){
          return cb(err)
        }
        return waterfall(arg, tasks.slice(1), cb) 
    });
}

waterfall(3, [
  asyncAddOne,
  asyncDouble,
  asyncTimesTen
], function(error, result) {
  console.log('Test 1');
  if (error) {
    console.log('test failed, ' + error)
  }
  else if (result !== 80) {
    console.log('test failed, expected 80 but got', result)
  }
  else {
    console.log('Test 1 passed!')
  }
})
```

### Glossary:
- [Error-first callbacks](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/)
- [Compose function](http://blakeembrey.com/articles/2014/01/compose-functions-javascript/)
- [Synchronous vs. Asynchronous](http://rowanmanning.com/posts/javascript-for-beginners-async/)
