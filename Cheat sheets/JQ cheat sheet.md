# Flags

- `-f path`: read filter from `path`
- `--indent n`: change output indentation
- `-r`: output strings without quotes

# Examples

## Input JSON

```json
{
  "foo": {
    "bar": [
      { "name": "Apple", "color": "Red" },
      { "name": "Orange", "color": "Orange" },
      { "name": "Banana", "color": "Yellow" }
    ]
  }
}
```

## Get list of names

- `.[]` spreads an array

```jq
.foo.bar | .[].name
```
