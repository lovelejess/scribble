---
title: angular & typescript constructors
date: 2018-01-13 11:45:10
---

## Constructors in TypeScript
- when constructing objects in typescript, you can use a similar java-esque convention by **creating the properties on the object and then intializing them in the constructor**, like below: 


  ```
  export class Ingredient {
    public name: string;
    public amount: number;

      constructor(name: string, amount: number) {
        this.name = name;
        this.amount = amount;
      }
  }

  ```
-  or you can **prefix the property names in the constructor with `public` or `private`** and it automatically creates the properties for you! yay for less code!

    ```
    export class Ingredient {
        constructor(public name: string, public amount: number) {}
    }
    ```