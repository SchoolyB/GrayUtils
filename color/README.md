# Color Module

Utilities for working with colors and converting between RGB, hex, and byte color representations.

## Usage

Simply copy the `color` dir or the `color.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/color"
```

### RGB to Hex

```gray
fn main(){
  mut hex = rgb_to_hex(255, 128, 0) 
  println(hex)
  //Output: FF8000
}
```

### Hex to RGB

```gray
fn main(){
    mut r, g, b = hex_to_rgb("FF8000")
    println(r) //Output: 255 
    println(g) //Output: 128
    println(b) //Output: 0
}
```

### Byte to Hex

```gray
fn main(){
    mut high, low = byte_to_hex(255)
    println(high) //Output: 'F'
    println(low)  //Output: 'F'
}
```

### Hex to Byte

```gray
fn main(){
    mut byte = hex_to_byte('F', 'F')
    println(byte) //Output: 255
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `rgb_to_hex(r, g, b u8) -> string` | Convert RGB values to a hex string |
| `hex_to_rgb(hex string) -> (u8, u8, u8)` | Convert a hex string to RGB values |
| `byte_to_hex(b u8) -> (char, char)` | Convert a byte to two hex characters |
| `hex_to_byte(high, low char) -> u8` | Convert two hex characters to a byte |
