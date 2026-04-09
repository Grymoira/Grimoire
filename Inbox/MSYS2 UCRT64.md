
> [*Address link*: https://msys2.org](https://msys2.org)

---

# 1. `UCRT64`

编译器：`GCC` (`GNU Compiler Collectioin`)
C标准库： `UCRT` (`Universal C Runtime`)
用途： 用于编译原生64位`windows`应用程序，并链接到现代的`windows` `C`运行时库

---

# 2. `MINGW64`

编译器： `GCC` (和 `UCRT64` 一样)
C标准库： `MSVCRT` (`Microsoft Visual C++ Runtime`)
用途： 用于编译原生64位`windows`应用程序，但是链接到的传统的 `msvcrt.dll` ；在 `UCRT` 成为主流之前，这是最常用的环境

---

> `UCRT64 vs MINGW64`: 他们之间唯一的区别就是链接的`C`标准库不同

---

# 3. `CLANG64`

编译器： `clang` 是一个由`LLVM`项目开发的`C/C++`编译器，是`GCC`的主要替代品，它以其快速的编译速度和更加清晰的错误提示而闻名
`C`标准库： `UCRT`
用途： 和`UCRT64`一样，用于编译原生64位`windows`应用程序，但是使用的编译器是`clang`而不是`GCC`

---

# 4. `MSYS`

编译器： 这个环境默认不包含用于编译`windows`程序的编译器
库： 它使用的是一套基于`cygwin`库，主要目的为了运行一个`unix/linux`风格的`shell`环境本身（比如`bash`, `grep`, `sed`等）
用途： 这是`msys2`的核心管理环境，它的主要作用是运行`pacman`来安装和更新其他环境的工具链

---

# 5. `CLANGARM64`

编译器： `c`lang`
目标平台： `ARM64`架构
用途： 这是一个交叉编译环境，就可以在`x86-64`电脑上使用它来编译能在`windows on ARM`设备 （比如某些 `Surface Pro X` 型号） 上运行的程序

