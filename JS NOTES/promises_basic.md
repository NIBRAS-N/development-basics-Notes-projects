# Promises use:

```js
const a = new Promise((resolve , reject)=>{
    let success = true;
    if(!success) resolve("Everything okey")
    else reject("Error occurred");
});

a.then((abc)=>{
    console.log(abc)
}).catch((error)=>{
    console.log(error)
})
```
## example 2   
```js
const ar = [];

const abc = (cd = []) =>{
    return new Promise((resolve, reject)=>{
        setTimeout(() => {
            cd.push({name:"nib"});
            if(cd.length>0)resolve("done")
            else reject("wtf you done")
        }, 2000);
        
    });


};


abc(ar).then((msg)=>{
    console.log(msg)
}).catch((msg)=>{
    console.log(msg);
})

// checking if its pass by reference or pass by value..
// 
setTimeout(() => {
    console.log(ar)    
}, 3000);

```