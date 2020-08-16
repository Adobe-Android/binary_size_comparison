# binary_size_comparison
Binary size comparison across various programming languages

## C++
Note: *C++ binaries will be much smaller than other languages becuase they are linked dynamically by default rather than statically.*

#### Idiomatic
```cpp
#include <iostream>

int main()
{
    std::string hello_str = "Hello C++\n";
    std::cout << hello_str;
}
```

Compiler:
Visual Studio 2019 16.7 - MSVC 19.27 - amd64 - Windows 64-bit release build

Size:
12K

Size (with UPX compression):
9K

Compiler:
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

Size:
11K

Size (with UPX compression):
8K

Compiler:
GCC 9.3 - Linux 64-bit build

Size:
24K

Size (stripped binary):
19K

#### C-style
```cpp
#include <cstdio>

int main()
{
    char hello_str[] = "Hello C++\n";
    printf("%s", hello_str);
}
```

Compiler:
Visual Studio 2019 16.7 - MSVC 19.27 - amd64 - Windows 64-bit release build

Size:
11K

Size (with UPX compression):
8K

Compiler:
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

Size:
9K

Size (with UPX compression):
7K

Compiler:
GCC 9.3 - Linux 64-bit build

Size:
17K

Size (stripped binary):
15K

## Rust
```rust
fn main() {
    println!("Hello, world!");
}
```

Compiler:
Rustc 1.43 - Linux 64-bit release build

Size:
7M

Size (with LTO):
1.8M

Size (with LTO and stripped):
195K

## Go
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world!")
}
```

Compiler:
Go 1.13 - Linux 64-bit build

Size:
2.0M

Size (-ldflags "-s -w"):
1.4M

## Zig
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().outStream();
    try stdout.print("Hello, {}!\n", .{"world"});
}
```

Compiler:
Zig 0.6.0 - Linux 64-bit release build

Size (--release-fast):
167K

Size (--release-small):
78K

# Resources:
* http://timelessname.com/elfbin/
* https://lifthrasiir.github.io/rustlog/why-is-a-rust-executable-large.html
* https://stackoverflow.com/questions/3861634/how-to-reduce-compiled-file-size
* https://golang.org/cmd/link/
* https://ziglang.org/documentation/0.6.0/
* https://ziglang.org/documentation/0.6.0/#Build-Mode
