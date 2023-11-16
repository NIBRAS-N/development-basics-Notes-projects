# Routing Steps in react:

## step1:

- run this command in terminal:
    - npm i react-router-dom
- Then run the file:
    - npm start

## step2:

- the code for the routing in header file:

```jsx
import React from "react"
import {Link} from "react-router-dom"

const Header = () {
    return(
        <>
            <div>
                <Link to="/">Home</Link>
                <Link to="/About">About</Link>
                <Link to="/Contact">Contact</Link>


            </div>
        </>
    )
}
```

## Step-3:

- In App.jsx file do these change
```jsx

import {BrowserRouter,Route,Routes} from "react-router-dom"
function App{
    return(
        <BrowserRouter>
        
        <Header/>
        // Only linked pages will be used here  

        <Routes>
            <Route path="/" element={<Home />}  />
            <Route path="/About" element={<About />} />
            <Route path="/Contact" element={<Contact />} />
        </Routes>
        
        // Jegolo only 1 bar render howa dorkar ogolo routes er vitore thakbe.
        
        </BrowserRouter>
    )
}
```
## Dynamic Routing:

- `Note:` `**user/**` er porey jay thakuk oytake id consider kora hobe

- id change holew 2 tar route ekoy hishebe dhora hoy..

- Tay specific id er jonno specific page dekhanor jonno Routes tag use kora hoy.

- 
```jsx
<>
<Routes>
    <Route path="/user/[id]" element={<User />}  />
    <Route path="/user/[id2]" element={<Contact />}  />
</Routes>

</>
```


## Use `navigation` to go from any button to any page:

```jsx
import {useParams , useNavigate}  from "react-router-dom"
const params = useParams()
const navigation = useNavigate();

console.log(params.id);// for dynamic routing, not for navigation

<Button onClick={()=>navigation("/about")} >Click me</Button>
```