# Practice Exercises — 09 File Handling

Complete all 35 exercises. Use temporary test files and avoid overwriting important data while practising.

## 1. Create `hello.txt` and write `Hello, Python!` into it.

**Hint:** Use `open` with `w` and UTF-8 encoding.

## 2. Read and print the full contents of `hello.txt`.

**Hint:** Use `read()` inside a `with` block.

## 3. Read only the first line of a text file.

**Hint:** Use `readline()`.

## 4. Read a text file line by line without loading it all into memory.

**Hint:** Iterate over the file object.

## 5. Append a new line to `log.txt`.

**Hint:** Use mode `a` and include `\n`.

## 6. Explain the difference between `w` and `a` modes.

**Hint:** `w` truncates; `a` appends.

## 7. Create a file only if it does not already exist.

**Hint:** Use mode `x` or `Path.touch(exist_ok=False)`.

## 8. Write three lines to a file using a context manager.

**Hint:** Use `with open(..., 'w', encoding='utf-8')`.

## 9. Use `pathlib` to get the suffix of `data.csv`.

**Hint:** Use `Path('data.csv').suffix`.

## 10. Check whether `report.txt` exists.

**Hint:** Use `Path.exists()`.

## 11. Create an `output/reports` directory including missing parents.

**Hint:** Use `mkdir(parents=True, exist_ok=True)`.

## 12. List all `.txt` files in the current directory.

**Hint:** Use `Path('.').glob('*.txt')`.

## 13. Handle `FileNotFoundError` when reading a file.

**Hint:** Wrap the read in `try/except`.

## 14. Explain why specifying UTF-8 is useful for text files.

**Hint:** It makes decoding/encoding behavior explicit and consistent.

## 15. Count the number of lines in a text file.

**Hint:** Iterate and increment a counter.

## 16. Count words in a text file using whitespace splitting.

**Hint:** Read text and count `text.split()`.

## 17. Replace a word in a text file and save the result to a new file.

**Hint:** Read, replace, then write to a separate path.

## 18. Copy a binary file from one path to another.

**Hint:** Open source `rb` and destination `wb`.

## 19. Read a CSV file with a header using `csv.DictReader`.

**Hint:** Open with `newline=''` and UTF-8.

## 20. Write dictionaries to a CSV file with column names.

**Hint:** Use `csv.DictWriter`, `writeheader`, and `writerows`.

## 21. Serialize a Python dictionary into `config.json`.

**Hint:** Use `json.dump` with `indent=2`.

## 22. Load JSON data from a file into Python.

**Hint:** Use `json.load`.

## 23. Explain `json.dump` versus `json.dumps`.

**Hint:** One writes to a file-like object; the other returns a string.

## 24. Find the size of `notes.txt` in bytes.

**Hint:** Use `Path.stat().st_size`.

## 25. Check whether a path is a file or a directory.

**Hint:** Use `is_file()` and `is_dir()`.

## 26. Write a log entry with a timestamp by appending to a file.

**Hint:** Use `datetime` and append mode.

## 27. Copy a large file in chunks rather than reading it all at once.

**Hint:** Loop over fixed-size chunks until empty.

## 28. Explain what `UnicodeDecodeError` means.

**Hint:** The bytes cannot be decoded with the chosen text encoding.

## 29. Explain why `pickle.load` should not be used on untrusted files.

**Hint:** Pickle loading can execute arbitrary code.

## 30. Read a JSON file and handle the case where it is missing.

**Hint:** Catch `FileNotFoundError` around `json.load`.

## 31. Use `Path` to build a portable path to `data/input.csv`.

**Hint:** Use `Path('data') / 'input.csv'`.

## 32. Read all lines and remove only trailing newline characters.

**Hint:** Use `line.rstrip('\n')` for each line.

## 33. Find all `.csv` files under a folder recursively.

**Hint:** Use `Path(folder).rglob('*.csv')`.

## 34. Explain why manually splitting CSV lines on commas can fail.

**Hint:** Quoted fields may contain commas or escaped quotes.

## 35. Write a function that reads a UTF-8 text file and returns its content.

**Hint:** Use `Path(path).read_text(encoding='utf-8')`.

