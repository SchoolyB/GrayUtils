# Matrix Module

Utilities for creating and working with matrices of `f64` values, including the 4x4 transform matrices used in 3D graphics.

## Usage

Simply copy the `matrix` dir or the `matrix.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/matrix"
```

A `Matrix` holds its `rows`, its `columns` and its values in `data`, stored row by row. Each side can be from 1 to 100.

Every function that can fail returns an `Error`. Check the error before using the value; when an error is returned the value is `-1.0e100`, or an empty 0x0 matrix, and should not be used.

Each struct function that takes a matrix as its first parameter can be called either as `matrix.Matrix.add(a, b)` or as `a.add(b)`.

### Creating Matrices

```gray
fn main(){
    mut zeros, zerosErr = matrix.Matrix.create(2, 3)          // 2x3 matrix of zeros
    mut identity, identityErr = matrix.Matrix.identity(3)     // 3x3 identity matrix
    mut values, valuesErr = matrix.Matrix.from_array(2, 2, {1.0, 2.0, 3.0, 4.0})
}
```

### Arithmetic

```gray
fn main(){
    mut a, aErr = matrix.Matrix.from_array(2, 2, {1.0, 2.0, 3.0, 4.0})
    mut b, bErr = matrix.Matrix.from_array(2, 2, {5.0, 6.0, 7.0, 8.0})

    mut sum, sumErr = a.add(b)                  // {6.0, 8.0, 10.0, 12.0}
    mut difference, differenceErr = b.subtract(a)   // {4.0, 4.0, 4.0, 4.0}
    mut scaled, scaledErr = a.scale(2.0)        // {2.0, 4.0, 6.0, 8.0}
    mut product, productErr = a.multiply(b)     // {19.0, 22.0, 43.0, 50.0}
    mut flipped, flippedErr = a.transpose()     // {1.0, 3.0, 2.0, 4.0}
}
```

### Determinant and Inverse

```gray
fn main(){
    mut a, aErr = matrix.Matrix.from_array(2, 2, {1.0, 2.0, 3.0, 4.0})

    mut determinant, determinantErr = a.determinant()   // Returns -2.0
    mut inverted, invertedErr = a.inverse()             // Approximately {-2.0, 1.0, 1.5, -0.5}
}
```

### Getting and Setting Values

```gray
fn main(){
    mut a, aErr = matrix.Matrix.from_array(2, 2, {1.0, 2.0, 3.0, 4.0})

    mut value, valueErr = a.get(1, 0)       // Returns 3.0
    mut setErr = a.set(0, 1, 9.0)           // a is now {1.0, 9.0, 3.0, 4.0}
}
```

### Transforms

```gray
fn main(){
    mut move, moveErr = matrix.Matrix.translation(1.0, 2.0, 3.0)
    mut grow, growErr = matrix.Matrix.scaling(2.0, 2.0, 2.0)
    mut turn, turnErr = matrix.Matrix.rotation_z(1.5707963267948966)   // 90 degrees

    mut point, pointErr = matrix.Matrix.from_array(4, 1, {1.0, 0.0, 0.0, 1.0})
    mut moved, movedErr = turn.multiply(point)   // The point rotated around the z axis
}
```

### Error Handling

```gray
fn main(){
    mut a, aErr = matrix.Matrix.from_array(2, 2, {1.0, 2.0, 2.0, 4.0})
    mut inverted, err = a.inverse()
    if err != nil {
        println("Error: ${err}")   // Matrix is singular and has no inverse
    }
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `create(rows i64, columns i64) -> (Matrix, Error)` | Creates a matrix of zeros with the passed rows(rows) and columns(columns). Each must be from 1 to 100 |
| `identity(size i64) -> (Matrix, Error)` | Creates an identity matrix with the passed size(size) as both its rows and columns |
| `from_array(rows i64, columns i64, values [f64]) -> (Matrix, Error)` | Creates a matrix with the passed rows(rows) and columns(columns) from the passed values(values), given row by row |
| `get(matrix Matrix, row i64, column i64) -> (f64, Error)` | Gets the value at the passed row(row) and column(column) of the passed matrix(matrix). Indexes start at 0 |
| `set(&matrix Matrix, row i64, column i64, value f64) -> Error` | Sets the value at the passed row(row) and column(column) of the passed matrix(matrix) to the passed value(value). Indexes start at 0 |
| `add(left Matrix, right Matrix) -> (Matrix, Error)` | Adds the passed matrices(left, right). Both must have the same size |
| `subtract(left Matrix, right Matrix) -> (Matrix, Error)` | Subtracts the passed matrix(right) from the passed matrix(left). Both must have the same size |
| `scale(matrix Matrix, factor f64) -> (Matrix, Error)` | Multiplies each value of the passed matrix(matrix) by the passed factor(factor) |
| `multiply(left Matrix, right Matrix) -> (Matrix, Error)` | Multiplies the passed matrices(left, right). The columns of left must equal the rows of right |
| `transpose(matrix Matrix) -> (Matrix, Error)` | Transposes the passed matrix(matrix), swapping its rows and columns |
| `determinant(matrix Matrix) -> (f64, Error)` | Calculates the determinant of the passed square matrix(matrix) |
| `inverse(matrix Matrix) -> (Matrix, Error)` | Inverts the passed square matrix(matrix). Returns an error if it is singular |
| `translation(x, y, z f64) -> (Matrix, Error)` | Creates a 4x4 matrix that translates by the passed x, y and z distances |
| `scaling(x, y, z f64) -> (Matrix, Error)` | Creates a 4x4 matrix that scales by the passed x, y and z factors |
| `rotation_x(angle f64) -> (Matrix, Error)` | Creates a 4x4 matrix that rotates around the x axis by the passed angle in radians(angle) |
| `rotation_y(angle f64) -> (Matrix, Error)` | Creates a 4x4 matrix that rotates around the y axis by the passed angle in radians(angle) |
| `rotation_z(angle f64) -> (Matrix, Error)` | Creates a 4x4 matrix that rotates around the z axis by the passed angle in radians(angle) |

### Errors

| Code | Returned when |
|------|---------------|
| `MATRIX_INVALID_SIZE` | rows or columns is not from 1 to 100 |
| `MATRIX_DATA_LENGTH_MISMATCH` | the number of values does not equal rows times columns |
| `MATRIX_INDEX_OUT_OF_RANGE` | `get` or `set` is passed a row or column outside the matrix |
| `MATRIX_DIMENSION_MISMATCH` | `add` or `subtract` is passed matrices of different sizes, or `multiply` is passed a left matrix whose columns do not equal the right matrix's rows |
| `MATRIX_NOT_SQUARE` | `determinant` or `inverse` is passed a matrix with a different number of rows and columns |
| `MATRIX_SINGULAR` | `inverse` is passed a matrix with no inverse |
| `MATRIX_VAL_NOT_A_NUMBER` | a value is NaN |
| `MATRIX_VAL_INFINITE` | a value is infinite |
| `MATRIX_VAL_TOO_LARGE` | a value has a magnitude above `1.0e100` |
| `MATRIX_RESULT_OUT_OF_RANGE` | the result overflowed |
