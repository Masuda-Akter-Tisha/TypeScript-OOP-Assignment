# Type Alias vs Interface in TypeScript

When I started learning TypeScript, I found that both `type` and `interface` are used to define types. At first, I was confused about when to use `type` and when to use `interface`. After learning about them, I understood their basic differences.

## 1. What is a Type Alias?

A Type Alias allows us to create a new name for a type. We can use it for primitive types, union types, tuples, and object types.

**Example 1: Primitive Type and Union Type**

```ts
type Id = number;

type Role = 'admin' | 'manager';
```

Here, `Id` represents a number, and `Role` can be either `'admin'` or `'manager'`.

**Example 2: Using Type Alias with an Object**

```ts
type Id = number;

interface User {
    name: string;
    id: Id;
    role: 'admin' | 'member';
}

const user: User = {
    name: 'Tisha',
    id: 40498,
    role: 'member'
};
```

We can also use a type alias to define an object structure.

## 2. What is an Interface?

An Interface is mainly used to describe the structure of an object. It defines which properties an object should have and their types.

**Example: Using `extends`**

```ts
interface Animal {
    species: string;
}

interface Cat extends Animal {
    breed: 'Felix Catus';
}

const cat: Cat = {
    breed: 'Felix Catus',
    species: 'Persian'
};

console.log(cat.species);
```

Here, `Cat` extends `Animal`. That means `Cat` gets the `species` property from `Animal` and also has its own `breed` property.

## 3. Using Intersection (`&`) with Type Alias

We can combine multiple object types using the intersection (`&`) operator.

```ts
type Vehicle = {
    brand: string;
};

type Car = Vehicle & {
    model: string;
};

const car: Car = {
    model: 'Corolla',
    brand: 'Toyota'
};
```

Here, `Car` gets the `brand` property from `Vehicle` and adds a new property called `model`.

## 4. Declaration Merging in Interface

One important advantage of an Interface is that we can declare the same interface more than once. TypeScript automatically combines their properties. This is called Declaration Merging.

```ts
interface Iemployee {
    name: string;
}

interface Iemployee {
    id: string;
}

const employee: Iemployee = {
    name: 'Abdul',
    id: 'asdf45'
};
```

Here, both declarations of `Iemployee` are combined. So, the final interface contains both `name` and `id`.

**Note:** We cannot declare the same type alias more than once in the same scope. It will cause a duplicate identifier error.

## Conclusion

After learning these concepts, I understood that both `type` and `interface` are useful in TypeScript.

- We can use `type` for primitive types, union types, tuples, and object types.
- We commonly use `interface` to describe object structures.
- We can use `extends` with interfaces and `&` with type aliases to combine object structures.
- Interfaces support Declaration Merging, but type aliases do not.

Both are important. We should choose one based on what we need in our project.