# NumPy Practice Exercises

This module consolidates core NumPy skills through progressive, hands-on tasks. Use it after studying NumPy arrays, indexing, operations, broadcasting, reshaping, and statistical functions.

## Learning goals
- Create arrays with NumPy constructors.
- Inspect dimensions, shapes, and data types.
- Select and update elements using indexing and slicing.
- Perform vectorized arithmetic and Boolean filtering.
- Use broadcasting, reshape, and join/split arrays.
- Summarize data with statistical functions.
- Solve a small end-to-end data-analysis challenge.

## Setup

```python
import numpy as np
```

Install NumPy if needed:

```bash
python -m pip install numpy
```

## Suggested workflow
1. Read each task in `practice.md`.
2. Attempt the task without looking at `solutions.md`.
3. Test boundary cases and inspect `.shape` and `.dtype`.
4. Run the examples in `examples.ipynb`.
5. Explain the output in your own words.

## Skills covered

| Skill | Useful NumPy tools |
|---|---|
| Array creation | `array`, `arange`, `linspace`, `zeros`, `ones`, `full`, `eye` |
| Metadata | `shape`, `ndim`, `size`, `dtype` |
| Indexing | integer indexing, slices, Boolean masks, fancy indexing |
| Operations | arithmetic operators, `sum`, `min`, `max` |
| Filtering | Boolean conditions, `where`, `nonzero` |
| Reshaping | `reshape`, `ravel`, `flatten`, `expand_dims`, `squeeze` |
| Combining | `concatenate`, `stack`, `vstack`, `hstack` |
| Statistics | `mean`, `median`, `std`, `var`, `percentile` |
| Random data | `default_rng`, `integers`, `normal` |

## Best practices
- Prefer vectorized NumPy expressions over Python loops for numerical array operations.
- Check dimensions and data types when debugging.
- Remember that slice views may share memory with the original array.
- Use a Boolean mask for filtering and ensure the condition has a compatible shape.
- Use `np.random.default_rng(seed)` for reproducible random examples.
- Be explicit about the axis when reducing multidimensional arrays.
- Use `np.array_equal` or `np.allclose` to validate results.

## Completion checklist
- [ ] Complete all 35 exercises in `practice.md`.
- [ ] Compare your answers with `solutions.md`.
- [ ] Run every notebook code cell.
- [ ] Finish the mini-project and explain three findings from the data.

## Further reading
- [NumPy quickstart](https://numpy.org/doc/stable/user/quickstart.html)
- [NumPy reference](https://numpy.org/doc/stable/reference/)
