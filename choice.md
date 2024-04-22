# Choices, choices...

So, which scheme to formally verify first?

## Gen 1

> Current solutions are so much better!

\- Gentry, Eurocrypt 2021 invited talk

Gen 1 was a long time ago and nobody uses it anymore.
There's no point.

## Gen 2

This generation started in 2011 as the first practical construction.

### Pros

* Well established: These are the "classic" FHE that won the Gödel Prize.
* Deployed today in the Microsoft Edge password monitor.

### Cons

This generation is only used for shallow circuits in practice.
The issue is that the encryption picks up "noise" as the computation progresses, until decryption eventually becomes impossible.
It is possible in principle to reset the noise using "bootstrapping", but that is too costly in practice for gen 2 schemes.
This is a dilemma because we'd either:
1. Verify the bootstrapping feature that nobody currently uses, and is in fact a shifting goalpost thanks to the active theoretical research, or
2. Verify only what is used in practice, though that gives up on the *full* homomorphism.

## Gen 3

A new blueprint for FHEs was proposed in 2013 and has undergone many iterations since.
These FHEs feature faster bootstrapping, but operate only on booleans and small integers.
TFHE is the most representative project in generation 3.

### Pros

* General purpose: TFHE allows arbitrarily deep computations with a large class of supported operations. As such, it is the first target chosen by Google's FHE compiler.
* Sophisticated construction: Converts between different encryption formats suited for different tasks.
* Resources are easy to find, thanks to Zama's self-promotion efforts, as well as the cryptography community's general appreciation for this line of research.

### Cons

A fraction of our efforts might *not* be spent towards security proofs. Our strategy would be as follows:
1. Verify "IND-CPA" security: Without homomorphic evaluation, the data is securely encrypted.
2. Generalize to "IND-CPA+" security: It is ok to use and share decryption results of homomorphic evaluations.

The first step is relatively straightforward, as the initial encryption is relatively simple.
The second step amounts to showing that each supported operation is *correct* (with overwhelming probability).
These operation ranges from boolean gates to multiplexers and vectorized arithmetics;
none of them seem especially tricky, though the endless case-work and boilerplate perhaps becomes uninteresting to those who care only of NANDs and asymptotics.

On a sidenote, the analysis mentions some heuristics that might become annoying to verify.

## Gen 4

The CKKS encryption was proposed in 2017 as the first *approximate homomorphic encryption* scheme:
when `f(x)` is evaluated homomorphically, the result is `f(x) + e` for some small noise `e`.

### Pros

* Efficiency: CKKS doesn't need to "reset" the ciphertext noise as often.
* Popularity: Adopted by many leading open source libraries including Microsoft's SEAL, IBM's HElib, and OpenFHE. Planned for standardization too.
* The tumultuous history makes a fascinating story.
	* 2017: Proposed for shallow circuits
	* 2018: Added support for arbitrarily deep circuits
	* 2020: Totally broken. The noise `e` contains secret information. Major libraries quickly patched out known attacks or advised against sharing outputs.
	* 2022: Provably secure patch finally proposed
	* Today: The patch still seems missing in some major libraries
* Interesting proof: the new proof borrows tools from differential privacy

### Cons

Can't think of any right now. Might discover more as I read through the proofs.
