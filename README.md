# Rux Standard Library

The standard library for the [Rux](https://rux-lang.dev) programming language.

## Modules

### `Std`

Core types and utilities available by default.

#### `String`

Heap-allocated UTF-8 string.

| Method / Function                        | Description                               |
| ---------------------------------------- | ----------------------------------------- |
| `String::New() -> String`                | Create an empty string                    |
| `String::From(ptr, length) -> String`    | Copy from a raw `*const char8` pointer    |
| `String::From(slice: char8[]) -> String` | Copy from a `char8` slice                 |
| `Clone(self) -> String`                  | Deep-copy the string                      |
| `+(self, other: String) -> String`       | Concatenate two strings                   |
| `+(self, slice: char8[]) -> String`      | Concatenate a string with a `char8` slice |
| `Data(self) -> *char8`                   | Raw pointer to the UTF-8 bytes            |
| `Length(self) -> uint`                   | Byte length                               |
| `IsEmpty(self) -> bool8`                 | Returns `true` if length is zero          |
| `Repeat(self, count: uint) -> String`    | Repeat the string `count` times           |

#### `StringBuilder`

Growable UTF-8 string buffer.

| Method / Function                                 | Description                                      |
| ------------------------------------------------- | ------------------------------------------------ |
| `StringBuilder::New() -> StringBuilder`           | Create an empty builder                          |
| `StringBuilder::WithCapacity(n) -> StringBuilder` | Create a builder with pre-allocated capacity     |
| `Capacity(self) -> uint`                          | Current allocated capacity in bytes              |
| `Reserve(self, additional: uint)`                 | Ensure room for at least `additional` more bytes |
| `Shrink(self)`                                    | Release excess capacity                          |
| `Append(self, c: char8)`                          | Append a single byte                             |
| `Append(self, str: char8[])`                      | Append a `char8` slice                           |
| `Append(self, str: String)`                       | Append a `String`                                |
| `ToString(self) -> String`                        | Copy the buffer into a new `String`              |
| `IntoString(self) -> String`                      | Move the buffer into a `String` (resets builder) |
| `Length(self) -> uint`                            | Current byte length                              |
| `IsEmpty(self) -> bool8`                          | Returns `true` if length is zero                 |
| `Clear(self)`                                     | Reset length to zero without freeing memory      |

#### `Display`

Interface for types that can be converted to a string.

```rux
interface Display {
    func ToString() -> String;
}
```

#### Free functions

| Function                                           | Description                                                        |
| -------------------------------------------------- | ------------------------------------------------------------------ |
| `Format(fmt: char8[], args: Display...) -> String` | String interpolation using `{}` placeholders                       |
| `ToString(value) -> String`                        | Convert any primitive type to a `String` (see Display section)     |
| `Exit(code: uint32)`                               | Terminate the process with an exit code                            |
| `Fatal(message: char8[])`                          | Print a fatal error with location info and exit with code 1        |
| `Assert(condition: bool, message: char8[])`        | Print an assertion failure with location info and exit with code 2 |

`Fatal` and `Assert` automatically capture the source file, function name, line, and column.

---

### `Std::Io`

Console I/O.

| Function               | Description                                    |
| ---------------------- | ---------------------------------------------- |
| `Print(value)`         | Write a value to stdout (see overloads below)  |
| `PrintLine(value)`     | Write a value followed by a newline to stdout  |
| `PrintLine()`          | Write a newline to stdout                      |
| `ReadLine() -> String` | Read one UTF-8 line from stdin (strips `\r\n`) |

`Print` and `PrintLine` accept: `int8`, `int16`, `int32`, `int64`, `uint8`, `uint16`, `uint32`, `uint64`, `float32`, `float64`, `bool8`, `bool16`, `bool32`, `bool`, `char8`, `char16`, `char32`, `char`, `*const char8` + length, `*const char16` + length, `char8[]`, `char16[]`, `String`, and `(format: char8[], args: Display...)` for formatted output.

---

### `Std::Memory`

Low-level heap memory management backed by the target platform package.

| Function                                                 | Description                                              |
| -------------------------------------------------------- | -------------------------------------------------------- |
| `Alloc(size: uint) -> *opaque`                           | Allocate `size` bytes                                    |
| `Realloc(ptr: *opaque, size: uint) -> *opaque`           | Resize an existing allocation                            |
| `Free(ptr: *opaque)`                                     | Release an allocation                                    |
| `Copy(dest, src: *const opaque, length: uint)`           | Copy `length` bytes from `src` to `dest`                 |
| `Zero(ptr: *opaque, size: uint)`                         | Zero-fill `size` bytes                                   |
| `Set(ptr: *opaque, size: uint, value: int32)`            | Fill `size` bytes with `value`                           |
| `Compare(lhs, rhs: *const opaque, length: uint) -> uint` | Compare `length` bytes; returns number of matching bytes |

---

### `Std::Math`

Mathematical constants and functions.

| Symbol                       | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| `Pi`, `E`                    | Mathematical constants (`float64`)                     |
| `Sin`, `Cos`, `Tan`          | Trigonometric functions (`float32` / `float64`)        |
| `Sqrt`, `Pow`                | Square root and exponentiation (`float32` / `float64`) |
| `Abs`, `Floor`, `Ceil`       | Rounding and absolute value (`float32` / `float64`)    |
| `Round`                      | Round to nearest integer (`float64` only)              |
| `Add(x: int, y: int) -> int` | Integer addition                                       |

---

### `Std::Time`

Time utilities and sleep functions. Currently implemented for **Windows** only.

| Function                        | Description                            |
| ------------------------------- | -------------------------------------- |
| `SleepMs(ms: uint32)`           | Sleep for `ms` milliseconds            |
| `SleepSeconds(seconds: uint32)` | Sleep for `seconds` seconds            |
| `SleepMinutes(minutes: uint32)` | Sleep for `minutes` minutes            |
| `TickMs() -> uint64`            | Milliseconds elapsed since system boot |
| `LocalTime() -> SystemTime`     | Current local date and time            |
| `UtcTime() -> SystemTime`       | Current UTC date and time              |

---

### `Std::Random`

Pseudo-random number generation using xorshift64.

All functions take a `seed: uint64` parameter.

| Function                                              | Description                             |
| ----------------------------------------------------- | --------------------------------------- |
| `RandomInt8/16/32/64/Int(min, max, seed) -> T`        | Random signed integer in `[min, max)`   |
| `RandomUInt8/16/32/64/UInt(min, max, seed) -> T`      | Random unsigned integer in `[min, max)` |
| `RandomFloat32/64(min: T, max: T, seed: uint64) -> T` | Random float in `[min, max)`            |
| `RandomBool(seed: uint64) -> bool`                    | Random boolean                          |

---

## `Display` implementations

The following types implement the `Display` interface and can be passed to `Format` / `Print`:

| Type                                  | Rendered as                                                                                                   |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `bool8`, `bool16`, `bool32`           | `"true"` or `"false"`                                                                                         |
| `int8`, `int16`, `int32`, `int64`     | Signed decimal integer                                                                                        |
| `uint8`, `uint16`, `uint32`, `uint64` | Unsigned decimal integer                                                                                      |
| `float32`                             | Decimal (delegated to `float64` formatting)                                                                   |
| `float64`                             | Decimal with 6 fractional digits; scientific notation for very large/small values; `"NaN"`, `"Inf"`, `"-Inf"` |
| `char8`                               | Single UTF-8 byte as a string                                                                                 |
| `char16`                              | UTF-8 encoded character (1–3 bytes)                                                                           |
| `char32`                              | UTF-8 encoded character (1–4 bytes)                                                                           |
| `String`                              | Returns self                                                                                                  |

## Usage

```rux
import Std::{ String, Format };
import Std::Io::{ Print, PrintLine };

func Main() {
    let msg = Format("Hello, {}!", "world");
    PrintLine(msg);
}
```

## Platform support

Target-specific implementations are selected at compile time via the `@[Target("...")]` attribute.

| Module        | Linux | Windows | macOS | BSD  | Illumos |
| ------------- | ----- | ------- | ----- | ---- | ------- |
| `Std::Memory` | ✓     | ✓       | stub  | stub | stub    |
| `Std::Io`     | ✓     | ✓       | stub  | stub | stub    |
| `Std` (Exit)  | ✓     | ✓       | stub  | stub | stub    |
| `Std::Time`   | —     | ✓       | —     | —    | —       |

Platform packages are resolved via `[Dependencies.Target.*]` in `Rux.toml`.

## Requirements

- Rux compiler with target-specific dependency support.
- The target platform package resolved through `Rux.toml` (`BSD`, `Illumos`, `Linux`, `MacOS`, or `Windows`).

## License

[MIT](LICENSE)
