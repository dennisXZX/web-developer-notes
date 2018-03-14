## Typescript

#### Shortcut for creating a class for model data

```ts
export class Hero {
  constructor(
    public id: number,
    public name: string) { }
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
  private _name: string;
  
  // the initial name is optional
  constructor(name?: string) {
    this._name = name;
  }
  
  // by default, any field and method is public
  speak() {
    console.log('My name is: ' + this._name);
  }
}

// create a Cat instance
// you do not need to specify Cat type in the instance creation, as it is implied
const fluffy = new Cat('Dennis');
fluffy.speak();
```

Because it is so common to declare a private variable in a class and then assign a value to it in the constructor, Typescript has a short-cut syntax for this.

```ts
class Cat {
  constructor(private _name: string, public _color?: string) {
    // other code
  }
}
```

#### Properties for private variable

```ts
private _x;

get x() {
  return this._x
}

set x(value) {
  if (value < 0) {
    throw new Error('value cannot be less than 0');
  }
  
  this._x = value;
}
```

Now you can get and set the private value x using `let x = point.X` and `point.X = 10`.

#### Define an interface

```ts
// you can define an optional value in an interface using '?' operator
interface Point {
  x: number,
  y: number,
  name?: string
}

// specify the parameter to be a Point object
let drawPoint = (point: Point) => {
  // ...
}
```

#### Typescript types

Typescript can assign types implicitly but it is recommeded to always assign types in an explicit way.

```ts
// string type
let myName: string = "Dennis";

// array type
let hobbies: string[] = ["Coding", "Reading"];

// tuple type
let address: [string, number] = ["Zetland", 906];

// enum can group similar values together
// Typescript automatically assign value 0 to the first element, each subsequent element gets an increased value
enum Color { Red = 0, Greed = 1, Blue = 2 };
let bgColor = Color.Red;

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

```ts
// type assertions
let message;
message = 'abc';
let endsWithC = (<string>message).endsWith('c');
```
