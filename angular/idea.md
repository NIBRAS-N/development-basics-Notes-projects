# crud  app:

- for crud app we need 2 things.
  
  1. A class which property will be the data of my card. we need to make an object of that class.
       - we need to bind the property with necessary input elements in html file.
  2. A array of that class where i will store all the data on first loading of the component
 
- in saveMethod ,
    - we will first fetch all data.
    - if theres no data initially , we will give the id 1.
        - create a array.. oush the object in that array.
        - then save the object.
    - else
        - create a new array..
        - in that array push all the prev. data.
        - push the current object in that array
        - save the array
-  in delete ,
    - sent the item through parameter.
    - find the id using findIndex
    - delete using splice from array
    - save the array
- in edit,
    - send the item through parameter
    - initialize the object with this item
    - click update
- in update ,
    - save the object.
