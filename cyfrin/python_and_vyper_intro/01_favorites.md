# Section 01. Favorites

## Intro

In the repo link, you can find a repo for each of the lessons.
I.e. for this **Favorites** exercise, you can find the associated repo [here](https://github.com/Cyfrin/remix-favorites-cu)

Introduction to Remix and then we will create our first `favorites.vy` file.

## Basics

To declare the pragma version:
(we will be using 0.4.0 which has a lot of additions and cool features)

```vyper
# pragma version 0.4.0
# This is a comment
```

To add a license:

```vyper
# @license MIT
```

## Compiling

Make the exercise of compiling this:

```vyper
# pragma version 0.4.0
```

Interestingly, we already get a ton of information in the compilation details:

A minimal ABI:

```
{
  "evmVersion": "",
  "version": "0.4.0"
}
```

A bytecode:

`61000361000f6000396100036000f35f5ffd84038000a1657679706572830004000011`

And a runtime bytecode:

`5f5ffd`

Notice that the comments are ignored by the compiler, are whatever you want in the comments, add a line, a haiku, a whole novel in the comments, the output bytecode will be exactly the same.

## Our First Contract

We are going to build a minimal Vyper contract that will store:

- A list of favorite things
- A list of favorite numbers
- A list of favorite people with their favorite numbers

## Vyper Types

You can take a look at the types available in Vyper [here](https://docs.vyperlang.org/en/stable/types.html).

But don't get overwhelmed, for now, just know that these types exist (you probably already are familiar with them):

- `uint<N>`
- `int<N>`
- `bool`
- `address`
- `bytes<N>`: Remember this is for _fixed-size byte arrays_, useful for hashes and fixed-length data.

And we will later learn about these more complex ones:

- `bytes`
- `string`
- `decimal`
- `struct`
- `array`
- `mapping`

No need to learn them by heart.

This is how some of the 1st examples would look (although this won't compile):

```vyper
my_number: uint256 = 42
is_active: bool = True
owner: address = 0x1234567890abcdef1234567890abcdef12345678
```

## Variable Visibility

Do this exercise, deploy this contract inside the Remix VM:

```vyper
# pragma version 0.4.0
# @license MIT

my_number: uint256
```

You will not see anything after deploy. Now deploy this one:

```vyper
# pragma version 0.4.0
# @license MIT

my_number: public(uint256)
```

And you will see a blue button for your variable. Since you made it public, you can access it publicly and Remix gives you a nice button to do that.

If you don't add the public visibility, by default your storage variable will only be accessible from within the contract, but remember, any smart engineer can access internal variables in a blockchain.

## ❓ Questions and Resources

- [Vyper Docs](https://docs.vyperlang.org/en/stable/)
- [Repo Link](https://github.com/Cyfrin/moccasin-full-course-cu)
- [Remix](https://remix.ethereum.org/)

```

```

```

```

```

```

```

```

```

```

```

```
