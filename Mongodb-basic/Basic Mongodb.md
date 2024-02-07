# Basic kng:

- - db.products.find(`query`).explain('executionStats') 

- reducer in js:
    - `array.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
`
- ```js
    const numbers = [1, 2, 3, 4, 5];

    const sum = numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
    }, 0); // Initial value of the accumulator is 0

    console.log(sum); // Output: 15 (1 + 2 + 3 + 4 + 5 = 15)

  ```


# what is mongoDB?
- Mongodb is an open source , document oriented NoSql databse management system.

- `document oriented` means a database that stores information in documetns.

- Stores in the from of `json` documents

- `10gen` company created mongodb.. now its name is `MONGODB`

- Here in a database there will be multiple collection and under a collection there will be multiple documents




# Download applications:

- mongodb for winddows. 
    - install and copy-paste the bin folder in environment setting
- mongodb database tools
- mongosh

# cmd command:

- mongod --version
- mongosh 
    - show dbs

# mongodb basic command:
- in cmd write mongosh
- show databases:
    - show dbs / show databases
- create database:
    - use  `database-name` ;
- delete database:
    - dp.dropdatabase();
- show collection:
    - show collections
- create collectioni:
    - db.createCollection(`collection name`);
- delete collection:
    - db.`collection_name`.drop();

# Crud operations:

- ## Insert Operation:
    - ### Inserting document in mongodb:
    ```js
        db.`collection_name`.insertOne({  
            field1:value,
            field2:value  
        })
    ```
    ```js
        db.`collection_name`.insertMany([
            {field1:value,field2:value},
            {fiedl3:value,field4:value}
        ])
    ```
    - ### `ordered inserts` and `unordered inserts`:
        - this is the default behavior in MongoDB.
        - If an error occurs while processing a document (for example, a duplicate key error or a validation error), MongoDB will `stop processing further` documents in the array but will continue `inserting any before documents` that do not raise errors. 
        -  When performing an `unordered insert`, MongoDB attempts to insert all the documents in the provided array regardless of any errors encountered during the process
    ```js
    // Ordered insert (default behavior)
    db.collection.insertMany([ 
        /* array of documents */ 
    ]);

    // Ordered insert explicitly specified
    db.collection.insertMany([ 
        /* array of documents */ 
    ], { ordered: true });

    // Unordered insert
    db.collection.insertMany([ 
        /* array of documents */ 
    ], { ordered: false });

    ```

    - ## Find all data in a collection:
    ```js
        db.`collection name`.find()
        db.`collection name`.find({key:value})
        db.`collection name`.findOne({key:value})


    ```

    - ## Read operations in MONGODB:

        - ### Reading documents in MONGODB:
            - ### import JSON in MONGODB:

            ```js
                // if the documents is not in array
                mongoimport `jsonfile.json` -d `database_name` -c `collection name`

                // if the documents is in array

                mongoimport `jsonfile.json` -d `database_name` -c `collection name` --jsonArray
            ```
        - ### Comparison operators:
            - $eq
            - $ne
            - $gt
            - $gte
            - $It
            - $Ite
            - $in
            - $nin
        ```js
        db.`collection_name`.find({`field`:{$eq:`value`}});
        
        db.`collection_name`.find({`field`:{$eq:`value`}}).count();

        db.`collection_name`.find({`field`:{$in:[`val1`,`val2`,`val3`]}});
        ```
        - ### Logical Operators:

            - $and
            ```js
            db.products.find({
            $and: [
                { category: "Electronics" },
                { price: { $gt: 500 } },
                { inStock: true }
            ]
            });

            ```
            - $or
            - $not
            - $nor

        - ### Cursors in mongodb:
            - In MongoDB, a cursor is a `pointer to the result set of a query`. When you query a collection in MongoDB, instead of returning all the matching documents at once, MongoDB returns a cursor. The cursor is an iterable object that allows you to access the documents in the result set in a controlled manner, typically fetching a batch of documents at a time.
        ```js
        // Querying documents from a collection
        const cursor = db.collection.find({ /* query criteria */ });

        // Iterating through the cursor to access documents
        cursor.forEach(doc => {
            // Process each document here
            console.log(doc);
        });

        ```
        - ### Cursor Method:

            - `count()`
            - `limit(`)
            ```js
            //books is a collection name.year is a field name. 1 means ascending order.
            db.books.find().sort({ year: 1 }).limit(3);
            ```
            - `skip()`
            ```js
            //books is a collection name.year is a field name. 1 means ascending order.
             db.books.find().sort({ year: 1 }).skip(2).limit(2);
            ```
            - `sort()`
            ```js
            //books is a collection name.year is a field name. 1 means ascending order.
            // -1 means descending order
            db.books.find().sort({ year: 1 });
            // will show all the documents under books collection sorting the year in ascending order..  

            ```
            - `Combining all methods:`
            ```js
             db.books.find().sort({ year: 1 }).skip(1).limit(2);
            ```

            - `Note : skip() can be inefficient for large offsets. . using sort() on large set may impact in performance`

            - `$expr`
                - The $expr operator in MongoDB allows you to use aggregation expressions within regular query operations. It's particularly useful when you need to compare fields from the same document or perform more complex comparisons that aren't directly supported by regular query operators.
            ```js
            db.employees.find({
            $expr: { $gt: [ { $multiply: ["$salary", 2] }, "$vacationDays" ] }
            });
            ```
        - ### Elements in MONGODB:

            - $exists
            ```js
            db.inventory.find({ tags: { $exists: true } });

            ```
            - $type
            ```js
            db.inventory.find({ quantity: { $type: "number" } });
            ```
            - $size
            ```js
            db.inventory.find({ tags: { $size: 2 } });

            db.orders.aggregate([
            {
                $project: {
                numberOfItems: { $size: "$items" }
                }
            }
            ])

            ```

    ## Projection:
    - Projection is used to specify which fields you want to retrieve or exclude in query results. Here are some examples of projection:
    - Creating a collection name `students`and inserting these data:

    ```js
    db.students.insertMany([
  { name: "Alice", age: 20, grade: "A", subjects: ["Math", "Physics", "Chemistry"] },
  { name: "Bob", age: 22, grade: "B", subjects: ["History", "Literature", "Geography"] },
  { name: "Charlie", age: 21, grade: "B", subjects: ["Biology", "Computer Science"] },
  // ... more student records
    ]);
    ```
    - `Include Specific Fields:` Retrieve only the `name` and `age` fields.
        ```js
        const nameAndAgeOnly = db.students.find({}, { name: 1, age: 1, _id: 0 });

        ```
    - `Exclude Specific Fields:` Exclude the grade field from the results.
        ```js
        const withoutGrade = db.students.find({}, { grade: 0 });
        ```

    - `Include Fields with Array Elements`:
        ```js
        const nameAndFirstSubject = db.students.find({}, { name: 1, "subjects.0": 1, _id: 0 });

        ```

    - ## $elemMatch and $all
    
        - The $elemMatch and $all operators in MongoDB are used for querying arrays within documents.

        - Create this collection:
        ```js
        db.products.insertOne({
        name: "Laptop",
        reviews: [
            { user: "Alice", rating: 4 },
            { user: "Bob", rating: 5 },
            { user: "Charlie", rating: 3 }
        ]
        });

        ```
        - `$elemMatch` Example:   
            - Suppose you want to find products where at least one review has a rating greater than 4. You can use $elemMatch to match elements within the reviews array:
            ```js
                const productsWithHighRatings = db.products.find({
                reviews: {
                    $elemMatch: { rating: { $gt: 4 } }
                }
                });

            ```
        - $all example:
            - Now, let's say you want to find products that have received specific ratings from multiple users.
            ```js
            const productsWithSpecificRatings = db.products.find({
            reviews: {
                $all: [
                { $elemMatch: { user: "Alice", rating: 4 } },
                { $elemMatch: { user: "Bob", rating: 5 } }
                ]
            }
            });

            ```

- ## Upadate Operations in MONGODB:
    - `Basic Update method:`[adding new field and updating current field]
    ```js
        db.`collection_name`.updateOne(
            {filter},
            {
                $set:{
                    `existing_filed`:`new_Value`,
                    `new Field`:`value`
                }
            }
        )
        db.`collection_name`.updateMany(
            {filter},
            {
                $set:{
                    `existing_filed`:`new_Value`,
                    `new Field`:`value`
                }
            }
        )
    ```
    - `Rename and remove` any field:
        - **removing any field**
    ```js
        db.`collection name`.updateOne(
            {filter},
            {
                $unset:{fieldName:1}
            }
        );
    ```
    - **Renaming any field:**
    ```js
        db.`collection name`.updateOne(
            {filter},
            {
                $rename:{oldFieldname:Newfieldname}
            }
        );
    ```
    - **update in array**
        - **add value in array using push operator:**
        ```js
        db.`collectionName`.updateOne(
            {filter},
            {
                $push:{arrayField:newElement}
            }
        )
        ```
        - **last element of array will be deleted using pop**
        ```js
        db.`collectionName`.updateOne(
            {filter},
            {
                $pop:{arrayField:1 or 0}
            }
        )
        ```
        -  **value of array will be updated in this way:**

        ```js
        db.students.insertOne({
        name: "Alice",
        grades: [
            { subject: "Math", score: 85 },
            { subject: "Physics", score: 75 },
            { subject: "Chemistry", score: 90 }
        ]
        });
        db.students.updateOne(
        { name: "Alice", "grades.subject": "Physics" },
        { $set: { "grades.$.score": 80 } }
        );

        db.`collectionName`.updateOne(
            {filter},
            {
                $set:{"arrayField.$.text":"updatedValue" }
            }
        )
        ```
    - ## delete documents:
        - One document delte:
        ```js
            db.`collection name`.deleteOne({_id:1})
        ```
        - Multiple documents delete:
        ```js
            db.`collection name`.deleteMany({_id:1})
        ```

- # Indexes in MONGODB:

    - ### What are indexes
        - In MongoDB, indexes are data structures that store a small portion of a collection's data in an easy-to-traverse form. They enhance the efficiency of query execution by allowing MongoDB to quickly locate documents based on the indexed fields.
    - ### Benifits of indexes
        - faster query
        - efficient sorting
        - Improved aggregation
        - indexing on muliple fields
    - ### Managing indexes
    ```js
        db.`collection name`.getIndexes()

        db.`collection name`.createIndex({field:1})
            - 1 for ascending order
            - -1 for descending order

        


        db.`collection name`.dropIndex({field:1})

        db.`collection name`.dropIndex("index_name")
    ```
    - ### Unique , Text index
        - **Unique:**
        ```js
        db.`collection name`.createIndex({field:1},{unique:true})
        ```

        - **$text:**
            - In MongoDB, the $text operator and text indexes are used for performing full-text searches across text fields in a collection. Text indexes allow for efficient and language-aware searches for text content within documents.

            ```js
                db.articles.insertMany([
                { _id: 1, title: "Introduction to MongoDB", content: "MongoDB is a NoSQL database..." },
                { _id: 2, title: "Querying in MongoDB", content: "Query operations in MongoDB..." },
                { _id: 3, title: "Indexing in MongoDB", content: "Indexes improve query performance..." },
                // ... more articles
                ]);


                // creating text index

                db.articles.createIndex({ content: "text" });
                // heere content is field


                //Use the $text operator to perform a full-text search:

                // Search for documents containing the word "MongoDB"
                db.articles.find({ $text: { $search: "MongoDB" } });



            ```

            - Text indexes are `case-insensitive` and can include various text analysis features like stemming, language-specific stop words, and more.
            - The $text operator can also utilize additional search options like $language, $caseSensitive, $diacriticSensitive, etc., to customize search behavior.
        ```js
        // Search for documents with case sensitivity and language specified
        db.articles.find({ $text: { $search: "MongoDB", $caseSensitive: true, $language: "english" } });

        ```
    - ### When not to use Indexes

        - rarely used fields
        - might take overspace
        - indexing small collection

- # Aggregation in MONGODB:

    - In MongoDB, aggregation refers to the process of transforming and manipulating documents in a collection to perform complex data processing tasks.
    - the aggregation framework in MongoDB provides a powerful set of tools for data aggregation, grouping, filtering, and performing computations.
    - `Pipeline stages`: Aggregation consists of multiple pipeline stages.each performing a specific operation on the input data.

    - ### `$match`
        ```js
            db.employees.insertMany([
            { 
                name: "Alice", department: "HR", age: 28, salary: 60000 
            },
            { name: "Bob", department: "Engineering", age: 35, salary: 75000 },
            { name: "Charlie", department: "HR", age: 30, salary: 55000 },
            // ... more employee data
            ]);

            // we want to find employees who are part of the HR department and are younger than 32 years old:

            db.employees.aggregate([
            {
                $match: {
                department: "HR",
                age: { $lt: 32 }
                }
            }
            ]);


        ```
    - ### $group:
        - The $group stage in MongoDB's aggregation pipeline is used to group documents by a specified expression and perform aggregate operations on them.

        ```js
        db.sales.insertMany([
        { product: "A", category: "Electronics", quantity: 10, price: 100 },
        { product: "B", category: "Clothing", quantity: 20, price: 50 },
        { product: "A", category: "Electronics", quantity: 15, price: 110 },
        { product: "C", category: "Electronics", quantity: 12, price: 95 },

        ]);

        db.sales.aggregate([
        {
            $group: {
            _id: "$category", 

            // Grouping by the 'category' field

            totalQuantity: { $sum: "$quantity" }, 
            
            // Calculating total quantity for each category


            // Calculating total quantity for each category
            totalCategory: { $sum: 1 }, 


            averagePrice: { $avg: "$price" } // Calculating average price for each category
            }
        }
        ]);

        //multiple stage aggregation
        db.sales.aggregate([
        {
            $match:{
                price:{$gt:200}
            }
        },

        {
            $group: {
            _id: "$category", 

            // Grouping by the 'category' field

            totalQuantity: { $sum: "$quantity" }, 

            // Calculating total quantity for each category


            // Calculating total quantity for each category
            totalCategory: { $sum: 1 }, 


            averagePrice: { $avg: "$price" } // Calculating average price for each category
            }
        }
        ]);


        ```
    - **$sort**
        - `{ $sort: { <field1>: <order>, <field2>: <order>, ... } }
`
    ```js
        db.students.insertMany([
        { name: "Alice", age: 20, grade: "A" },
        { name: "Bob", age: 22, grade: "B" },
        { name: "Charlie", age: 21, grade: "B" },
        // ... more student records
        ]);

        //sorting Students by Age in Ascending Order:

        db.students.aggregate([
        { $sort: { age: 1 } }
        ]);

        //Sorting Students by Grade in Descending Order:

        db.students.aggregate([
        { $sort: { grade: -1 } }
        ]);

        //Sorting Students by Multiple Fields:
        db.students.aggregate([
        { $sort: { grade: 1, age: -1 } }
        ]);

    ```

    - ### `$project`

        -  The $project stage in MongoDB's aggregation pipeline allows you to reshape documents, include or exclude fields, create computed fields, and rename existing fields.

    ```js
        db.sales.insertMany([
    { _id: 1, product: "A", quantity: 10, price: 100 },
    { _id: 2, product: "B", quantity: 20, price: 50 },
    { _id: 3, product: "C", quantity: 15, price: 110 },
    // ... more sales data
    ]);

    //Projection to Include Specific Fields

        db.sales.aggregate([
    {
        $project: {
            product: 1,
            totalPrice: { $multiply: ["$quantity", "$price"] }
        }
    }
    ]);

    //Projection to Exclude Specific Fields:

    db.sales.aggregate([
    {
        $project: {
            quantity: 0
        }
    }
    ]);

    //Renaming Fields:

    db.sales.aggregate([
    {
        $project: {
            productName: "$product",
            totalCost: { $multiply: ["$quantity", "$price"] }
        }
    }
    ]);

    //Using Conditional Projection:

    db.sales.aggregate([
    {
        $project: {
            product: 1,
            price: 1,
            discounted: {
                $cond: {
                if: { $gte: ["$quantity", 15] },
                then: true,
                else: false
            }
        }
        }
    }
    ]);

    ```
    - ### `$push`:
        - The `$push` operator in MongoDB's aggregation framework is used within the $group stage to accumulate values into an array for each grouped document.
        ```js
            db.orders.insertMany([
            { _id: 1, orderId: "A001", product: "Phone", quantity: 2 },
            { _id: 2, orderId: "A002", product: "Laptop", quantity: 1 },
            { _id: 3, orderId: "A001", product: "Charger", quantity: 3 },
            { _id: 4, orderId: "A002", product: "Headphones", quantity: 2 },
            // ... more orders
            ]);


            // use the $group stage with $push to aggregate orders by their orderId, creating an array of products for each order:

            db.orders.aggregate([
            {
                $group: {
                _id: "$orderId",
                products: { $push: "$product" }
                }
            }
            ]);


            //To avoid duplicate use $addToSet
            db.orders.aggregate([
            {
                $group: {
                _id: "$orderId",
                products: { $addToSet: "$product" }
                }
            }
            ]);

            // result:

            [
                { _id: "A001", products: ["Phone", "Charger"] },
                { _id: "A002", products: ["Laptop", "Headphones"] },
                // ... more grouped orders
            ]

        ```

    - ### $unwind
        - products: ["Phone", "Charger"]
        - Using $unwind, you can create separate documents for each product within the products array:
        - `db.orders.aggregate([
              { $unwind: "$products" }
            ]);
`
    - ### $size
    - ### $count
    ```js
            db.orders.aggregate([
        {
            $count: "totalOrders"

        }
        ]);


        // result:

        [
            { totalOrders: 3 }
        ]


    ```
    - ### $limit
    - ### $skip
    - ### $filter
        - the $filter operator in MongoDB's aggregation framework allows you to filter the elements of an array based on specified conditions within the aggregation pipeline. It's particularly useful for extracting a subset of array elements that match certain criteria.
        ```js
        db.students.insertMany([
        { _id: 1, name: "Alice", grades: [85, 90, 75, 92, 88] },
        { _id: 2, name: "Bob", grades: [70, 68, 75, 72, 65] },
        // ... more student data
        ]);


        db.students.aggregate([
        {
            $project: {
            name: 1,
            highGrades: {
                $filter: {
                input: "$grades",
                as: "grade",
                cond: { $gt: ["$$grade", 80] }
                }
            }
            }
        }
        ]);


            // results

        [
        { _id: 1, name: "Alice", highGrades: [85, 90, 92, 88] },
        { _id: 2, name: "Bob", highGrades: [] }
        ]

        ```


    - ### $addFields
    ```js  
    db.students.aggregate([
    {
        $addFields: {
            totalScore: { $sum: "$grades" }
        }
    }
    ])

    ```
    - ### $arrayElemAt
    ```js
    db.students.aggregate([
  {
    $project: {
      secondGrade: { $arrayElemAt: ["$grades", 1] }
            }
        }
        ])
    ```
    
- ### $lookup;
    - It uses left outer join.
    - Meanse matching element will come , besides non matched elemenets from left parent document will also come
```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerInfo"
    }
  }
])

```
