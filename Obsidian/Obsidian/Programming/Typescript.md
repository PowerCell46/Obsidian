
## Type System

- Optional Function parameter
```typescript
function printPersonDetails(name: string, age?: number): string { 
// optional parameter

    return age ? `Hello, ${name} you are ${age} years old.` : `Hello, ${name}!`;

}

console.log(printPersonDetails("Peter", 20));

console.log(printPersonDetails("Peter"));
```

```typescript
const tuple:[string, number] = ["Peter", 20]; // tuple
```

## Generics and Interfaces

- Optional interface parameter
```typescript
interface UserDetails {

    firstName: 'Tsveti' | "Gosho" | "Kaloyan",

    lastName: string,

    email?: string, // Optional ?:

    tellDetails: () => void;

}
```

- Interface for a function
```typescript
interface NameTest { // Interface for a function

    (param1: number, param2: string): string

}

  

const getName: NameTest = (param1: number, param2: string): string => {

    return `Name: ${param2}, Number: ${param1}`;

};
```


## Namespaces and modules
- Initializing and calling a namespace
```typescript
namespace PersonUtils {

    interface Introduction {

        (): string

    }

  

    export interface PersonInterface {

        name: string,

        age: number,

        introduction: Introduction

    }

  

    export class Person implements PersonInterface {

        name: string;

        age: number;

  

        constructor(name: string, age: number) {

            this.age = age;

            this.name = name;

        }

  

        introduction() {

            return `${this.name} of ${this.age} years of age.`;

        }

    }

}

  
  

const meInstance:PersonUtils.PersonInterface = new PersonUtils.Person("Peter", 20);

  

console.log(meInstance.introduction());
```

- Using/Importing a namespace from a different file
```typescript
/// <reference path="./namespaces1.ts"/>
```
##### *coupling and cohesion*
