---
theme: simple
highlightTheme: zenburn
css: css/holocronSlides.css
---
# Style Guides in Software Development

---

# What is a Style Guide?

- Set of conventions and rules for writing code.
- Ensures consistency, readability, and maintainability.
- Facilitates teamwork and communication.
- Simplifies updating and refactoring code.
- Provides a standard for all developers.

---

# Why Use a Style Guide?

- **Consistency**: Code looks the same across the project.
- **Readability**: Easier to understand each other’s code.
- **Maintainability**: Simplifies updating and refactoring.
- **Collaboration**: Facilitates teamwork.
- **Quality**: Ensures high-quality code.

---

# Naming Conventions

- **Descriptive**: Use meaningful names.
- **Consistent**: Follow the same pattern.
- **Avoid Abbreviations**: Use full words.
- **CamelCase/snake_case**: Follow language-specific convention.
- **Types**: Variables, functions, classes, constants.

---

# Examples of Naming Conventions

1. **Variables**: `totalAmount`, `total_amount`.
2. **Functions/Methods**: `calculateSum()`, `fetchData()`.
3. **Classes**: `UserAccount`.
4. **Constants**: `MAX_USERS`.
5. **Descriptive & Consistent** names.

---

# Internal Documentation

- **Purpose**: Explain code logic and context.
- **Comments**: Explain intentions and details.
- **Docstrings**: Detailed descriptions of functions/classes.
- **Inline Documentation**: Clarify complex logic.
- **Consistent Formatting**: Follow a standard format.

---

# Internal Documentation

1. **Comments**:
    - Explain code logic.
    - Example: `// Calculate the sum of two numbers`.

2. **Inline Documentation**:
	- Explain complex logic.

```plaintext
// Base case: if n is 0 or 1, return 1
// Recursive case: n * factorial of (n - 1)
```


--

# Docstrings / Function Header

- Detailed descriptions.

```plaintext
/**
* Calculate the sum of two numbers.
* @param {number} a - The first number.
* @param {number} b - The second number.
* @return {number} The sum of the two numbers.
*/
```

---

# Summary

- **Style Guides**: Ensure consistency, readability, and maintainability.
- **Naming Conventions**: Use descriptive, consistent names.
- **Internal Documentation**: Explain code logic and context.

---

# Questions?

Feel free to ask any questions or share your experiences with style guides and internal documentation!
