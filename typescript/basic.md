# TS Datatype:
### Declarations:
- let isDone: boolean
- let isDone: boolean = false
- function add (a: number, b: number): number {
    return a + b
  }
- const abc = (a:number , b:number):string => {
    return String(a*b)
  }
- const abc = (a:number , b:number) => {
    return String(a*b)
  } `// automatically detect the return type`

### Basic Types:
- any
- void

- boolean
- number
- string

- null
- undefined

- bigint
- symbol

- string[]          `/* or Array<string> */`
- [string, number]  `/* tuple */`
  - let myTuple: [number, string, boolean] = [1, "hello", true];
    - console.log(myTuple[0]);  // Output: 1
    - console.log(myTuple[1]);  // Output: hello
    - console.log(myTuple[2]);  // Output: true
  
- string | null | undefined   `/* union */`

- never  `/* unreachable */`
- unknown
---
- enum Color {
    Red,
    Green,
    Blue = 4
  };
  - **let c: Color = Color.Green**
---
# Type Aliases:

- Type aliases in TypeScript allow you to create custom names for types. They're especially useful for complex types or when you want to give descriptive names to existing types. Here's an example demonstrating type aliases
- 
```ts


type Name = string | number
let abc:Name = 5
 
// Define a type alias for a complex type

type Point = {
    x: number;
    y: number;
};

// Define a type alias for a function type
type MathFunction = (x: number, y: number) => number;

// Usage of type aliases
let origin: Point = { x: 0, y: 0 };
let add: MathFunction = function(x, y) {
    return x + y;
};

console.log(origin); // Output: { x: 0, y: 0 }
console.log(add(2, 3)); // Output: 5
```
# Array:

```ts
// Declaring an array of numbers
let numbers: number[] = [1, 2, 3, 4, 5];

// Declaring an array of strings
let fruits: string[] = ["apple", "banana", "orange"];

// Declaring an array using the Array constructor
let colors: Array<string> = ["red", "green", "blue"];

// Declaring an array with mixed types
let mixedArray: (string | number | boolean)[] = ["hello", 42, true];

// Declaring a multidimensional array
let matrix: number[][] = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

// Declaring an empty array
let emptyArray: any[] = [];

// Declaring an array with a specified length
let fixedLengthArray: number[] = new Array(5);

// Declaring and initializing an array using the spread operator
let numbersCopy: number[] = [...numbers];

```

# Object:

```ts
// Declaring an object with explicit type annotation
let person: { name: string; age: number } = {
    name: "Alice",
    age: 30
};

// Declaring an object using an interface
interface Point {
    x: number;
    y: number;
}
let point: Point = {
    x: 10,
    y: 20
};

// Declaring an object with optional properties
interface Car {
    make: string;
    model?: string; // Optional property
    year: number;
}
let car: Car = {
    make: "Toyota",
    year: 2022
};

// Declaring an object with index signature
interface Dictionary {
    [key: string]: number;
}
let scores: Dictionary = {
    math: 90,
    science: 85,
    history: 88
};

// Declaring an empty object with explicit type annotation
let emptyObject: {} = {};

// Declaring an object with dynamic property names
let dynamicObject: { [key: string]: number } = {
    a: 1,
    b: 2,
    c: 3
};

```

# Function

```ts
type FuncType = (n: number, m: number, l?: number) => number;

// // Optional Parameter
const func: FuncType = (n, m, l) => {
   if (typeof l === "undefined") return n * m;

   return n * m * l;
 };

 func(25, 23);

// // Default Parameter
 type FuncType = (n: number, m: number, l?: number) => number;
const func: FuncType = (n, m, l = 20) => {
   return n * m * l;
};

func(25, 23);

// // Rest Operator
type FuncType = (...m: number[]) => number[];
const func: FuncType = (...m) => {
   return m;
};
func(25, 23, 34, 6, 67, 8, 9);

function lol(n:number):number{
     return 45
}

```

# Function with object

```ts
interface Product {
   name: string;
   stock: number;
   price: number;
   photo: string;
   readonly id: string;
}

type GetDataType = (product: Product) => void;

const getData: GetDataType = (product) => {
   console.log(product);
};

const productOne: Product = {☺
   name: "Macbook",
   stock: 46,
   price: 999999,
   photo: "samplephotourl",
   id: "asdnasjdasjkdas",
};

getData(productOne);

//Never Type

// const errorHandler = (): never => {
//   throw new Error();
// };
```
