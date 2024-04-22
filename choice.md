# Choices, choices...

So, which scheme to formally verify first?

## Gen 1

This is a long time ago and nobody uses it anymore.
There's no point.

## Gen 2

This generation started in 2011 as the first usable scheme.

### Pros

* Well established: These are the "classic" FHE that won the Gödel Prize.
* Deployed today in the Microsoft Edge password monitor.

### Cons

It is only used for shallow circuits in practice.
The issue is that the encryption picks up "noise" as the computation progresses, and resetting the noise by "bootstrapping" is too costly.
This is a dilemma because we'd either:
1. Verify the bootstrapping feature that nobody currently uses, and is in fact a shifting goalpost, or
2. Verify only the shallow circuit version, in that case it is not even *fully* homomorphic anymore.

## Gen 3

A new blueprint for FHEs was proposed in 2013 and has undergone constant iterations since.
These FHEs feature faster bootstrapping, but operated only on booleans and recently on small integers too.
TFHE is the most representative project in generation 3.

### Pros

* General purpose: TFHE allows arbitrarily deep computations with a large class of supported operations. As such, it is the first target chosen by Google's FHE compiler.
* Sophisticated construction: Converts between different encryption formats suited for different tasks.
* Resources are easy to find, thanks to Zama's self-promotion efforts, as well as the cryptography community's general appreciation for this line of research.

### Cons

A fraction of our efforts might *not* be spent towards security proofs. Our strategy would be as follows:
1. Verify "IND-CPA" security: Without homomorphic evaluation, the data is encrypted as expected.
2. Generalize to "IND-CPA+" security: It is ok to use and share decryption results of homomorphic evaluations.

The first step is relatively straightforward, as the initial encryption is relatively simple.
The second step amounts to showing that each supported operation is *correct* (with overwhelming probability).
These operation ranges from boolean gates to multiplexers and vectorized arithmetics;
none of them seem especially tricky, though the endless case-work and boilerplate perhaps becomes uninteresting to those who care only of NANDs and asymptotics.

On a sidenote, the analysis mentions some heuristics that might become annoying to verify.

## Gen 4

The CKKS encryption was proposed in 2017 as the first *approximate homomorphic encryption* scheme:
when `f(x)` is evaluated homomorphically, the result is `f(x) + epsilon` for some small noise `epsilon`.

### Pros

* Efficiency: CKKS doesn't need to "reset" the ciphertext noise as often.
* Popularity: Adopted by many leading open source libraries including Microsoft's SEAL, IBM's HElib, and OpenFHE. Planned for standardization too.
* The tumultuous history makes a fascinating story.
	* 2017: Proposed for shallow circuits
	* 2018: Added support for arbitrarily deep circuits
	* 2020: Totally broken. The error `epsilon` contains secret information. Major libraries quickly patched out known attacks or advised against sharing outputs.
	* 2022: Provably secure patch finally proposed
	* Today: The patch still seems missing in some major libraries
* Interesting proof: the new proof borrows tools from differential privacy

### Cons

Can't think of any right now. Might discover more as I read through the proofs.
