# Type Narrowing in TypeScript

When I started learning TypeScript, I learned that sometimes a variable can have more than one possible type. For example, a variable can be a `string`, `number`, or even `null`.

But TypeScript does not always know which type we are working with at a particular moment. So, we need to check the type before using its specific properties or methods.

This process is called **Type Narrowing**.

In this blog, I will explain some common ways of narrowing types in TypeScript.

## 1. Type Narrowing with `typeof`

We can use the `typeof` operator to check the type of a value. It is useful for primitive types like `string`, `number`, and `boolean`.

**Example:**

```ts
const printValue = (value: string | number) => {
    if (typeof value === 'string') {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
};

printValue('hello');
printValue(25);
```

Here, TypeScript checks the type of `value`.

- If the value is a string, it uses `toUpperCase()`.
- Otherwise, it treats the value as a number and uses `toFixed()`.

## 2. Type Narrowing with Equality Checking

We can also narrow types by comparing two values using equality operators like `===` or `!==`.

**Example:**

```ts
const compare = (
    x: number | string,
    y: number | boolean
) => {
    if (x === y) {
        console.log(x, y);
    }
};
```

Here, `x` can be a number or string, and `y` can be a number or boolean.

When we check `x === y`, TypeScript knows that both values must have a compatible common type. In this example, the common type is `number`.

## 3. Type Narrowing with `in` Operator

The `in` operator checks whether a particular property exists in an object.

It is useful when we have a union of different object types.

**Example:**

```ts
type Bird = {
    fly: () => void;
};

type Fish = {
    swim: () => void;
};

const move = (animal: Bird | Fish) => {
    if ('fly' in animal) {
        animal.fly();
        return;
    }

    animal.swim();
};
```

Here, `animal` can be either a `Bird` or a `Fish`.

- If the `fly` property exists, TypeScript understands that the animal is a `Bird`.
- Otherwise, it understands that the animal is a `Fish`.

So, we can safely call the correct method.

## 4. Type Narrowing with `instanceof`

The `instanceof` operator checks whether an object belongs to a particular class or constructor.

It is commonly used with built-in objects like `Date` and `Error`.

**Example:**

```ts
const showDate = (x: Date | string) => {
    if (x instanceof Date) {
        console.log(x.toISOString());
        return;
    }

    const date = new Date(x);
    console.log(date.toISOString());
};

showDate('12-16-1971');
```

Here, `x` can be either a `Date` object or a string.

- If `x` is a `Date` object, we can directly use `toISOString()`.
- Otherwise, we first convert the string into a `Date` object.

## 5. Type Narrowing with Discriminated Unions

Discriminated Union is another useful way to narrow down union types.

For this, we use a common property with literal types in different interfaces. Usually, this property is named `kind` or `type`.

**Example:**

```ts
interface Circle {
    kind: 'circle';
    radius: number;
}

interface Square {
    kind: 'square';
    sideLength: number;
}

const getArea = (shape: Circle | Square) => {
    if (shape.kind === 'circle') {
        console.log(shape.radius);
    } else {
        console.log(shape.sideLength);
    }
};
```

Here, both `Circle` and `Square` have a common property called `kind`.

- If `shape.kind` is `'circle'`, TypeScript understands that the shape is a `Circle`.
- Otherwise, it understands that the shape is a `Square`.

That's why we can access `radius` for a circle and `sideLength` for a square without getting a type error.

## 6. A Small Caveat of Type Guard

Sometimes, we need to be careful while checking types, especially when a value can be `null` or an array.

**Example:**

```ts
const printAll = (x: string | string[] | null) => {
    if (x !== null) {
        if (typeof x === 'object') {
            for (let str of x) {
                console.log(str);
            }
            return;
        }

        console.log(x);
        return;
    }

    console.log('');
};

printAll('asd');
```

Here, `x` can be a string, an array of strings, or `null`.

- First, we check whether `x` is not `null`.
- Then, we check whether its type is an object. In JavaScript, arrays are also objects.
- If it is an array, we use a `for...of` loop to print each string.
- Otherwise, we print the string directly.

This example helps us understand why checking `null` and arrays carefully is important when using type guards.

## Conclusion

After learning Type Narrowing, I understood that it helps TypeScript identify the specific type of a variable inside different conditions.

Some common ways of narrowing types are:

- `typeof` – checks primitive value types.
- Equality checking – compares values and narrows their possible types.
- `in` – checks whether a property exists in an object.
- `instanceof` – checks whether an object belongs to a particular class or constructor.
- Discriminated Union – uses a common literal property to identify a specific object type.

Type Narrowing makes our TypeScript code safer and helps us avoid accessing properties or methods that do not belong to a particular type.