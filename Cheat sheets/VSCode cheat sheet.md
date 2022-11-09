---
aliases: Visual Studio Code cheat sheet
---

- Use `#region` comments to fold code regions in many languages

# Keyboard Shortcuts

> [!attention]
> Some of these aren't default!

| Shortcut | Action                                  |
| -------- | --------------------------------------- |
| ⌘G       | Find next                               |
| ⌘D       | Select next occurrence of selected text |
| ⇧⌘L      | Select all occurrences of selected text |
| ⌘U       | Undo cursor move                        |
| ⌘K ⌘Q    | Move to last edit position              |
| ⌃⇧⌘←/→   | Contract/expand current selection       |
| ⌘K ⌘L    | Toggle folding                          |
| ⌘K ⌘,    | Create manual folding range             |
| ⌘K ⌘.    | Remove manual folding ranges            |
| ⇧⌘K      | Delete current line                     |
| ⌘ Enter  | Insert line below                       |
| ⌘K F     | Focus selection                         |

# Type Checking

- Add `// @ts-check` comment to top of JS file for type checking
- Type annotations: `/** @type {number} */`
- To type check an entire JS project, create a `jsconfig.json` file in the root with the following:

```json
{
    "compilerOptions": {
        "checkJs": true
    }
}
```

- and add globals to `globals.d.ts` somewhere in the project
