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
