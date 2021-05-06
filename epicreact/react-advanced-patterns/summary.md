# Advanced React Patterns
The following patterns are brought up in the "Advanced React Patterns" course module at epicreact.dev, by Kent C. Dodds.  
Github repo: [https://github.com/kentcdodds/advanced-react-patterns](https://github.com/kentcdodds/advanced-react-patterns)

The course content with background information for each pattern, can be found here: [https://advanced-react-patterns.netlify.app/](https://advanced-react-patterns.netlify.app/)

## Context Module Functions

What does this even mean? Well, the idea with this pattern is that you **create an importable "helper"-function** that accepts **dispatch**.

### Pros

✅ Can help reduce duplication

✅ May have performance benefits → "You only pay for what you use, where you use it." - Dan Abramov ([https://twitter.com/dan_abramov/status/1125774170154065920](https://twitter.com/dan_abramov/status/1125774170154065920))

✅ Helps avoid mistakes in dependency lists

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM2MDk3MDddfQ==
-->