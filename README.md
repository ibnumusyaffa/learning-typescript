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