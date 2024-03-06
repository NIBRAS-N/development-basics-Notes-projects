# CLasses and OOP:
---
## Encapsulation:
```ts
// Define a class representing a Car
class Car {
    // Properties
    private _brand: string;
    private _model: string;
    private _year: number;

    // Constructor
    constructor(brand: string, model: string, year: number) {
        this._brand = brand;
        this._model = model;
        this._year = year;
    }

    // Methods
    accelerate(speed: number): void {
        console.log(`${this._brand} ${this._model} is accelerating at ${speed} km/h.`);
    }

    brake(): void {
        console.log(`${this._brand} ${this._model} is braking.`);
    }

    // Encapsulation: Getter and setter methods
    get brand(): string {
        return this._brand;
    }

    set brand(value: string) {
        this._brand = value;
    }
}

// Create an object of type Car
let myCar = new Car("Toyota", "Corolla", 2022);

// Accessing properties and calling methods
console.log(myCar.brand); // Output: Toyota
myCar.accelerate(100); // Output: Toyota Corolla is accelerating at 100 km/h.
myCar.brake(); // Output: Toyota Corolla is braking.

```

## Abstraction:

```ts
abstract class Shape {
    abstract calculateArea(): number;
    abstract calculatePerimeter(): number;
}

class Circle extends Shape {
    constructor(private radius: number) {
        super();
    }

    calculateArea(): number {
        return Math.PI * this.radius * this.radius;
    }

    calculatePerimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(private width: number, private height: number) {
        super();
    }

    calculateArea(): number {
        return this.width * this.height;
    }

    calculatePerimeter(): number {
        return 2 * (this.width + this.height);
    }
}

let circle = new Circle(5);
console.log("Circle Area:", circle.calculateArea()); // Output: Circle Area: 78.53981633974483

let rectangle = new Rectangle(4, 6);
console.log("Rectangle Perimeter:", rectangle.calculatePerimeter()); // Output: Rectangle Perimeter: 20

```

## Polymorphism

```ts
class Animal {
    sound(): void {
        console.log("Animal makes a sound");
    }
}

class Dog extends Animal {
    sound(): void {
        console.log("Dog barks");
    }
}

class Cat extends Animal {
    sound(): void {
        console.log("Cat meows");
    }
}

let animal: Animal;

animal = new Dog();
animal.sound(); // Output: Dog barks

animal = new Cat();
animal.sound(); // Output: Cat meows

```

## Inheritance

```ts
class Vehicle {
    protected speed: number;

    constructor(speed: number) {
        this.speed = speed;
    }

    accelerate(): void {
        console.log("Vehicle is accelerating at", this.speed, "km/h");
    }
}

class Car extends Vehicle {
    constructor(speed: number) {
        super(speed);
    }
}

let car = new Car(100);
car.accelerate(); // Output: Vehicle is accelerating at 100 km/h
```
```ts


 class Player {
   public readonly id: string;
   constructor(
     private height: number,
     public weight: number,
     protected power: number
   ) {
     this.id = String(Math.random() * 100);
   }

   get getMyHeight(): number {
     return this.height;
   }

   set changeHeight(val: number) {
     this.height = val;
   }
 }

 const abhi = new Player(100, 150, 23);

 console.log("Heigfht", abhi.getMyHeight);
 abhi.changeHeight = 500;
 console.log("Heigfht", abhi.getMyHeight);

 class Player2 extends Player {
   special: boolean;
   constructor(height: number, weight: number, power: number, special: boolean) {
     super(height, weight, power);
     this.special = special;
   }
   getMyPower = () => this.power;
 }

 const abhi = new Player2(100, 150, 23, true);
 console.log("Weight", abhi.weight);
 console.log("Height", abhi.getMyHeight());
 console.log("Power", abhi.getMyPower());

 interface ProductType {
   name: string;
   price: number;
   stock: number;
   offer?: boolean;
 }

 interface GiveId {
   getId: () => string;
 }

 class Product implements ProductType, GiveId {
   private id: string = String(Math.random() * 1000);
   constructor(
     public name: string,
     public price: number,
     public stock: number
   ) {}
   getId = () => this.id;
 }

 const product1 = new Product("Macbook", 2000, 20);

```
