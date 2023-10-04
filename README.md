# soroban_decimal_numbers

This is a tool for handling decimal point numbers in Rust with `no_std`. It is designed to be used in the [Soroban](https://medium.com/r/?url=https%3A%2F%2Fsoroban.stellar.org%2F) smart contracts but it can be used anywhere if needed.

The motivation behind this is that Soroban does not support decimal point numbers.

## Installation

```
cargo add soroban_decimal_numbers
```

## Usage

You can use [the test file](./src/decimal_number_wrapper_test.rs) as a reference. Here are some examples:

```
use soroban_decimal_numbers::DecimalNumberWrapper;

fn main() {
  let a = DecimalNumberWrapper::new("34.56");
  let b = DecimalNumberWrapper::new("12.34");

  assert!(DecimalNumberWrapper::add(a, b).as_tuple() == (46, 900));
  assert!(DecimalNumberWrapper::sub(a, b).as_tuple() == (22, 220));
  assert!(DecimalNumberWrapper::mul(a, b).as_tuple() == (426, 470));
  assert!(DecimalNumberWrapper::div(a, b).as_tuple() == (2, 800));

  assert!(DecimalNumberWrapper::cmp(a, b) == 1);
  assert!(DecimalNumberWrapper::cmp(b, a) == 2);
  assert!(DecimalNumberWrapper::cmp(a, a) == 0);
}
```

## Constraints

This tool is in the early stage of development. It may be developed more in the future.

It uses decimal numbers with a maximum of 3 decimal places. This value is hardcoded for now but it may be parameterized in the future.
Please note that you will lose precision if your calculations go beyond this constraint. For example, if you try to calculate `12.34/34.56` which results in `0.35706018518`, you are going to get `0.357` - it just cuts the result, so the operation is `floor`, not `round` or anything else like that.

Negative numbers are not supported at the moment. If you perform a subtraction that would result in a negative number (like `3-5`), the result will be `0`.

Please be careful when using the tuple representation. You will need to provide all decimal point numbers. If you want to represent for example `1.2`, you need to do it like this: `DecimalNumberWrapper::from((1, 200))`, if you do `DecimalNumberWrapper::from((1, 2))`, you are going to get `1.002`. A strongly recommended approach to initialize the numbers is to use string slices: `DecimalNumberWrapper::from("1.2");`.
