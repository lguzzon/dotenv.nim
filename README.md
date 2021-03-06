# dotenv.nim ![Build Status](https://api.travis-ci.org/euantorano/dotenv.nim.svg) [![CircleCI](https://circleci.com/gh/euantorano/dotenv.nim/tree/master.svg?style=svg)](https://circleci.com/gh/euantorano/dotenv.nim/tree/master)

[dotenv](https://github.com/bkeepers/dotenv) implementation for Nim. Loads environment variables from `.env`

Storing [configuration in the environment](http://12factor.net/config) is one of the tenets of a [twelve-factor app](http://12factor.net). Anything that is likely to change between deployment environments–such as resource handles for databases or credentials for external services–should be extracted from the code into environment variables.

## Installation

`dotenv` can be installed using Nimble:

```
nimble install dotenv
```

Or add the following to your `.nimble` file:

```
# Dependencies

requires "dotenv >= 1.1.0"
```

## [Documentation](https://htmlpreview.github.io/?https://github.com/euantorano/dotenv.nim/blob/master/docs/dotenv.html)

## Usage

### Create a `.env` file

Create a `.env` file in the root of your project (or anywhere else - just be sure to specify the path when loading).

```
DB_HOST=localhost
DB_USER=root
DB_PASS=""
DB_NAME=test
```

Variables values are always strings, and can be either wrapped in quotes or left without.

Multiline strings can also be used using the `"""` syntax:

```
SOME_LONG_STRING="""This string is very long.

It will span multiple lines.
"""
```

You can also add comments, using the `#` symbol:

```
# Comments can fill a whole line
DB_NAME=test # Or they can follow an assignment
```

### Loading the `.env` file

You can load the `.env` file from the current working directory as follows:

```nim
import dotenv

let env = initDotEnv()
env.load()

# You can now access the variables using os.getEnv()
```

Or, you can specify the path to the directory and/or file:

```nim
import dotenv

let env = initDotEnv("/some/directory/path", "custom_file_name.env")
env.load()

# You can now access the variables using os.getEnv()
```

By default, `dotenv` does not overwrite existing environment variables, though this can be done using `overload` rather than `load`:

```nim
import dotenv

let env = initDotEnv()
env.overload()

# You can now access the variables using os.getEnv()
```

### Loading from a string

You can also load environment variables directly from a string without instantiating a `DotEnv` instance:

```nim
import dotenv, os

loadEnvFromString("""hello = world
    foo = bar
    """)
assert getEnv("foo") == "bar"
```

## Planned features

* Allow the usage of other environment variables inside variable values.
* Add validation of variable values, specifying variables have to be integer, or boolean, or a value from a predefined set.
