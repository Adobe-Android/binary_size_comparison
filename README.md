# binary_size_comparison
Binary size comparison across various programming languages

## C++
**Note:** *Extraneous information like BuildID omitted from file output.*

**Note:** *Windows build 2004 and/or Ubuntu Linux 20.04 used for testing.*
#### Idiomatic
```cpp
#include <iostream>

int main()
{
    std::string hello_str = "Hello C++\n";
    std::cout << hello_str;
}
```

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - amd64 - Windows 64-bit release build

**Size:**
12K

**Size (with UPX compression):**
9K

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

**Size:**
11K

**Size (with UPX compression):**
8K

**Compiler:**
GCC 9.3 - Linux 64-bit build

**Size:**
18K

**Command:**
```sh
strip hello
```

**Size (stripped binary):**
15K

#### C-style
```cpp
#include <cstdio>

int main()
{
    char hello_str[] = "Hello C++\n";
    printf("%s", hello_str);
}
```

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - amd64 - Windows 64-bit release build

**Size:**
11K

**Size (with UPX compression):**
8K

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

**Size:**
9K

**Size (with UPX compression):**
7K

**Compiler:**
GCC 9.3 - Linux 64-bit build

**Size:**
17K

**Command:**
```sh
strip hello
```

**Size (stripped binary):**
15K

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
```

## Rust
```rust
fn main() {
    println!("Hello, world!");
}
```

**Compiler:**
Rustc 1.43 - Linux 64-bit release build

**Command:**
```sh
cargo build --release
```

**Size:**
7M

**Size (with LTO):**
1.8M

**Command:**
```sh
strip hello
```

**Size (with LTO and stripped):**
195K

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
```

## Go
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world!")
}
```

**Compiler:**
Go 1.13 - Linux 64-bit build

**Size:**
2.0M

**Size (-ldflags "-s -w"):**
1.4M

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped
```

## Zig
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().outStream();
    try stdout.print("Hello, {}!\n", .{"world"});
}
```

**Compiler:**
Zig 0.6.0 - Linux 64-bit release build

Command:
```sh
zig build-exe hello.zig --release-fast
```

**Size (--release-fast):**
167K

Command:
```sh
zig build-exe hello.zig --release-small
```

**Size (--release-small):**
78K

**Output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, with debug_info, not stripped
```

Command:
```sh
strip hello
```

**Size (--release-small and stripped):**
5.3K

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped
```

# Resources:
* http://timelessname.com/elfbin/
* https://lifthrasiir.github.io/rustlog/why-is-a-rust-executable-large.html
* https://stackoverflow.com/questions/3861634/how-to-reduce-compiled-file-size
* https://golang.org/cmd/link/
* https://ziglang.org/documentation/0.6.0/
* https://ziglang.org/documentation/0.6.0/#Build-Mode
