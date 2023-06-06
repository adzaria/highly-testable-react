# highly-testable-react

[WORK IN PROGRESS]

Let's be real. In the realm of software development there is a prevalent misconception that client code does not necessitate the rigorous testing 
or the sophisticated patterns customary in server-side development. For years, I myself perceived the client-side as just a mere extension of the server,
a simplistic piece that couldn't cause substantial problems. This is wrong, and I was wrong.

Your client will handle significant logic. It will handle concurrent operations. 
Your server won't serve a clean API. You will have 10 front-end developers pushing code faster than it can be reviewed. They will follow different standards and re-write logic they didn't know existed, 
introducing different sources of truth for the same feature. Hopefully, you will scale in terms of traffic and human resources.

Sooner or later, you will have to make a change. And it's only when you hit rock bottom 
that you will find the light: patterns exist for a reason, to solve universally encountered problems.

This repository illustrates how to implement these standards, emphasizing Dependency Injection, the Repository pattern, 
and strict rules enforcement. It offers an unapologetically opinionated view on these matters, that I follow as a 
developer and enforce as a team leader. These principles aim to cultivate a coding culture that prioritizes robustness, 
readability, and maintainability above all else.

The bottom line is simple:

- Something is not supposed to happen ? Make it impossible to happen.

- It works ? It didn't break ? Prove it.

## Strict code quality enforcement

### Pipeline

**The pipeline must be the only source of truth regarding what is acceptable and what is not.**

The pipeline is commonly used to enforce a strict set of rules. Use it. Any other rule-enforcement (including git hooks) 
can be bypassed. And if it can, it will.

### Linting

**The linter must only throw errors, and they must break the pipeline.**

Warnings are pretty useless. What is the value of a rule you allow developers to break ? It is not really a rule, is it ? It will end being just noise in the terminal. "Oh later we will fix it". Right... We've all been there, we will not fix it, it will grow out of hands.
We don't have time to fix them.

*NB: Warnings can be useful if you refactor a codebase and want to keep track of the progress. But they should be temporary. 
You can also keep them as warnings in local development, and let them break the pipeline, but it sounds pretty hypocrite. Why should a warning break a pipeline ?*

You can get some good rules from here:
- No unused variables
- No unused imports
- No commented code
- No console.log

If you want any of those to happen, you must be on drugs right now.

### Code formatting

**Do not force the rules with a pre-commit hook.**

Guess what, pre-commit hooks can be disabled pretty easily or even by accident. And they will be (oh yes I did that also).
For this reason, the industry prefers now to have rules enforced by the pipeline.

### Typescript

TypeScript is great*. if you have it, enforce all files to use it in the compiler rules.

(*) It is great but you know... It's just a slightly better JavaScript...

*NB: Again, in special scenario like migrating the codebase to typescript, having some JS files can be ok.*

## Architecture and Design Patterns

### Use functional components

They are just smaller, easier to read, to test, and became a de-facto standard.

### Keep a clear separation of concerns

A React application is layered just like a server is layered. The layers are:

- The presentation layer
- The business logic layer
- The data access layer
- The state management layer

They should be kept separate.

### Learn how to test each layer

Each layer has a different testing strategy. 

- The presentation layer is tested with unit tests and snapshot tests. 

- The business logic layer is tested with unit tests. 

- The data access layer is tested with unit tests and integration tests.

- The state management layer is tested with unit tests and integration tests.

### Use dependency Injection between the layers 💉

Your code must be testable and tested. Achieving this requires a clear understanding of the testing strategies.
One of the most classic and effective approach involves dependency injection:

- Inject dependencies to your business logic layer. This makes the business logic more modular, independent, and therefore more testable.
- Inject your business logic layer to your components. This allows components to be tested in isolation from the business logic, and vice versa.

Also understand you have multiple ways to test in React:
- Unit tests on the business logic
- Unit tests on the component
- Snapshot tests on the component
- Integration tests

Those are considered side effects that should be injected:
- API calls
- Redirects
- Access to local storage
- Access to cookies
- Access to the window object

### Follow the Repository pattern to inject dependencies in the business layer that needs them

Sometime (God forbids) people perform API calls directly from the state management library, 
or from the component itself. Both are wrong. As stated previously, dependencies must be injected to the business layer, 
and the business layer must be injected to the component. How do we achieve this ? By grouping dependencies in a repository that 
will be passed to the . It would be acceptable to perform data fetching from a hook in some very simple situations.

### Encapsulate Business Logic

We sometime put business logic in components. This is not correct. 
Business logic should not be all over the place. Business logic should be encapsulated in its own layer. A very simple operation 
like a filter could be happening in a component. We can consider using useMemo for it.

### Use pure functions

All functions called by the business logic layer must be pure. A pure function must not modify the state of the application. 
It must not perform API calls. It must not perform a redirect. It must not perform a side effect. 
It must not access its outer scope, even in read only.

### Keep the presentation layer out of business logic and hooks

Do not return React elements or components from the business layer or from the hooks.

### Keep Your React Components Dumb

Following the encapsulation of the business logic, the components should be as dumb as possible, and do as little as possible.

### Minimize libraries

Sorry guys. There is a library for everything, it does not mean we should use a library for everything. It introduces a risk in terms of security, maintainability, and can conflict with other dependencies.

### Do not 
