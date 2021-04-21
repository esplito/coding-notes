## Advanced React Patterns

### Context Module Functions

This is overkill, but it can in some situations help you reduce duplication, improve performance and help you avoid mistakes in the dependency lists.

>🦉 Tip: You may notice that the context provider/consumers in React DevTools just display as `Context.Provider` and `Context.Consumer`. That doesn’t do a good job differentiating itself from other contexts that may be in your app. Luckily, you can set the context `displayName` and it’ll display that name for the `Provider` and `Consumer`. Hopefully in the future this will happen automatically ([learn more](https://github.com/babel/babel/issues/11241)).

**Why do we even use this pattern?**
Dodds:
> We pretty much just moved something into another function. Why is this even a pattern?
> 
> **The benefit** of this pattern is that **we can take all of this stuff and put it into a separate function**, and then pass the things that are required. Each one of these is going to require the dispatch function.
> 
> **The benefit** of doing things this way is that **when we have multiple dispatches** that we're going to be calling, if we just leave that up to the user of our context, it's very **possible that they might miss a dispatch call**. It's better to pass that user dispatch to this context module function, so it can ensure that we're calling the dispatch in the right order.

### Compound Components



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcwOTI4NTMwNywxNTc1Njg4MDM1LC0xMj
U5OTQwMjQ2LDY0MjQ0MTg2MSwtNDIxMzIwNjJdfQ==
-->