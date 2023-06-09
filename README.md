- [The Type System](#the-type-system)
  - [Kind of error](#kind-of-error)
    - [Syntax Error](#syntax-error)
    - [Type Error](#type-error)
  - [Assignability](#assignability)
  - [Type Annotations](#type-annotations)
    - [Unnecessary Type Annotations](#unnecessary-type-annotations)
  - [Type Shapes](#type-shapes)
- [Union And Literal](#union-and-literal)
  - [Union Types](#union-types)
    - [Declaring Union Type](#declaring-union-type)
    - [Union Properties](#union-properties)
  - [Narrowing](#narrowing)
    - [Assignment Narrowing](#assignment-narrowing)
    - [Conditional Checks](#conditional-checks)
    - [Typeof Checks](#typeof-checks)
  - [Literal](#literal)
  - [Strict Null Checking](#strict-null-checking)
    - [Truthiness Narrowing](#truthiness-narrowing)
    - [Variables Without Initial Values](#variables-without-initial-values)
  - [Type Alias](#type-alias)
    - [combining type alias](#combining-type-alias)
- [Object](#object)
  - [Object Types](#object-types)
    - [Declaring Object Types](#declaring-object-types)
    - [Aliased Object Types](#aliased-object-types)
  - [Structural Typing](#structural-typing)
    - [Usage Checking](#usage-checking)
    - [Excess Property Checking](#excess-property-checking)
    - [Nested Object Types](#nested-object-types)
    - [Optional Properties](#optional-properties)
  - [Unions of Object Types](#unions-of-object-types)
    - [Inferred Object-Type Unions](#inferred-object-type-unions)
    - [Explicit Object-Type Unions](#explicit-object-type-unions)
    - [Narrowing Object Types](#narrowing-object-types)
    - [Discriminated Unions](#discriminated-unions)
  - [Intersection Types](#intersection-types)
- [Function](#function)
  - [Function paramater](#function-paramater)
    - [Required Paramater](#required-paramater)
    - [Optional Paramater](#optional-paramater)
    - [Default Parameter](#default-parameter)
    - [Rest Parameters](#rest-parameters)
  - [Return Types](#return-types)
    - [Implicit Return Types](#implicit-return-types)
    - [Explicit Return Types](#explicit-return-types)
  - [Function Types](#function-types)
    - [Function Type Parentheses](#function-type-parentheses)
    - [Function Type Aliases](#function-type-aliases)
  - [More Return Types](#more-return-types)
    - [Void Return](#void-return)
    - [Never Returns](#never-returns)
    - [Function Overloading](#function-overloading)
- [Array](#array)
  - [Array Types](#array-types)
    - [Array and Function Types](#array-and-function-types)
    - [Union Type Array](#union-type-array)
    - [Evolving Any Array](#evolving-any-array)
    - [Multidimensional Array](#multidimensional-array)
  - [Spreads And Rests](#spreads-and-rests)
    - [Spreads](#spreads)
    - [Spreading Rest Parameters](#spreading-rest-parameters)
  - [Tuples](#tuples)
- [Interface](#interface)
  - [Types of Properties](#types-of-properties)
    - [Optional Properties](#optional-properties-1)
    - [Read-Only Properties](#read-only-properties)
    - [Functions and Methods](#functions-and-methods)
    - [Call Signatures](#call-signatures)
    - [Index Signatures](#index-signatures)
    - [Nested Interface](#nested-interface)
  - [Interface Extensions](#interface-extensions)
    - [Overridden Properties](#overridden-properties)
    - [Extending Multiple Interface](#extending-multiple-interface)
    - [Interface Merging](#interface-merging)
    - [Member Naming Conflicts](#member-naming-conflicts)
- [Classes](#classes)
- [Type Modifiers](#type-modifiers)
  - [Top Types](#top-types)
  - [Type Predicates](#type-predicates)
  - [Type Operators](#type-operators)
    - [keyof](#keyof)
    - [typeof](#typeof)
    - [keyof typeof](#keyof-typeof)
  - [Type Assertions](#type-assertions)
    - [Non-Null Assertions](#non-null-assertions)
  - [Const Assertions](#const-assertions)
    - [Literal to Primitive](#literal-to-primitive)
    - [Read-Only Objects](#read-only-objects)
- [Generic](#generic)
  - [Generic Functions](#generic-functions)
    - [Explicit Generic Call Types](#explicit-generic-call-types)
    - [Multiple Function Type Parameters](#multiple-function-type-parameters)
  - [Generic Interfaces](#generic-interfaces)
    - [Inferred Generic Interface Types](#inferred-generic-interface-types)
  - [Generic Classes](#generic-classes)
    - [Extending Generic Classes](#extending-generic-classes)
    - [Implementing Generic Interfaces](#implementing-generic-interfaces)
    - [Method Generics](#method-generics)
    - [Static Class Generics](#static-class-generics)
  - [Generic Type Aliases](#generic-type-aliases)
    - [Generic Discriminated Unions](#generic-discriminated-unions)
  - [Generic Modifiers](#generic-modifiers)
    - [Generic Defaults](#generic-defaults)
  - [Constrained Generic Types](#constrained-generic-types)
  - [Promises](#promises)
    - [Creating Promises](#creating-promises)
    - [Async Functions](#async-functions)
- [Type Operations](#type-operations)
  - [Mapped Types](#mapped-types)
    - [Mapped Types from Types](#mapped-types-from-types)
    - [Changing Modifiers](#changing-modifiers)
    - [Generic Mapped Types](#generic-mapped-types)
  - [Conditional Types](#conditional-types)
  - [Template Literal Types](#template-literal-types)
    - [Template Literal Keys](#template-literal-keys)

# The Type System

## Kind of error
### Syntax Error
```javascript
let let wat;
//      ~~~
// Error: ',' expected.
```
### Type Error
```typescript
console.blub("Nothing is worth more than laughter.");
//      ~~~~
// Error: Property 'blub' does not exist on type 'Console'.

```
## Assignability

TypeScript uses the initial value of a variable to determine its allowed type. If the variable is later assigned a new value, TypeScript checks if the new value's type matches the variable's type.

```ts
let firstName = "Carole"; // type : string
firstName = "Joan"; // Ok

let lastName = "King"; // type : string
lastName = true; // Error: Type 'boolean' is not assignable to type 'string'.
```
## Type Annotations
Without initial value It will consider the variable by default to be implicitly the any type
```typescript
let rocker; // Type: any
rocker = "Joan Jett"; // Type: string
rocker.toUpperCase(); // Ok
rocker = 19.58; // Type: number
rocker.toPrecision(1); // Ok
rocker.toUpperCase();
//     ~~~~~~~~~~~
// Error: 'toUpperCase' does not exist on type 'number'.
```
TypeScript provides a syntax for declaring the type of a variable without having to assign it an initial value, called a type annotation
```ts
let rocker: string;
rocker = "Joan Jett";

let rocker2: string;
rocker2 = 19.58;
// Error: Type 'number' is not assignable to type 'string'.
```
### Unnecessary Type Annotations
The following : string type annotation is redundant because TypeScript could already infer that firstName be of type string:
```ts
let firstName: string = "Tina";
// with initial value typescript can infer that lastname is type of string
let lastName = "Doe";
```

## Type Shapes

If you attempt to access a property of a variable, TypeScript will make sure that property is known to exist on that variable’s type.
```ts
let rapper = "Queen Latifah";
rapper.length; // ok

rapper.push('!');
//     ~~~~
// Property 'push' does not exist on type 'string'.
```
object works too
```ts


let cher = {
  firstName: "Cherilyn",
  lastName: "Sarkisian",
};

cher.middleName;
//   ~~~~~~~~~~
//   Property 'middleName' does not exist on type
```

# Union And Literal
## Union Types
```ts
let mathematician = Math.random() > 0.5 
    ? undefined
    : "Mark Goldberg"; // string | undefined
```
### Declaring Union Type
```ts
let thinker: string | null = null;

if (Math.random() > 0.5) {
    thinker = "Susanne Langer"; // Ok
}
```
### Union Properties


In the following snippet, physicist is of type number | string.
While .toString() exists in both types and is allowed to be used, .toUpperCase() and .toFixed() are not because .toUpperCase() is missing on the number type and .toFixed() is missing on the string type:
```ts
let physicist = Math.random() > 0.5
    ? "Marie Curie"
    : 84; //  number | string



physicist.toString(); // Ok, exist in both number type and string type

physicist.toUpperCase(); // only exist in string type
//        ~~~~~~~~~~~
// Error: Property 'toUpperCase' does not exist on type 'string | number'.
//   Property 'toUpperCase' does not exist on type 'number'.

physicist.toFixed(); // only exist in number type
//        ~~~~~~~
// Error: Property 'toFixed' does not exist on type 'string | number'.
//   Property 'toFixed' does not exist on type 'string'.

Restricting access to properties that don’t exist on all union type
```

## Narrowing
Narrowing in TypeScript refers to the process of narrowing down the type of a variable from a more general type to a more specific type based on certain conditions or checks performed in the code. This allows for more precise type checking and enables safer and more efficient code.
### Assignment Narrowing
```ts
let admiral: number | string;

admiral = "Grace Hopper";

admiral.toUpperCase(); // Ok: string

admiral.toFixed(); // Error
```
### Conditional Checks
```ts
// Type of scientist: number | string
let scientist = Math.random() > 0.5
    ? "Rosalind Franklin"
    : 51;

if (scientist === "Rosalind Franklin") {
    // typescript smart enough to know that scientist is string
    // Type of scientist: string
    scientist.toUpperCase(); // Ok
}

// Type of scientist: number | string 
scientist.toUpperCase(); // not ok, because toUpperCase only exist in string
//        ~~~~~~~~~~~
// Error: Property 'toUpperCase' does not exist on type 'string | number'.
//   Property 'toUpperCase' does not exist on type 'number'.
```
### Typeof Checks

```ts
let researcher = Math.random() > 0.5
    ? "Rosalind Franklin"
    : 51;

if (typeof researcher === "string") {
    researcher.toUpperCase(); // Ok: string
}
```
Logical negations from ! and else statements work as well:
```ts
if (!(typeof researcher === "string")) {
    researcher.toFixed(); // Ok: number
} else {
    researcher.toUpperCase(); // Ok: string
}
```

## Literal
```ts
const philosopher = "Hypatia";
```
What type is philosopher?

At first glance, you might say string—and you’d be correct.
philosopher is indeed a string.

But!
philosopher is not just any old string.
It’s specifically the value "Hypatia".
Therefore, the philosopher variable’s type is technically the more specific "Hypatia".

```ts
let lifespan: number | "ongoing" | "uncertain";

lifespan = 89; // Ok
lifespan = "ongoing"; // Ok

lifespan = true;
// Error: Type 'true' is not assignable to
// type 'number | "ongoing" | "uncertain"'
```

```ts
let specificallyAda: "Ada";

specificallyAda = "Ada"; // Ok

specificallyAda = "Byron";
// Error: Type '"Byron"' is not assignable to type '"Ada"'.

let someString = ""; // Type: string

specificallyAda = someString;
// Error: Type 'string' is not assignable to type '"Ada"'.
```

## Strict Null Checking


With the strictNullChecks option set to false, the following code is considered totally type safe.
That’s wrong, though; nameMaybe might be undefined when 
.toLowerCase is accessed from it:

```ts
let nameMaybe = Math.random() > 0.5
    ? "Tony Hoare"
    : undefined;

nameMaybe.toLowerCase();
//strictNullChecks: true -> Error: Object is possibly 'undefined'.
//strictNullChecks: false -> Ok, but has potential runtime error: Cannot read property 'toLowerCase' of undefined.
```
### Truthiness Narrowing

```ts
let geneticist = Math.random() > 0.5
    ? "Barbara McClintock"
    : undefined;

if (geneticist) {
    geneticist.toUpperCase(); // Ok: string
}

geneticist.toUpperCase();
// Error: Object is possibly 'undefined'.
```
### Variables Without Initial Values

Variables declared without an initial value default to undefined in JavaScript.

TypeScript is smart enough to understand that the variable is undefined until a value is assigned.
It will report a specialized error message if you try to use that variable
```ts
let mathematician: string;

mathematician?.length;
// Error: Variable 'mathematician' is used before being assigned.

mathematician = "Mark Goldberg";
mathematician.length; // Ok
```
## Type Alias

```ts
//without type alias
let rawDataFirst:  number | string;

//with type alias
type RawData = number | string //type alias declaration,  PascalCase
let rawDataFirst: RawData;
```
### combining type alias
```ts
type Id = number | string;

// Equivalent to: number | string | undefined | null
type IdMaybe = Id | undefined | null;
```

# Object
## Object Types
TypeScript understands that the following poet variable’s type is that of an object with two properties: born, of type number, and name, of type string.
Accessing those members would be allowed, but attempting to access any other member name would cause a type error for that name not existing:
```ts
const poet = {
    born: 1935,
    name: "Mary Oliver",
};

poet['born']; // Type: number
poet.name; // Type: string

poet.end;
//   ~~~
// Error: Property 'end' does not exist on
// type '{ born: number; name: string; }'.
```
### Declaring Object Types
```ts
let poetLater: {
    born: number;
    name: string;
};

// Ok
poetLater = {
    born: 1935,
    name: "Mary Oliver",
};

poetLater = "Sappho";
// Error: Type 'string' is not assignable to
// type '{ born: number; name: string; }'
```
### Aliased Object Types
```ts
type Poet = {
    born: number;
    name: string;
};

let poetLater: Poet;

// Ok
poetLater = {
    born: 1935,
    name: "Sara Teasdale",
};

poetLater = "Emily Dickinson";
// Error: Type 'string' is not assignable to 'Poet'.
```
## Structural Typing
TypeScript’s type system is structurally typed: meaning any value that happens to satisfy a type is allowed to be used as a value of that type.
```ts
type WithFirstName = {
  firstName: string;
};

type WithLastName = {
  lastName: string;
};

const hasBoth = {
  firstName: "Lucille",
  lastName: "Clifton",
};

// Ok: `hasBoth` contains a `firstName` property of type `string`
let withFirstName: WithFirstName = hasBoth;

// Ok: `hasBoth` contains a `lastName` property of type `string`
let withLastName: WithLastName = hasBoth;
```
### Usage Checking

```ts
type FirstAndLastNames = {
  first: string;
  last: string;
};

// Ok
const hasBoth: FirstAndLastNames = {
  first: "Sarojini",
  last: "Naidu",
};

const hasOnlyOne: FirstAndLastNames = {
  first: "Sappho"
};
// Property 'last' is missing in type '{ first: string; }'
// but required in type 'FirstAndLastNames'.
```

### Excess Property Checking
```ts
type Poet = {
    born: number;
    name: string;
}

// Ok: all fields match what's expected in Poet
const poetMatch: Poet = {
  born: 1928,
  name: "Maya Angelou"
};

const extraProperty: Poet = {
    activity: "walking",
    born: 1935,
    name: "Mary Oliver",
};
// Error: Type '{ activity: string; born: number; name: string; }'
// is not assignable to type 'Poet'.
//   Object literal may only specify known properties,
//   and 'activity' does not exist in type 'Poet'.
```


This extraPropertyButOk variable does not trigger a type error with the previous example’s Poet type because its initial value happens to structurally match Poet:
```ts
const existingObject = {
    activity: "walking",
    born: 1935,
    name: "Mary Oliver",
};

const extraPropertyButOk: Poet = existingObject; // Ok
```
### Nested Object Types
```ts
type Poem = {
    author: {
        firstName: string;
        lastName: string;
    };
    name: string;
};
```
It is generally a good idea to move nested object types into their own type name like this, both for more readable code and for more readable TypeScript error messages.
```ts
type Author = {
    firstName: string;
    lastName: string;
};

type Poem = {
    author: Author;
    name: string;
};
```
### Optional Properties

```ts
type Book = {
  author?: string;
  pages: number;
};

// Ok
const ok: Book = {
    author: "Rita Dove",
    pages: 80,
};

const missing: Book = {
    author: "Rita Dove",
};
// Error: Property 'pages' is missing in type
```
```ts
type Writers = {
  author: string | undefined;
  editor?: string;
};

// Ok: author is provided as undefined
const hasRequired: Writers = {
  author: undefined,
};

const missingRequired: Writers = {};
```
## Unions of Object Types
### Inferred Object-Type Unions
```ts
const poem = Math.random() > 0.5
  ? { name: "The Double Image", pages: 7 }
  : { name: "Her Kind", rhymes: true };
// Type:
// {
//   name: string;
//   pages: number;

//   rhymes?: undefined;
// }
// |
// {
//   name: string;
//   pages?: undefined;
//   rhymes: boolean;
// }

poem.name; // string
poem.pages; // number | undefined
poem.rhymes; // boolean | undefined
```
### Explicit Object-Type Unions
```ts
type PoemWithPages = {
    name: string;
    pages: number;
};

type PoemWithRhymes = {
    name: string;
    rhymes: boolean;
};

type Poem = PoemWithPages | PoemWithRhymes;

const poem: Poem = Math.random() > 0.5
  ? { name: "The Double Image", pages: 7 }
  : { name: "Her Kind", rhymes: true };

poem.name; // Ok

poem.pages;
//   ~~~~~
// Property 'pages' does not exist on type 'Poem'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.

poem.rhymes;
//   ~~~~~~
// Property 'rhymes' does not exist on type 'Poem'.
//   Property 'rhymes' does not exist on type 'PoemWithPages'.
```
### Narrowing Object Types
```ts
if ("pages" in poem) {
    poem.pages; // Ok: poem is narrowed to PoemWithPages
} else {
    poem.rhymes; // Ok: poem is narrowed to PoemWithRhymes
}
```
### Discriminated Unions
```ts
type PoemWithPages = {
    name: string;
    pages: number;
    type: 'pages';
};

type PoemWithRhymes = {
    name: string;
    rhymes: boolean;
    type: 'rhymes';
};

type Poem = PoemWithPages | PoemWithRhymes;

const poem: Poem = Math.random() > 0.5
  ? { name: "The Double Image", pages: 7, type: "pages" }
  : { name: "Her Kind", rhymes: true, type: "rhymes" };

if (poem.type === "pages") {
    console.log(`It's got pages: ${poem.pages}`); // Ok
} else {
    console.log(`It rhymes: ${poem.rhymes}`);
}

poem.type; // Type: 'pages' | 'rhymes'

poem.pages;
//   ~~~~~
// Error: Property 'pages' does not exist on type 'Poem'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.
```
## Intersection Types
```ts
type Artwork = {
    genre: string;
    name: string;
};

type Writing = {
    pages: number;
    name: string;
};

type WrittenArt = Artwork & Writing;
// Equivalent to:
// {
//   genre: string;
//   name: string;
//   pages: number;
// }
```
# Function

```ts
function sing(song: string) {
  console.log(`Singing: ${song}!`);
}
```
## Function paramater
### Required Paramater
```ts
function singTwo(first: string, second: string) {
  console.log(`${first} / ${second}`);
}

// Logs: "Ball and Chain / undefined"
singTwo("Ball and Chain");
//      ~~~~~~~~~~~~~~~~
// Error: Expected 2 arguments, but got 1.

// Logs: "I Will Survive / Higher Love"
singTwo("I Will Survive", "Higher Love"); // Ok

// Logs: "Go Your Own Way / The Chain"
singTwo("Go Your Own Way", "The Chain", "Dreams");
//                                      ~~~~~~~~
// Error: Expected 2 arguments, but got 3.
```
### Optional Paramater
```ts
function announceSong(song: string, singer?: string) {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
}

announceSong("Greensleeves"); // Ok
announceSong("Greensleeves", undefined); // Ok
announceSong("Chandelier", "Sia"); // Ok
```
```ts
function announceSongBy(song: string, singer: string | undefined) { /* ... */ }

announceSongBy("Greensleeves");
// Error: Expected 2 arguments, but got 1.

announceSongBy("Greensleeves", undefined); // Ok
announceSongBy("Chandelier", "Sia"); // Ok
```
### Default Parameter
```ts
function rateSong(song: string, rating = 0) {
  console.log(`${song} gets ${rating}/5 stars!`);
}

rateSong("Photograph"); // Ok
rateSong("Set Fire to the Rain", 5); // Ok
rateSong("Set Fire to the Rain", undefined); // Ok

rateSong("At Last!", "100");
```
### Rest Parameters
```ts
function singAllTheSongs(singer: string, ...songs: string[]) {
  for (const song of songs) {
    console.log(`${song}, by ${singer}`);
  }
}

singAllTheSongs("Alicia Keys"); // Ok
singAllTheSongs("Lady Gaga", "Bad Romance", "Just Dance", "Poker Face"); // Ok

singAllTheSongs("Ella Fitzgerald", 2000);
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```
## Return Types
### Implicit Return Types
```ts
// Type: (songs: string[]) => number
function singSongs(songs: string[]) {
  for (const song of songs) {
    console.log(`${song}`);
  }

  return songs.length;
}
```
### Explicit Return Types
```ts
// Type: (songs: string[]) => number

function singSongs(songs: string[]): number {
  return songs.length
}
```
Arrow function
```ts
const addNumbers = (a: number, b: number): number => {
  return a + b;
};
```

## Function Types
```ts
let nothingInGivesString: () => string;
let inputAndOutput: (songs: string[], count?: number) => number;
function runOnSongs(getSongAt: (index: number) => string) {}
```
### Function Type Parentheses
```ts
// Type is a function that returns a union: string | undefined
let returnsStringOrUndefined: () => string | undefined;

// Type is either undefined or a function that returns a string
let maybeReturnsString: (() => string) | undefined;
```
### Function Type Aliases
```ts
type StringToNumber = (input: string) => number;

let stringToNumber: StringToNumber;

stringToNumber = (input) => input.length; // Ok

stringToNumber = (input) => input.toUpperCase();
//                          ~~~~~~~~~~~~~~~~~~~
// Error: Type 'string' is not assignable to type 'number'.
```
## More Return Types
### Void Return
```ts
function logSong(song: string | undefined): void {
  if (!song) {
    return; // Ok
  }

  console.log(`${song}`);

  return true;
  // Error: Type 'boolean' is not assignable to type 'void'.
}
```
```ts
function returnsVoid() {
  return;
}

let lazyValue: string | undefined;

lazyValue = returnsVoid();
// Error: Type 'void' is not assignable to type 'string | undefined'.
```
### Never Returns

Never-returning functions are those that always throw an error or run an infinite loop
```ts
function fail(message: string): never {
    throw new Error(`Invariant failure: ${message}.`);
}

function workWithUnsafeParam(param: unknown) {
    if (typeof param !== "string") {
        fail(`param should be a string, not ${typeof param}`);
    }

    // Here, param is known to be type string
    param.toUpperCase(); // Ok
}
```
### Function Overloading
```ts
function createElement(tagName: 'input'): HTMLInputElement;
function createElement(tagName: 'img'): HTMLImageElement;
function createElement(tagName: string): HTMLElement {
  return document.createElement(tagName);
}
```
# Array
In this example, TypeScript knows the warriors array initially contains string typed values, so while adding more string typed values is allowed, adding any other type of data is not:
```ts
const warriors = ["Artemisia", "Boudica"];

// Ok: "Zenobia" is a string
warriors.push("Zenobia");

warriors.push(true);
//            ~~~~
// Argument of type 'boolean' is not assignable to parameter of type 'string'.
```
## Array Types
```ts
let arrayOfNumbers: number[];

arrayOfNumbers = [4, 8, 15, 16, 23, 42];
```
### Array and Function Types
```ts
// Type is a function that returns an array of strings
let createStrings: () => string[];

// Type is an array of functions that each return a string
let stringCreators: (() => string)[];
```
### Union Type Array
```ts
// Type is either a string or an array of numbers
let stringOrArrayOfNumbers: string | number[];

// Type is an array of elements that are each either a number or a string
let arrayOfStringOrNumbers: (string | number)[];
```
### Evolving Any Array
```ts
// Type: any[]
let values = [];

// Type: string[]
values.push('');

// Type: (number | string)[]
values[0] = 0;
```
### Multidimensional Array
```ts
let arrayOfArraysOfNumbers: number[][];

arrayOfArraysOfNumbers = [
  [1, 2, 3],
  [2, 4, 6],
  [3, 6, 9],
];
```

## Spreads And Rests
### Spreads
```ts
// Type: string[]
const soldiers = ["Harriet Tubman", "Joan of Arc", "Khutulun"];

// Type: number[]
const soldierAges = [90, 19, 45];

// Type: (string | number)[]
const conjoined = [...soldiers, ...soldierAges];
```
### Spreading Rest Parameters
```ts
function logWarriors(greeting: string, ...names: string[]) {
  for (const name of names) {
    console.log(`${greeting}, ${name}!`);
  }
}

const warriors = ["Cathay Williams", "Lozen", "Nzinga"];

logWarriors("Hello", ...warriors);

const birthYears = [1844, 1840, 1583];

logWarriors("Born in", ...birthYears);
//                     ~~~~~~~~~~~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```
## Tuples

Although JavaScript arrays may be any size in theory, it is sometimes useful to use an array of a fixed size—also known as a tuple.
Tuple arrays have a specific known type at each index that may be more specific than a union type of all possible members of the array.
The syntax to declare a tuple type looks like an array literal, but with types in place of element values.
```ts
let yearAndWarrior: [number, string];

yearAndWarrior = [530, "Tomyris"]; // Ok

yearAndWarrior = [false, "Tomyris"];
//                ~~~~~
// Error: Type 'boolean' is not assignable to type 'number'.

yearAndWarrior = [530];
// Error: Type '[number]' is not assignable to type '[number, string]'.
//   Source has 1 element(s) but target requires 2.
```
# Interface
```ts
interface Poet {
  born: number;
  name: string;
}

let valueLater: Poet;
// Ok
valueLater = {
  born: 1935,
  name: 'Sara Teasdale',
};


valueLater = "Emily Dickinson";
// Error: Type 'string' is not assignable to 'Poet'.

valueLater = {
  born: true,
  // Error: Type 'boolean' is not assignable to type 'number'.
  name: 'Sappho'
};
```
## Types of Properties
### Optional Properties
```ts
interface Book {
  author?: string;
  pages: number;
};
```
### Read-Only Properties
```ts
interface Page {
    readonly text: string;
}

function read(page: Page) {
    // Ok: reading the text property doesn't attempt to modify it
    console.log(page.text);

    page.text += "!";
    //   ~~~~
    // Error: Cannot assign to 'text'
    // because it is a read-only property.
}
```
### Functions and Methods
```ts
interface HasBothFunctionTypes {
  property: () => string;
  method(): string;
}

const hasBoth: HasBothFunctionTypes = {
  property: () => "",
  method() {
    return "";
  }
};

hasBoth.property(); // Ok
hasBoth.method(); // Ok


interface OptionalReadonlyFunctions {
  optionalProperty?: () => string;
  optionalMethod?(): string;
}
```
### Call Signatures
```ts
type FunctionAlias = (input: string) => number;

interface CallSignature {
  (input: string): number;
}

// Type: (input: string) => number
const typedFunctionAlias: FunctionAlias = (input) => input.length; // Ok

// Type: (input: string) => number
const typedCallSignature: CallSignature = (input) => input.length; // Ok
```
```ts
interface FunctionWithCount {
  count: number;
  (): void;
}

let hasCallCount: FunctionWithCount;

function keepsTrackOfCalls() {
  keepsTrackOfCalls.count += 1;
  console.log(`I've been called ${keepsTrackOfCalls.count} times!`);
}

keepsTrackOfCalls.count = 0;

hasCallCount = keepsTrackOfCalls; // Ok

function doesNotHaveCount() {
  console.log("No idea!");
}

hasCallCount = doesNotHaveCount;
// Error: Property 'count' is missing in type
// '() => void' but required in type 'FunctionWithCalls'
```
### Index Signatures
```ts
interface WordCounts {
  [i: string]: number;
}

const counts: WordCounts = {};

counts.apple = 0; // Ok
counts.banana = 1; // Ok

counts.cherry = false;
// Error: Type 'boolean' is not assignable to type 'number'.
```
```ts
interface HistoricalNovels {
  Oroonoko: number;
  [i: string]: number;
}

// Ok
const novels: HistoricalNovels = {
  Outlander: 1991,
  Oroonoko: 1688,
};

const missingOroonoko: HistoricalNovels = {
  Outlander: 1991,
};
// Error: Property 'Oroonoko' is missing in type
// '{ Outlander: number; }' but required in type 'HistoricalNovels'.
```
```ts
interface ChapterStarts {
  preface: 0;
  [i: string]: number;
}

const correctPreface: ChapterStarts = {
  preface: 0,
  night: 1,
  shopping: 5
};

const wrongPreface: ChapterStarts = {
  preface: 1,
  // Error: Type '1' is not assignable to type '0'.
};
```
```ts
// Ok
interface MoreNarrowNumbers {
  [i: number]: string;
  [i: string]: string | undefined;
}

// Ok
const mixesNumbersAndStrings: MoreNarrowNumbers = {
  0: '',
  key1: '',
  key2: undefined,
}

interface MoreNarrowStrings {
  [i: number]: string | undefined;
  // Error: 'number' index type 'string | undefined'
  // is not assignable to 'string' index type 'string'.
  [i: string]: string;
}
```
### Nested Interface
```ts
interface Novel {
    author: {
        name: string;
    };
    setting: Setting;
}

interface Setting {
    place: string;
    year: number;
}
```
## Interface Extensions
```ts
interface Writing {
    title: string;
}

interface Novella extends Writing {
    pages: number;
}

// Ok
let myNovella: Novella = {
    pages: 195,
    title: "Ethan Frome",
};

let missingPages: Novella = {
 // ~~~~~~~~~~~~
 // Error: Property 'pages' is missing in type
 // '{ title: string; }' but required in type 'Novella'.
    title: "The Awakening",
}

let extraProperty: Novella = {
 // ~~~~~~~~~~~~~
 // Error: Type '{ genre: string; name: string; strategy: string; }'
 // is not assignable to type 'Novella'.
 //   Object literal may only specify known properties,
 //   and 'strategy' does not exist in type 'Novella'.
    pages: 300,
    strategy: "baseline",
    style: "Naturalism"
};
```
### Overridden Properties
```ts
interface WithNullableName {
    name: string | null;
}

interface WithNonNullableName extends WithNullableName {
    name: string;
}

interface WithNumericName extends WithNullableName {
    name: number | string;
}
// Error: Interface 'WithNumericName' incorrectly
// extends interface 'WithNullableName'.
//   Types of property 'name' are incompatible.
//     Type 'string | number' is not assignable to type 'string | null'.
//       Type 'number' is not assignable to type 'string'.
```
### Extending Multiple Interface
```ts
interface GivesNumber {
  giveNumber(): number;
}

interface GivesString {
  giveString(): string;
}

interface GivesBothAndEither extends GivesNumber, GivesString {
  giveEither(): number | string;
}

function useGivesBoth(instance: GivesBothAndEither) {
  instance.giveEither(); // Type: number | string
  instance.giveNumber(); // Type: number
  instance.giveString(); // Type: string
}
```
### Interface Merging
```ts
interface Merged {
  fromFirst: string;
}

interface Merged {
  fromSecond: number;
}

// Equivalent to:
// interface Merged {
//   fromFirst: string;
//   fromSecond: number;
// }
```
### Member Naming Conflicts
```ts
interface MergedProperties {
  same: (input: boolean) => string;
  different: (input: string) => string;
}

interface MergedProperties {
  same: (input: boolean) => string; // Ok

  different: (input: number) => string;
  // Error: Subsequent property declarations must have the same type.
  // Property 'different' must be of type '(input: string) => string',
  // but here has type '(input: number) => string'.
}
```
# Classes
Soon
# Type Modifiers
## Top Types
```ts
function greetComedian(name: any) {
    // No type error
    // Possibly Runtime error: name.toUpperCase is not a function
    console.log(`Announcing ${name.toUpperCase()}!`);
}

function greetComedian(name: unknown) {
    console.log(`Announcing ${name.toUpperCase()}!`);
    //                        ~~~~
    // Error: Object is of type 'unknown'.
}

function myFunction(name: unknown) {
    if (typeof value === "string") {
        console.log(`Hello ${name.toUpperCase()}!`); // Ok
    } 
    console.log(name.toUpperCase())
}
```
## Type Predicates
```ts


function isNumberOrString(value: unknown): value is number | string {
    return ['number', 'string'].includes(typeof value);
}

function logValueIfExists(value: number | string | null | undefined) {
    if (isNumberOrString(value)) {
        // Type of value: number | string
        value.toString(); // Ok
    } else {
        console.log("value does not exist:", value);
    }
}
```
```ts
interface Comedian {
    funny: boolean;
}

interface StandupComedian extends Comedian {
    routine: string;
}

function isStandupComedian(value: Comedian): value is StandupComedian {
    return 'routine' in value;
}


function workWithComedian(value: Comedian) {
    if (isStandupComedian(value)) {
        // Type of value: StandupComedian
        console.log(value.routine); // Ok
    }

    // Type of value: Comedian
    console.log(value.routine);
    //                ~~~~~~~
    // Error: Property 'routine' does not exist on type 'Comedian'.
}
```
## Type Operators
### keyof
keyof only work on type not value
```ts
interface Ratings {
    audience: number;
    critics: number;
}

function getCountKeyof(ratings: Ratings, key: keyof Ratings): number {
    return ratings[key]; // Ok
}

const ratings: Ratings = { audience: 66, critic: 84 };

getCountKeyof(ratings, 'audience'); // Ok

getCountKeyof(ratings, 'not valid');
//                     ~~~~~~~~~~~
// Error: Argument of type '"not valid"' is not
// assignable to parameter of type 'keyof Ratings'

```
### typeof
typeof work on value
```ts
const original = {
    medium: "movie",
    title: "Mean Girls",
};

let adaptation: typeof original;

if (Math.random() > 0.5) {
    adaptation = { ...original, medium: "play" }; // Ok
} else {
    adaptation = { ...original, medium: 2 };
    //                          ~~~~~~
    // Error: Type 'number' is not assignable to type 'string'.
}
```
### keyof typeof
```ts
const ratings = {
    imdb: 8.4,
    metacritic: 82,
};

function logRating(key: keyof typeof ratings) {
    console.log(ratings[key]);
}

logRating("imdb"); // Ok

logRating("invalid");
//        ~~~~~~~~~
// Error: Argument of type '"missing"' is not assignable
// to parameter of type '"imdb" | "metacritic"'.
```
## Type Assertions
```ts
const rawData = `["grace", "frankie"]`;

// Type: any
JSON.parse(rawData);

// Type: string[]
JSON.parse(rawData) as string[];

// Type: [string, string]
JSON.parse(rawData) as [string, string];

// Type: ["grace", "frankie"]
JSON.parse(rawData) as ["grace", "frankie"]
```

### Non-Null Assertions

Instead of writing out as and the full type of whatever a value is excluding null and undefined, you can use a ! to signify the same thing.
In other words, the ! non-null assertion asserts that the type is not null or undefined.

```ts
// Inferred type: Date | undefined
let maybeDate = Math.random() > 0.5
    ? undefined
    : new Date();

// Asserted type: Date
maybeDate as Date;

// Asserted type: Date
maybeDate!;
```
```ts
const seasonCounts = new Map([
    ["I Love Lucy", 6],
    ["The Golden Girls", 7],
]);

// Type: string | undefined
const maybeValue = seasonCounts.get("I Love Lucy");

console.log(maybeValue.toUpperCase());
//          ~~~~~~~~~~
// Error: Object is possibly 'undefined'.

// Type: string
const knownValue = seasonCounts.get("I Love Lucy")!;
console.log(knownValue.toUpperCase()); // Ok
```
## Const Assertions
### Literal to Primitive
```ts
// Type: (number | string)[]
[0, ''];

// Type: readonly [0, '']
[0, ''] as const;



// Type: () => string
const getName = () => "Maria Bamford";

// Type: () => "Maria Bamford"
const getNameConst = () => "Maria Bamford" as const;

```
```ts


interface Joke {
    quote: string;
    style: "story" | "one-liner";
    
}

function tellJoke(joke: Joke) {
    if (joke.style === "one-liner") {
        console.log(joke.quote);
    } else {
        console.log(joke.quote.split("\n"));
    }
}

// Type: { quote: string; style: "one-liner" }
const narrowJoke = {
    quote: "If you stay alive for no other reason do it for spite.",
    style: "one-liner" as const,
};

tellJoke(narrowJoke); // Ok

// Type: { quote: string; style: string }
const wideObject = {
    quote: "Time flies when you are anxious!",
    style: "one-liner",
};

tellJoke(wideObject);
// Error: Argument of type '{ quote: string; style: string; }'
// is not assignable to parameter of type 'LogAction'.
//   Types of property 'style' are incompatible.
//     Type 'string' is not assignable to type '"story" | "one-liner"'.
```
### Read-Only Objects
```ts
function describePreference(preference: "maybe" | "no" | "yes") {
    switch (preference) {
        case "maybe":
            return "I suppose...";
        case "no":
            return "No thanks.";
        case "yes":
            return "Yes please!";
    }
}

// Type: { movie: string, standup: string }
const preferencesMutable = {
    movie: "maybe"
    standup: "yes",
};

describePreference(preferencesMutable.movie);
//                 ~~~~~~~~~~~~~~~~~~~~~~~~
// Error: Argument of type 'string' is not assignable
// to parameter of type '"maybe" | "no" | "yes"'.

preferencesMutable.movie = "no"; // Ok

// Type: readonly { readonly movie: "maybe", readonly standup: "yes" }
const preferencesReadonly = {
    movie: "maybe"
    standup: "yes",
} as const;


describePreference(preferencesReadonly.movie); // Ok

preferencesReadonly.movie = "no";
//                  ~~~~~
// Error: Cannot assign to 'movie' because it is a read-only property.
```
# Generic
## Generic Functions
```ts
function identity<T>(input: T) {
    return input;
}

const numeric = identity("me"); // Type: "me"
const stringy = identity(123); // Type: 123
```
### Explicit Generic Call Types
```ts
function logWrapper<T>(callback: (input: T) => void) {
    return (input: T) => {
        console.log("Input:", input);
        callback(input);
    };
}

// Type: (input: string) => void
logWrapper((input: string) => {
    console.log(input.length);
});

// Type: (input: unknown) => void
logWrapper((input) => {
    console.log(input.length);
    //                ~~~~~~
    // Error: Property 'length' does not exist on type 'unknown'.
});

// Type: (input: string) => void
logWrapper<string>((input) => {
    console.log(input.length);
});
```
### Multiple Function Type Parameters
```ts
function makePair<Key, Value>(key: Key, value: Value) {
    return { key, value };
}
makePair("abc", 123); // Type: { key: string; value: number }
```
## Generic Interfaces
```ts
interface Box<T> {
    inside: T;
}


let stringyBox: Box<string> = {
    inside: "abc",
};
```
### Inferred Generic Interface Types
```ts
interface LinkedNode<Value> {
    next?: LinkedNode<Value>;
    value: Value;
}

function getLast<Value>(node: LinkedNode<Value>): Value {
    return node.next ? getLast(node.next) : node.value;
}

// Inferred Value type argument: Date
let lastDate = getLast({
    value: new Date("09-13-1993"),
});

// Inferred Value type argument: string
let lastFruit = getLast({
    next: {
        value: "banana",
    },
    value: "apple",
});
```
## Generic Classes
```ts
class Secret<Key, Value> {
    key: Key;
    value: Value;

    constructor(key: Key, value: Value) {
        this.key = key;
        this.value = value;
    }

    getValue(key: Key): Value | undefined {
        return this.key === key
            ? this.value
            : undefined;
    }
}

const storage = new Secret(12345, "luggage"); // Type: Secret<number, string>

storage.getValue(1987); // Type: string | undefined
```
### Extending Generic Classes
```ts


class Quote<T> {
    lines: T;

    constructor(lines: T) {
        this.lines = lines;
    }
}

class SpokenQuote extends Quote<string[]> {
    speak() {
        console.log(this.lines.join("\n"));
    }
}

new Quote("The only real failure is the failure to try.").lines; // Type: string
new Quote([4, 8, 15, 16, 23, 42]).lines; // Type: number[]

new SpokenQuote([
    "Greed is so destructive.",
    "It destroys everything",
]).lines; // Type: string[]

new SpokenQuote([4, 8, 15, 16, 23, 42]);
//              ~~~~~~~~~~~~~~~~~~~~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```
### Implementing Generic Interfaces
```ts
interface ActingCredit<Role> {
    role: Role;
}

class MoviePart implements ActingCredit<string> {
    role: string;
    speaking: boolean;

    constructor(role: string, speaking: boolean) {
        this.role = role;
        this.speaking = speaking;
    }
}

const part = new MoviePart("Miranda Priestly", true);

part.role; // Type: string

class IncorrectExtension implements ActingCredit<string> {
    role: boolean;
    //    ~~~~~~~
    // Error: Property 'role' in type 'IncorrectExtension' is not
    // assignable to the same property in base type 'ActingCredit<string>'.
    //   Type 'boolean' is not assignable to type 'string'.
}
```
### Method Generics
```ts
class CreatePairFactory<Key> {
    key: Key;

    constructor(key: Key) {
        this.key = key;
    }

    createPair<Value>(value: Value) {
        return { key: this.key, value };
    }
}

// Type: CreatePairFactory<string>
const factory = new CreatePairFactory("role");

// Type: { key: string, value: number }
const numberPair = factory.createPair(10);

// Type: { key: string, value: string }
const stringPair = factory.createPair("Sophie");
```
### Static Class Generics
```ts
class BothLogger<OnInstance> {
    instanceLog(value: OnInstance) {
        console.log(value);
        return value;
    }

    static staticLog<OnStatic>(value: OnStatic) {
        let fromInstance: OnInstance;
        //                ~~~~~~~~~~
        // Error: Static members cannot reference class type arguments.

        console.log(value);
        return value;
    }
}

const logger = new BothLogger<number[]>;
logger.instanceLog([1, 2, 3]); // Type: number[]

// Inferred OnStatic type argument: boolean[]
BothLogger.staticLog([false, true]);

// Explicit OnStatic type argument: string
BothLogger.staticLog<string>("You can't change the music of your soul.");
```
## Generic Type Aliases
```ts
type Nullish<T> = T | null | undefined;


type CreatesValue<Input, Output> = (input: Input) => Output;

// Type: (input: string) => number
let creator: CreatesValue<string, number>;

creator = text => text.length; // Ok

creator = text => text.toUpperCase();
//                ~~~~~~~~~~~~~~~~~~
// Error: Type 'string' is not assignable to type 'number'.
```
### Generic Discriminated Unions
```ts
type Result<Data> = FailureResult | SuccessfulResult<Data>;

interface FailureResult {
    error: Error;
    succeeded: false;
}

interface SuccessfulResult<Data> {
    data: Data;
    succeeded: true;
}


function handleResult(result: Result<string>) {
    if (result.succeeded) {
        // Type of result: SuccessfulResult<string>
        console.log(`We did it! ${result.data}`);
    } else {
        // Type of result: FailureResult
        console.error(`Awww... ${result.error}`);
    }

    result.data;
    //     ~~~~
    // Error: Property 'data' does not exist on type 'Result<string>'.
    //   Property 'data' does not exist on type 'FailureResult'.
}
```
## Generic Modifiers
### Generic Defaults
```ts
interface Quote<T = string> {
    value: T;
}

let explicit: Quote<number> = { value: 123 };

let implicit: Quote = { value: "Be yourself. The world worships the original." };

let mismatch: Quote = { value: 123 };
//                                     ~~~
// Error: Type 'number' is not assignable to type 'string'.
```

## Constrained Generic Types
```ts
interface WithLength {
    length: number;
}

function logWithLength<T extends WithLength>(input: T) {
    console.log(`Length: ${input.length}`);
    return input;
}

logWithLength("No one can figure out your worth but you."); // Type: string
logWithLength([false, true]); // Type: boolean[]
logWithLength({ length: 123 }); // Type: { length: number }

logWithLength(new Date());
//            ~~~~~~~~~~
// Error: Argument of type 'Date' is not
// assignable to parameter of type 'WithLength'.
//   Property 'length' is missing in type
```
```ts
function get<T, Key extends keyof T>(container: T, key: Key) {
    return container[key];
}

const roles = {
    favorite: "Fargo",
    others: ["Almost Famous", "Burn After Reading", "Nomadland"],
};

const favorite = get(roles, "favorite"); // Type: string
const others = get(roles, "others"); // Type: string[]

const missing = get(roles, "extras");
```
```ts
function get<T>(container: T, key: keyof T) {
    return container[key];
}

const roles = {
    favorite: "Fargo",
    others: ["Almost Famous", "Burn After Reading", "Nomadland"],
};
const found = get(roles, "favorite"); // Type: string | string[]
```
## Promises
### Creating Promises
```ts
// Type: Promise<unknown>
const resolvesUnknown = new Promise((resolve) => {
    setTimeout(() => resolve("Done!"), 1000);
});

// Type: Promise<string>
const resolvesString = new Promise<string>((resolve) => {
    setTimeout(() => resolve("Done!"), 1000);
});
```
### Async Functions
```ts
// Type: (text: string) => Promise<number>
async function lengthImmediately(text: string) {
    return text.length;
}

// Ok
async function givesPromiseForString(): Promise<string> {
    return "Done!";
}

async function givesString(): string {
    //                        ~~~~~~
    // Error: The return type of an async function
    // or method must be the global Promise<T> type.
    return "Done!";
}
```
#  Type Operations
## Mapped Types
```ts
type Animals = "alligator" | "baboon" | "cat";

type AnimalCounts = {
    [K in Animals]: number;
};
// Equivalent to:
// {
//   alligator: number;
//   baboon: number;
//   cat: number;
// }
```
### Mapped Types from Types
```ts
interface AnimalVariants {
    alligator: boolean;
    baboon: number;
    cat: string;
}

type AnimalCounts = {
    [K in keyof AnimalVariants]: number;
};
// Equivalent to:
// {
//   alligator: number;
//   baboon: number;
//   cat: number;
// }
```
```ts
interface BirdVariants {
    dove: string;
    eagle: boolean;
}

type NullableBirdVariants = {
    [K in keyof BirdVariants]: BirdVariants[K] | null,
};
// Equivalent to:
// {
//   dove: string | null;
//   eagle: boolean | null;
// }
```
### Changing Modifiers
```ts
interface Environmentalist {
    area: string;
    name: string;
}

type ReadonlyEnvironmentalist = {
    readonly [K in keyof Environmentalist]: Environmentalist[K];
};
// Equivalent to:
// {
//   readonly area: string;
//   readonly name: string;
// }
```
```ts
interface Conservationist {
    name: string;
    catchphrase?: string;
    readonly born: number;
    readonly died?: number;
}

type WritableConservationist = {
    -readonly [K in keyof Conservationist]: Conservationist[K];
};

// Equivalent to:
// {
//   name: string;
//   catchphrase?: string;
//   born: number;
//   died?: number;
// }



type RequiredWritableConservationist = {
    [K in keyof WritableConservationist]-?: WritableConservationist[K];
};
// Equivalent to:
// {
//   name: string;
//   catchphrase: string;
//   born: number;
//   died: number;
// }
```
### Generic Mapped Types
```ts
type MakeReadonly<T> = {
    readonly [K in keyof T]: T[K];
}

interface Species {
    genus: string;
    name: string;
}

type ReadonlySpecies = MakeReadonly<Species>;
// Equivalent to:
// {
//   readonly genus: string;
//   readonly name: string;
// }
```
## Conditional Types
Soons
## Template Literal Types
```ts
type Greeting = `Hello${string}`;

let matches: Greeting = "Hello, world!"; // Ok

let outOfOrder: Greeting = "World! Hello!";
//  ~~~~~~~~~~
// Error: Type '"World! Hello!"' is not assignable to type '`Hello ${string}`'.

let missingAltogether: Greeting = "hi";
//  ~~~~~~~~~~~~~~~~~
// Error: Type '"hi"' is not assignable to type '`Hello ${string}`'.
```
```ts
type Brightness = "dark" | "light";
type Color =  "blue" | "red";

type BrightnessAndColor = `${Brightness}-${Color}`;
// Equivalent to: "dark-red" | "light-red" | "dark-blue" | "light-blue"

let colorOk: BrightnessAndColor = "dark-blue"; // Ok

let colorWrongStart: BrightnessAndColor = "medium-blue";
//  ~~~~~~~~~~~~~~~
// Error: Type '"medium-blue"' is not assignable to type
// '"dark-blue" | "dark-red" | "light-blue" | "light-red"'.
```
### Template Literal Keys
```ts
type DataKey = "location" | "name" | "year";

type ExistenceChecks = {
    [K in `check${Capitalize<DataKey>}`]: () => boolean;
};
// Equivalent to:
// {
//   checkLocation: () => boolean;
//   checkName: () => boolean;
//   checkYear: () => boolean;
// }

function checkExistence(checks: ExistenceChecks) {
    checks.checkLocation(); // Type: boolean
    checks.checkName(); // Type: boolean

    checks.checkWrong();
    //     ~~~~~~~~~~
    // Error: Property 'checkWrong' does not exist on type 'ExistenceChecks'.
}
```