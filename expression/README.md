# Expression Module

Evaluates math expressions written as text, such as `"2 * (3 + 4)"`, and returns the result as an `f64`.

## Usage

Simply copy the `expression` dir or the `expression.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/expression"
```

Both functions return `(f64, Error)`. Check the error before using the value; when an error is returned the value is `-1.0e300` and should not be used.

### Evaluating

```gray
fn main(){
    mut total, totalErr = expression.evaluate("2 * (3 + 4)")      // Returns 14.0
    mut root, rootErr = expression.evaluate("sqrt(9 + 7)")        // Returns 4.0
    mut power, powerErr = expression.evaluate("2 ^ 3 ^ 2")        // Returns 512.0
    mut circle, circleErr = expression.evaluate("pi * 2 ^ 2")     // Returns 12.5664
}
```

### Variables

Names are matched to values by position. A variable takes priority over a built in constant of the same name.

```gray
fn main(){
    mut result, err = expression.evaluate_with("x * 2 + y", {"x", "y"}, {3.0, 4.0})   // Returns 10.0
}
```

### Error Handling

```gray
fn main(){
    mut result, err = expression.evaluate("10 / (5 - 5)")
    if err != nil {
        println("Error: ${err}")   // Division by zero at position 12
    }
}
```

## What an Expression Can Contain

| Part | Details |
|------|---------|
| Numbers | `12`, `3.5`, `.5`, `2e3`, `5e-1` |
| Operators | `+` `-` `*` `/` `%` `^`, from lowest to highest precedence: `+ -`, then `* / %`, then unary `-` and `+`, then `^` |
| Grouping | Parentheses. `^` groups from the right, so `2 ^ 3 ^ 2` is `2 ^ 9`, and `-2 ^ 2` is `-4` |
| Constants | `pi`, `e`, `tau` |
| Functions | `sqrt`, `cbrt`, `abs`, `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `ln`, `log2`, `log10`, `exp`, `floor`, `ceil`, `round`, `trunc` (one argument each), `pow`, `atan2`, `hypot` (two arguments), `min`, `max` (one or more arguments) |
| Spaces | Spaces, tabs and line breaks are ignored between parts |

An expression can be at most 10000 characters long and nested at most 200 levels deep.

## API Reference

| Function | Description |
|----------|-------------|
| `evaluate(text string) -> (f64, Error)` | Evaluates the passed text(text), a math expression |
| `evaluate_with(text string, names [string], values [f64]) -> (f64, Error)` | Evaluates the passed text(text) like evaluate, with each name in the passed names(names) standing for the value at the same position in the passed values(values) |

### Errors

| Code | Returned when |
|------|---------------|
| `EXPRESSION_EMPTY` | the text is empty or only spaces |
| `EXPRESSION_TOO_LONG` | the text is longer than 10000 characters |
| `EXPRESSION_TOO_DEEP` | parentheses, function calls or signs are nested more than 200 levels |
| `EXPRESSION_UNEXPECTED_CHARACTER` | a character is not allowed where it appears, such as `1 $ 2` or `1 2` |
| `EXPRESSION_UNEXPECTED_END` | the text ends where a value was expected, such as `1 +` |
| `EXPRESSION_UNBALANCED_PARENTHESES` | a `(` has no `)`, or a `)` has no `(` |
| `EXPRESSION_INVALID_NUMBER` | a number cannot be read |
| `EXPRESSION_UNKNOWN_NAME` | a name is not a variable or constant |
| `EXPRESSION_UNKNOWN_FUNCTION` | a function call names an unknown function |
| `EXPRESSION_WRONG_ARGUMENT_COUNT` | a function is called with the wrong number of arguments |
| `EXPRESSION_DIVIDE_BY_ZERO` | dividing or taking a remainder by zero, or raising 0 to a negative power |
| `EXPRESSION_DOMAIN_ERROR` | a function has no real result for its argument, such as `sqrt(-1)` or `ln(0)` |
| `EXPRESSION_RESULT_OUT_OF_RANGE` | a result overflowed |
| `EXPRESSION_VARIABLE_MISMATCH` | `evaluate_with` is passed a different number of names and values |
| `EXPRESSION_INVALID_VARIABLE` | a variable name is not a valid name, or its value is NaN or infinite |
