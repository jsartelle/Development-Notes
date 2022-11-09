# Configuration

- `strict`: enables much stricter type checking

## TypeScript ESLint

- `no-floating-promises`: warns if you forget to await an async function
- `no-misused-promises`: warns if you use Promises in locations that don't make sense

# Types vs. Interfaces

## Extending types

```ts
type Foo = {
  a: number
}

type Bar = {
  b: number
}

type Baz = Foo & Bar & {
  c: number
}
```

## Extending interfaces

```ts
interface Foo {
  a: number
}

interface Bar {
  b: number
}

interface Baz extends Foo, Bar {
  c: number
}
```

# Examples

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

## Pick properties of `T`, omitting properties of `BaseModel`

```ts
type ModelFields<T> = Omit<T, keyof BaseModel>
```

## Get union type from array values

```ts
const animals = ['cat', 'dog', 'mouse'] as const

// 'cat' | 'dog' | 'mouse'
type Animal = typeof animals[number]
// 'dog'
type Dog = typeof animals[1]
```

## Get union type from property in array of objects

```ts
const animals = [
  { species: 'cat', name: 'Fluffy' },
  { species: 'dog', name: 'Fido' },
  { species: 'mouse', name: 'Trevor' }
] as const

// 'cat' | 'dog' | 'mouse'
type Animal = typeof animals[number]['species']
```

## Pick all properties of type `Value` from `T`

```ts
type PickByType<T, Value> = {
  [P in keyof T as T[P] extends Value | undefined ? P : never]: T[P]
}
```

# See also

- [[JavaScript cheat sheet]]
- [[Use TypeScript Mapped Types Like a Pro]]
