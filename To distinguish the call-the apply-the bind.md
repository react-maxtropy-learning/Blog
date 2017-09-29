### Shaallow discuss the apply, the call, the bind.
- The existence of apply and call is to change the direction of this inside of the function body. let take a chestnut :
``` 
function People(){}
People.prototype = {
   age=18,
sayAge = function(){
    return this.age
}
let me = new People()
me.sayAge() //18
let you = {age: 22}
me.sayAge.apply(you) // 22
me.sayAge.call(you) // 22
```
We can see that the function of apply and call is the same, it's all dynamically changeing this direction.
`  apply(this, [arg1, arg2,arg3])  `
apply the parameter as an array
` call(this,arg1,arg2,arg3) `
call parameter are passed in order
- The MDN's explanation for bind is : The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.
` fun.bind(thisArg[, arg1[, arg2[, ...]]])`
` thisArg ` The value to be passed as the this parameter to the target function when the bound function is called. The value is ignored if the bound function is constructed using the new operator.
` arg1,arg2... `Arguments to prepend to arguments provided to the bound function when invoking the target function.
- apply,call is an immediate function, and bind is when you change the context and not execute immediately, but when the callback is ececuted.

- Dang, Dang, Dang, knock on the blackboard ,and sum up
 1. apply, call, bind are all points to which this object is used to change the function.
 2. The first parameter of apply, call, bind, is the object to be pointed to, whitch is the context to be specified.
 3. apply, call, bind all can use the following parameter to pass the reference.
 4. bind is to return the corresponding function for later invocation; apply, call is an immediate call
- Today, xiao shifu took a course under the class and asked for the maximum minimum of the array, and then it's also out there
 1. The es6 solution ` Math.max(...[1,2,3])  // 3 `   use ` ... ` perform arrar deconstruction.
  2. `  Math.max.apply(null, [1,2,3]) // 3 `  
     `  Math.max([1,2,3]) // NaN`  It's just amazing! hahahaha!!!!!