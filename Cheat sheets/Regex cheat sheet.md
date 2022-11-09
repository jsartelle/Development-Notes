---
aliases: Regular expressions cheat sheet
---

# Tokens

|         |                     |
| ------- | ------------------- |
| (?:...) | non-capturing group |

## Lookarounds

|          |                     |
| -------- | ------------------- |
| (?=...)  | positive lookahead  |
| (?!...)  | negative lookahead  |
| (?<=...) | positive lookbehind |
| (?<!...) | negative lookbehind |

## Substitutions

|     |                             |
| --- | --------------------------- |
| $1  | contents of capture group 1 |
| $&  | contents of match           |
| $\` | contents before match       |
| $'  | contents after match        |

### Change Case

> [!warning]
> These work in VSCode, but not in JavaScript!

|     |                    |
| --- | ------------------ |
| \\U | start uppercasing  |
| \\L | start lowercasing  |
| \\E | stop changing case |

ex. Title case Markdown headers:

```regex
Find:
(#\s*.)(.+)$

Replace:
\U$1\L$2
```

# JavaScript

- `RegExp.test(str)` - returns true/false
- `String.search(regexp)` - returns match index (-1 if not found)
- `RegExp.exec(str)` - returns [match, ...captureGroups]
    - array has extra properties: `index`, `input`, `groups` (named capturing groups), `indices` (start/end indices of each capture group, only if `d` flag is used)
- `String.match(regexp)`
    - if `g` flag is **not** used, same as `RegExp.exec`
    - if `g` flag is used, returns array of all matches
- `String.matchAll(regexp)` - returns iterator of `RegExp.exec` results
    - access using `[...str.matchAll(regexp)]`
