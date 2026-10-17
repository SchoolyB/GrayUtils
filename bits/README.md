# Bits Module

Utilities for inspecting and changing the individual bits of unsigned integers. Every function works on any unsigned integer type (`u8`, `u16`, `u32`, `u64`, `u128`, `u256`) by taking the type as its first argument.

## Usage

Simply copy the `bits` dir or the `bits.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/bits"
```

The first argument of every function is the type of the value, written as the type name. The type is never guessed from the value, so a bit count always matches the type you asked for:

```gray
fn main(){
    mut eight, eightErr = bits.count_ones(u8, 255)    // Returns 8
    mut sixteen, sixteenErr = bits.leading_zeros(u16, 0)   // Returns 16
}
```

Signed types are not supported, since the shift and rotate results would depend on the sign bit. Passing one returns `BITS_SIGNED_TYPE`.

Every function returns an `Error` alongside its value. Check the error before using the value; when an error is returned the value is `0` (or `-1` for counts, `false` for checks, and an empty string for strings) and should not be used.

### Counting Bits

```gray
fn main(){
    mut ones, onesErr = bits.count_ones(u8, 0b10110000)          // Returns 3
    mut zeros, zerosErr = bits.count_zeros(u8, 0b10110000)       // Returns 5
    mut leading, leadingErr = bits.leading_zeros(u32, 256)       // Returns 23
    mut trailing, trailingErr = bits.trailing_zeros(u8, 0b1000)  // Returns 3
    mut odd, oddErr = bits.has_odd_parity(u8, 7)                 // Returns true
}
```

### Single Bits

Bit positions start at 0 for the lowest bit.

```gray
fn main(){
    mut isSet, isSetErr = bits.is_bit_set(u8, 0b101, 2)    // Returns true
    mut raised, raisedErr = bits.set_bit(u8, 0, 7)         // Returns 128
    mut lowered, loweredErr = bits.clear_bit(u8, 255, 0)   // Returns 254
    mut flipped, flippedErr = bits.toggle_bit(u16, 5, 1)   // Returns 7
}
```

### Rotating and Reversing

```gray
fn main(){
    mut left, leftErr = bits.rotate_left(u8, 0b10010001, 1)     // Returns 0b00100011
    mut right, rightErr = bits.rotate_right(u8, 0b10010001, 1)  // Returns 0b11001000
    mut reversed, reversedErr = bits.reverse_bits(u8, 0b00001100)   // Returns 0b00110000
    mut swapped, swappedErr = bits.swap_bytes(u16, 0x1234)      // Returns 0x3412
}
```

### Extracting Bit Fields

```gray
fn main(){
    // Take 3 bits starting at bit 2 of 0b11010110
    mut field, fieldErr = bits.extract_bits(u8, 0b11010110, 2, 3)   // Returns 0b101
    mut isPower, isPowerErr = bits.is_power_of_two(u32, 64)         // Returns true
}
```

### Binary Strings

```gray
fn main(){
    mut text, textErr = bits.to_binary_string(u8, 5)             // Returns "00000101"
    mut value, valueErr = bits.from_binary_string(u8, "101")     // Returns 5
}
```

### Error Handling

```gray
fn main(){
    mut raised, err = bits.set_bit(u8, 0, 8)
    if err != nil {
        println("Error: ${err}")   // position 8 is out of range, must be from 0 to 7
    }
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `count_ones(kind, value) -> (i64, Error)` | Counts the 1 bits in the passed value(value) of the passed unsigned integer type(kind) |
| `count_zeros(kind, value) -> (i64, Error)` | Counts the 0 bits in the passed value(value) of the passed unsigned integer type(kind) |
| `leading_zeros(kind, value) -> (i64, Error)` | Counts the 0 bits before the highest 1 bit of the passed value(value). Returns the number of bits in kind when value is 0 |
| `trailing_zeros(kind, value) -> (i64, Error)` | Counts the 0 bits after the lowest 1 bit of the passed value(value). Returns the number of bits in kind when value is 0 |
| `is_bit_set(kind, value, position i64) -> (bool, Error)` | Checks if the bit at the passed position(position) of the passed value(value) is 1 |
| `set_bit(kind, value, position i64) -> (kind, Error)` | Sets the bit at the passed position(position) of the passed value(value) to 1 |
| `clear_bit(kind, value, position i64) -> (kind, Error)` | Clears the bit at the passed position(position) of the passed value(value) to 0 |
| `toggle_bit(kind, value, position i64) -> (kind, Error)` | Flips the bit at the passed position(position) of the passed value(value) |
| `rotate_left(kind, value, count i64) -> (kind, Error)` | Rotates the bits of the passed value(value) left by the passed count(count), moving the bits that leave the top back in at the bottom |
| `rotate_right(kind, value, count i64) -> (kind, Error)` | Rotates the bits of the passed value(value) right by the passed count(count), moving the bits that leave the bottom back in at the top |
| `reverse_bits(kind, value) -> (kind, Error)` | Reverses the order of the bits of the passed value(value) |
| `swap_bytes(kind, value) -> (kind, Error)` | Reverses the order of the bytes of the passed value(value), converting between little and big endian |
| `has_odd_parity(kind, value) -> (bool, Error)` | Checks if the passed value(value) has an odd number of 1 bits |
| `is_power_of_two(kind, value) -> (bool, Error)` | Checks if the passed value(value) is a power of two |
| `extract_bits(kind, value, start i64, count i64) -> (kind, Error)` | Extracts the passed count(count) of bits from the passed value(value), starting at the passed position(start) and counting up from the lowest bit |
| `to_binary_string(kind, value) -> (string, Error)` | Writes the passed value(value) as a string of 0 and 1 characters, one for every bit of kind |
| `from_binary_string(kind, text string) -> (kind, Error)` | Reads the passed text(text) of 0 and 1 characters as a value of the passed unsigned integer type(kind). The text must be from 1 to the number of bits in kind characters long |

### Errors

| Code | Returned when |
|------|---------------|
| `BITS_SIGNED_TYPE` | a signed integer type is passed as kind |
| `BITS_POSITION_OUT_OF_RANGE` | a bit position is below 0 or not below the number of bits in kind |
| `BITS_COUNT_OUT_OF_RANGE` | a rotate count is negative, or `extract_bits` is asked for a count that does not fit |
| `BITS_INVALID_BINARY_STRING` | `from_binary_string` is passed text that is empty, too long, or holds a character other than 0 or 1 |
