# Tuples

```ts
type NameAndAge = [string, number]

const person: NameAndAge = ['Bob', 42]
const [name, age] = person
```

# Types vs. Interfaces

> [!tip]
> Prefer interfaces when possible

- interfaces can have fields [[#Declaration merging|added to them after creation]], types can't
- interface names show up in error messages more consistently

# Allow only certain values in an array

```ts
type Fruit = ['apple', 'banana', 'orange'] as const
const fruit: Fruit = []

fruit.push('apple')
fruit.push('carrot') // error
```

# Type Manipulation

## Intersection types

```ts
type Foo = { a: number }
interface Bar { b: number }

// you can combine types (including object literals) & interfaces, and the result will be a type
type Baz = Foo & Bar & {
  c: number
}
```

- properties with the same name and different object types are merged

```ts
type Foo = { x: { y: number } }
type Bar = { x: { z: string } }

type Baz = Foo & Bar

const x: Baz = { x: { y: 123, z: 'abc' } }
```

## Extending interfaces

```ts
interface Foo { a: number }
interface Bar { b: number }

interface Baz extends Foo, Bar {
  c: number
}
```

- properties with the same name and different object types cause an error

```ts
interface Foo { x: { y: number } }
interface Bar { x: { z: string } }

interface Baz extends Foo, Bar // error: Named property 'x' of types 'Foo' and 'Bar' are not identical
```

## Declaration merging

used to add new properties to an existing interface

```ts
interface Book {
  title: string
  author: string
}

interface Book {
  year: number
}
```

### Add properties to `window`

```ts
// {filename}.d.ts
export {}

declare global {
	interface Window {
		foo: string
	}
}

```

## Combine types (intersection type)

```ts
interface Identity {
    id: number;
    name: string;
}

interface Contact {
    email: string;
    phone: string;
}

type Employee = Identity & Contact
```

## Omit keys from a type

```ts
type Cube = {
    length: number
    width: number
    depth: number
}

type Square = Omit<Cube, 'depth'>
type Line = Omit<Cube, 'width' | 'depth'>
```

## Get union type from property names (`keyof`)

```ts
interface Person {
    first_name: string
    last_name: string
    age: number
}

// 'first_name' | 'last_name' | 'age'
type PersonFields = keyof Person
```

## Union type from array values

```ts
const animals = ['cat', 'dog', 'mouse'] as const

// 'cat' | 'dog' | 'mouse'
type Animal = typeof animals[number]
// 'dog'
type Dog = typeof animals[1]
```

## Union type from property in array of objects

```ts
const animals = [
  { species: 'cat', name: 'Fluffy' },
  { species: 'dog', name: 'Fido' },
  { species: 'mouse', name: 'Trevor' }
] as const

// 'cat' | 'dog' | 'mouse'
type Animal = typeof animals[number]['species']
```

## Pick properties of `T`, omitting properties of `BaseModel`

```ts
type ModelFields<T> = Omit<T, keyof BaseModel>
```

## Pick all properties of type `Value` from `T`

```ts
type PickByType<T, Value> = {
  [P in keyof T as T[P] extends Value | undefined ? P : never]: T[P]
}
```

# Configuration

- `strict`: enables much stricter type checking

## TypeScript ESLint

- `no-floating-promises`: warns if you forget to await an async function
- `no-misused-promises`: warns if you use Promises in locations that don't make sense

# See also

- [[JavaScript cheat sheet]]
- [[Use TypeScript Mapped Types Like a Pro]]
