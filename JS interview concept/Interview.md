- ## What is the output?
```JS
console.log("what is in age? ", age );// undefined

var age = 20;

console.log("What is in age? ", age)//20
```
- ### Ans:
  - `Hoisting concept.`
  -  In JS there is `Global excecuting context`. There is 2 phase there.. `Memory and  code`.
  -  JS will store all variables  in `memory phase` with `undefined value`.
  -  In code phase one by one line will run. now age = 20
 
- ## What will be the output?
```js
console.log("what is in age? ", age );// ERROR

let age = 20;

console.log("What is in age? ", age)//20
```

- ### Ans:
    - `temporal dead zone`
    - will apply for `let,const`. Not for var.
    - before declaration of let and const, the area will be deadzone.. Not accessible there.
 
- ## What is the output?
```Js
myFunc();

var myFunc = function(){
  console.log("first);
}

myFunc();
function myFunc(){
  console.log("second");
}

myFunc();

// output:
  second
  first
  first
```
- ### Ans:
    - In memory phase , 
        - first myFunc = undefined;
        - second myFunc = function();
    - In code phase,
        - first line will execude, function will call. print second.
        - then re intialize the function as first.
        - third same.

- ## What is answer?

```JS
for(var i=0;i<10;i++){
  setTimeOut(()=>console.log(i),0);
}// 10 10 10 10 10 10 10....

for(let i=0;i<10;i++){
    setTimeOut(()=>console.log(i),0);
}// 1 2 3 4 5 6 7 8 9
```

- ### Reason:
    - let will create every local scope.
    - var doesnt create local scope.
 
- ## What is the answer?

```JS

var fullName = "hello abc";

var obj = {
  fullName : "hello cd";
  prop:{
    fullName:"inside prop",
    getFullname: function (){
      return this.fullName;    
    },
  },
  getFullName : function(){
    return this.fullName;
  },

  getFullNameV2: ()=>this.fullName,

  getFullNameV3:( function(){return this.fullName;})();
};
console.log(obj.prop.getFullName());//inside prop
console.log(obj.getFullName());//hello cd
console.log(obj.getFullNameV2());// undefined. cause arrow function's 'this' refers global object or window function.
console.log(obj.getFullNameV3());// error. because its iife. its a property. its not a function
console.log(obj.getFullNameV3);//undefined. No this in iife. if we return any string, it will show then.
```

- ### What is the answer:

```JS
  const john = {
    name:"john doe",
    sayName:function () {
        console.log(this.name);
    }
  }
  const dow = {
    name:"doe doe",
    sayName:function () {
        console.log(this.name);
    }
  }

  dow.sayName.call(john);//john doe . cause execution context has changed
  setTimeout(dow.sayName,3*1000); // undefined . cause execution context has changed
  setTimeout(dow.sayName.bind(dow),3*1000); // "doe doe", 
  
```
