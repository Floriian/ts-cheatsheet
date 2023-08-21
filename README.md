<h1 style="text-align: center">Typescript Cheatsheet<h1>

### Define type in Typescript

In TypeScript, you can define types in five ways: "interface", "type", "class", or using two discouraged methods: defining custom types within curly brackets or using the "<{typedefinition}>" syntax.

## Variables

You can infer a variable's type by placing the type after the variable name.

The following syntax is this:

```
const variableName: TYPE = value; // Use const or let based on your needs
```

```typescript
let job: string = "Software Engineer";
```

If you attempt to assign a number to job, it will result in an error:

```typescript
job = 1; // Type 'number' is not assignable to type 'string'.ts(2322)
```

Variable type declaration is not always required; the TypeScript compiler can often infer the type.

<span style="color:red">IMPORTANT</span>:

When you define a constant, its type is not just a string or any other type; the constant's type is determined by the constant's value.

Example:

![Const type](/assets/const_type.png)

## Built-in types

TypeScript provides several built-in types, with some of the most important ones being:

```typescript
string; //For a string type
number; //For number, floats etc.
boolean; //For boolean
Date; //For dates
Array<T>; //For arrays. Theese are generic, see generics below.
Record<string, valueType>; //For objects. DONT USE 'Object' type!!
Promise<T>; //For promise (wow)
```

## Defining an array

You can define array by two way.

1:

```typescript
const arrayOfStrings: string[] = ["NestJS", "Express", "Moleculer"];
```

2:

```typescript
const arrayOfStrings: Array<string> = ["NestJS", "Express", "Moleculer"];
```

For multi-dimensional arrays:

1:

```typescript
const 2DArray: string[][] = [['NestJS'], ['Express'], ['Moleculer']]
const 3DArray: string[][][] = [...];
```

2:

```typescript
const 2DArray: Array<string[]> = [['NestJS'], ['Express'], ['Moleculer']];
//Or:
const 3DArray: Array<string[][]> = [...];
```

Yo can also wrap Array with Array type

```typescript
const 2DArray: Array<Array<string>> = [['NestJS'], ['Express'], ['Moleculer']];
const 3DArray: Array<Array<Array<string>>> = [...];
```

## Interfaces

Typescript interfaces are similar to Java/C# interfaces. An interface can describe what methods should be present in a class. Interfaces can extends or implements each other, similar to classes.

The syntax for declaring an interface is:

```typescript
interface INTERFACE_NAME {...}
```

Examples for a class.

```typescript
interface IAnimal {
  age: number;
  //other properties
}
```

```typescript
interface ICat extends IAnimal {
  meow: () => void; // Meow's return type is void.
  //other properties
}
```

```typescript
class Animal implements IAnimal {
  public age: number = 0; // Value initialization is required!
}
```

```typescript
class Cat extends Animal implements ICat {
  public meow() {
    console.log("meooooooooooooooooooooooow");
  }
}
```

Interfaces can also be used to declare object structures:

```typescript
interface ICat {
    age: number;
    meow: () => void;
}

const cat: ICat {
    age: 2,
    meow: () => {
        console.log("MEOOOOOOOOOOOOW");
    }
};
```

## Types

Types are similar to interfaces. I tend to use types for unions (read more below) or to define simple objects. However, I find that interfaces are more powerful for development. With a "type", you can't fully leverage OOP techniques, and creating generics is easier with interfaces.

The syntax for declaring a "type" is:

```typescript
type YOUR_TYPE_NAME = {...}
```

Example:

```typescript
type MoleculerJS = {
  author: string;
  version: number;
};

const moleculer: MoleculerJS = {
  author: "icebob",
  version: 1.0, //The number type accepts floats.
};
```

## Destructuring

You can destructure types, just like objects:

```typescript
interface Props {
  something: string;
  value: number;
}

function someFunction(props: Props) {
  return props;
}

const a = someFunction({ something: "asd", value: 1 });
// or
const { something, value } = someFunction({ something: "asd", value: 1 });
```

## Return types

In TypeScript, you can specify the expected return type of a function:

Examples:
<br/>
Non-Async

```typescript
function sayHello(name: string): string {
  return `Hello ${name}!`;
}
```

For async functions, you need to wrap the return type with the 'Promise' built-in type:

```typescript
type Response = {
  message: string;
};

async function getData(): Promise<Response> {
  return await axios<Response>("...");
}

const { message } = await getData();
console.log({ message }); //The message.
```

For 'const' based functions:

```typescript
const getData: Promise<Response> = () => {
  return await axios<Response>("...");
};
```

If you want to access other properties, you should type them explicitly to avoid errors.

## Generics

TypeScript generics enable you to create reusable functions. Generics are usually denoted with the 'T' character.

Example for reusable types:

```typescript
interface IResponse<T> {
  statusCode: number;
  data: T;
}

type Animal = {
  name: string;
  age: number;
};

type AnimalResponse = IResponse<Animal>;
type AnimalResponseWithArray = IResponse<Animal[]>;

function objectExample(): IResponse<Animal> {
  //Or AnimalResponse
  return {
    statusCode: 200,
    data: {
      name: "Meow",
      age: 2,
    },
  };
}

function arrayExample(): IResponse<Animal[]> {
  //Or IResponse<Array<Animal>>, AnimalResponseWithArray.
  return {
    statusCode: 200,
    data: [
      {
        name: "Kitty",
        age: 2,
      },
      {
        name: "Ciu",
        age: 1,
      },
    ],
  };
}
```

But you can create a reusable typed function

```typescript
function createArray<T>(item: T, length: number): T[] {
  const array: T[] = [];
  for (let i = 0; i < length; i++) {
    array.push(item);
  }
  return array;
}
const stringArray = createArray<string>("hello", 5);
```

## Unions

Unions combine multiple types. They mostly created with the 'type' keyword.

```typescript
type A = "A";
type B = "B";

type AB = A | B; // "A" | "B"
```

## Making constant truly constant

In JavaScript, you can mutate the properties of an object declared as a constant:

```typescript
const something = {
  a: "B",
  b: "A",
};

something.a = "C"; // No error is thrown
```

But if you add 'as const' keyword to end of the object, it will makes all values to readonly.

```typescript
const something = {
  a: "B",
  b: "A",
} as const;

something.a = "C"; // Error is thrown
```

![as const](/assets/as_const.png)

## The 'as' keyword.

The as keyword is a Type Assertion in TypeScript which tells the compiler to consider the object as another type than the type the compiler infers the object to be. (https://stackoverflow.com/questions/55781559/what-does-the-as-keyword-do)

## Rules of Typescript development:

<ul>
    <li>Avoid using ENUMS. Here's why

[![](https://markdown-videos-api.jorgenkh.no/youtube/jjMbPt_H3RQ)](https://youtu.be/jjMbPt_H3RQ)

</li>
    <li>
    Never use the 'any' keyword; it negates TypeScript's benefits.
    </li>
    <li>Always use the project's TypeScript version; check the workspace TypeScript version in VSCode.</li>
</ul>

## Useful extensions for VSCode

<ul>
    <li><a href='https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens'>Error Lens</a></li>
    <li><a href='https://marketplace.visualstudio.com/items?itemName=yoavbls.pretty-ts-errors'>Pretty TS Error</a></li>
    <li><a href='https://marketplace.visualstudio.com/items?itemName=meganrogge.template-string-converter'>Template string converter</a></li>
</ul>

## Useful links

<ul>
    <li><a href='https://www.youtube.com/@mattpocockuk'>Matt Pocock's Channel</a></li>
    <li><a href='https://www.totaltypescript.com/tips'>Total Typescript</a></li>
    <li><a href='https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html'>Typescript in 5 minutes</a></li>
    <li><a href='https://www.typescriptlang.org/docs/handbook/intro.html'>Typescript Handbook</a></li>
</ul>

## Useful things

Sometimes, VSCode's Typescript server is broken, here is a custom keyboard shortcut, to restart the TS server.

```json
{
  "key": "alt alt",
  "command": "typescript.restartTsServer"
}
```

### Get values from an object:

```typescript
type ObjectValue<T> = T[keyof T];
```

### Typed fetch function:

```typescript
export function publicFetch<T>(url: string): Promise<T> {
  return fetch(`${url}`)
    .then((r) => {
      if (!r.ok) {
        throw new Error(r.statusText);
      }

      return r.json() as Promise<{ data: T }>;
    })

    .then((data) => {
      return data.data;
    });
}
```

### <a href='https://zod.dev'>Zod</a> - Validation library with full support for Typescript.
