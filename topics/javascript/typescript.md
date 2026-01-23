- [Install](#install)
- [Hello World](#hello-world)
- [Configuration](#configuration)
- [Types](#types)
- [Inferred types](#inferred-types)
- [Intellisense](#intellisense)
- [any](#any)
- [Arrays](#arrays)
- [Tuples](#tuples)
- [Enums](#enums)
- [Functions](#functions)
- [Objects](#objects)
- [Type Aliases](#type-aliases)
- [Optional type](#optional-type)
- [Union types](#union-types)
- [Intersection types](#intersection-types)
- [Literal types](#literal-types)
- [Nullable types](#nullable-types)
- [Generics](#generics)
- [Optional chaining](#optional-chaining)
- [Readonly](#readonly)
- [Type vs Interface](#type-vs-interface)

# Install

```bash
npm i -g typescript
```

```bash
# Check version
tsc -v
```

# Hello World

**index.ts**

```js
let age: number = 20;
```

Transpilation...

```bash
tsc index.ts
```

**index.js**

```js
var age = 20;
```

# Configuration

Creates a `tsconfig.json` settings file.

```bash
tsc --init
```

Setup the input and output locations inside `tsconfig.json`.

> **Use `"strict": true` to prevent ALL gotchas.**

```json
{
    "compilerOptions": {
        "rootDir": "./src",
        "outDir": "./dist",
        "strict": true
    }
}
```

Transpile all files in the project from `src` into `dist`.

```bash
tsc
```

The source map specifies how each line of the typescript code maps to the generated javascript code.

# Types

Javascript

- number
- string
- boolean
- null
- undefined
- object

Typescript

- any
- unknown
- never
- enum
- tuple

# Inferred types

```ts
let sales: number = 123_456_789;
let course: string = "TypeScript";
let isPublished: boolean = true;
```

This is the same as...

```ts
let sales = 123_456_789;
let course = "TypeScript";
let isPublished = true;
```

The types are inferred due to the initial values.

When there is no value, the type defaults to `any`.

```ts
let foo; // any
```

Type inference = TypeScript automatically figures out what type something is without you telling it.

Duck typing = TypeScript checks if an object has the right shape (properties/methods), not if it's explicitly declared as that type.

# Intellisense

When the type is known, the autocomplete will only show the methods for that type.

# any

Avoid using this type as much as possible, because it defeats the whole purpose.

You can prevent this in the `tsconfig.json` file via `"noImplicitAny": true`.

# Arrays

```ts
let numbers: number[] = [1, 2, 3];

let numbers; // any[]

let xArr: any[] = ["John", 1, true];
```

# Tuples

```ts
let numbers: [number, string] = [1, "John"];
```

Adding a third element will result in an error.

```ts
numbers.push(2);
```

This will work however...

# Enums

List of related constants.

```ts
// const small = 'S';
// const medium = 'M';
// const large = 'L';

enum Size {
    Small = "S",
    Medium = "M",
    Large = "L",
}

let mySize: Size = Size.Medium;

console.log(mySize); // M
```

# Functions

Always annotate the parameter and return types. Also, enable the 3 compiler options below.

```ts
//           parameter type ----|         |----- return type
//                              V         V
function calculateTax(income: number): number {
    if (income < 50_000) {
        return income * 1.2;
    }
    return income * 1.3;
}
```

Compiler options inside `tsconfig.json`:

- `"noUnusedParameters": true` - Prevent unused function parameters i.e. `income` must be used.
- `"noImplicitReturns": true` - Prevent `undefined` returns i.e. must always return a value.
- `"noUnusedLocals": true` - Prevent unused scoped variables like `let x`.

# Objects

```ts
let employee = { id: 1 };

employee.name = "Mosh"; // error
```

All object properties must be annotated...

```ts
let employee: { id: number; name: string } = { id: 1, name: "" };

employee.name = "Mosh"; // OK
```

This gets a bit messy...

```ts
let employee: { id: number; name: string; retire: (date: Date) => void } = {
    id: 1,
    name: "",
    retire: (date: Date) => {
        console.log(date);
    },
};
```

So we use type aliases to make it cleaner.

# Type Aliases

Makes `Employee` reusable, while making the code cleaner.

```ts
type Employee = { id: number; name: string; retire: (date: Date) => void };

let employee: Employee = {
    id: 1,
    name: "",
    retire: (date: Date) => {
        console.log(date);
    },
};

employee.retire(new Date());
```

Another way to handle functons (without arrow functions) is this...

```ts
type Employee = { id: number; name: string; retire(date: Date): void };

let employee: Employee = {
    id: 1,
    name: "",
    retire(date: Date) {
        console.log(date);
    },
};

employee.retire(new Date());
```

# Optional type

```ts
type User {
    id: number,
    name: string,
    age?: number // Age is not required i.e.  will not throw error if null.
}
```

# Union types

Assign more than one type with `|` i.e. one `OR` another.

```ts
function kgToLbs(weight: number | string): number {
    // Narrowing
    if (typeof weight == "number") {
        return weight * 2.2;
    } else {
        return parseInt(weight) * 2.2;
    }
}

kgToLbs(10);
kgToLbs("10kg");
```

# Intersection types

Combine types with `&` i.e. multiple types at the same time i.e. one `AND` another.

```ts
type Draggable = {
    drag: () => void;
};

type Resizable = {
    resize: () => void;
};

type UIWidget = Draggable & Resizable;

// To initialize this, you need to implement all members of both types.
let textBox: UIWidget = {
    drag: () => void,
    resize: () => void
}
```

Another example...

```ts
interface BusinessPartner {
    name: stirng;
    creditScore: number;
}

interface UserIdentity {
    id: number;
    email: string;
}

type Employee = BusinessPartner & UserIdentity;

function signContract(employee: Employee): void=>{
    console.log(`Signed by ${employee,name} with email: ${employee.email}`)
}

signContract({
    name:"John",
    creditScore: 800,
    id: 34,
    email: "john@gmail.com"
})
```

# Literal types

Limit the values that can be assigned to a variable.

```ts
let Quantity = 50 | 100;
let quantity: Quantity = 100;
```

# Nullable types

By default, typescript is very strict about using `null` and `undefined` values, as they are a common source of bugs.

`strictNullChecks: true` compiler option inside `tsconfig.json`.

```ts
function greet(name: string | null | undefined) {
    if (name) {
        console.log(name.toUpperCase());
    } else {
        console.log("Hola!");
    }
}

greet(undefined);
```

# Generics

Generics let you create reusable code that works with any type, while still keeping type safety.

Think of it like a placeholder for a type that gets filled in later.

```ts
//                     |--------Placeholder for different types that can be used
//                     V
class StorageContainer<T> {
    private contents: T[];

    constructor() {
        this.contents = [];
    }

    addItem(item: T): void {
        this.contents.push(item);
    }

    getItem(idx: number): T | undefined {
        return this.contents[idx];
    }
}

const usernames = new StorageContainer<string>();
usernames.addItem("john");
usernames.addItem("mike");
console.log(usernames.getItem(0)); // john

const fiendsCount = new StorageContainer<number>();
fiendsCount.addItem(42);
fiendsCount.addItem(1337);
console.log(fiendsCount.getItem(0)); // 42
```

Another example...

```ts
type ApiResponse<Data> = {
    data: Data;
    isError: boolean;
};

type UserResponse = ApiResponse<{ name: string; age: number }>;
type BlogResponse = ApiResponse<{ title: string }>;

const responseUser: UserResponse = {
    data: {
        name: "John",
        age: 28,
    },
    isError: false,
};

const responseBlog: BlogResponse = {
    data: {
        title: "Lorem ipsum",
    },
    isError: false,
};
```

# Optional chaining

When working with nullable objects, we often have to do null checks.

```ts
type Customer = {
    birthday: Date;
};

function getCustomer(id: number): Customer | null | undefined {
    return id === 0 ? null : { birthday: new Date() };
}

let customer = getCustomer(0);

// BAD
// if (customer !== null && customer !== undefined) {
//     console.log(customer.birthday);
// }

// BETTER
console.log(customer?.birthday);
```

# Readonly

```ts
interface Employee {
    readonly employeeId: number;
    readonly startDate: Date;

    name: string;
    department: string;
}

const employee: Employee = {
    employeeId: 42,
    startDate: new Date(),
    name: "John",
    department: "Finance",
};

employee.name = "Jessica"; // OK
employee.employeeId = 123456; // Error
```

# Type vs Interface

> **Interfaces are mostly used for objects.**

Both `interface` and `type` can be used to define object shapes, but they have some key differences:

**Main Differences**

**Extensibility**: Interfaces can be reopened and extended through declaration merging, while types cannot. If you declare the same interface twice, TypeScript merges them:

```typescript
interface User {
    name: string;
}

interface User {
    age: number;
}

// Result: User has both name and age
```

**What they can represent**: Types are more flexible and can represent primitives, unions, tuples, and more complex types:

```typescript
type ID = string | number; // Union
type Coordinates = [number, number]; // Tuple
type Callback = (data: string) => void; // Function
```

Interfaces are specifically for object shapes and can't represent these other types.

**Extending**: Both can extend, but with different syntax:

```typescript
// Interface extending
interface Animal {
    name: string;
}

interface Dog extends Animal {
    breed: string;
}

// Type extending (using intersection)
type Animal = {
    name: string;
};

type Dog = Animal & {
    breed: string;
};
```

**Computed properties**: Types handle computed property names more naturally, while interfaces require an index signature workaround.

**When to Use Which**

Use **interface** when:

- Defining object shapes, especially for classes or public APIs
- You want declaration merging capabilities
- Working with object-oriented patterns

Use **type** when:

- Creating unions, tuples, or primitives
- You need more complex type manipulations
- Working with utility types or mapped types

In practice, for simple object shapes, either works fine and it often comes down to team preference. Many codebases pick one as a default and stick with it for consistency.
