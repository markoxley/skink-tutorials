# Tutorial 27: Tensors and math

This tutorial introduces `std/tensor` for numerical computing and `std/math` for mathematical functions.

## Creating tensors

```skink
module main

import "std/tensor"

fn main() -> int {
    a := tensor.Zeros([2, 3])
    b := tensor.Ones([2, 3])

    if a.Rank() == 2 && b.Rank() == 2 {
        print("shape: ", len(a.Shape()), " dimensions")
        print("ones shape: ", len(b.Shape()), " dimensions")
    }
    return 0
}
```

## Element-wise operations

```skink
import "std/tensor"

a := tensor.Zeros([2, 3])
b := tensor.Ones([2, 3])

sum := tensor.Add(a, b)
print("sum rank: ", sum.Rank())

diff := tensor.Sub(a, b)
print("diff rank: ", diff.Rank())

prod := tensor.Mul(a, b)
print("prod rank: ", prod.Rank())

scaled := tensor.Scale(a, 2.0)
print("scaled rank: ", scaled.Rank())
```

## Indexing and setting values

```skink
import "std/tensor"

a := tensor.Zeros([2, 3])
a.Set([0, 1], 5.0)
val := a.Get([0, 1])
print("value at (0,1): ", val)
```

## Matrix multiplication

```skink
m := tensor.Zeros([2, 3])
n := tensor.Zeros([3, 4])
result := tensor.MatMul(m, n)
print("result shape: ", result.Shape())
```

## Math helpers

`std/math` wraps common C math functions:

```skink
import "std/math"

fn main() -> int {
    angle := math.Pi / 2.0
    s := math.Sin(angle)
    print("sin: ", s)
    print("sqrt: ", math.Sqrt(16.0))
    return 0
}
```

## Exercises

1. Create two 2x2 matrices and add them.
2. Multiply a 3x2 matrix by a 2x3 matrix.
3. Apply `tensor.Sigmoid` to a tensor and verify all values are between 0 and 1.

## Next

[Building a custom std module](28-building-a-custom-std-module.md)
