```ts


function printId(id: string | number) {
  if (typeof id === "string") {
    // এখানে TypeScript জানে যে id অবশ্যই একটি string
    console.log(id.toUpperCase()); // string-এর মেথড ব্যবহার করা যাচ্ছে
  } else {
    // এখানে TypeScript জানে যে id অবশ্যই একটি number
    console.log(id.toFixed(2)); // number-এর মেথড ব্যবহার করা যাচ্ছে
  }
}


```

```ts

function printId(id: string | number) {
  if (typeof id === "string") {
    // TypeScript knows id is string here
    console.log(id.toUpperCase());
  } else {
    // TypeScript knows id is number here
    console.log(id.toFixed(2));
  }
}
```


```ts
interface Dog {
  breed: string;
  bark(): void;
}

interface Cat {
  breed: string;
  meow(): void;
}

function makeSound(pet: Dog | Cat) {
  if ("bark" in pet) {
    // TypeScript knows pet is Dog
    pet.bark();
  } else {
    // TypeScript knows pet is Cat
    pet.meow();
  }
}


```


```ts
interface Fish { swim(): void; }
interface Bird { fly(): void; }

// ১. এটি একটি কাস্টম টাইপ গার্ড ফাংশন
function isFish(pet: Fish | Bird): pet is Fish {
  // ২. এখানে আমরা চেক করছি pet-এর ভেতর swim মেথড আছে কি না
  // (pet as Fish) ব্যবহার করে আমরা সাময়িকভাবে টাইপ কাস্ট করছি চেক করার জন্য
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  // ৩. এখানে isFish ফাংশনটি কল করা হচ্ছে
  if (isFish(pet)) {
    // যদি true হয়, টাইপস্ক্রিপ্ট ১০০% নিশ্চিত যে pet হলো Fish
    pet.swim(); 
  } else {
    // যদি false হয়, টাইপস্ক্রিপ্ট অটোমেটিক বুঝে নেয় pet হলো Bird
    pet.fly(); 
  }
}
```

```ts

// ১. প্রতিটি ইন্টারফেসে 'kind' নামে একটি common প্রপার্টি আছে
interface Square {
  kind: "square"; // এটিই হলো discriminant
  size: number;
}

interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}

interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Rectangle | Circle;

// ২. এখন টাইপ ন্যারো করা খুব সহজ হয়ে যায়
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "square":
      // এখানে TypeScript জানে shape অবশ্যই Square
      return shape.size * shape.size;

    case "rectangle":
      // এখানে TypeScript জানে shape অবশ্যই Rectangle
      return shape.width * shape.height;

    case "circle":
      // এখানে TypeScript জানে shape অবশ্যই Circle
      return Math.PI * shape.radius ** 2;
  }
}
```


```ts
function assertIsString(val: any): asserts val is string {
  if (typeof val !== "string") {
    throw new Error("Not a string!");
  }
}

function processValue(value: string | number) {
  assertIsString(value); // এখানে এসে কোড চেক করবে। 
  
  // এই লাইনের পরে TypeScript ১০০% নিশ্চিত যে value একটি string।
  // যদি string না হতো, তবে উপরের লাইনেই এরর আসত এবং নিচের লাইনটি চলত না।
  console.log(value.toUpperCase()); 
}
```


```ts
function assertIsDefined<T>(val: T): asserts val is NonNullable<T> {
  if (val === undefined || val === null) {
    throw new Error(`Expected 'val' to be defined, but received ${val}`);
  }
}

```
