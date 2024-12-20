# syntax spec

This document defines the syntax of a language for expressing programs that are sequences of finite field operations.

The terms MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are used as defined in [RFC2119](https://www.rfc-editor.org/rfc/rfc2119).

## Comments

Comments MUST begin with the `#` character and continue until the next newline character.

## Identifiers

Identifiers are names for user defined functions and variables. Identifiers MUST match the following regular express: `^[a-zA-Z\_][a-zA-Z0-9\_]*$`.

Identifiers MUST NOT be one of the following reserved words:
- `if`
- `for`
- `loop`
- `while`
- `yield`
- `continue`
- `break`
- `return`

## Scopes

Two types of scopes exist. The "global" scope, and the "local" scope.

### global

Globally scoped identifiers MAY be referenced in any local scope.

Identifiers defined in non-global scopes MUST _shadow_ globally scoped identifiers.

Globally scoped identifiers MUST only be defined during dependence resolution. Global identifiers MUST NOT collide with other global identifiers.

### local

Locally scoped identifiers are defined by the programmer in the program. Such identifiers include variables.

Local scopes must be surrounded by curly brackets `{}`.

Identifiers in a local scope MAY be referenced in nested scopes, and MUST NOT be referenced once the current scope ends.

## Functions

Each file contains a single function. Each function MAY contain a "function header" as the first non-whitespace, non-comment line in the function.

Functions are imported by the compiler and identified according to their filename. Specifically the function identifier is the filename transformed by the following javascript:

```js
// accepts a filename and calculates the function identifier
function function_name(filename) {
  return filename.split('.')[0]
}
```

An empty file is a valid function, specifically it is a `noop`.

Examples of valid functions:

```
# a no-op function
```

```
(foo, bar)
# ... do things with foo and bar
```

```
# calculate the fifth power of a
(a)
let b = a * a
return b * b * a
```

### Function header

A function header defines arguments the function receives. It consists of a set of single parentheses containing zero or more comma separated variable names. The argument list MUST NOT contain a trailing comma following the final argument.

Examples of valid function headers:

Example 1: `(arg1, arg2, arg3)`
Example 2: `()` - a function header containing no variables

Example 3:
```
(
  arg1,
  arg2,
  arg3
)
```

A function header MAY be defined for a function.

The first statement in a function MUST be the header if it is defined.

### Function returns

A function MAY use the `return` keyword to halt execution and return to the calling function.

The `return` keyword MAY be followed by an expression that will be returned to the caller.

### Function invocation

Functions MUST be invoked using their identifier followed by parentheses containing a comma separated list of variables to provide as arguments.

## Statements

Statements are lines in a program. Statements MUST end with a newline. Statements MAY contain expressions and comments.

