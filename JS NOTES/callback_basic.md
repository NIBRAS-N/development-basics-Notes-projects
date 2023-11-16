# Callback

```js

const gamelist = [];

const fetch = (callback) => {
    setTimeout(() => {
        gamelist.push({
            name:"hello",
            key:"G1"
        },
        {
            name:"kill me",
            key:"G2"
        },
        {
            name:"fuck you",
            key:"G3"
        }

        )
        console.log("inner" , gamelist);
        callback()
    }, 4000 );
}

// console.log("outer 1",gamelist);

// console.log("outer 2",gamelist); 

const displayGame = () => {
    setTimeout(() => {
        
        for(let i=0;i<gamelist.length;i++){
            let zz = document.createElement("p");
            zz.innerText = gamelist[i].name;
            document.body.append(zz);
            
        }    
        
    }, 1000);
    
}

fetch(displayGame)
```