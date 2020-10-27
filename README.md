# binary size comparison
A binary size comparison across various programming languages

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

<br/>

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - amd64 - Windows 64-bit release build

**Size:**
12K

**Size (with UPX compression):**
9K

<br/>

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

**Size:**
11K

**Size (with UPX compression):**
8K

<br/>

**Compiler:**
GCC 9.3 - Linux 64-bit build

**Size:**
18K

**Size (stripped binary):**
15K

**Command:**
```sh
strip hello
```

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
```

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

<br/>

**Compiler:**
Visual Studio 2019 16.7 - MSVC 19.27 - x86 - Windows 32-bit release build

**Size:**
9K

**Size (with UPX compression):**
7K

<br/>

**Compiler:**
GCC 9.3 - Linux 64-bit build

**Size:**
17K

**Size (stripped binary):**
15K

**Command:**
```sh
strip hello
```

**Final output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
```

**Further detail on GCC compilation:**
Please see [Note 1](#note-1)

**Further reduce binary size leveraging musl libc:**
Please see [Note 2](#note-2)

**Get truly small binaries with some trade-offs:**
Please see [Note 3](#note-3)

## Rust
```rust
fn main() {
    println!("Hello, world!");
}
```

**Compiler:**
Rustc 1.43 - Linux 64-bit release build

**Size:**
7M

**Command:**
```sh
cargo build --release
```

**Size (with LTO):**
1.8M

see [Cargo.toml](https://github.com/Adobe-Android/binary_size_comparison/blob/master/Cargo.toml)

**Size (with LTO and stripped):**
195K

**Command:**
```sh
strip hello
```

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

**Command:**
```sh
go build hello.go
```

**Output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```

**Size (-ldflags "-s -w"):**
1.4M

**Command:**
```sh
go build -ldflags "-s -w" hello.go
```

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

**Size (--release-fast):**
167K

**Command:**
```sh
zig build-exe hello.zig --release-fast
```

**Size (--release-small):**
78K

**Command:**
```sh
zig build-exe hello.zig --release-small
```

**Output from [file(1)](https://www.freebsd.org/cgi/man.cgi?query=file):**
```sh
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, with debug_info, not stripped
```

**Size (--release-small and stripped):**
5.3K

**Command:**
```sh
strip hello
```

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

# Notes:

### Note 1
You can achieve a near identical result to what is shown here by running something like below.

* -Os optimizes for size.
* -s stips the binary of debug information.

**hello.c**
```c
#include <stdio.h>

int main()
{
    char hello_str[] = "Hello C\n";
    printf("%s", hello_str);
}
```
**Command:**
```sh
gcc -Os hello.c -o gcc-hello -s
```

### Note 2
You can further reduce binary size by leveraging something like [musl libc](https://musl.libc.org/). Becuase our program is already so small and dynamically linked, we shouldn't expect to see a large improvement over the starting size of 14,472 bytes or a little over 14K.

Here we are leveraging musl-gcc, part of the *[musl-tools](https://packages.ubuntu.com/focal/musl-tools)* package, to easily consume musl libc.

**Command:**
```sh
musl-gcc -Os hello.c -o musl-hello -s
```

We reduced our binary size to 14,024 bytes by using the slimmer musl C library implementation. Where musl really shines though is with static builds. Static builds can be preferred because they will not rely on specific versions of libraries on a given system. Because of this, they are also more portable.

**Command:**
```sh
gcc -Os hello.c -o gcc-hello -s -static
```

**Size (GCC static build):**
798,480 bytes or roughly 780K

**Command:**
```sh
musl-gcc -Os hello.c -o musl-hello -s -static
```

**Size (Musl-gcc static build):**
26,048 bytes or roughly 26K

The GCC statically linked build is **roughly 30x larger!**

### Note 3

**Warning!** Trade-offs will be made to achieve even smaller binaries. In this example, we're going to use [TCC](https://bellard.org/tcc/tcc-doc.html), the Tiny C Compiler. It supports ANSI C (C89), but also most of the C99 standard. That means that anything beyond C99 will likely never be supported in this compiler. It's up to you to decide if that's an issue. From what I can tell, it only supports dynamic builds so that's what we'll be comparing.

**Command:**
```sh
tcc hello.c -o tcc-hello
```

**Size (TCC dynamic build):**
3,132 bytes or roughly 3.1K

The TCC dynamically linked build is **roughly 4x smaller** than our musl dynamic build!
