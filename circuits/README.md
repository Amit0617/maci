# MACI Circuits

## About Poseidon Hash Functions

We use the [Poseidon hash function](https://eprint.iacr.org/2019/458.pdf) to reduce the number of constraints in the circuits.

Poseidon hash functions need to be parameterized for the number of inputs.
If the input has `n` field elements, the parameter `t` would be `n + 1`, for security reasons (see below).

We have these hash functions defined:

| Poseidon t | number of elements | Use cases                            |
| ---------- | -----------------: | ------------------------------------ |
| t = 3      |                  2 | Build Merkle tree with HashLeftRight |
| t = 6      |                  5 | Hash stateLeaf                       |

The Message has 11 elements and we hash it with a Hasher11, which is a combination of t3 and t6.

## Why `t := n+1`?

Here's a quote from the poseidon-hash [website](https://www.poseidon-hash.info/). Notations are changed to the same with our context.

> ..., you determine the width `t`, measured in the number of F elements, of Poseidon permutation as follows:

> - Reserve `c` elements for capacity so that `c` elements of F contain `2M` or more bits.
> - If messages have fixed length `n` which is reasonably small (10 or less), then set `t = c+n`.

We target 128 bits of security (`M=128`) and use BN128, aka BN254, curve in circom. A BN128 field element has roughly 256 bits, so we reserve only one element (`c=1`) to contain `2M` (2 \* 128 = 256) bits.

That's why if the message has length `n` then use `t = 1 + n`