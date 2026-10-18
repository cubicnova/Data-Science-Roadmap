# Python Interview Preparation

A practical, interview-focused Python module for data science, machine learning, analytics, and general software roles.

## Learning outcomes

By completing this module, you should be able to:

- Explain Python's core data types, mutability, equality, identity, and truthiness.
- Work confidently with strings, lists, tuples, sets, dictionaries, comprehensions, and unpacking.
- Write functions using positional/keyword arguments, `*args`, `**kwargs`, and safe default values.
- Explain scope, closures, iterators, generators, decorators, and common object-oriented concepts.
- Handle exceptions and files safely, and use context managers.
- Analyze time and space complexity for common operations.
- Solve typical coding-round problems cleanly and test edge cases.
- Discuss Python choices in a data-science workflow.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Roadmap, interview strategy, and topic checklist |
| `examples.ipynb` | Executable examples with explanations and mini-interview prompts |
| `practice.md` | Questions and coding exercises arranged by difficulty |
| `solution.md` | Worked solutions, reasoning, complexity, and edge cases |

## Suggested study plan

### Day 1 — Core language and collections
Review built-in types, mutability, slicing, membership, dictionary/set operations, comprehensions, and unpacking. Solve Practice 1–5.

### Day 2 — Functions and Python semantics
Review argument passing, mutable defaults, scope, `lambda`, closures, shallow vs deep copies, and sorting. Solve Practice 6–9.

### Day 3 — OOP, exceptions, and iteration
Review classes, inheritance, `self`, `classmethod`, `staticmethod`, exceptions, iterators, generators, and context managers. Solve Practice 10–12.

### Day 4 — Coding round
Solve the remaining problems without looking at solutions. Explain the approach, complexity, assumptions, and edge cases out loud.

### Day 5 — Mock interview and revision
Pick 10 theory questions at random, complete two coding problems in a timed session, and review mistakes.

## High-value concepts to know

- **Mutable objects:** lists, dictionaries, and sets can be modified in place; strings and tuples are immutable (though a tuple can contain mutable objects).
- **`==` vs `is`:** `==` compares values; `is` checks object identity. Use `is None` for `None` checks.
- **Dictionary lookup:** average-case \(O(1)\) lookup; keys must be hashable.
- **List vs tuple:** lists are mutable; tuples are immutable and can be hashable if all their elements are hashable.
- **Generator:** produces values lazily, often reducing peak memory for large streams.
- **`finally`:** runs during normal completion and exception unwinding; use context managers for resource cleanup.
- **Default arguments:** evaluated once when the function is defined; avoid mutable defaults such as `items=[]`.
- **Comprehensions:** concise for transformations and filtering, but ordinary loops may be clearer for complex logic.
- **Complexity:** state both time and auxiliary space, and mention assumptions such as average-case hash-table behavior.

## Interview answer framework

For a conceptual question:
1. Give a one-sentence definition.
2. Explain the practical distinction.
3. Show a tiny example.
4. Mention one common pitfall or use case.

For a coding question:
1. Clarify input constraints and expected output.
2. Start with a straightforward approach.
3. Improve it using an appropriate data structure or algorithm.
4. State time and space complexity.
5. Test empty input, duplicates, negative values, and boundary cases where relevant.

## Quick self-assessment

- [ ] I can explain mutability and object identity with an example.
- [ ] I can use dictionaries and sets to solve membership/counting problems.
- [ ] I can write a function without mutable-default bugs.
- [ ] I can explain iterators, generators, decorators, and context managers.
- [ ] I can implement the practice problems without copying the solutions.
- [ ] I can justify complexity and test edge cases.

## Further reading

- [The Python Tutorial](https://docs.python.org/3/tutorial/)
- [Python data structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Classes and objects](https://docs.python.org/3/tutorial/classes.html)
- [Errors and exceptions](https://docs.python.org/3/tutorial/errors.html)
- [Built-in functions](https://docs.python.org/3/library/functions.html)
- [Time complexity of Python operations (Python Wiki)](https://wiki.python.org/moin/TimeComplexity)

> Version note: examples target modern Python 3 and use broadly available language features. Exact performance characteristics can depend on the Python implementation and workload.
