# Planning

## Intro and landscape

* Primitives: signature and encryption (or KEM).
	* These two are important: mention NIST competition
	* What is a signature?
	* Encryption examples: Kyber, ECDH, homomorphic
	* Signature examples: Dilithium, ECDSA
* Applications: HTTPS, Payment Protocol, Cryptocurrency
* Building block
	* Elliptic curve ops
	* Lattices (linear algebra over ring + noise)
	* Number theory (for RSA), isogeny, etc.

## What to verify?

* Signature and encryption
	* What: functional correctness AND security
	* Who: EasyCrypt
* Applications
	* What: properties of certain data, maybe out of the scope of the discussion today
	* Who: Tamarin
	* Example: [Payment protocol verification](https://ieeexplore.ieee.org/document/9519404)
* Building blocks
	* What: correctness
	* Fiat Crypto seems to belong here

## Fiat Crypto

* Main contributions
	* Optimized elliptic curve operations
	* Correctness proof in Coq
	* Before CryptOpt: Output C code, 2x slower than handwritten asm, need to trust gcc
	* [CryptOpt](https://arxiv.org/abs/2211.10665) (PLDI): Asm output from Fiat IR
* Fiat IR limitations
	* No loop. Can't do Dilithium for example. E-graphs can't deal with loops.
	* Deterministic. Might be annoying dealing with Gaussian sampling (i.e. from [Falcon](https://falcon-sign.info/) or some FHE).

## Homomorphic Encryption: Theory

* My understanding is a bit outdated from [GSW](https://eprint.iacr.org/2013/340.pdf).
* All known constructions are lattice-based
	* Yes, that makes it post quantum.
	* Still surprised that we're interested though. This isn't their selling point.
	* *None* of the NIST winners are homomorphic, somewhat ironically.
* GSW construction: "approximate eigenvectors"
	1. Intuition: As=e is hard to solve
	1. pk = A, sk = s
	1. Enc(m) = mI + RA where R is random 0/1
	1. Dec(M): Find m such that Ms ~ ms
	1. Eval(M1, M2, +) = M1 + M2. Same with multiplication
	1. Issue: Error grows too quickly
	1. Actual construction: Enc(m) = f(mI + b(RA)).
		* Forgot how f and b are defined. Some bitwise decomposition magic...
		* Decryption changed accordingly. Things cancel out "nicely"...

## Homomorphic Encryption: Implementations

* Google's [C++ Transpiler](https://github.com/google/fully-homomorphic-encryption) and [HEIR](https://heir.dev/)
* Use the [TFHE](https://tfhe.github.io/tfhe/) library based on [CGGI19](https://eprint.iacr.org/2018/421), an improvement over GSW.

# Question

What is [Knox](https://github.com/anishathalye/knox)? How does it compare with Kami?
