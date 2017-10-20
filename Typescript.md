## Typescript

#### Compile Typescript to Javascript

compile one file
```
tsc fileName.ts
```
set up a tsconfig.json to monitor a Typescript project folder
```
tsc --init

// now using command tsc will automatically look for any .ts files and convert them into .js ones
tsc

// use -w or --watch option to monitor any file changes
tsc -w
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

