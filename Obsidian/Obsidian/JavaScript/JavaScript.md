![[Pasted image 20250108004253.png]]

![[Pasted image 20250108004634.png]]

![[Pasted image 20250107152401.png]]

![[Pasted image 20250107153056.png]]


![[Pasted image 20250107153140.png]]

```javascript
function sayHello() {

    /*

        let a = true;

        let b = true;

        if (a ^ b)

            console.log("One is true");

        else if (a && b)

            console.log("Both are true");

        else if (a || b)

            console.log("One or both are true");

        else

            console.log("Both are true");

    */

    /*

     let str = "Hello";

     // str[1] = "a";

     console.log(str);

     */

  

    // let fuckedUpArr = [1, str, {name: "Peter", age: 21}, [1, 2, 3], sayHello];

  

    // const sumTwoNums = function (a, b) { return a + b; }

    // console.log(sumTwoNums(5, 10));

    // const greet = (name) => console.log(`Hello there, ${name}!`);

    // greet("Peter");

  

    /*

    const sayHi = function() {

        console.log(this.name + " says Hi, with age " + this.age + "!");

    }

  

    const cat1 = {name: "Meowstic", age: 6 };

    const cat2 = {name: "Shadow", age: 2 };

  

    sayHi.call(cat1);

    sayHi.call(cat2);

  
  

    for (const [key, value] of Object.entries(cat1))

        console.log(key, value);

    */

  

    // const arr = [1, 2, 3];

    // arr[-5] = 10;

  

    // console.log(arr[-5]);

  

    // const sliced = arr.slice(0, 2);

  

    // for (const num of sliced)

    //     console.log(num);

  

    /*

    const numbers = [1, 2, 3, 4, 5];

  

    let middle = numbers.splice(2, 3);

  

    console.log("Original array: ", numbers);

    console.log("Spliced array: ", middle);

    */

  

    // const obj = {name: "Peter", age: 21, city: "Sofia", isProgrammer: true, class: "First class"};

  

    // for (const [key, value] of Object.entries(obj))

    //     console.log(`${key} -> ${value}`);

  

    // console.log(obj.hasOwnProperty("name"));

  

    // delete obj["name"];

    // const foods = {pizza: 2, apples: 10, bananas: 12, cakes: 3, proteinShakes: 11};

    // const orderedFoods = Object.entries(foods).sort(([key1, val1], [key2, val2]) => val2 - val1);

  

    // for (const [key, value] of orderedFoods)

    //     console.log(`${key} -> ${value}`);

  

    // const nums = [1, 2, 3, 4, 5];

    // let [first, second] = nums;

  
  

    // const person = {name: "Peter", age: 21};

  

    // const {name, age} = person;

  

    // const numbers = new Set([1, 1, 2, 2, 3, 3]);

    // numbers.add(5);

  

    // console.log(numbers.has(4));

  

    // const word = "Hello";

    // console.log(word.lastIndexOf('l'));

    // console.log('*'.repeat(10));

  

    // console.log("  hello    ".trim());

    // console.log("Hello there, General Kenobi".startsWith("He"));

    // console.log("Hello there, General Kenobi".endsWith("Peter"));

}

sayHello();
```

```javascript
function main(variable = 5) { // default value of a function parameter

    let nums = [1, 2, 3, 4, 5];

    const [first, second, ...others] = nums;

  

    nums.splice(1, 0, 1.5);

  

    nums = nums.concat([6, 7, 8, 9, 10]);

  

    nums

        .forEach(num => console.log(num));

  

    console.log(nums.some(num => num % 2 == 0)); // tests whether at least one element in the array passes the test

  

    console.log(nums.reduce((acc, curr) => acc += curr, 0));

}

  

main(10);
```


```javascript
function main() {

  

    function greet(person) { // Composing Objects with Behavior

        return this.fullName() + " says 'Hello there!'";

    }

  

    function canAge(person) { // decorator function

        person.birthday = function() {

            this.age++;

        }

    }

  

    const person = {

        firstName: "Peter",

        lastName: "Gerdzhikov",

        age: 21,

        fullName: function() {

            return this.firstName + ' ' + this.lastName;

        },

        // fullName: () => firstName + ' ' + lastName // arrow functions cannot access this

        greet

    }

  

    const secondPerson = {

        firstName: "Ivan",

        lastName: "Gerdzhikov",

        age: 17,

        greet

    }

  

    secondPerson.fullName = person.fullName;

  

    canAge(person);

    person.birthday();

  

    console.log(person.age);    

}

  

main();
```