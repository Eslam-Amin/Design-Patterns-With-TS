# **Design Patterns With TypeScript**

## Table of Contents

### Creational Patterns

- [1. Singleton Pattern](#1--singleton-pattern)
- [2. Factory Pattern](#2--factory-pattern)
- [3. Abstract Factory Pattern](#3--abstract-factory-pattern)
- [4. Builder Pattern](#4--builder-pattern)
- [5. Prototype Pattern](#5--prototype-pattern)

---

## **Creational Patterns**

Creational design patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.

---

### **1. Singleton Pattern**

📖 **Description**
Ensures a class has only one instance and provides a global access point to it.

**Pros**

- Saves memory by reusing the same instance.
- Useful for managing shared resources (config, logging, DB connections).

**Cons**
Harder to test due to global state.

- Can create hidden dependencies.

**Code**

```ts
class Singleton {
  private static instance: Singleton;
  private name: string;

  private constructor() {
    this.name = "I am a Singleton instance!";
  }

  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  public getData(): string {
    return this.name;
  }

  public setData(newName: string): void {
    this.name = newName;
  }
}

const instance1 = Singleton.getInstance();
const instance2 = Singleton.getInstance();

instance1.setData("It's the same");

console.log(instance2.getData()); // "It's the same"
```

---

### **2. Factory Pattern**

📖 **Description**
Provides an interface for creating objects, letting subclasses decide which class to instantiate.

**Pros**

- Simplifies object creation logic.
- Increases flexibility by decoupling instantiation from implementation.

**Cons**

- Debugging can be harder due to abstraction.

**Code**

```ts
interface Vehicle {
  type: string;
}

class Car implements Vehicle {
  type = "Car";
}

class Truck implements Vehicle {
  type = "Truck";
}

class VehicleFactory {
  public static createVehicle(vehicleType: string): Vehicle {
    switch (vehicleType) {
      case "car":
        return new Car();
      case "truck":
        return new Truck();
      default:
        throw new Error("Unknown vehicle type");
    }
  }
}

const myCar = VehicleFactory.createVehicle("car");
console.log(myCar.type); // "Car"
```

---

### **3. Abstract Factory Pattern**

📖 **Description**
Provides an interface for creating families of related objects without specifying concrete classes.

**Pros**

- Encapsulates object creation logic in one place.
- Makes it easier to scale and extend families of products.

**Cons**

- Can result in many factory classes.

**Code**

```ts
interface Vehicle {
  create(): string;
}

class Car implements Vehicle {
  create(): string {
    return "Car created";
  }
}

class Truck implements Vehicle {
  create(): string {
    return "Truck created";
  }
}

// Factory
class LandVehicleFactory {
  public static createVehicle(vehicleType: string): Vehicle {
    switch (vehicleType) {
      case "car":
        return new Car();
      case "truck":
        return new Truck();
      default:
        throw new Error("Unknown land vehicle type");
    }
  }
}

// Abstract Factory
class VehicleFactory {
  public static getFactory(type: string) {
    switch (type) {
      case "land":
        return LandVehicleFactory;
      default:
        throw new Error("Unknown factory type");
    }
  }
}

const landFactory = VehicleFactory.getFactory("land");
const myCar = landFactory.createVehicle("car");

console.log(myCar.create()); // "Car created"
```

---

### **4. Builder Pattern**

📖 **Description**
Separates object construction from its representation.

**Pros**

- Improves readability when constructing complex objects.
- Provides step-by-step customization.

**Cons**

- Adds unnecessary complexity for simple objects.

**Code**

```ts
class Car {
  engine: string | null = null;
  wheels: number | null = null;
}

class CarBuilder {
  private car: Car;

  constructor() {
    this.car = new Car();
  }

  public addEngine(engine: string): CarBuilder {
    this.car.engine = engine;
    return this;
  }

  public addWheels(wheels: number): CarBuilder {
    this.car.wheels = wheels;
    return this;
  }

  public build(): Car {
    return this.car;
  }
}

const car = new CarBuilder().addEngine("V8").addWheels(4).build();

console.log(car); // { engine: "V8", wheels: 4 }
```

---

### **5. Prototype Pattern**

📖 **Description**
Creates new objects by cloning existing ones (a prototype).

**Pros**

- Faster object creation (no constructor calls).
- Saves memory by sharing structure.

**Cons**
Objects may accidentally share mutable properties.

- Debugging prototype chains can be tricky.

**Code**

```ts
interface VehiclePrototype {
  type: string;
  getType(): string;
}

const vehiclePrototype: VehiclePrototype = {
  type: "Vehicle",
  getType() {
    return this.type;
  }
};

const car = Object.create(vehiclePrototype);
car.type = "Car";

console.log(car.getType()); // "Car"
console.log(vehiclePrototype.getType()); // "Vehicle"
```

---
