<h1 align="center">ft_printf</h1>

<p align="center">
  <img src="https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white" alt="C"/>
  <img src="https://img.shields.io/badge/GNU%20Make-A42E2B?style=for-the-badge&logo=gnu&logoColor=white" alt="GNU Make"/>
</p>

<p align="center"><strong>A reimplementation of printf, packaged as a static library the later C projects link against.</strong></p>

---

## 📌 Overview

`printf` is variadic, so it has to read its arguments at runtime from a type it cannot check, walking the format string and pulling the next value off the stack for each conversion it finds. Reimplementing it is the 42 project that forces you to actually use `<stdarg.h>` and to think about what a partial write or a null string argument should do.

This version parses the format string once, splitting it into literal runs and conversion specifiers. Each piece is turned into a node holding a heap buffer and its length, appended to a linked list. When parsing is done, a single loop writes every node to the file descriptor and sums the bytes, which becomes the return value. Building the output before writing keeps the byte count exact even when a conversion fails partway.

It handles the mandatory conversions: `c`, `s`, `p`, `d`, `i`, `u`, `x`, `X` and `%%`. Flags, width and precision are out of scope. It compiles to `libftprintf.a` and bundles the `libft` functions it depends on.

## 🎯 Objectives

- Read a variadic argument list with `va_start` / `va_arg` / `va_end`, driven by the format string.
- Support the standard conversions `c s p d i u x X %`.
- Return the number of characters written, matching the real `printf`.
- Manage every intermediate allocation without a leak.
- Ship the result as a Makefile-built static library (`all`, `clean`, `fclean`, `re`, `.PHONY`).

## 🛠️ Tech Stack

<p>
  <img src="https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white" alt="C"/>
  <img src="https://img.shields.io/badge/GNU%20Make-A42E2B?style=for-the-badge&logo=gnu&logoColor=white" alt="GNU Make"/>
</p>

## 🚀 Getting Started

```bash
git clone https://github.com/acardona123/42_ft_printf.git
cd 42_ft_printf
make
```

`make` builds the bundled `libft` first, then links it into `libftprintf.a`.

## 📖 Usage

Link the archive and include the header:

```c
#include "ft_printf.h"

int main(void)
{
    int n = ft_printf("hex %x, pointer %p, string %s\n", 255, &n, "ok");
    ft_printf("wrote %d bytes\n", n);
    return (0);
}
```

```bash
cc main.c libftprintf.a -o demo && ./demo
```

## 📁 Structure

```
ft_printf_main.c        format-string parsing and the write loop
ft_printf_add_arg*.c    per-conversion formatting (c/s/p, d/i/u, x/X, %)
ft_printf_add_str.c     literal runs
ft_printf_lst.c         the output-chunk linked list
ft_printf.h             public prototype and bundled libft declarations
libft/                  the libft functions this depends on
```

## 🧪 Tests

`test.sh` runs the project against the community testers (Tripouille's `printfTester` and Paulo-Santana's `ft_printf_tester`), which it clones on the fly.

## 📚 Resources

- `man 3 printf`
- Built on [42_libft](https://github.com/acardona123/42_libft).

---

<p align="center"><sub>🏫 Project from the <strong>42</strong> common core — School 42 Paris.</sub></p>
