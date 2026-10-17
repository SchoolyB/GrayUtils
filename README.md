# GrayUtils

A collection of utility modules for the Grayscale programming language: unit conversions, statistics, vectors, matrices, geometry, complex numbers, finance, bit manipulation, expression evaluation, color conversion and terminal styling.

## Usage

Copy the module directory you want into your project, then import it at the top of the file that uses it:

```gray
import "relative/path/to/units"

fn main(){
    mut kilometers, err = units.mile_to_kilometer(5.0)
    if err != nil {
        println("Error: ${err}")
    } otherwise {
        println(kilometers)   // 8.0467
    }
}
```

Each module is a directory and can be copied on its own. Every module has a README with usage examples and a full API reference.

## Error handling

Functions that can fail return an `Error` alongside their value, and every error has its own error code so you can tell what went wrong:

```gray
mut result, err = expression.evaluate("10 / (5 - 5)")
if err != nil {
    when err.code {
        is .EXPRESSION_DIVIDE_BY_ZERO { println("cannot divide by zero") }
        default                       { println(err.msg) }
    }
}
```

When an error is returned, the value should not be used.

## Modules

| Module | What it does |
|--------|--------------|
| [units](units/README.md) | Conversions for length, area, weight, volume, temperature, speed, data size, pressure, energy, power, angle and frequency, plus basic physics calculations |
| [stats](stats/README.md) | Mean, median, mode, variance, standard deviation and percentiles |
| [vector](vector/README.md) | 2D and 3D vectors: add, scale, dot, cross, length, normalize and angle between |
| [matrix](matrix/README.md) | Matrices with multiply, transpose, determinant, inverse and 4x4 transforms |
| [geometry](geometry/README.md) | Volume and surface area of solids, and points, rectangles, circles and line segments |
| [complex](complex/README.md) | Complex numbers: arithmetic, magnitude, phase, roots, exponentials and powers |
| [finance](finance/README.md) | Interest, loan payments and schedules, present and future value, and growth rates |
| [bits](bits/README.md) | Counting, setting, rotating and reversing the bits of unsigned integers |
| [expression](expression/README.md) | Evaluates math expressions written as text, such as `"2 * (3 + 4)"` |
| [color](color/README.md) | Converts between RGB values and hex strings |
| [style](style/README.md) | ANSI colors and text styling for terminal output |
