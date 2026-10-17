# Vector Module

Utilities for working with 2D and 3D vectors with `f64` components.

## Usage

Simply copy the `vector` dir or the `vector.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/vector"
```

Every function returns an `Error` alongside its value. Check the error before using the value; when an error is returned the value (or each component of a returned vector) is `-1.0e100` and should not be used.

Each struct function takes the vector as its first parameter, so it can be called either as `vector.Vector2D.add(a, b)` or as `a.add(b)`.

### Vector2D

```gray
fn main(){
    mut a vector.Vector2D = vector.Vector2D{x: 3.0, y: 4.0}
    mut b vector.Vector2D = vector.Vector2D{x: 1.0, y: -2.0}

    mut sum, sumErr = a.add(b)                // Returns Vector2D{x: 4.0, y: 2.0}
    mut length, lengthErr = a.length()        // Returns 5.0
    mut dot, dotErr = a.dot(b)                // Returns -5.0
    mut unit, unitErr = a.normalize()         // Returns Vector2D{x: 0.6, y: 0.8}
    mut angle, angleErr = a.angle_between(b)  // Returns the angle in radians
}
```

### Vector3D

```gray
fn main(){
    mut a vector.Vector3D = vector.Vector3D{x: 1.0, y: 0.0, z: 0.0}
    mut b vector.Vector3D = vector.Vector3D{x: 0.0, y: 1.0, z: 0.0}

    mut cross, crossErr = a.cross(b)   // Returns Vector3D{x: 0.0, y: 0.0, z: 1.0}
}
```

### Error Handling

```gray
fn main(){
    mut zero vector.Vector2D = vector.Vector2D{x: 0.0, y: 0.0}
    mut unit, err = zero.normalize()
    if err != nil {
        println("Error: ${err}")   // Vector has a length of 0.0
    }
}
```

## API Reference

`Vector2D` has the fields `x` and `y`. `Vector3D` has the fields `x`, `y` and `z`. The functions below exist on both unless noted.

| Function | Description |
|----------|-------------|
| `add(left, right) -> (Vector, Error)` | Adds the passed vectors(left, right) |
| `subtract(left, right) -> (Vector, Error)` | Subtracts the passed vector(right) from the passed vector(left) |
| `scale(vector, factor f64) -> (Vector, Error)` | Multiplies each component of the passed vector(vector) by the passed factor(factor) |
| `negate(vector) -> (Vector, Error)` | Negates each component of the passed vector(vector) |
| `dot(left, right) -> (f64, Error)` | Calculates the dot product of the passed vectors(left, right) |
| `length_squared(vector) -> (f64, Error)` | Calculates the squared length of the passed vector(vector) |
| `length(vector) -> (f64, Error)` | Calculates the length of the passed vector(vector) |
| `distance(first, second) -> (f64, Error)` | Calculates the distance between the passed vectors(first, second) |
| `normalize(vector) -> (Vector, Error)` | Scales the passed vector(vector) to a length of 1.0. Returns an error if its length is 0.0 |
| `angle_between(left, right) -> (f64, Error)` | Calculates the angle in radians between the passed vectors(left, right). Returns an error if either has a length of 0.0 |
| `cross(left, right) -> (Vector3D, Error)` | Calculates the cross product of the passed vectors(left, right). `Vector3D` only |

### Errors

| Code | Returned when |
|------|---------------|
| `VECTOR_VAL_NOT_A_NUMBER` | a component is NaN |
| `VECTOR_VAL_INFINITE` | a component is infinite |
| `VECTOR_VAL_TOO_LARGE` | a component has a magnitude above `1.0e100` |
| `VECTOR_ZERO_LENGTH` | `normalize` or `angle_between` is passed a vector with a length of 0.0 |
