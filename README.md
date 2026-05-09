# Rux Standard Library

The standard library for the [Rux](https://rux-lang.dev) programming language.

## Modules

### `Std`

Core types and utilities available by default.

| Type / Function        | Description                                                            |
| ---------------------- | ---------------------------------------------------------------------- |
| `String`               | Heap-allocated UTF-8 string with `From`, `+`, `Data`, `Length`         |
| `StringBuilder`        | Growable buffer with `New`, `Append` (char, slice, String), `ToString` |
| `Display`              | Interface requiring `ToString() -> String`                             |
| `Format(fmt, args...)` | String interpolation using `{}` placeholders                           |
| `Exit(code)`           | Terminate the process with an exit code                                |

### `Std::Math`

Mathematical constants and functions.

| Symbol                          | Description                                     |
| ------------------------------- | ----------------------------------------------- |
| `Pi`, `E`                       | Mathematical constants (`float64`)              |
| `Sin`, `Cos`, `Tan`             | Trigonometric functions (`float32` / `float64`) |
| `Sqrt`, `Pow`                   | Square root and exponentiation                  |
| `Abs`, `Floor`, `Ceil`, `Round` | Rounding and absolute value                     |

### `Std::Memory`

Low-level heap memory management backed by the Windows process heap.

| Function                | Description                    |
| ----------------------- | ------------------------------ |
| `Alloc(size)`           | Allocate `size` bytes          |
| `Realloc(ptr, size)`    | Resize an existing allocation  |
| `Free(ptr)`             | Release an allocation          |
| `Copy(dest, src, len)`  | Copy `len` bytes               |
| `Zero(ptr, size)`       | Zero-fill `size` bytes         |
| `Set(ptr, size, value)` | Fill `size` bytes with `value` |

### `Std::Io`

Console I/O.

| Function              | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| `Print(str: String)`  | Write a string to stdout (UTF-8 → UTF-16 via `WriteConsoleW`) |
| `Print(fmt, args...)` | Format and print (work in progress)                           |

## `Display` implementations

The following built-in types implement the `Display` interface and can be passed to `Format` / `Print`:

- `bool8` — renders as `"true"` or `"false"`
- `int64` — decimal integer rendering
- `float64` — decimal with 6 fractional digits, plus `"NaN"`, `"Inf"`, `"-Inf"`

## Usage

```rux
import Std::{ String, Format };
import Std::Io::Print;

func Main() {
    let msg = Format("Hello, {}!", "world");
    Print(msg);
}
```

## Requirements

- Windows (memory and I/O modules depend on the `Windows` package)

## License

MIT — see [LICENSE](LICENSE).
