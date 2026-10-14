# Practice Exercises — NumPy Reshaping and Joining

Complete the following 35 exercises. Try to solve each one before consulting `solutions.md`.

1. **Inspect shape metadata:** Create `np.arange(12)` and report its shape, dimensions, and element count.

2. **Reshape to a matrix:** Reshape values 0–11 into a 3 × 4 array.

3. **Infer columns:** Reshape 18 values into 3 rows using `-1` to infer the second dimension.

4. **Infer rows:** Reshape 20 values into 4 columns using `-1`.

5. **Invalid reshape:** Try to reshape 10 values into shape `(3, 4)`. Explain why it fails.

6. **Reshape a 3-D array:** Reshape 24 values into shape `(2, 3, 4)`.

7. **Flatten with flatten:** Flatten a 2 × 3 matrix using `flatten()`.

8. **Flatten with ravel:** Flatten the same matrix using `ravel()`.

9. **Compare copy behavior:** Modify an element in a `flatten()` result and check whether the original changes.

10. **Check ravel sharing:** Use `np.shares_memory` to investigate whether a raveled contiguous array shares memory with its source.

11. **Row vector:** Convert a 1-D array of 5 values to shape `(1, 5)`.

12. **Column vector:** Convert a 1-D array of 5 values to shape `(5, 1)`.

13. **Expand dimensions:** Use `np.expand_dims` to add an axis at position 1.

14. **Squeeze singleton axes:** Convert an array of shape `(1, 3, 1)` to shape `(3,)`.

15. **Squeeze a specific axis:** Remove only axis 0 from an array of shape `(1, 2, 1)`.

16. **Concatenate rows:** Join two 2-D arrays row-wise using `concatenate`.

17. **Concatenate columns:** Join two arrays with two rows along axis 1.

18. **Concatenate 1-D arrays:** Join `[1, 2]` and `[3, 4, 5]` into one array.

19. **Stack arrays vertically:** Use `vstack` on two 1-D arrays of equal length.

20. **Stack arrays horizontally:** Use `hstack` on two 1-D arrays.

21. **Create a stack:** Use `stack` to combine two same-shaped 1-D arrays along axis 0.

22. **Stack on another axis:** Combine two arrays of shape `(3,)` with `stack(axis=1)` and inspect the result.

23. **Compare stack and concatenate:** Use the same inputs to demonstrate how `stack` differs from `concatenate`.

24. **Depth stack:** Use `dstack` to combine two 2 × 2 arrays and print the resulting shape.

25. **Split into equal parts:** Split 12 values into 3 equal arrays.

26. **Split into uneven parts:** Split 10 values into 3 arrays using `array_split`.

27. **Split matrix rows:** Split a 6 × 2 matrix into 3 row sections.

28. **Split matrix columns:** Split a 2 × 6 matrix into 3 column sections.

29. **Choose an axis:** For a 3 × 4 matrix, show the shapes after splitting into 3 parts along axis 1.

30. **Diagnose join mismatch:** Attempt to concatenate shapes `(2, 3)` and `(2, 2)` along axis 0. Explain the mismatch.

31. **Join matching dimensions:** Fix the previous example by joining along axis 1 instead.

32. **Join unequal rows:** Concatenate arrays with shapes `(1, 3)` and `(2, 3)` along axis 0.

33. **Split and recombine:** Split a 12-element array into 4 parts, then concatenate the parts back.

34. **Reshape and join:** Reshape 6 values into `(2, 3)`, reshape another 6 into `(2, 3)`, and stack them to create a 3-D array.

35. **Mini challenge: batch data:** Create 24 values, reshape them into 4 batches of 6, and split the batches into two equal groups.

