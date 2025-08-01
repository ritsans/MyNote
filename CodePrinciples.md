## 1. Code design principles (KISS, DRY, SOLID, etc.)

**DRY Principle (Don't Repeat Yourself)**

- Avoid duplicating the same logic; extract shared functionality into functions or components.
- Manage configuration values and constants in a single location to enable reuse.
- When encountering similar patterns, consider designing a generic solution.

**KISS Principle (Keep It Simple, Stupid)**

- Favor simple and easily understandable solutions over complex implementations.
- Each function or component should have only one responsibility.
- Avoid excessive abstraction and premature optimization.

**YAGNI Principle (You Aren't Gonna Need It)**

- Do not implement features that are not currently needed.
- Prevent overengineering and unnecessary work by keeping the code simple and understandable.
- Add features only when there is a confirmed need.

**SOLID Principles**

- **Single Responsibility Principle**: Each class or function should have only one reason to change.
- **Open/Closed Principle**: Software should be open for extension but closed for modification.
- **Liskov Substitution Principle**: Subtypes must be substitutable for their base types.
- **Interface Segregation Principle**: Do not force clients to depend on methods they do not use.
- **Dependency Inversion Principle**: Depend on abstractions, not on concrete implementations.

**Other Key Principles**

- **Readability First**: Code is read more often than it is written.
- **Consistency**: Use a unified style and pattern across the entire project.
- **Testability**: Design with test-friendly structure in mind.
- **Error Handling**: Include proper exception handling and consider edge cases.
- **Performance**: Use efficient algorithms and data structures when needed.

## 2. Type Safety (TypeScript)

- **Avoid any**: Do not use the any type unless absolutely necessary. When used, always include a comment explaining the reason.
- **Leverage Type Inference**: Allow TypeScript to infer return types, but always explicitly annotate parameter types and props.
- **Keep props Type Definitions Simple**: Prefer concise and clear type definitions over verbose ones.
- **Use type or interface for Type Definitions**: Define types with type or interface, emphasizing extensibility and readability.

## 3. React Coding Guidelines

- **Use Function Components and Hooks**: Function components with Hooks are the standard; avoid class components.
- **Destructure props**: Always use destructuring for props to improve readability and maintainability.
- **Avoid Unnecessary Fragments**: Use <></> only when necessary.
- **One Component per File**: Each component should be in its own file and typically range from 100 to 500 lines. Split large components into smaller ones.
- **Minimize Re-renders and Side Effects**: Use useEffect, useMemo, and useCallback sparingly and only when needed.
- **Prefer Pure Functions**: Design components and hooks as pure functions where output is determined solely by input, avoiding side effects.

## 4. Naming Conventions and General Practices

- Prefer const for Declarations: Avoid reassignments and never use var.
- Clear Naming for Variables and Functions: Use descriptive names. Use plural forms for arrays and start function names with verbs.
- No Magic Numbers or Hardcoding: Replace literals with constants where needed.
- Avoid Type Assertions (as): Do not use type assertions unless absolutely necessary.

## 5. coding convention tools / automatic formatting

- **ESLint + Prettier** to ensure automatic and consistent code quality and style.

## Additional Best Practices

- **Directory/File Structure**: Follow best practices such as those in “bulletproof-react” to ensure a scalable and understandable project structure.
- **Use Object Parameters When There Are Many Arguments**: Prevent issues with argument order and maintain readability by using an object for parameter grouping.
- **Consider Accessibility and Error Handling**: Implement accessibility features and robust error handling mechanisms where appropriate.
- **Use Typed Contexts and Custom Hooks**: Enhance extensibility and robustness by using strongly typed React Context and custom hooks.
- **Before submitting code, verify the following against these principles**:
1. Is there any duplicated code?
2. Is each function and component responsible for only one concern?
3. Is the structure easy to understand and maintain?
4. Is the design extensible and reusable?
5. Is it consistent with the existing patterns of the project?

When providing code, include concise **comments in Japanese** explaining which principles were applied and why. If alternative solutions could improve quality, include them as well.
