# nelua-mpfr

[Nelua](https://nelua.io/) bindings for the [GNU MPFR](https://www.mpfr.org/)
library, version 4.1.

The MPFR library is a C library for multiple-precision floating-point
computations with correct rounding.

The MPFR documentation is available [here](https://www.mpfr.org/mpfr-current/mpfr.html).

## Prerequisites

- The GNU MPFR library must be installed in the system or otherwise available
  to the C compiler.
- GNU MPFR requires, as a hard dependency, the [GNU GMP](https://gmplib.org/)
  library.

## Usage

Drop the [`mpfr.nelua`](mpfr.nelua) file in your project and use it with a `require 'mpfr'`.

In restricted environments where `stdio.h` is not available you can define the
`NELUA_MPFR_NO_STDIO` symbol in Nelua's preprocessor to remove all IO functions
that require the FILE type to be available:

```lua
## NELUA_MPFR_NO_STDIO = 1
require `mpfr`
```

## Example

The main type used by GNU MPFR is the `mpfr_t` record.

Thanks to the [automatic referencing](https://nelua.io/overview/#automatic-referencing-and-dereferencing)
performed by the nelua compiler, variables defined as `mpfr_t` can be passed to
most of the functions without the `&` operator.

However, some functions, namely those using varargs, are defined in
such a way that require the reference operator. For an example look at the
`mpfr_printf` call in the following program.

```lua
require 'mpfr'

local s: mpfr_t

-- Init 's' with 100 bits of precision and set its value
mpfr_init2(s, 100)
mpfr_set_str(s, '1.3337612', 10, MPFR_RNDN)

-- Multiply by 42 and print the result to stdout
mpfr_mul_ui(s, s, 42, MPFR_RNDN)
mpfr_printf("1.3337612 * 42 is: %.30Rf\n", &s)

-- Clean-up
mpfr_clear(s)
mpfr_free_cache()
```

A more comprehensive example is available in the [`mpfr-test.nelua`](mpfr-test.nelua)
file.

Please refer to the MPFR documentation for more datails.
