## Typescript

#### Decorator

__class decorator__

- Decorator itself is a function
- Decorator function accepts a constructor as parameter
- Decorator function is executed when a class is defined, instead of when a new instance is created from the class
- You can apply multiple decorators to a class, the execution order of decorators is from right to left

```ts
function testDecorator(isEnabled: boolean) {
  if (isEnabled) {
    return function (constructor: any) {
      constructor.prototype.getName = () => {
        console.log("Dennis");
      };
    };
  } else {
    return function (constructor: any) {};
  }
}

@testDecorator(true)
class Test {}
```

#### Namespace

```ts
/** components.ts */

// create a namespace
namespace Components {
  export class Header {
    constructor() {
      const divElement = document.createElement("div");
      divElement.innerText = "Header";
      document.body.appendChild(divElement);
    }
  }

  export class Footer {
    constructor() {
      const divElement = document.createElement("div");
      divElement.innerText = "Footer";
      document.body.appendChild(divElement);
    }
  }
}
```

```ts
/** page.ts */

///<reference path="./components.ts">
namespace Home {
    export class Page {
        constructor() {
            new Components.Header();
            new Components.Footer();
        }
    }
}
```

#### Generic function

```ts
// define a function with generic type
function identity<T>(arg: T): T {
    return arg;
}

// call the generic function with a string type
let output = identity<string>("myString");  // type of output will be 'string'

// call the generic function with a number type
let output = identity<number>(123);  // type of output will be 'number'
```

```ts
// multiple generic types
function join<T, P>(first: T, second: P[]): string {
    return `${first} ${second}`
}

join<string, number>('1', [2, 3]);
```

__Generic constraint__

```ts
interface funcArgs {
  length: number;
}

function getLength<T extends funcArgs>(args: T) : number {
  return args.length;
}
```

We’ve created a generic constraint using an interface. Furthermore, we also extended our function getlength() with this interface. It now needs length as a required parameter.

#### Generic class

```ts
interface DataItem {
  name: string;
}

interface StudentData {
  name: string;
  age: number;
  dob: string;
}

// class DataManager<T extends number | string>, 
// this would limit the data type to be either number or string
class DataManager<T extends DataItem> {
  constructor(private data: T[]) {}

  getItem(index: number): string {
    return this.data[index].name;
  }
}

const mockStudentData = [
  {
    name: "dennis",
    age: 33,
    dob: "1971-01-01"
  }
];

const studentData = new DataManager<StudentData>(mockStudentData);
const studentName = studentData.getItem(0);
```

#### Shortcut for creating a class for model data

```ts
export class Hero {
  constructor(
    public id: number,
    public name: string
  ) { 
    ... code
  }
}
```

The above code is equivalent to

```ts
export class Hero {
  public id: number;
  public name: string;

  constructor(id: number, name: string) {
    this.number = number;
    this.name = name;
  }
}
```

Use the Hero class `new Hero(1, 'Windstorm')`.

#### 'as' operator

Recall how to write a type assertion:

```ts
const foo = <foo>bar;
```

Here we are asserting the variable bar to have the type foo. Since TypeScript also uses angle brackets for type assertions, JSX’s syntax introduces certain parsing difficulties. As a result, TypeScript disallows angle bracket type assertions in .tsx files.

To make up for this loss of functionality in `.tsx` files, a new type assertion operator has been added: `as`. The above example can easily be rewritten with the as operator.

```ts
const foo = bar as foo;
```

The `as` operator is available in both .ts and .tsx files, and is identical in behavior to the other type assertion style.

#### Compile Typescript to Javascript

```ts
// compile fileName.ts to fileName.js
tsc fileName.ts --target ES5

// compile Typescript file and execute the compiled Javascript file
tsc fileName.ts && node fileName.js
```

set up a tsconfig.json to monitor the whole Typescript project folder.

```ts
// create a default config file 
tsc --init

// now using command tsc will automatically look for any .ts files and convert them into .js ones
tsc

// use -w or --watch option to monitor any file changes
tsc -w
```

#### Define a class

```ts
class Cat {
  // define a private field
  private name: string;
  
  // the initial name is optional
  constructor(name?: string) {
    this.name = name;
  }
  
  // by default, any field and method is public
  speak(): void {
    console.log('My name is: ' + this.name);
  }
}

// create a Cat instance
// you do not need to specify Cat type in the instance creation, as it is implied
const fluffy: Cat = new Cat('Dennis');

fluffy.speak();
```

Because it is so common to declare a private variable in a class and then assign a value to it in the constructor, Typescript has a short-cut syntax for this.

```ts
class Cat {
  constructor(private name: string, public _color?: string) {
    // other code
  }
}
```

#### Properties for private variable

```ts
private x;

get x() {
  return this.x
}

set x(value) {
  if (value < 0) {
    throw new Error('value cannot be less than 0');
  }
  
  this.x = value;
}
```

Now you can get and set the private value x using `let x = point.X` and `point.X = 10`.

#### Define an interface

```ts
// you can define an optional value in an interface using '?' operator
interface Point {
  readonly x: number;  // read-only property
  readonly y: number;
  name?: string;  // optional property
  [propName: string]: any;  // could have any number of other properties
  
  getX(): number  // define a method
}

// specify the parameter to be a Point object
const drawPoint = (point: Point) => {
  ...code
}

// function interface
interface DoubleValueFunc {
  (num1: number, num2: number): number;
}

const myDoubleFunc: DoubleValueFunc = (num1: number, num2: number) => (num1 + num2) * 2;

// create a class that implements Point interface,
// so all the Point properties and methods need to be implemented in WeirdPoint
class WeirdPoint implements Point {
  ...code
}

// create an interface that extends Point interface
interface NewPoint extends Point {
  brand: string
}
```

#### Typescript types

Typescript can assign types implicitly (type inference) but it is recommeded to always assign types in an explicit way (type annotation).

```ts
// string type
let myName: string = "Dennis";

// array type
let hobbies: string[] = ["Coding", "Reading"];
let hobbies: Array<string> = ["Coding", "Reading"];
let strOrNumArr: (number | string)[] = [1, '2', 3];

// type alias
type Person = {
    name: string,
    age: number
}

let personArr: Person[] = [{ name: 'dennis', age: 18 }];

// object type
let wizard: object = {
  name: 'Dennis'
}

// tuple type
let address: [string, number, boolean] = ["Zetland", 906, true];

// enum can group similar values together
// Typescript automatically assign value 0 to the first element, each subsequent element gets an increased value
enum Color { Red = 0, Greed = 1, Blue = 2 };
let bgColorNum: number = Color.Red; // => 0
let bgColorStr: string = Color[0];  // => 'Red'

// union type
let id: string | number = 'TX-101'; // can be type string or number
let username: string | undefined; // can be type string or undefined

// any type
let car: any = "BMW"; // should refrain from using any type

// function returns a specific type
function getName(): string {
  return this.name;
}

// function that returns nothing
function sayHello(): void {
  console.log('Hello');
}

// function that would never return anything (always throws an exception, or a never ending while loop), because it would not be finished executing
function throwError(): never {
  throw Error('Couldn't connect to server');
}

// specify arguments type
function multiply(x: number, y: number): number {
  return x * y;
}

// declare a variable that accepts a specific type of function
let myFunc: (val1: number, val2: number) => number;

// params destructuring
function add({ first, second }: { first: number; second: number }): number {
  return first + second;
}

// type alias
type UserData = { 
  name: string, 
  age: number
}

let userData: UserData = {
  name: 'dennis',
  age: 32
}
```

A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data.

```ts
// type assertions example 1
let message;
message = 'abc';

// use angle-bracket syntax to cast type
let endsWithC = (<string>message).endsWith('c');

// type assertions example 2
interface Army {
  type: string,
  magic: string
}

let dog = {} as Army; // treat dog as a type of Army
dog.magic; // so we can access the magic property no matter what
```

#### Declaration Files

Declaration file is used to give type support to code originally written in Javascript. You can find declaration files for older libraries at [DefinitelyTyped](https://definitelytyped.org).
