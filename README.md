# HsLua Module: System

This module provides access to system information and functionality via
Haskell's `System` module.

Intended usage for this package is to preload it by adding the loader
function to `package.preload`. Note that the Lua `package` library must
have already been loaded before the loader can be added.

## Example

``` haskell
loadProg :: Lua Status
loadProg = do
  openlibs
  preloadModule "system"
  -- create a temporary directory, print it's path, then delete it again.
  dostring $ "system = require 'system'\n"
          ++ "system.with_tmpdir('.', 'foo', print)"
```

## Documentation

### Fields

#### arch

The machine architecture on which the program is running.

#### compiler_name

The Haskell implementation with which the host program was compiled.

#### compiler_version

The version of `compiler_name` with which the host program was compiled.

#### os

The operating system on which the program is running.

### General Functions

#### env

`env ()`

Retrieve the entire environment.

Returns:

- A table mapping environment variables names to their string value
  (table).

#### getenv

`getenv (var)`

Return the value of the environment variable `var`, or `nil` if there
is no such value.

Parameters:

`var`
:   name of the environment variable (string)

Returns:

- value of the variable, or nil if the variable is not defined (string
  or nil).

#### getwd

`getwd ()`

Obtain the current working directory as an absolute path.

Returns:

- The current working directory (string).

#### ls

`ls ([directory])`

List the contents of a directory.

Parameters:

`directory`
:   Path of the directory whose contents should be listed (string).
    Defaults to `.`.

Returns:

- A table of all entries in `directory` without the special entries (`.`
  and `..`).

#### mkdir

`mkdir (dirname [, create_parent])`

Create a new directory which is initially empty, or as near to
empty as the operating system allows. The function throws an
error if the directory cannot be created, e.g., if the parent
directory does not exist or if a directory of the same name is
already present.

If the optional second parameter is provided and truthy, then all
directories, including parent directories, are created as
necessary.

Parameters:

`dirname`
:   name of the new directory

`create_parent`
:   create parent directories if necessary

#### rmdir

`rmdir (dirname [, recursive])`

Remove an existing, empty directory. If `recursive` is given,
then delete the directory and its contents recursively.

Parameters:

`dirname`
:   name of the directory to delete

`recursive`
:   delete content recursively

#### setenv

`setenv (var, value)`

Set the specified environment variable to a new value.

Parameters:

`var`
:   name of the environment variable (string).

`value`
:   new value (string).

#### setwd

`setwd (directory)`

Change the working directory to the given path.

Parameters:

`directory`
:   Path of the directory which is to become the new working
    directory (string)

#### tmpdirname

`tmpdirname ()`

Returns the current directory for temporary files.

On Unix, `tmpdirname()` returns the value of the `TMPDIR` environment
variable or "/tmp" if the variable isn't defined. On Windows, the
function checks for the existence of environment variables in the
following order and uses the first path found:

- TMP environment variable.
- TEMP environment variable.
- USERPROFILE environment variable.
- The Windows directory

The operation may fail if the operating system has no notion of
temporary directory.

The function doesn't verify whether the path exists.

Returns:

- The current directory for temporary files (string).

#### with\_env

`with_env (environment, callback)`

Run an action within a custom environment. Only the environment
variables given by `environment` will be set, when `callback` is
called. The original environment is restored after this function
finishes, even if an error occurs while running the callback
action.

Parameters:

`environment`
:   Environment variables and their values to be set before
    running `callback`. (table with string keys and string
    values)

`callback`
:   Action to execute in the custom environment (function)

Returns:

- The result(s) of the call to `callback`

#### with\_tmpdir

`with_tmpdir ([parent_dir,] templ, callback)`

Create and use a temporary directory inside the given directory.
The directory is deleted after use.

Parameters:

`parent_dir`
:   Parent directory to create the directory in (string). If this
    parameter is omitted, the system's canonical temporary directory is
    used.

`templ`
:   Directory name template (string).

`callback`
:   Function which takes the name of the temporary directory as its
    first argument (function).

Returns:

- The result of the call to `callback`.

#### with\_wd

`with_wd (directory, callback)`

Run an action within a different directory. This function will
change the working directory to `directory`, execute `callback`,
then switch back to the original working directory, even if an
error occurs while running the callback action.

Parameters:

`directory`
:   Directory in which the given `callback` should be executed
    (string)

`callback`
:   Action to execute in the given directory (function)

Returns:

- The result(s) of the call to `callback`

### Path Manipulation Functions

This library also includes wrappers around the following functions from the [filepath](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath.html) library:

- `take_directory (filepath)` wraps [System.FilePath.takeDirectory](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:takeDirectory)
- `take_filename (filepath)` wraps [System.FilePath.takeFilename](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:takeFilename)
- `take_extensions (filepath)` wraps [System.FilePath.takeExtensions](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:takeExtensions)
- `drop_extension (filepath)` wraps [System.FilePath.dropExtension](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:dropExtension)
- `has_extension (filepath)` wraps [System.FilePath.hasExtension](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:hasExtension)
- `split_directories (filepath)` wraps [System.FilePath.splitDirectories](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:splitDirectories)
- `join_path (filepath, ...)` wraps [System.FilePath.joinPath](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:joinPath)
- `is_relative (filepath)` wraps [System.FilePath.isRelative](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:isRelative)
- `is_absolute (filepath)` wraps [System.FilePath.isAbsolute](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:isAbsolute)
- `normalise (filepath)` wraps [System.FilePath.normalise](https://hackage.haskell.org/package/filepath-1.4.2.1/docs/System-FilePath-Posix.html#v:normalise:)

## License

This package is licensed under the MIT license. See [`LICENSE`](LICENSE)
for details.
