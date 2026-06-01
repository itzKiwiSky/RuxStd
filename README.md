# Rux Standard Library

The standard library for the Rux programming language.

The standard library provides core types, string manipulation, console I/O, memory management, formatting, random number generation, assertions, and platform abstractions.

## Modules

| Module        | Description                          |
| ------------- | ------------------------------------ |
| `Std`         | Core types and utilities             |
| `Std::Io`     | Console input and output             |
| `Std::Memory` | Low-level memory management          |
| `Std::Math`   | Mathematical constants and functions |
| `Std::Time`   | Time utilities                       |
| `Std::Random` | Pseudo-random number generation      |

---

# Std

Core types and utilities available by default.

## String

Heap-allocated UTF-8 string.

### Constructors

| Function                    | Description                |
| --------------------------- | -------------------------- |
| `String::New()`             | Create an empty string     |
| `String::From(ptr, length)` | Copy from raw UTF-8 memory |
| `String::From(slice)`       | Copy from a UTF-8 slice    |

### Basic Operations

| Function        | Description           |
| --------------- | --------------------- |
| `Clone()`       | Deep copy             |
| `Data()`        | Raw UTF-8 pointer     |
| `Length()`      | Length in bytes       |
| `IsEmpty()`     | Returns true if empty |
| `Repeat(count)` | Repeat string         |
| `+(String)`     | Concatenate strings   |
| `+(char8[])`    | Concatenate slice     |

### String Utilities

| Function             | Description                   |
| -------------------- | ----------------------------- |
| `ToUpper()`          | Convert to uppercase          |
| `ToLower()`          | Convert to lowercase          |
| `Capitalize()`       | Capitalize first letter       |
| `TitleCase()`        | Convert words to title case   |
| `Trim()`             | Remove surrounding whitespace |
| `Split(delimiter)`   | Split into a StringArray      |
| `StartsWith(prefix)` | Prefix test                   |
| `EndsWith(suffix)`   | Suffix test                   |

### In-place Utilities

| Function              | Description         |
| --------------------- | ------------------- |
| `ToUpperInPlace()`    | Uppercase in place  |
| `ToLowerInPlace()`    | Lowercase in place  |
| `CapitalizeInPlace()` | Capitalize in place |
| `TitleCaseInPlace()`  | Title case in place |
| `TrimInPlace()`       | Trim in place       |

---

## StringArray

Returned by `String::Split()`.

```rux
struct StringArray {
    data: *String;
    length: uint;
}
```

---

## StringBuilder

Growable UTF-8 string builder.

| Function                                | Description                 |
| --------------------------------------- | --------------------------- |
| `StringBuilder::New()`                  | Create empty builder        |
| `StringBuilder::WithCapacity(capacity)` | Preallocate capacity        |
| `Capacity()`                            | Current capacity            |
| `Length()`                              | Current length              |
| `IsEmpty()`                             | Returns true if empty       |
| `Reserve(additional)`                   | Reserve additional capacity |
| `Shrink()`                              | Shrink to fit               |
| `Append(char8)`                         | Append character            |
| `Append(char8[])`                       | Append slice                |
| `Append(String)`                        | Append string               |
| `ToString()`                            | Copy into String            |
| `IntoString()`                          | Move into String            |

---

## Display

Interface implemented by printable types.

```rux
interface Display {
    func ToString() -> String;
}
```

---

## Free Functions

| Function                     | Description                               |
| ---------------------------- | ----------------------------------------- |
| `Format(fmt, args...)`       | Format string using `{}` placeholders     |
| `ToString(value)`            | Convert value to String                   |
| `Exit(code)`                 | Exit process                              |
| `Fatal(message)`             | Print fatal error and terminate           |
| `Assert(condition, message)` | Assert condition and terminate on failure |

`Fatal` and `Assert` automatically include file, function, line, and column information.

---

# Std::Io

Console input and output.

## Input

| Function     | Description                    |
| ------------ | ------------------------------ |
| `ReadLine()` | Read one UTF-8 line from stdin |

## Output

| Function           | Description             |
| ------------------ | ----------------------- |
| `Print(value)`     | Print value             |
| `PrintLine(value)` | Print value and newline |
| `PrintLine()`      | Print newline           |

Supported types:

* bool8
* bool16
* bool32
* bool
* int8
* int16
* int32
* int64
* uint8
* uint16
* uint32
* uint64
* float32
* float64
* char8
* char16
* char32
* char
* String
* char8[]
* char16[]
* formatted strings using `Display`

---

# Std::Memory

Platform-backed heap memory management.

| Function                    | Description        |
| --------------------------- | ------------------ |
| `Alloc(size)`               | Allocate memory    |
| `Realloc(ptr, size)`        | Resize allocation  |
| `Free(ptr)`                 | Release allocation |
| `Copy(dest, src, length)`   | Copy bytes         |
| `Zero(ptr, size)`           | Fill with zeros    |
| `Set(ptr, size, value)`     | Fill with value    |
| `Compare(lhs, rhs, length)` | Compare memory     |

---

# Std::Math

Mathematical constants and functions.

## Constants

| Constant | Value          |
| -------- | -------------- |
| `Pi`     | π              |
| `E`      | Euler's number |

## Functions

| Function  |
| --------- |
| `Add()`   |
| `Abs()`   |
| `Sin()`   |
| `Cos()`   |
| `Tan()`   |
| `Sqrt()`  |
| `Pow()`   |
| `Floor()` |
| `Ceil()`  |
| `Round()` |

> ⚠️ Several mathematical functions are currently placeholder implementations and may return incorrect values until fully implemented.

---

# Std::Time

Time utilities.

Currently implemented only on Windows.

| Function                | Description             |
| ----------------------- | ----------------------- |
| `SleepMs(ms)`           | Sleep milliseconds      |
| `SleepSeconds(seconds)` | Sleep seconds           |
| `SleepMinutes(minutes)` | Sleep minutes           |
| `TickMs()`              | Milliseconds since boot |
| `LocalTime()`           | Local system time       |
| `UtcTime()`             | UTC system time         |

---

# Std::Random

Pseudo-random number generation using xorshift64.

All functions require a seed.

Using the same seed produces the same sequence.

| Function          |
| ----------------- |
| `RandomInt8()`    |
| `RandomInt16()`   |
| `RandomInt32()`   |
| `RandomInt64()`   |
| `RandomInt()`     |
| `RandomUInt8()`   |
| `RandomUInt16()`  |
| `RandomUInt32()`  |
| `RandomUInt64()`  |
| `RandomUInt()`    |
| `RandomFloat32()` |
| `RandomFloat64()` |
| `RandomBool()`    |

---

# Display Implementations

The following types implement `Display`:

* bool8
* bool16
* bool32
* int8
* int16
* int32
* int64
* uint8
* uint16
* uint32
* uint64
* float32
* float64
* char8
* char16
* char32
* String

---

# Examples

## Hello World

```rux
import Std::Io::PrintLine;

func Main() {
    PrintLine("Hello, World!");
}
```

## String Formatting

```rux
import Std::{ Format };
import Std::Io::PrintLine;

func Main() {
    PrintLine(
        Format("Hello {}!", "Rux")
    );
}
```

## String Utilities

```rux
import Std::Io::PrintLine;

func Main() {
    let text = String::From("hello world");

    PrintLine(text.ToUpper());
    PrintLine(text.TitleCase());
}
```

## StringBuilder

```rux
import Std::{ StringBuilder };
import Std::Io::PrintLine;

func Main() {
    let builder = StringBuilder::New();

    builder.Append("Hello ");
    builder.Append("World");

    PrintLine(builder.ToString());
}
```

## Random Numbers

```rux
import Std::Random::*;
import Std::Io::PrintLine;

func Main() {
    PrintLine(
        RandomInt(1, 100, 12345)
    );
}
```

---

# Platform Support

Target-specific implementations are selected using the `@[Target(...)]` attribute.

| Module      | Linux | Windows | macOS   | BSD     | Illumos |
| ----------- | ----- | ------- | ------- | ------- | ------- |
| Std::Memory | ✓     | ✓       | Planned | Planned | Planned |
| Std::Io     | ✓     | ✓       | Planned | Planned | Planned |
| Std (Exit)  | ✓     | ✓       | Planned | Planned | Planned |
| Std::Time   | —     | ✓       | —       | —       | —       |

---

## License

MIT
