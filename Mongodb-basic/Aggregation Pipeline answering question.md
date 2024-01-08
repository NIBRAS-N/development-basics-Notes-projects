# Set up environment of schema coluser , books , and author and insert the json data

-`User sample collection`:
```json
{
    "_id":{"$oid":"6595660a27f4241f1acdf9de"},  
    "index":{"$numberInt":"0"},
    "name":"Aurelia Gonzales",
    "isActive":false,
    "registered":
    {
        "$date":
        {
            "$numberLong":"1423628559000"
        }
    },
    "age":{"$numberInt":"20"},
    "gender":"female",
    "eyeColor":"green","favoriteFruit":"banana",
    "company":{
        "title":"YURTURE",
        "email":"aureliagonzales@yurture.com",
        "phone":"+1 (940) 501-3963",
        "location":{
            "country":"USA",
            "address":"694 Hewes Street"
        }
    },
    "tags":["enim","id","velit","ad","consequat"]
}
```

# Questions:

- ### Q1: How many users are active?
```js
[
  {
    $match: {
      isActive: true,
    },
  },
  {
    $count: "activeMember",
  },
]
```

- ### Q2: What is the average age of all users?
    - #### Based on gender
    ```js
        [
        {
            $group: {
            _id: "$gender",
            avgAge:{$avg:"$age"}
            }
        }
        ]
    ```
    - ### Based on overall
    ```js
        [
        {
            $group: {
            _id: null,
            avgAge:{$avg:"$age"}
            }
        }
        ]
    ```
- ### Q3 List the top 5 most common fruits among the users:
```js
[
  {
    $group: {
      _id: "$favoriteFruit",
      favFruits:{$sum:1}
      
    }
      
  },
  
  {
    $sort:{favFruits:-1}
  },
  {
    $limit:5
  }
]
```
- ### Q4 Find total numbers of males and females:

```js
[
  {
    
  $group:{
  	_id:"$gender",
    number:{$sum:	1}
  }
  }
  
]

```

- ### Q5: Which country has the highest number of registered users?
```js
[
  {
    $group:{
      _id:"$company.location.country",
      total:{$sum:1}
    }
  },
  {
    $sort:{total:-1}
  },
  {
    $limit:2
  }
]
```
- ### List all the unique eye collor:
```js
//way1
[
  
  {
    $group:{
      _id:null,
      allColor:{
        $addToSet:{
          eyeColor:"$eyeColor"
        }
      }
      
    }
  }
  
]

// way2

[
  
  {
    $group:{
      _id:"$eyeColor",

  }
  }
]
```
- ### What is the avg number of tags per user:
```js
//way 1:

[
  {
    
      $addFields: {
        TagSize: {
        	// $size:"$tags"
          $size:{$ifNull : ["$tags",[]]}
        }
      }
  },
  {
    	$group: {
    	  _id:null,
    	  avgTag:{$avg:"$TagSize"}
    	}
      
        
      
    
  }
  
]

//way2
  [
  {$unwind: "$tags"
   },
  {
    $group: {
      _id:"$_id",
    	numberOfTags:{
    		$sum:1
      }
    }
    
  },
  {
    $group: {
      _id: null,
      avgTags:{
        $avg:"$numberOfTags"
      }
    }
  }
]
```

- ### How many users have 'enim' as one of their tags?
```js
[
  //way 1
  {
    $unwind: "$tags"
  },
  {
    $match: {
      tags:"enim"
   		   
    }
  },
  {
    $group:{
      _id:null,
      total:{$sum:1}
    }
  }
]
//way 2
[
  {
    $match: {
      tags:"enim",
      
    }
  },
  {
    $count:'totalEnim'
  }
]
```

- ### what are the name and age of the users who are inactives and have velit in their tag:

```js
[
  {$match: {
    isActive:false, tags:"velit"
  }},
  
  {
    $project:{
      name:1,age:1,tags:1,isActive:1
    }
  }
 
]

```

- ### How many users have a phone number starting with `+1(940)`?

```js
  	[
    {
      $match:{
        "company.phone":/^(\+1 \(940\))/
			
      }
      
    },
    {
      $count: 'startNumber'
    }
  ]
```

- ### Who has registered most recently?:

```js

[
  {
    $sort:{
      registered:-1
    }
  },
  {
    $limit:2
  }
]
```
- ### Categorized user by their favorite food:

```js
[
  {
    $match: {
    	name:{$exists:true},
      favoriteFruit:{$exists:true}
    }
  },
  {
    $group: {
      _id: "$favoriteFruit",
      user:{
        $push:"$name"
      }
        
    }
  }
  
]
```

- ### Find those users who have 'ad' in the tag in 2nd position
```js
[
  {
    $project:{
      userWithAdIn:{
        $arrayElemAt:["$tags",2]
      }
    }
  },
  {
    $match:{
      userWithAdIn:"ad"
    }
  },
  {
    $count:"userWithAdIn"
  }
]

//way2

[
  {

    
  $match:{
  	"tags.1":"ad"
  }
  },
  {
    $count:"secondTagAd"
  }
]
```

- ### Find users who have both  'enim' and 'id' in their tags:
```js
[
  {    
  $match:{
    $and:[
      {tags:{$in:["enim"]}},
      {tags:{$in:["id"]}},
    ]
  }
  },
  {
  	$count:"kj"
  }
]

// way 2
[
  {    
  $match:{
   	tags:{
      $all:["enim","id"]
    }
  }},
]
```

- ### List all the users in the usa with their corresponding user count:

```jsx
[
  
  {
    $match: {
      "company.location.country":"USA"
    }
  },
  
]
```

- ### 