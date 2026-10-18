# Data Science Coding Challenges

A practical coding-practice module for data science interviews. It covers Python fundamentals, NumPy, Pandas, SQL-style data manipulation, statistics, machine learning, evaluation, and debugging. The goal is to write correct, readable, testable code and explain the trade-offs.

## Learning objectives

- Solve common Python problems using dictionaries, sets, lists, comprehensions, and functions.
- Work with NumPy arrays, Boolean masks, broadcasting, and vectorized calculations.
- Use Pandas for filtering, grouping, missing-value handling, joins, and time-based features.
- Implement core statistics and metrics from first principles.
- Build clean scikit-learn preprocessing and modeling pipelines.
- Test edge cases, explain complexity, and avoid data leakage.

## Files

| File | Purpose |
|---|---|
| `README.md` | Roadmap, coding workflow, and interview checklist |
| `examples.ipynb` | Runnable coding examples with assertions |
| `practice.md` | Coding prompts organized by difficulty and topic |
| `solution.md` | Reference implementations, explanations, and complexity notes |

## Suggested environment

```bash
python -m pip install numpy pandas scikit-learn jupyter
jupyter notebook examples.ipynb
```

The notebook uses small local examples and requires no external datasets.

## Recommended problem-solving workflow

1. **Clarify:** confirm input types, constraints, output format, and whether mutation is allowed.
2. **Start simple:** write a straightforward correct solution before optimizing.
3. **Check edge cases:** empty input, one element, duplicates, missing values, zeros, negative values, and ties.
4. **Explain complexity:** estimate time and space costs in terms of input size.
5. **Test:** include representative, boundary, and adversarial examples.
6. **Optimize:** profile or reason about bottlenecks before making code more complex.
7. **Communicate:** describe why the approach works and what assumptions it relies on.

## Learning path

1. Python data structures, loops, comprehensions, and string processing.
2. Arrays, hash maps, sets, sorting, searching, and two-pointer patterns.
3. NumPy vectorization, broadcasting, and numerical stability.
4. Pandas filtering, grouping, joining, and cleaning.
5. Probability, descriptive statistics, and metric implementation.
6. ML preprocessing, validation, baseline models, and evaluation.
7. Testing, debugging, readability, and complexity analysis.

## Coding interview checklist

- [ ] Can I state assumptions and clarify ambiguous requirements?
- [ ] Can I handle empty inputs, duplicates, ties, and missing values?
- [ ] Can I explain time and space complexity?
- [ ] Do I use vectorized operations when they improve clarity and speed?
- [ ] Do I avoid modifying input unexpectedly?
- [ ] Do I test the result against a small hand-worked example?
- [ ] Do I avoid leakage by splitting data before fitting preprocessing?
- [ ] Can I explain why the solution is correct, not just that it runs?

## Interview-ready habits

- Prefer descriptive names and small functions.
- Separate data preparation from evaluation.
- Avoid hard-coded answers that only work for the sample input.
- For floating-point values, use tolerances where exact equality is inappropriate.
- For ML tasks, use a simple baseline and a suitable metric before trying a complex model.
- State whether a solution is intended for production-scale data or an interview-sized input.
