# Practice Exercises — 10 Exception Handling

Complete all 35 exercises. Test successful inputs and expected failures. Avoid suppressing exceptions silently.

## 1. Explain the difference between a syntax error and a runtime exception.

**Hint:** One prevents parsing; the other occurs during execution.

## 2. Handle `ValueError` when converting user text to an integer.

**Hint:** Wrap `int(text)` in `try/except ValueError`.

## 3. Write a function that divides two numbers and handles a zero denominator.

**Hint:** Raise or handle `ZeroDivisionError` intentionally.

## 4. Use `else` to print a result only when integer conversion succeeds.

**Hint:** Put the success message in `else`.

## 5. Use `finally` to print a cleanup message regardless of success or failure.

**Hint:** Add a `finally` clause.

## 6. Explain why `except:` is usually discouraged.

**Hint:** It catches too broadly and can hide unexpected problems.

## 7. Catch `ValueError` and `ZeroDivisionError` separately in a calculator.

**Hint:** Use separate `except` blocks.

## 8. Raise `ValueError` when a supplied age is negative.

**Hint:** Validate the age before returning it.

## 9. Write `safe_index(items, index)` that raises a clear error for an invalid index.

**Hint:** Check bounds or catch `IndexError` and re-raise clearly.

## 10. Handle a missing dictionary key with `KeyError`.

**Hint:** Use `try/except KeyError` or `.get()` if absence is normal.

## 11. Handle `FileNotFoundError` when reading a file.

**Hint:** Catch it around the file read.

## 12. Explain when a `finally` block is useful.

**Hint:** For cleanup that must happen regardless of errors.

## 13. Define a custom `InsufficientFundsError` exception.

**Hint:** Subclass `Exception`.

## 14. Write a withdrawal function that raises a custom exception if funds are insufficient.

**Hint:** Compare requested amount with balance.

## 15. Use exception chaining when converting text to an integer for a score field.

**Hint:** Raise a clearer error `from exc`.

## 16. Explain the difference between `raise` and `return`.

**Hint:** `raise` signals an error; `return` supplies a normal result.

## 17. Catch `TypeError` when a function receives an unsuitable argument type.

**Hint:** Use a narrow handler if recovery is appropriate.

## 18. Explain what `KeyError` means.

**Hint:** A requested mapping key is absent.

## 19. Explain what `IndexError` means.

**Hint:** A sequence index is out of range.

## 20. Write a function that validates a percentage is between 0 and 100.

**Hint:** Raise `ValueError` outside the range.

## 21. Use `logging.exception()` in a `ValueError` handler.

**Hint:** Call it inside the handler to include traceback details.

## 22. Explain why exception messages should not contain passwords or tokens.

**Hint:** Logs may be retained or exposed.

## 23. Write a `try/except/else` example that parses a floating-point number.

**Hint:** Place success-only output in `else`.

## 24. Handle JSON decoding errors when parsing a string.

**Hint:** Catch `json.JSONDecodeError`.

## 25. Handle a missing CSV file without terminating a simple report script.

**Hint:** Catch `FileNotFoundError` and report clearly.

## 26. Explain why a broad `try` block can be problematic.

**Hint:** It may catch errors from unrelated statements.

## 27. Explain why returning `None` for every error can be misleading.

**Hint:** It hides the failure reason and may look like valid output.

## 28. Write a custom `InvalidRecordError` exception.

**Hint:** Subclass `Exception`.

## 29. Validate that a list is non-empty before calculating its average.

**Hint:** Raise `ValueError` for empty input.

## 30. Write a function that rejects non-positive withdrawal amounts.

**Hint:** Raise `ValueError` when amount is zero or negative.

## 31. Explain the purpose of `raise ... from exc`.

**Hint:** It preserves the original exception as the cause.

## 32. Write a `finally` block that closes a manually opened file.

**Hint:** Call `file.close()` in `finally`; prefer `with` in real code.

## 33. Explain why context managers are often better than manual cleanup.

**Hint:** They reliably release resources when a block exits.

## 34. Design tests for successful and failing inputs to a division function.

**Hint:** Test normal division, zero divisor, and invalid types.

## 35. Describe three best practices for exception handling.

**Hint:** Catch specific errors, keep try blocks small, avoid silent failures.

