# How to call Api and handle things


```JS
const url= "https://catfact.ninja/fact";
const z = document.querySelector("h1");
// console.log(z)
const fetchData = () =>{
    fetch(url)
    .then((response)=>response.json())
    .then((abc)=>z.innerHTML=abc.fact)
    .catch((error)=>console.log("error"))
    .finally(()=>console.log("all done") )
}

fetchData()

```

## Using Async:

```JS
const url= "https://catfact.ninja/fact";
const z = document.querySelector("h1");

// Async way

const fetchData = async ()=>{
    try{
        // throw new Error("Helelo");

        const response = await fetch(url);

        const data = await response.json()
        
        z.innerText = data.fact;



    }catch(error){
        console.log(error)
    }
}

fetchData()
```