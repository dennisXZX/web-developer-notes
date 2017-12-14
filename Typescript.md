## Typescript

#### Compile Typescript to Javascript

compile a Typescript file

```
tsc fileName.ts
```

set up a tsconfig.json to monitor the whole Typescript project folder.

```
// create a default config file 
tsc --init

// now using command tsc will automatically look for any .ts files and convert them into .js ones
tsc

// use -w or --watch option to monitor any file changes
tsc -w
```

#### Define a class

```
class Cat {
  // define a private property
  private _name: string;
  
  constructor(name) {
    this._name = name;
  }
  
  // by default, any property and method is public
  speak() {
    console.log('My name is: ' + this._name);
  }
}

// create a Cat instance
const fluffy = new Cat('Dennis');
fluffy.speak();
```

Because it is so common to declare a private variable in a class and then assign a value to it in the constructor, Typescript has a short-cut syntax for this.

```
class Cat {
  constructor(private _name, private _color) {
  
  }
}
```

#### Define an interface

```
// you can define an optional value in an interface using '?' operator
interface ICat {
  name: string;
  age?: number;
}
```

#### Define a specific type

Typescript can assign types implicitly but it is recommeded to always assign types in an explicit way.

```
// string type
let myName: string = "Dennis";

// array type
let hobbies: string[] = ["Coding", "Reading"];

// tuple type
let address: [string, number] = ["Zetland", 906];

// enum type
enum Color {
  Gray,
  Green,
  Blue
}

let myColor: Color = Color.Green;

// any type
let car: any = "BMW";

// function returns a specific type
function getName(): string {
  return this.name;
}

// function returns nothing
function sayHello(): void {
  console.log('Hello');
}

// specify arguments type
function multiply(x: number, y: number): number {
  return x * y;
}

// declare a variable that accepts a specific type of function
let myFunc: (val1: number, val2: number) => number;
```

