# Official Documentation

The official documentation is [here](https://chialisp.com/)

Additional notes may appear in the wiki and merged into official documentation later.

## Operators

### `softfork`

The `softfork` operator takes at least one parameter `cost`. So `(softfork cost arg_1 ... arg_n)`.

At the moment, `softfork` always returns `0` (aka `()` or nil), and takes `cost` amount of cost.

At first glance, it seems pretty useless since it doesn't do anything, and just wastes cost doing it.

The idea is, after a soft fork, the meaning of the arguments may change. In fact, we can hide completely new dialects of ChiaLisp inside
here, that has new operators that calculate new things.

For example, suppose we want to add `secp256k1` operators like `+s` for adding two points on this ECDSA curve for bitcoin compatibility. We can't just do this in vanilla clvm because that would make a program `(+s p1 p2)` return different values before and after the soft fork. So instead we hide it under `softfork`.

`(mod (cost p1 p2 p3 p4) (softfork cost 1 (assert (= (+s p1 p2) (+s p3 p4)))))`

Pre-softfork, this always passes and returns `()` at a cost of `COST` (plus a bit of overhead).

Post-softfork, this also returns `()` at a cost of `COST`... but may also fail if `p1 + p2 ≠ p3 + p4`! We can't export the sum outside the `softfork` boundary, but we can calculate the sum and compare it to another thing inside.

One more thing -- we take the cost of running the program inside the `softfork` boundary and ensure it exactly matches `COST`, and raise an exception if it's wrong. That way, the program really does have the same cost pre and post-softfork (or it fails post-softfork).

