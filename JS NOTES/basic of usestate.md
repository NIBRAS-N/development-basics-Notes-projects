```jsx

import React,{ useState } from "react";

const Home = () => {

  const [Change , setChange] = useState(0);

  let inputvalue=0;
  const func =(e)=>{
    let given=e.target.value;
    console.log(given);
  };
  const increment = () => {
    setChange(Change+1);
  };
  const decrement = () => {
    setChange(Change-1);
  };
  const btnstyle = {
    margin:2 ,
    padding: 6,
    border: "2px solid navy",
    backgroundColor:"grey",
    color:"red" 
  };
  return (
    <>
    <input type="number"
        placeholder="Input something" 
        style={{ padding: 4, border:"1px soldi black"}}
        onChange={ (e) => {
          setChange(e.target.value)
        }}
        // onChange={func}
        value={Change}
    />
    <button style={btnstyle } onClick={increment}>+</button>
    <button style={btnstyle} onClick={decrement}>-</button>
    </>
  );
};

export default Home;

```