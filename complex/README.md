# Complex Module

Utilities for working with complex numbers made of two `f64` parts.

## Usage

Simply copy the `complex` dir or the `complex.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/complex"
```

A `Complex` has a `real` part and an `imaginary` part.

Every function returns an `Error` alongside its value. Check the error before using the value; when an error is returned the value is `-1.0e100`, or `-1.0e100` in each part of a returned complex number, and should not be used.

Each struct function that takes a complex number as its first parameter can be called either as `complex.Complex.add(a, b)` or as `a.add(b)`.

### Arithmetic

```gray
fn main(){
    mut a complex.Complex = complex.Complex{real: 3.0, imaginary: 4.0}
    mut b complex.Complex = complex.Complex{real: 1.0, imaginary: -2.0}

    mut sum, sumErr = a.add(b)                  // Returns 4.0 + 2.0i
    mut difference, differenceErr = a.subtract(b)   // Returns 2.0 + 6.0i
    mut product, productErr = a.multiply(b)     // Returns 11.0 - 2.0i
    mut quotient, quotientErr = a.divide(b)     // Returns -1.0 + 2.0i
}
```

### Magnitude and Phase

```gray
fn main(){
    mut number complex.Complex = complex.Complex{real: 3.0, imaginary: 4.0}

    mut magnitude, magnitudeErr = number.magnitude()   // Returns 5.0
    mut phase, phaseErr = number.phase()               // Returns 0.927295 radians
    mut conjugate, conjugateErr = number.conjugate()   // Returns 3.0 - 4.0i
    mut rebuilt, rebuiltErr = complex.Complex.from_polar(5.0, 0.9272952180016122)   // Returns 3.0 + 4.0i
}
```

### Roots, Exponentials and Powers

```gray
fn main(){
    mut negative complex.Complex = complex.Complex{real: -4.0, imaginary: 0.0}
    mut number complex.Complex = complex.Complex{real: 3.0, imaginary: 4.0}

    mut root, rootErr = negative.sqrt()     // Returns 0.0 + 2.0i
    mut natural, naturalErr = number.ln()   // Returns 1.60944 + 0.927295i
    mut squared, squaredErr = number.power(2.0)   // Returns -7.0 + 24.0i
}
```

### Error Handling

```gray
fn main(){
    mut one complex.Complex = complex.Complex{real: 1.0, imaginary: 1.0}
    mut zero complex.Complex = complex.Complex{real: 0.0, imaginary: 0.0}
    mut quotient, err = one.divide(zero)
    if err != nil {
        println("Error: ${err}")   // Cannot divide by a complex number of zero
    }
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `from_polar(magnitude f64, phase f64) -> (Complex, Error)` | Creates a complex number from the passed magnitude(magnitude) and phase in radians(phase) |
| `add(left Complex, right Complex) -> (Complex, Error)` | Adds the passed complex numbers(left, right) |
| `subtract(left Complex, right Complex) -> (Complex, Error)` | Subtracts the passed complex number(right) from the passed complex number(left) |
| `multiply(left Complex, right Complex) -> (Complex, Error)` | Multiplies the passed complex numbers(left, right) |
| `divide(left Complex, right Complex) -> (Complex, Error)` | Divides the passed complex number(left) by the passed complex number(right). Returns an error if right is zero |
| `negate(number Complex) -> (Complex, Error)` | Negates both parts of the passed complex number(number) |
| `conjugate(number Complex) -> (Complex, Error)` | Finds the conjugate of the passed complex number(number), which has its imaginary part negated |
| `magnitude(number Complex) -> (f64, Error)` | Calculates the magnitude of the passed complex number(number) |
| `phase(number Complex) -> (f64, Error)` | Calculates the phase in radians of the passed complex number(number). Returns an error if it is zero |
| `sqrt(number Complex) -> (Complex, Error)` | Calculates the principal square root of the passed complex number(number) |
| `exp(number Complex) -> (Complex, Error)` | Calculates e raised to the passed complex number(number) |
| `ln(number Complex) -> (Complex, Error)` | Calculates the principal natural logarithm of the passed complex number(number). Returns an error if it is zero |
| `power(number Complex, exponent f64) -> (Complex, Error)` | Raises the passed complex number(number) to the passed exponent(exponent) using its principal value. Returns an error if number is zero |

### Errors

| Code | Returned when |
|------|---------------|
| `COMPLEX_VAL_NEGATIVE` | `from_polar` is passed a negative magnitude |
| `COMPLEX_VAL_NOT_A_NUMBER` | a part is NaN |
| `COMPLEX_VAL_INFINITE` | a part is infinite |
| `COMPLEX_VAL_TOO_LARGE` | a part has a magnitude above `1.0e100` |
| `COMPLEX_DIVIDE_BY_ZERO` | `divide` is passed a divisor of zero |
| `COMPLEX_ZERO_VALUE` | `phase`, `ln` or `power` is passed zero |
| `COMPLEX_RESULT_OUT_OF_RANGE` | the result overflowed |
