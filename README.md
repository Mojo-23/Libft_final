*This project has been created as part of the 42 curriculum by malquran.*

# Libft — Your Very First Own Library

## Description

**Libft** is a static C library that re-implements a selection of standard C
library functions, adds a set of useful utility functions that libc does not
provide, and implements a complete singly linked list API.

The project exists because the 42 curriculum forbids the use of most external
functions. Instead of relying on libc, every student builds a personal toolbox
first, and then reuses it in every C project that follows — `ft_printf`,
`get_next_line`, `push_swap`, `minishell`, and so on. Linking against
`libft.a` means those projects can call `ft_strlen`, `ft_split`, or `ft_itoa`
without writing them again.

Working through Libft teaches:

- How standard functions actually behave, including their edge cases and their
  exact return values.
- Manual memory management with `malloc` and `free`, and who owns which
  pointer.
- Pointer arithmetic, pointer casts, and why `unsigned char` matters when
  working on raw bytes.
- The difference between a size in bytes (`size_t`) and a character count.
- Function pointers, and how they allow generic code.
- Building and linking a static library with `ar` and `make`.
- Writing code that respects a strict style standard (the 42 Norm).

The build produces a single archive, `libft.a`, which can be linked into any
C program.

The library is divided into three mandatory parts:

1. **Part 1 — Libc functions**: re-implementations of standard functions such
   as `strlen`, `memcpy`, `memmove`, `atoi`, `calloc`, and `strdup`.
2. **Part 2 — Additional functions**: functions that are not part of libc but
   are useful in practice, such as `ft_split`, `ft_itoa`, `ft_strtrim`, and
   the file-descriptor output functions.
3. **Part 3 — Linked list functions**: a complete API built on the `t_list`
   structure for creating, measuring, traversing, transforming, and freeing
   lists.

All three parts are mandatory and all of them are built by the default `make`
target.

---

## Detailed Description

### Part 1 — Libc Re-implementations

#### Character Classification and Conversion

These functions take their argument as an `int` so that they can also accept
`EOF`. The five classification functions return `1` when the character belongs
to the class and `0` when it does not.

| Function | Prototype | Description |
| --- | --- | --- |
| **ft_isalpha** | `int ft_isalpha(int c)` | Returns `1` if `c` is an alphabetic character (`a`–`z` or `A`–`Z`), otherwise `0`. |
| **ft_isdigit** | `int ft_isdigit(int c)` | Returns `1` if `c` is a decimal digit (`0`–`9`), otherwise `0`. |
| **ft_isalnum** | `int ft_isalnum(int c)` | Returns `1` if `c` is a letter or a digit, otherwise `0`. Built on top of `ft_isalpha` and `ft_isdigit`. |
| **ft_isascii** | `int ft_isascii(int c)` | Returns `1` if `c` is a valid ASCII value (`0`–`127`), otherwise `0`. |
| **ft_isprint** | `int ft_isprint(int c)` | Returns `1` if `c` is a printable character, space included (`32`–`126`), otherwise `0`. |
| **ft_toupper** | `int ft_toupper(int c)` | Returns the uppercase equivalent of `c`. Any value that is not a lowercase letter is returned unchanged. |
| **ft_tolower** | `int ft_tolower(int c)` | Returns the lowercase equivalent of `c`. Any value that is not an uppercase letter is returned unchanged. |

#### Memory Functions

These functions work on raw bytes. The buffer is read and written through
`unsigned char` so that byte values above `127` behave consistently.

| Function | Prototype | Description |
| --- | --- | --- |
| **ft_memset** | `void *ft_memset(void *s, int c, size_t n)` | Writes the byte `c` (converted to `unsigned char`) into the first `n` bytes of `s`. Returns `s`. With `n == 0` nothing is written. |
| **ft_bzero** | `void ft_bzero(void *s, size_t n)` | Sets the first `n` bytes of `s` to zero. Implemented as `ft_memset(s, 0, n)`. Returns nothing. |
| **ft_memcpy** | `void *ft_memcpy(void *dst, const void *src, size_t n)` | Copies `n` bytes from `src` to `dst`. The two regions must **not** overlap. Returns `dst`. |
| **ft_memmove** | `void *ft_memmove(void *dst, const void *src, size_t n)` | Copies `n` bytes from `src` to `dst` and is safe when the two regions **overlap**: when `dst` is above `src` the bytes are copied from the end towards the start. Returns `dst`. |
| **ft_memchr** | `void *ft_memchr(const void *s, int c, size_t n)` | Searches the first `n` bytes of `s` for the byte `c` (converted to `unsigned char`). Returns a pointer to the first match, or `NULL` if the byte does not occur. Unlike `ft_strchr` it does not stop at a null byte. |
| **ft_memcmp** | `int ft_memcmp(const void *s1, const void *s2, size_t n)` | Compares the first `n` bytes of `s1` and `s2` as `unsigned char`. Returns `0` when they are identical, otherwise the difference between the first pair of differing bytes. |
| **ft_calloc** | `void *ft_calloc(size_t nmemb, size_t size)` | Allocates memory for `nmemb` elements of `size` bytes each and sets every byte to zero. Returns the pointer, or `NULL` if the multiplication `nmemb * size` would overflow or if the allocation fails. When `nmemb` or `size` is `0` it returns a valid, unique, freeable pointer. |

#### String Functions

| Function | Prototype | Description |
| --- | --- | --- |
| **ft_strlen** | `size_t ft_strlen(const char *s)` | Returns the number of characters in `s`, not counting the terminating `'\0'`. |
| **ft_strlcpy** | `size_t ft_strlcpy(char *dst, const char *src, size_t size)` | Copies at most `size - 1` characters from `src` to `dst` and always null-terminates when `size > 0`. Returns the length of `src`, so the caller can detect truncation by comparing it to `size`. When `size == 0` nothing is written. |
| **ft_strlcat** | `size_t ft_strlcat(char *dst, const char *src, size_t size)` | Appends `src` to the string already in `dst`, writing at most `size` bytes in total including the terminating `'\0'`. Returns the length of the string it tried to build (`strlen(dst) + strlen(src)`), so the caller can detect truncation. When `size` is smaller than or equal to the current length of `dst`, nothing is appended and `size + strlen(src)` is returned. |
| **ft_strchr** | `char *ft_strchr(const char *s, int c)` | Returns a pointer to the **first** occurrence of `c` (converted to `char`) in `s`, or `NULL` if it does not occur. The terminating `'\0'` counts as part of the string, so searching for `'\0'` returns a pointer to the end of `s`. |
| **ft_strrchr** | `char *ft_strrchr(const char *s, int c)` | Returns a pointer to the **last** occurrence of `c` (converted to `char`) in `s`, or `NULL` if it does not occur. Searching for `'\0'` returns a pointer to the end of `s`. |
| **ft_strncmp** | `int ft_strncmp(const char *s1, const char *s2, size_t n)` | Compares at most `n` characters of `s1` and `s2`, treating them as `unsigned char`, and stops early at a terminating `'\0'`. Returns `0` if the compared parts are equal, a negative value if `s1` is smaller, a positive value if `s1` is greater. |
| **ft_strnstr** | `char *ft_strnstr(const char *big, const char *little, size_t len)` | Looks for the first occurrence of `little` inside `big`, examining at most `len` characters of `big`. A match is only accepted if it fits entirely within those `len` characters. Returns a pointer to the start of the match, `big` if `little` is the empty string, or `NULL` if there is no match. |
| **ft_atoi** | `int ft_atoi(const char *nptr)` | Converts the beginning of `nptr` into an `int`: it skips leading whitespace, accepts one optional `+` or `-` sign, then reads consecutive digits and stops at the first character that is not a digit. Returns the converted value, or `0` when there is no number to read. |
| **ft_strdup** | `char *ft_strdup(const char *s)` | Allocates a new string with `malloc` and copies `s` into it. Returns the new string, or `NULL` if the allocation fails. The caller owns the returned pointer and must `free` it. |

---

### Part 2 — Additional Functions

Every function in this part that returns a `char *` or a `char **` returns
freshly allocated memory that the caller owns and must `free`. They all return
`NULL` when an allocation fails.

| Function | Prototype | Description |
| --- | --- | --- |
| **ft_substr** | `char *ft_substr(char const *s, unsigned int start, size_t len)` | Returns a newly allocated substring of `s` that starts at index `start` and is at most `len` characters long. If `start` is at or past the end of `s`, an empty string is returned. If `len` goes past the end of `s`, the result is cut short at the end of `s`. Returns `NULL` on allocation failure. |
| **ft_strjoin** | `char *ft_strjoin(char const *s1, char const *s2)` | Returns a newly allocated string containing `s1` followed by `s2`. Neither input is modified or freed. Returns `NULL` on allocation failure. |
| **ft_strtrim** | `char *ft_strtrim(char const *s1, char const *set)` | Returns a newly allocated copy of `s1` with every character that appears in `set` removed from the **beginning and the end**. Characters in the middle are kept. If every character is trimmed, an empty string is returned. Returns `NULL` on allocation failure. |
| **ft_split** | `char **ft_split(char const *s, char c)` | Splits `s` into words separated by the character `c` and returns a newly allocated, `NULL`-terminated array of newly allocated strings. Repeated and leading or trailing delimiters produce no empty words. If any allocation fails, every string already allocated and the array itself are freed, and `NULL` is returned. |
| **ft_itoa** | `char *ft_itoa(int n)` | Returns a newly allocated string holding the decimal representation of `n`. Negative values get a leading `-`. `INT_MIN` is handled correctly because the implementation never negates `n`; it takes the digits with `n % 10` and makes each digit positive instead. Returns `NULL` on allocation failure. |
| **ft_strmapi** | `char *ft_strmapi(char const *s, char (*f)(unsigned int, char))` | Applies `f` to every character of `s`, passing the index and the character, and returns a **new** string built from the results. `s` is not modified. Returns `NULL` on allocation failure. |
| **ft_striteri** | `void ft_striteri(char *s, void (*f)(unsigned int, char *))` | Applies `f` to every character of `s`, passing the index and the **address** of the character, so `f` can modify the string **in place**. Nothing is allocated and nothing is returned. |
| **ft_putchar_fd** | `void ft_putchar_fd(char c, int fd)` | Writes the character `c` to the file descriptor `fd`. |
| **ft_putstr_fd** | `void ft_putstr_fd(char *s, int fd)` | Writes the string `s` to the file descriptor `fd`, without a trailing newline. |
| **ft_putendl_fd** | `void ft_putendl_fd(char *s, int fd)` | Writes the string `s` to the file descriptor `fd`, followed by a newline. |
| **ft_putnbr_fd** | `void ft_putnbr_fd(int n, int fd)` | Writes the decimal representation of `n` to the file descriptor `fd`. Negative values get a leading `-`, and `INT_MIN` is handled as a special case so that no overflow can happen. |

---

### Part 3 — Linked List Functions

Part 3 is mandatory. It builds a singly linked list where each node stores a
`void *`, which means a list can hold anything: strings, integers wrapped in a
pointer, structures, or other lists.

```c
typedef struct s_list
{
	void			*content;
	struct s_list	*next;
}					t_list;
```

- `content` points to the data the node holds. The list does not know what
  that data is, which is why the deletion functions take a `del` function
  pointer: only the caller knows how to free the content.
- `next` points to the following node, or `NULL` for the last node.

A list is addressed by a pointer to its first node. Functions that can change
which node is first (`ft_lstadd_front`, `ft_lstadd_back`, `ft_lstclear`) take
a **double pointer** `t_list **lst` so that they can update the caller's head
pointer.

| Function | Prototype | Description |
| --- | --- | --- |
| **ft_lstnew** | `t_list *ft_lstnew(void *content)` | Allocates one new node, stores `content` in it, and sets `next` to `NULL`. Returns the node, or `NULL` on allocation failure. The content itself is not copied. |
| **ft_lstadd_front** | `void ft_lstadd_front(t_list **lst, t_list *new)` | Inserts `new` at the **beginning** of the list and updates `*lst` so that it points to `new`. Works on an empty list. |
| **ft_lstsize** | `int ft_lstsize(t_list *lst)` | Walks the list from the head to the end and returns the number of nodes. Returns `0` for an empty list. |
| **ft_lstlast** | `t_list *ft_lstlast(t_list *lst)` | Returns the last node of the list, that is the node whose `next` is `NULL`. Returns `NULL` if the list is empty. |
| **ft_lstadd_back** | `void ft_lstadd_back(t_list **lst, t_list *new)` | Appends `new` to the **end** of the list. If the list is empty, `new` becomes the first node and `*lst` is updated. |
| **ft_lstdelone** | `void ft_lstdelone(t_list *lst, void (*del)(void *))` | Frees the content of a single node using `del`, then frees the node itself. It does **not** free or touch `lst->next`, so the caller must keep that pointer first if the rest of the list is still needed. |
| **ft_lstclear** | `void ft_lstclear(t_list **lst, void (*del)(void *))` | Frees every node of the list starting at `*lst`, using `del` on each content, and finally sets `*lst` to `NULL` so that the caller is not left with a dangling pointer. |
| **ft_lstiter** | `void ft_lstiter(t_list *lst, void (*f)(void *))` | Walks the list and calls `f` on the `content` of every node. `f` can modify the content in place. Nothing is allocated. |
| **ft_lstmap** | `t_list *ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *))` | Walks `lst`, applies `f` to each content, and builds a **new** list from the results, keeping the original order. The original list is left untouched. If any node allocation fails, the partially built new list is freed with `ft_lstclear` using `del`, and `NULL` is returned. |

---

## Instructions

### Build

The repository contains the `Makefile`, `libft.h`, and all 43 source files at
its root. Run `make` from that directory.

```bash
make        # Builds libft.a (all three parts)
make clean  # Removes the object files
make fclean # Removes the object files and libft.a
make re     # Runs fclean, then builds again from scratch
```

`all` is the default target, so `make` and `make all` do the same thing.
Everything is compiled with:

```
cc -Wall -Wextra -Werror
```

and the object files are collected into the archive with `ar rcs`. Running
`make` twice in a row recompiles nothing, because each object file depends only
on its own source file and on `libft.h`.

### Using the library in another project

Put `libft.a` and `libft.h` where your project can find them, include the
header, and link against the archive.

```c
#include "libft.h"

int	main(void)
{
	char	**words;

	words = ft_split("hello 42 world", ' ');
	ft_putendl_fd(words[1], 1);
	/* free words here */
	return (0);
}
```

Compile and link, either by naming the archive directly:

```bash
cc -Wall -Wextra -Werror main.c libft.a -o my_program
```

or by using the linker's search path, where `-L.` is the directory holding the
archive and `-lft` selects `libft.a`:

```bash
cc -Wall -Wextra -Werror main.c -L. -lft -o my_program
```

If `libft.h` is not in the same directory as your source file, add
`-I<path to the header>`.

---

## Resources

**The subject**

- The official 42 **Libft subject, version 19.3** is the primary reference for
  this project. It defines every prototype, every return value, and the list
  of external functions that are allowed.

**C reference and manual pages**

- [Linux man pages](https://man7.org/linux/man-pages/) — the reference for the
  exact behaviour and return values of the libc functions reproduced in Part 1.
  Section 3 covers the library functions.
- [cppreference — C library](https://en.cppreference.com/w/c.html) — a
  standard-oriented reference, useful for checking what the C standard
  guarantees rather than what one particular platform does.
- [BSD `strlcpy` and `strlcat` manual page](https://man.openbsd.org/strlcpy.3)
  — `strlcpy` and `strlcat` are BSD functions, not standard C, so the OpenBSD
  page is the authoritative description of their return values and truncation
  behaviour.

**Memory allocation**

- [`malloc(3)` manual page](https://man7.org/linux/man-pages/man3/malloc.3.html)
  — `malloc`, `free`, and `calloc`, including what happens when a size of zero
  is requested.
- [Dynamic memory allocation in C](https://www.geeksforgeeks.org/c/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/)
  — a gentler introduction to the heap, useful before reading the man page.

**Linked lists and function pointers**

- [learn-c.org — Linked lists](https://www.learn-c.org/en/Linked_lists) — a
  short interactive introduction to nodes and traversal.
- [Linked list in C](https://www.geeksforgeeks.org/c/linked-list-in-c/) — more
  worked examples of insertion, deletion, and traversal.
- [Function pointers in C](https://www.geeksforgeeks.org/c/function-pointer-in-c/)
  — needed to understand the `f` and `del` parameters of `ft_lstiter`,
  `ft_lstmap`, `ft_lstclear`, `ft_strmapi`, and `ft_striteri`.

**Make and static libraries**

- [Makefile tutorial](https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/)
  — a step-by-step introduction that explains targets, prerequisites, and
  variables.
- [GNU Make manual](https://www.gnu.org/software/make/manual/make.html) — the
  complete reference, including automatic variables such as `$<` and `$@` and
  the meaning of `.PHONY`.
- [`ar(1)` manual page](https://man7.org/linux/man-pages/man1/ar.1.html) — the
  archiver that turns the object files into `libft.a`, and what the `r`, `c`,
  and `s` flags do.

**Debugging and style**

- [Valgrind quick start](https://valgrind.org/docs/manual/quick-start.html) —
  how to find memory leaks and invalid reads or writes in a test program.
- [GCC online documentation](https://gcc.gnu.org/onlinedocs/) — the meaning of
  the warnings produced by `-Wall` and `-Wextra`.
- [Norminette](https://github.com/42School/norminette) — the official tool that
  checks the 42 Norm; the repository README lists the rules it enforces.

### AI Usage

AI was used to help me understand how certain C functions work and to recommend
useful learning resources for further independent study.
