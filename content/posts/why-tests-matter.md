---
title: "why tests matter?"
date: 2023-11-21T14:40:19.776
draft: true
categories: ['Tech']
---
Lately I have been studying cryptography and consensus algorithms.
One the foundations of it is `Caesar's cipher`, even though this post is not
about the technique itself, it's about `tests`.

## Introduction to Caesar's cipher
The idea is that you have a `shift` from 0 to 25, who as the name suggest, shift each letter it's value,
for example, a `shift` of 3 would output something like:

```sh
Plaintext:  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Ciphertext: QEB NRFZH YOLTK CLU GRJMP LSBO QEB IXWV ALD
```

So it means that to communicate using this cipher,
you would receive the **message** and the **shift** used to `encrypt`, so you can `decrypt` the message.

## Writing a test saved me from a wrong feature
With the basics in mind we can continue.

While implementing tests for this project (you can find it [here](https://github.com/1garo/caesar_cipher/blob/main/caesar_test.go)),
one of the tests was named `TestRandomShift` and the idea was to test a implementation
that assigns a random value to `shift` between 1 and 25 if 0 was passed.

While implementing this test, I realize a thing:
> How I can decrypt the message if I don't know the shift?

If I pass `shift = 0` the shift would be auto-generated, then I would not be able to validate if the decrypted text is equal to the input.


I expect this to be one of a series of posts talking about what I learned from writing **tests**.


The repository for the `Caesar's cipher` project is [here](https://github.com/1garo/caesar_cipher).

See you on the next one.



