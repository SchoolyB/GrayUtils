# Style Module

ANSI terminal color and text styling utilities for CLI applications.

## Usage

Simply copy the `style` dir or the `style.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:

Note: All functions in the API return a string but handling the return value is optional as the functions are given the `#discard` attribute.

```gray
import "relative/path/to/style"
```

### Text Styling

```gray
fn main(){
    mut text = "Hello World"
    make_bold(text)     
    make_italic(text)   
    make_underline(text)
    make_dim(text)      
    make_strike(text)   
    make_reverse(text)  
}
```

### Foreground Colors

```gray
fn main(){
    mut U = "U"
    mut S = "S"
    mut A = "A"
    text_to_red(U)   
    text_to_white(S) 
    text_to_blue(A)  
}
```

### Background Colors

```gray
fn main(){
    mut text = "Hello"
    make_bg_red(text)        
    make_bg_green(text)      
    make_bg_bright_cyan(text)
}
```

## API Reference

### Text Style Functions

| Function | Description |
|----------|-------------|
| `make_bold(t string) -> string` | Make text bold |
| `make_dim(t string) -> string` | Make text dim |
| `make_italic(t string) -> string` | Make text italic |
| `make_underline(t string) -> string` | Make text underlined |
| `make_reverse(t string) -> string` | Swap foreground/background colors |
| `make_strike(t string) -> string` | Strike through text |
| `reset() -> string` | Reset all styling |

### Foreground Color Functions

| Function | Description |
|----------|-------------|
| `text_to_black(t string) -> string` | Black text |
| `text_to_red(t string) -> string` | Red text |
| `text_to_green(t string) -> string` | Green text |
| `text_to_yellow(t string) -> string` | Yellow text |
| `text_to_blue(t string) -> string` | Blue text |
| `text_to_magenta(t string) -> string` | Magenta text |
| `text_to_cyan(t string) -> string` | Cyan text |
| `text_to_white(t string) -> string` | White text |

### Bright Foreground Color Functions

| Function | Description |
|----------|-------------|
| `text_to_bright_red(t string) -> string` | Bright red text |
| `text_to_bright_green(t string) -> string` | Bright green text |
| `text_to_bright_yellow(t string) -> string` | Bright yellow text |
| `text_to_bright_blue(t string) -> string` | Bright blue text |
| `text_to_bright_magenta(t string) -> string` | Bright magenta text |
| `text_to_bright_cyan(t string) -> string` | Bright cyan text |
| `text_to_bright_white(t string) -> string` | Bright white text |

### Background Color Functions

| Function | Description |
|----------|-------------|
| `bg_to_black(t string) -> string` | Black background |
| `bg_to_red(t string) -> string` | Red background |
| `bg_to_green(t string) -> string` | Green background |
| `bg_to_yellow(t string) -> string` | Yellow background |
| `bg_to_blue(t string) -> string` | Blue background |
| `bg_to_magenta(t string) -> string` | Magenta background |
| `bg_to_cyan(t string) -> string` | Cyan background |
| `bg_to_white(t string) -> string` | White background |

### Bright Background Color Functions

| Function | Description |
|----------|-------------|
| `bg_to_bright_black(t string) -> string` | Bright black background |
| `bg_to_bright_red(t string) -> string` | Bright red background |
| `bg_to_bright_green(t string) -> string` | Bright green background |
| `bg_to_bright_yellow(t string) -> string` | Bright yellow background |
| `bg_to_bright_blue(t string) -> string` | Bright blue background |
| `bg_to_bright_magenta(t string) -> string` | Bright magenta background |
| `bg_to_bright_cyan(t string) -> string` | Bright cyan background |
| `bg_to_bright_white(t string) -> string` | Bright white background |

### Constants

Text style constants: `RESET`, `BOLD`, `DIM`, `ITALIC`, `UNDERLINE`, `REVERSE`, `STRIKE`

Foreground color constants: `TEXT_BLACK`, `TEXT_RED`, `TEXT_GREEN`, `TEXT_YELLOW`, `TEXT_BLUE`, `TEXT_MAGENTA`, `TEXT_CYAN`, `TEXT_WHITE`

Bright foreground color constants: `TEXT_BRIGHT_BLACK`, `TEXT_BRIGHT_RED`, `TEXT_BRIGHT_GREEN`, `TEXT_BRIGHT_YELLOW`, `TEXT_BRIGHT_BLUE`, `TEXT_BRIGHT_MAGENTA`, `TEXT_BRIGHT_CYAN`, `TEXT_BRIGHT_WHITE`

Background color constants: `BG_BLACK`, `BG_RED`, `BG_GREEN`, `BG_YELLOW`, `BG_BLUE`, `BG_MAGENTA`, `BG_CYAN`, `BG_WHITE`

Bright background color constants: `BG_BRIGHT_BLACK`, `BG_BRIGHT_RED`, `BG_BRIGHT_GREEN`, `BG_BRIGHT_YELLOW`, `BG_BRIGHT_BLUE`, `BG_BRIGHT_MAGENTA`, `BG_BRIGHT_CYAN`, `BG_BRIGHT_WHITE`
