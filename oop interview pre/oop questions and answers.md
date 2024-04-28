# Why OOP?

- It helps us to think in terms of real world objects.
- give example of hospital,patients,and doctors

# What are the  4 pillers of OOP?

### 1. Abstraction:
  - Show only what is necessary.

```TS
abstract class Shape {
    abstract calculateArea(): number;
}

class Circle extends Shape {
    constructor(private radius: number) {
        super();
    }

    calculateArea(): number {
        return Math.PI * this.radius ** 2;
    }
}

let circle = new Circle(5);
console.log(circle.calculateArea());  // Output: 78.54

```

### 2. Polymorphism:
  - Object can act differently in different condition.
  - Polymorphism allows objects of different classes to be treated as objects of a common superclass
```TS
class Animal {
    makeSound(): void {
        console.log("Some generic sound");
    }
}

class Dog extends Animal {
    makeSound(): void {
        console.log("Bark");
    }
}

class Cat extends Animal {
    makeSound(): void {
        console.log("Meow");
    }
}

let animals: Animal[] = [new Dog(), new Cat()];
animals.forEach(animal => animal.makeSound());
// Output:
// Bark
// Meow

```
### 3. Inheritence:
  - Parent Child relationship. Super/parent class er `variable or method` child class theke access korte para.
  - Inheritance allows a class (subclass/child class) to inherit properties and methods from another class (superclass/parent class)
```TS
class Animal {
    move(): void {
        console.log("Moving...");
    }
}

class Dog extends Animal {
    bark(): void {
        console.log("Woof!");
    }
}

let myDog = new Dog();
myDog.move();  // Output: Moving...
myDog.bark();  // Output: Woof!
```
### 4. Encapsulation:
  - Hide complxity, change variable through method

```TS
class Car {
    private _speed: number;

    constructor(speed: number) {
        this._speed = speed;
    }

    getSpeed(): number {
        return this._speed;
    }

    accelerate(increment: number): void {
        this._speed += increment;
    }
}

let myCar = new Car(50);
console.log(myCar.getSpeed());  // Output: 50
myCar.accelerate(20);
console.log(myCar.getSpeed());  // Output: 70

```

# What is Class and Object?


