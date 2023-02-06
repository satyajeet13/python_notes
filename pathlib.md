# Using the `pathlib` module

## The File System
- Provides an abstract representation of the files stored on the computer
- Controls the storage and retrieval of file data

## The File System Hierarchy
- Root directory
  - Directory
    - File
    - Subdirectory
      - File

## File Paths
File paths are different on different operating systems

- **macOS:** `/Users/martin/Documents/hello.txt`
- **Ubuntu Linux:** `/home/martin/Documents/hello.txt`
- **Windows:** `C:\Users\martin\Documents\hello.txt`

## `pathlib` module

### Creating `Path` Objects
1. From a string
2. With the `Path.home()` and `Path.cwd()` class methods
3. With the `/` operator

Representing Windows Paths
- The backslash character (`\`) starts an escape sequence in Python
- To avoid syntax errors, you can:
  - Use a forward slash(`/`) instead:
  `pathlib.Path("C:/Users/spadhi/Desktop/hello.txt")`
  - Create a raw string by prefixing the string with `r`:
  `pathlib.Path(r"C:\Users\spadhi\Desktop\hello.txt")`
- Output: `WindowsPath('C:/Users/spadhi/Desktop/hello.txt')`

### Using `Path.home()` and `Path.cwd()`

`Path.home()`: Home directory paths

```python
>>> pathlib.Path.home()
WindowsPath('C:/Users/spadhi')
```
`Path.home()` gives you different paths depending on your operating system:
- **Windows:** `C:\Users\<username>`
- **macOS:** `/Users/<username>`
- **Ubuntu Linux:** `/home/<username>`

`Path.cwd()`: Current Working Directory - it is a dynamic reference to a directory where a process on the computer is currently working

```python
>>> pathlib.Path.cwd()
WindowsPath('C:/Program Files/Python310')
```

When running a script, it is usually the folder containing the script.

### Using the Forward Slash Operator

```python
>>> pathlib.Path.home() / "Desktop" / "hello.txt"
WindowsPath('C:/Users/spadhi/Desktop/hello.txt')
```
is the same as:

```python
>>> pathlib.Path.home().joinpath("Desktop").joinpath("hello.txt")
WindowsPath('C:/Users/spadhi/Desktop/hello.txt')
```
The concatenation can be merged as follows:

```python
>>> pathlib.Path.home() / "Desktop.hello.txt"
WindowsPath('C:/Users/spadhi/Desktop.hello.txt')
```

### Checking Whether a File Path Exists

You can create `Path` objects whether or not the file path exists
- `.exists()`: returns `True` if the path exists, else `False`
- `.is_file()`: returns `True` if the path points to a file, else `False`
- `.is_dir()`: returns `True` if the path points to a directory, else `False`

> Both `.is_file()` and `.is_dir()` return `False` if the path doesn't exist.

Example:
```python
>>> home = pathlib.Path.home()
>>> desk = home / "Desktop"
>>> docs = home / "Documents"
>>> hi_desk = desk / "hello.txt"
>>> hi_docs = docs / "hello.txt"
>>> hi_docs.exists()
False
>>> hi_desk.exists()
True
>>> hi_desk.is_file()
True
>>> hi_desk.is_dir()
False
>>> pathlib.Path("/Users/gremlin").is_dir()
False
>>> pathlib.Path("/Users/gremlin").exists()
False
```

### Absolute vs Relative Paths
- **Absolute:** `/Users/martin/Documents/hello.txt`
- **Relative:** `hello.txt`

```python
>>> import os
>>> import pathlib
>>> pathlib.Path.cwd()
WindowsPath('C:/Program Files/Python310')
>>> os.chdir(r"C:\Users\spadhi\Desktop")
>>> pathlib.Path.cwd()
WindowsPath('C:/Users/spadhi/Desktop')
>>> hi_abs_path = pathlib.Path(r"C:\Users\spadhi\Desktop\hello.txt")
>>> hi_abs_path
WindowsPath('C:/Users/spadhi/Desktop/hello.txt')
>>> hi_abs_path.is_absolute()
True
>>> hi_rel_path = pathlib.Path("hello.txt")
>>> hi_rel_path.is_absolute()
False
# Use resolve with caution
>>> hi_rel_path.resolve()
WindowsPath('C:/Users/spadhi/Desktop/hello.txt')
```

- `.is_absolute()`
  - Returns `True` for absolute paths, otherwise `False`
- `.resolve()` 
  - Creates an absolute path, resolving links(`.`,`..`, custom-made symbolic links)

### Accessing File Path Components

- `.parents`: Returns paths to all directories contained in the file path
- `.parent`: Returns the path to the first paretn directory
- `.anchor`: Returns a string representing the root directory if the path is absolute, else an empty string
- `.name`: Returns the name of the file or directory the path points to as a string
- `.stem`: Returns the filename before the dot
- `.suffix`: Returns the file extension

> If the path points to a resource without a dot in the name, then `.name` and `.stem` return the same string, and `.suffix` returns an empty string.

```python
>>> import pathlib
>>> f = pathlib.Path.home() / "Desktop/hello.txt"
>>> f
WindowsPath('C:/Users/spadhi/Desktop/hello.txt')
>>> f.parents
<WindowsPath.parents>
>>> for p in f.parents:
        print(p)
C:\Users\spadhi\Desktop
C:\Users\spadhi
C:\Users
C:\
>>> list(f.parents)
[WindowsPath('C:/Users/spadhi/Desktop'), WindowsPath('C:/Users/spadhi'), WindowsPath('C:/Users'), WindowsPath('C:/')]
# anchor method works only on absolute paths
>>> f.anchor
'C:\\'
>>> f.name
'hello.txt'
>>> f.stem
'hello'
>>> f.suffix
'.txt'
>>> d = pathlib.Path.home()
>>> d
WindowsPath('C:/Users/spadhi')
>>> d.is_dir()
True
>>> d.name
'spadhi'
>>> d.stem
'spadhi'
>>> d.suffix
''
```

## Common File System Operations

- `.mkdir()`: Create a **directory**
  - `exist_ok=True`: If the directory already exists, no error is thrown
  - `parents=True`: Create any missing parent directories
- `.touch()`: Create a **file**
  - Existing files don't raise an error
  - The directories containing the file must exist
  

Creating directories and subdirectories

```python
>>> from pathlib import Path
>>> notes_dir = Path.home() / "notes"
>>> notes_dir.exists()
False
>>> notes_dir.mkdir()
>>> notes_dir.exists()
True
>>> notes_dir.mkdir(exist_ok=True)
>>> monthly_dir = notes_dir / "plans" / "monthly"
>>> monthly_dir
WindowsPath('C:/Users/spadhi/notes/plans/monthly')
>>> monthly_dir.mkdir(parents=True)
>>> monthly_dir.exists()
True
>>> weekly_dir = notes_dir / "plans" / "weekly"
>>> weekly_dir.mkdir(parents=True, exist_ok=True)
```

Creating files

```python
>>> from pathlib import Path
>>> monthly_dir = Path.home() / "notes" / "plans" / "monthly"
>>> january_path = monthly_dir / "january.txt"
>>> january_path
WindowsPath('C:/Users/spadhi/notes/plans/monthly/january.txt')
>>> january_path.exists()
False
>>> january_path.touch()
>>> january_path.exists()
True
>>> january_path.is_file()
True
>>> too_much_future_path = Path.home() / "notes" / "yearly" / "3023.txt"
>>> too_much_future_path
WindowsPath('C:/Users/spadhi/notes/yearly/3023.txt')
>>> too_much_future_path.touch()
Traceback (most recent call last):
  File "<pyshell#10>", line 1, in <module>
    too_much_future_path.touch()
  File "C:\Program Files\Python310\lib\pathlib.py", line 1168, in touch
    self._accessor.touch(self, mode, exist_ok)
  File "C:\Program Files\Python310\lib\pathlib.py", line 331, in touch
    fd = os.open(path, flags, mode)
FileNotFoundError: [Errno 2] No such file or directory: 'C:\\Users\\spadhi\\notes\\yearly\\3023.txt'
>>> too_much_future_path.parent.mkdir(parents=True)
>>> too_much_future_path.touch()
>>> too_much_future_path.exists()
True
```

### Iterating over Directories

```python
>>> from pathlib import Path
>>> notes_dir = Path.home() / "notes"
>>> notes_dir.is_dir()
True
>>> notes_dir.iterdir()
<generator object Path.iterdir at 0x000001D9E2EA58C0>
>>> for path in notes_dir.iterdir():
...     print(path)
...
C:\Users\spadhi\notes\plans
C:\Users\spadhi\notes\yearly
>>> readme_file = notes_dir / "README.md"
>>> readme_file.touch()
>>> for path in notes_dir.iterdir():
...     print(path)
...
C:\Users\spadhi\notes\plans
C:\Users\spadhi\notes\README.md
C:\Users\spadhi\notes\yearly
>>> list(notes_dir.iterdir())
[WindowsPath('C:/Users/spadhi/notes/plans'), WindowsPath('C:/Users/spadhi/notes/README.md'), WindowsPath('C:/Users/spadhi/notes/yearly')]
```

### Searching for Files in a Directory
- `.glob()`: Returns an iterable of `Path` objects that match a pattern
- `*` is a wildcard character matching any number of other characters
- **Example:** `.glob("*.md")` matches all files ending with `.md` in the `Path` object you call it on

```python
>>> from pathlib import Path
>>> notes_dir = Path.home() / "notes"
>>> notes_dir
WindowsPath('C:/Users/spadhi/notes')
>>> for path in notes_dir.glob("*.md"):
...     print(path)
...
C:\Users\spadhi\notes\README.md
>>> list(notes_dir.glob("*.md"))
[WindowsPath('C:/Users/spadhi/notes/README.md')]
```

### Using common wildcard characters
- `*`: Any number of characters
- `?`: A single character
- `[abc]`: One character in the brackets

Using the `*` wildcard
```python
>>> paths = [
      notes_dir / "goals1.txt",
      notes_dir / "goals2.txt",
      notes_dir / "yearly" / "2033.txt",
      notes_dir / "yearly" / "2034.txt",
      notes_dir / "plans" / "goals3.txt",
      notes_dir / "plans" / "monthly" / "february.txt",
      notes_dir / "plans" / "monthly" / "march.md",
      ]
>>> for path in paths:
...     path.touch()
>>> list(notes_dir.glob("*.txt"))
[WindowsPath('C:/Users/spadhi/notes/goals1.txt'), WindowsPath('C:/Users/spadhi/notes/goals2.txt')]
>>> yearly_dir = notes_dir / "yearly"
>>> yearly_dir
WindowsPath('C:/Users/spadhi/notes/yearly')
>>> list(yearly_dir.glob("2*3*"))
[WindowsPath('C:/Users/spadhi/notes/yearly/2033.txt'), WindowsPath('C:/Users/spadhi/notes/yearly/2034.txt')]
>>> list(yearly_dir.glob("2*3.*"))
[WindowsPath('C:/Users/spadhi/notes/yearly/2033.txt')]
```

Using the `?` wildcard

```python 
>>> list(notes_dir.glob("goals?.txt"))
[WindowsPath('C:/Users/spadhi/notes/goals1.txt'), WindowsPath('C:/Users/spadhi/notes/goals2.txt')]
>>> list(notes_dir.glob("*.??"))
[WindowsPath('C:/Users/spadhi/notes/README.md')]
```

Using the `[]` wildcard

```python
>>> list(notes_dir.glob("goals[13].txt"))
[WindowsPath('C:/Users/spadhi/notes/goals1.txt')]
```

### Recursive Matching with the `**` Wildcard

It matches your pattern both in the current directory and in any of its subdirectories.
- Using the `**` wildcard
- Using `.rglob()`

```python
>>> list(notes_dir.glob("**/*.txt"))
[WindowsPath('C:/Users/spadhi/notes/goals1.txt'), WindowsPath('C:/Users/spadhi/notes/goals2.txt'), WindowsPath('C:/Users/spadhi/notes/plans/goals3.txt'), WindowsPath('C:/Users/spadhi/notes/plans/monthly/february.txt'), WindowsPath('C:/Users/spadhi/notes/plans/monthly/january.txt'), WindowsPath('C:/Users/spadhi/notes/yearly/2033.txt'), WindowsPath('C:/Users/spadhi/notes/yearly/2034.txt'), WindowsPath('C:/Users/spadhi/notes/yearly/3023.txt')]
>>> list(notes_dir.glob("**/*.md"))
[WindowsPath('C:/Users/spadhi/notes/README.md'), WindowsPath('C:/Users/spadhi/notes/plans/monthly/march.md')]
>> list(notes_dir.rglob("*.md"))
[WindowsPath('C:/Users/spadhi/notes/README.md'), WindowsPath('C:/Users/spadhi/notes/plans/monthly/march.md')]
```

## Moving and Deleting Files and Folders

Moving a file from one folder to another

```python
>>> from pathlib import Path
>>> notes_dir = Path.home() / "notes"
>>> old_path_goals_3 = notes_dir / "plans" / "goals3.txt"
>>> old_path_goals_3
WindowsPath('C:/Users/spadhi/notes/plans/goals3.txt')
>>> new_path_goals_3 = notes_dir / "goals3.txt"
>>> new_path_goals_3
WindowsPath('C:/Users/spadhi/notes/goals3.txt')
>>> old_path_goals_3.replace(new_path_goals_3)
WindowsPath('C:/Users/spadhi/notes/goals3.txt')
>>> list(notes_dir.iterdir())
[WindowsPath('C:/Users/spadhi/notes/goals1.txt'), WindowsPath('C:/Users/spadhi/notes/goals2.txt'), WindowsPath('C:/Users/spadhi/notes/goals3.txt'), WindowsPath('C:/Users/spadhi/notes/plans'), WindowsPath('C:/Users/spadhi/notes/README.md'), WindowsPath('C:/Users/spadhi/notes/yearly')]
>>> list((notes_dir / "plans").iterdir())
[WindowsPath('C:/Users/spadhi/notes/plans/monthly'), WindowsPath('C:/Users/spadhi/notes/plans/weekly')]
```

Moving a Folder

```python
>>> source = notes_dir / "yearly"
>>> destination = notes_dir / "plans" / "yearly"
>>> source
WindowsPath('C:/Users/spadhi/notes/yearly')
>>> destination
WindowsPath('C:/Users/spadhi/notes/plans/yearly')
>>> source.replace(destination)
WindowsPath('C:/Users/spadhi/notes/plans/yearly')
```

Deleting a File

`.unlink()` deletes a file:

```python
>>> from pathlib import Path
>>> notes_dir = Path.home() / "notes"
>>> file_path = notes_dir / "plans" / "monthly" / "march.md"
>>> file_path.exists()
True
>>> file_path.unlink()
>>> file_path.exists()
False
>>> file_path.unlink(missing_ok=True)
```
If the file doesn't exist, Python raises a `FileNotFoundError`.
You can pass `missing_ok=True` to avoid raising an error if the file doesn't exist.

Deleting an empty folder 
- `.rmdir()` deletes only empty folders

```python
>>> weekly_dir = notes_dir / "plans" / "weekly"
>>> weekly_dir.is_dir()
True
>>> weekly_dir.rmdir()
>>> weekly_dir.exists()
False
```

Deleting a non-empty folder
`shutil.rmtree()` allows you to delete a folder including all of its contents

```python
>>> import shutil
>>> yearly_dir = notes_dir / "plans" / "yearly"
>>> yearly_dir.exists()
True
>>> shutil.rmtree(yearly_dir)
>>> yearly_dir.exists()
False
```