# Bitwise Playground

Flip bits and watch every representation follow — AND, OR, XOR, shifts, rotates, sign extension, byte swaps and bitfield extraction at 8 to 128 bits, with two's complement handled properly. Runs entirely in your browser.

**Live:** <https://bitwise-playground.slippylabs.com/>

## What it does

- Two operands in any base (`0x2a`, `0b101010`, `0o52`, `42`, `-1`), at 8, 16, 32, 64 or 128 bits.
- Nineteen operations: the boolean set including NAND/NOR/XNOR and AND-NOT, logical and arithmetic shifts, rotates, bit reversal, byte swap, negate, and wrapping add/subtract/multiply.
- Click any bit to flip it. Every view updates: binary, hex, octal, decimal signed and unsigned, the bytes in both orders, and ASCII.
- Set bits, leading and trailing zeros, parity, bit length, power-of-two test, and `value[hi:lo]` bitfield extraction with its mask and sign extension.

## How it works

Every value is a `BigInt` held as the unsigned representation at the chosen width, converted to signed only for display. That is the only honest way to do it: JavaScript's own bitwise operators coerce to 32-bit signed, so `0xDEADBEEF | 0` is negative and `1 << 32` is 1. A tool about bits cannot inherit those.

Shift and rotate semantics are **stated rather than inherited**: a shift of width or more gives zero (all sign bits for an arithmetic right shift), and a rotate is taken modulo the width. C calls the first case undefined behaviour and x86 masks the count to 5 or 6 bits, so there is no natural answer to copy — only a documented one.

## Verification

Checked against a Python reference in which the width handling is written out explicitly, so the two share no machinery. The 8-bit case is **exhaustive** — every value, every operation, every shift amount from 0 to twice the width: **177,408 operations, all matching**. Wider widths get the edges (0, 1, all-ones, the sign bit, shift counts at and past the width) plus random values, and parsing, formatting, round-tripping, sign extension and bitfield extraction are checked separately.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/bitwise-playground.slippylabs.com.git
cd bitwise-playground.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
