# Why OOP?

- It helps us to think in terms of real world objects.
- give example of hospital,patients,and doctors


# What is Class and Object?

- Class is blurprint.
- Object is instances of class.

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
  - Encapsulation implements abstractiion.
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


# Abstraction vs encaptulation:

- For example, think of a car. We don't need to know how the engine works or how the transmission shifts gears to drive a car. We only need to know how to operate the steering wheel, accelerator, and brake pedals.
- n a class representing a bank account, we encapsulate the account balance and methods for depositing and withdrawing money. The balance is hidden from direct access and can only be modified through the provided methods, ensuring data integrity and sec

# Virtual keyword:


- In TypeScript and JavaScript, there is no direct equivalent to the virtual keyword found in languages like C++ and C#. This is because `TypeScript and JavaScript do not have the concept of virtual methods and do not support method overriding in the same way.`

- In TypeScript and JavaScript, methods are non-virtual by default, meaning they cannot be overridden by subclasses. However, you can achieve similar behavior using regular method overriding in subclasses.


```js
class Base {
    virtual display() {
        console.log("Base display() called");
    }
}

class Derived extends Base {
    display() {
        console.log("Derived display() called");
    }
}

const obj: Base = new Derived();
obj.display(); // This will call the display() method of Derived class


```
# Overloading vs Overriding:

- Overloading -> in same class, multiple method with different parameter.
- Overrriding -> Child class method have same name like parent class method.

# Runtime Polymorphism (Dynamic Polymorphism) vs Compile-Time Polymorphism (Static Polymorphism):

- Runtime Polymorphism (Dynamic Polymorphism):
  - Runtime polymorphism occurs when the method to be invoked is determined at runtime.
  - `It's achieved through method overriding.`
-  Compile-Time Polymorphism (Static Polymorphism):
    - Compile-time polymorphism occurs when the method to be invoked is determined at compile time.
    - `It's achieved through method overloading.`
 
# Abstract Class:

- It is partially defined parent class
- No object/instance can be created

# Interface in TS

```ts
interface Shape {
    calculateArea(): number;
}

class Circle implements Shape {
    constructor(private radius: number) {}

    calculateArea() {
        return Math.PI * this.radius * this.radius;
    }
}

const circle = new Circle(5);
console.log(circle.calculateArea()); // Output: ~78.54

```

# Multiple Inheritence of Interface:

```ts

interface Animal {
    eat(): void;
}

interface Mammal {
    walk(): void;
}

interface Bird {
    fly(): void;
}

interface Cat extends Animal, Mammal {
    meow(): void;
}

interface Sparrow extends Animal, Bird {
    chirp(): void;
}

class DomesticCat implements Cat {
    eat() {
        console.log("Cat eats");
    }

    walk() {
        console.log("Cat walks");
    }

    meow() {
        console.log("Cat meows");
    }
}

class HouseSparrow implements Sparrow {
    eat() {
        console.log("Sparrow eats");
    }

    fly() {
        console.log("Sparrow flies");
    }

    chirp() {
        console.log("Sparrow chirps");
    }
}

const cat = new DomesticCat();
cat.eat(); // Output: Cat eats
cat.walk(); // Output: Cat walks
cat.meow(); // Output: Cat meows

const sparrow = new HouseSparrow();
sparrow.eat(); // Output: Sparrow eats
sparrow.fly(); // Output: Sparrow flies
sparrow.chirp(); // Output: Sparrow chirps

```

# Interface Segregation Principle (ISP):

- The Interface Segregation Principle (ISP) states that a client should not be forced to depend on interfaces they do not use. In other words, it suggests that interfaces should be specific to the needs of the clients that use them, rather than being general-purpose interfaces that encompass a wide range of functionality.

```ts
// Interface for a Document
interface Document {
    read(): void;
    write(): void;
    print(): void;
}

// Printer interface
interface Printer {
    print(): void;
}

// Scanner interface
interface Scanner {
    scan(): void;
}

// Copier interface
interface Copier {
    copy(): void;
}

// Class that implements Document, Printer, and Scanner
class MultiFunctionPrinter implements Document, Printer, Scanner {
    read() {
        console.log("Reading document");
    }

    write() {
        console.log("Writing document");
    }

    print() {
        console.log("Printing document");
    }

    scan() {
        console.log("Scanning document");
    }

    copy() {
        console.log("Copying document");
    }
}

// Class that implements only Document interface
class SimplePrinter implements Document {
    read() {
        console.log("Reading document");
    }

    write() {
        console.log("Writing document");
    }

    print() {
        console.log("Printing document");
    }
}

// Class that implements only Printer interface
class NetworkPrinter implements Printer {
    print() {
        console.log("Printing document");
    }
}

// Class that implements only Scanner interface
class FlatbedScanner implements Scanner {
    scan() {
        console.log("Scanning document");
    }
}

// Client code
const multiFunctionPrinter = new MultiFunctionPrinter();
multiFunctionPrinter.read();
multiFunctionPrinter.write();
multiFunctionPrinter.print();
multiFunctionPrinter.scan();
multiFunctionPrinter.copy();

const simplePrinter = new SimplePrinter();
simplePrinter.read();
simplePrinter.write();
simplePrinter.print();

const networkPrinter = new NetworkPrinter();
networkPrinter.print();

const flatbedScanner = new FlatbedScanner();
flatbedScanner.scan();

```
