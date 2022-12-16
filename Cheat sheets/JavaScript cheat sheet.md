> [!warning]
> Remember that `array.sort` and `array.reverse` mutate the original array!

# Examples

## Destructure nested objects

```js
const user = {
	first_name: 'Bob',
	address: {
		street: '123 Fake St',
		city: 'New York',
		state: 'NY'
	}
}

const {
	first_name,
	address: {
		street: address_street
	} = {} // avoid error if address is missing
} = user

console.log(address_street)
```

## Options objects with defaults

```js
// Sets defaults per-property

function test({ name = 'banana', color = 'yellow' } = {}) {
	console.log(`${name}s are ${color}`)
}

test({ name: 'apple', color: 'red' })  // apples are red
test({ name: 'lime' })                 // limes are yellow
test({})                               // bananas are yellow
test()                                 // bananas are yellow

// Sets defaults for the entire object
// In TypeScript this will error if you leave out properties 

function test2(options = { name: 'banana', color: 'yellow' }) {
	console.log(`${options.name}s are ${options.color}`)
}

test2({ name: 'apple', color: 'red' }) // apples are red
test2({ name: 'lime' })                // limes are undefined
test2({})                              // undefineds are undefined
test2()                                // bananas are yellow
```

## Measure timings in console

- `timeLog` and `timeEnd` both log elapsed time to the console (`timeLog` is optional)
- timer name must match between calls
    - prefix with something like `[timer]` to make it easier to search for

```js
function longRunningFunction() {
    const timerName = '[timer] longRunningFunction'
    console.time(timerName)
    doSomething()
    console.timeLog(timerName)
    doSomethingElse()
    console.timeEnd(timerName)
}
```

## String.replace Function

```js
string.replace(/%%(\w+)%%/, (match, p1, p2 /*...pN*/, offset, string, groups) => {
    // groups is an object of named capturing groups
    return replacement
})
```

## Generate random string

- 10-11 characters

```js
(Math.random() + 1).toString(36).substring(2)
```

## Copy text to clipboard

```js
await navigator.clipboard.writeText('text')
```

## Media queries

```js
const mediaQuery = window.matchMedia("(orientation: portrait)")
const isPortrait = mediaQuery.matches
mediaQuery.addEventListener('change', adjustLayout)
```

## Resize observer

```js
const observer = new ResizeObserver((entries) => {
    entries.forEach(entry => {
        console.log('width', entry.contentBoxSize[0].inlineSize)
        console.log('height', entry.contentBoxSize[0].blockSize)
    })
})
observer.observe(document.querySelector('main'))
```

## Drag and drop

- add `draggable="true"` attribute

```js
element.addEventListener('dragstart', (event) => {
    // 'copy'|'move'|'link'|'none'
    event.dataTransfer.dropEffect = 'copy'
    // dragImage can be any Element
    event.dataTransfer.setDragImage(dragImage, 0, 0)
})
```

# See also

- [[TypeScript cheat sheet]]
