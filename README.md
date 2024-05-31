# Legato Math

Legato Math is a fixed-point math library for Sui Move that allows for computing the nth root and raising a number to the power of a fractional exponent in all fixed-point numbers. It extends Aptos's math library with custom code that utilizes the Newton-Raphson method. Initially made for Legato LBP, we separated it into an open-source repo that may benefit other projects seeking this solution.

Earlier, we wrote an article about the basics of fixed-point math in Sui, which you can find here:

https://blog.legato.finance/fixed-point-math-in-sui-move-09171a78593f

## How to Use

With Legato Math, you don't need to import any other library when dealing with fractional numbers on Sui Move. You can begin by adding this to your `Move.toml` file like so:

```
[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "framework/testnet" }
SuiSystem = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-system", rev = "framework/testnet" }
LegatoMath = {  git = "https://github.com/tamago-labs/legato-math.git", subdir = "packages/sui", rev = "main"  }
```

## Basic

Now let's try converting 3/4 into a fixed-point number using our library.

```
use legato_math::fixed_point64;

fixed_point64::create_from_rational(3,4); // represents 3/4 or 0.75
```

Then we can do the basic.

```
// 2.34+1.5
let num_a = fixed_point64::create_from_rational(234, 100);
let num_b = fixed_point64::create_from_rational(15, 10);
let output_1 = fixed_point64::add(num_a, num_b);

// 1000-77.3
let num_a = fixed_point64::create_from_u128(1000);
let num_b = fixed_point64::create_from_rational(773, 10);
let output_2 = fixed_point64::sub(num_a, num_b);
```

## N-th Root

Find the nth root of a fixed-point number.

```
use legato_math::legato_math;

// Asserts that the square root of 16 is 4.
let output_1 = legato_math::nth_root(  fixed_point64::create_from_u128(16), 2  );
assert!( fixed_point64::equal( output_1, fixed_point64::create_from_u128( 4  )) , 1 );

// Asserts that the 5th root of 625 is approximately 3.623.
let output_2 = legato_math::nth_root(  fixed_point64::create_from_u128(625), 5  );
assert!( fixed_point64::almost_equal( output_2, fixed_point64::create_from_rational( 3623, 1000 ), fixed_point64::create_from_u128(1)) , 2 );

// Asserts that the 4th root of 9/16 is approximately 0.866025404.
let output_3 = legato_math::nth_root(  fixed_point64::create_from_rational( 9, 16 ), 4  );
assert!( fixed_point64::almost_equal( output_3, fixed_point64::create_from_rational( 866025404, 1000000000 ), fixed_point64::create_from_u128(1)) , 3 );

```

## Fractional Powers

Raising a fixed-point number to a fractional exponent

```
use legato_math::legato_math;

// Asserts that the result of raising 10/9 to the power of 3/5 is approximately 0.566896603.
let output_1 = legato_math::power(  fixed_point64::create_from_rational( 3, 5 ), fixed_point64::create_from_rational( 10, 9 )  );
assert!( fixed_point64::almost_equal( output_1, fixed_point64::create_from_rational( 566896603, 1000000000 ), fixed_point64::create_from_u128(1)) , 1 );

// Asserts that the result of raising 2/5 to the power of 5 is approximately 0.01024.
let output_2 = legato_math::power(  fixed_point64::create_from_rational( 2, 5 ), fixed_point64::create_from_rational( 5, 1 )  );
assert!( fixed_point64::almost_equal( output_2, fixed_point64::create_from_rational( 1024, 100000 ), fixed_point64::create_from_u128(1)) , 2 );
```

## Natural Log

```
// Asserts that the result of ln(10) is 2.30258509299
let output = legato_math::ln( fixed_point64::create_from_u128(10) ); 
assert!( fixed_point64::almost_equal( output_1, fixed_point64::create_from_rational( 230258509299, 100000000000  ), fixed_point64::create_from_u128(1)) ,1 );
```
