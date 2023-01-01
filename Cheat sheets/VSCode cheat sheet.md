---
aliases: Visual Studio Code cheat sheet
---

- Use `#region` comments to fold code regions in many languages

# Keyboard Shortcuts

> [!attention]
> Shortcuts in **bold** aren't default and might require extensions

| Shortcut   | Action                                    |
| ---------- | ----------------------------------------- |
| ⌘G         | Find next                                 |
| ⌃⌥←/→      | Move between word parts (add ⇧ to select) |
| ⇧⌘\\       | Jump to matching bracket                  |
| ⇧⌥\\       | **Jump to matching HTML tag**             |
| ⌘K ⌘Q      | Jump to last edit position                |
| ⌘U         | Undo cursor move                          |
| ⌘D         | Select next occurrence of selected text   |
| ⇧⌘L        | Select all occurrences of selected text   |
| ⌃⇧⌘←/→     | Contract/expand current selection         |
| ⌘K F       | **Focus selection**                       |
| ⌘K ⌘F      | Format selection                          |
| ⌘K ⌘L      | Toggle folding                            |
| ⌘K ⌘,      | Create manual folding range               |
| ⌘K ⌘.      | Remove manual folding ranges              |
| ⇧⌘Enter    | Insert line above                         |
| ⌘Enter     | Insert line below                         |
| ⇧⌘K        | Delete current line                       |
| ⌃⌥Bksp/Del | Backspace/delete word parts               |

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

- add globals to `globals.d.ts` somewhere in the project
