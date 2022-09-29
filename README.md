# Programming best practices (according to me)

This documents my approach to programming and software engineering more generally.

I do not pretend that this contains anything novel, nor that it reflects the general consensus of the dev community. Everything is contextual, and so every idiom may be inappropriate for a given application, language, team composition, and timeline.

If you see something you disagree with or would like to add, feel free to open an issue or PR and I will consider adding it!

## Contents
[Code Patterns](#patterns)

[Code Documentation](#documentation)

[Readme Documentation](#readme)

[(Automated) Tests](#tests)

[Development Pipeline](#pipeline)

[Support](#support)

[Dependencies](#dependencies)

<a name="patterns"></a>
## Code Patterns

**Private first.** Aim to keep every function, every variable, every interface as minimally exposed as possible. When you feel the need to expose a function, variable, or interface, first ask yourself if it is general enough that it may need to be used in many places, or if there's a more narrowly-defined construct that can be placed in a shared location. Minimally exposed code reduces the amount of scrubbing through options when writing new code throughout the codebase, allows you to define simpler interfaces, and reduces the chances of using the wrong code for a problem.

**Write interfaces from the perspective of the user.** When you make a new function, interface, or variable, consider how it will appear to a caller/user who has no idea of its internals. This means minimizing required inputs to functions, narrowly-defining the scope, using clear names, and documenting usage and behavior.

**Group files with their users, not their peers.** Think of every directory like a miniature library. What do you put in a library? A set of interfaces, functions, and variables that all work together to achieve a well-defined purpose. This is not a hard rule, since following it strictly would sometimes lead to code duplication or a strange dependency tree.

**Be internally consistent and externally agnostic.** Data retrieved from remote sources, the sources themselves, and the method for retrieving those data may all change. The internals of your software shouldnt suffer, leading you to large refactors. Instead, remote API calls should be as isolated as possible into their own clients, and data fetched from those locations should be transformed into the format you want internally, as soon as possible. This decouples your data from external APIs and saves on refactoring.

**for readability in PRs and outsiders not using an IDE: declare types on the left side always, and on the right side when necessary.** TBD

**dont use esoteric language features unless absolutely necessary. Prioritize readability over convenience.** TBD

**code formatters are good, but they can be too aggressive with line breaks... we should do something about that... probably just set the line break limit to a very large value** TBD

**minimize returns** TBD

<a name="documentation"></a>
## Code Documentation
**Document behavior, not logic.** Documentation should communicate how your software should be used, and what the user should expect as a result of using it. In addition, documentation should not need to change (often) based on the internals of the code. And good documentation will not repeat the logic; they should be complementary.

**Document everything public.** Every public interface has the potential to be used away from the narrow context in which it was defined. Good code should not require its caller to know the internals of the code. Instead, your code's behavior should be reflected in its documentation. **Note** "public" here refers to both internal - exposed and used across your application, but not by end-users - and external-facing - exposed and used by end-users.

**Document anything opaque.** Strive to make all logic and names easy to understand. However, when that is not possible, unintuitive logic and names should be commented to help the reader get started.

**Self-documenting code is a requirement, not an excuse.** Well-scoped functions and well-named variables/functions/interfaces are necessary for code to be easy to digest. However, it is often impractical to write code that needs no documentation. The expected behavior of the code may be too nuanced to wrap into a function or variable name. When that happens, write documentation.

**When you change something, check/update the documentation.** Refactoring code and adding logic to existing code may result in the behavior of the code changing in such a way that the documentation is no longer accurate. Good code hygeine requires the documentation stay up-to-date with its behavior.

<a name="readme"></a>
## Readme Documentation
- Defining the purpose, users, contributors, getting started including setting up environment variables, caveats to usage, running locally, writing PRs, how to get support as a user or contributor

<a name="documentation"></a>
## (Automated) Tests

Good tests serve many purposes. They inspire confidence. They provide examples of how code should be used. They help solidify the scope of units of code. They increase the chances that your code actually works, reducing your support load and decreasing time to incremental improvements. To that end, work toward the following.

**100% code coverage is an aspiration, not a requirement.** Try to execute every line of code during tests, but not at the cost of well-defined APIs/interfaces. If your code is written such that it is impossible to test critical sections, then you probably need to refactor your API.

**Be TDD-lite.** Define and document public (function and object) interfaces, then write the tests, then write the internals.

**Tests must pass to merge.** Have a test suite that runs with every commit to a remote branch, and require it before a PR can be merged.

**Test logic, not markup.** If you're working on software with a UI, focus on testing the critical logic that makes the software function, rather than the way the software appears. This requires having a good separation of concerns, such that you can evaluate the logic separately from the UI. In other words, only minimally mix the two.

**Test interfaces, not internals.** Your tests should test the use of functions from the perspective of their callers. In other words, test the behavior of functions based on their signature and documentation. If your functions are well-scoped and well-documented, you will naturally end up testing all the important aspects of their internals. This also reduces future refactoring of tests, as your APIs/interfaces should change less frequently than the internals of your functions.

**ITs over UTs, but ideally both.** The most important tests are those that synthesize the many parts of the software at a high level (ITs), reflecting the way the software will be used and the users' expectations of its behavior. If you do a good job writing your interfaces this will happen automatically. At the same time, writing thorough UTs also reduces the chances your ITs will fail for mysterious reasons.

**test logic, not markup.** TBD

<a name="pipeline"></a>
## Development Pipeline
- write good PR descriptions, for ease of review and for posterity

<a name="support"></a>
## Support

<a name="dependencies"></a>
## Dependencies (on other software)

**Reinvent the wheel until you understand the wheel.** Rather than starting with "I need X. Where can I get a library that does X?" start with "What is X and how would I address it myself?" Struggling with solving a problem yourself gives you the context you need to understand whether someone else's solution is correct, concise, and necessary.

**Minimize dependencies.** It's usually better to solve your specific problem with your specific solution than to rely on someone else's general solution to a general version of your problem. This way you can minimize compilation size, minimize breaking changes from dependencies, and (in principle) create an optimal solution for your application. Only add a dependency when a problem is complex enough that it would take great effort to solve it correctly.
