---
aliases: Pull request checklist
---

- make sure there aren't any unwanted merges (ex. merging `sandbox` into a branch that will later go into `master`)
- **don't forget to use `await`!!!**
- debounce buttons to prevent duplicate actions if the user clicks rapidly
- variable/method/export naming should be clear & specific
    - add argument descriptions to JSDoc when not clear
    - prefer options objects over long argument lists
- failure states:
    - error handling
    - default to least access
    - handle missing env/config keys
- add `break` in switch statements
- use optional chaining (`?.`) where appropriate
- correct use of `this` in Vue
