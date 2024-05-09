# Game hops for CKKS

The CKKS security proof can be roughly divided into two parts:
1. Proving IND-CPA security
2. Generalizing to IND-CPA+ security

Roughly speaking, the first part shows that CKKS is an encryption scheme, without taking homomorphic evaluation into account.
The second part shows that homomorphic evaluation does not damage the security.

## IND-CPA Security

It is known that homomorphic encryption scheme cannot achieve "IND-CCA" security against "active" adversaries.
"IND-CPA" as below is the next best thing one can hope for.
Roughly speaking, the adversary cannot distinguish fresh encryptions of different messages.

### Encryption and Decryption Algorithms

To establish notation, let us recall the CKKS construction.
For clarity of notation, we define the following types:
```
plaintext = poly
ciphertext = poly * poly
public_key = poly * poly
evaluation_key = poly * poly
secret_key = poly
```
Note that we start with polynomial plaintexts.
In other words, for now we do not consider mapping complex vector plaintexts to polynomials by inverse Fourier transform.

Now we give the part of CKKS that is relevant to the IND-CPA security.
We leave the homomorphic evaluation for later.
We follow the style where public key (that is, *encryption* key) and evaluation key are separate.
```
int P // parameter, positive integer

def keygen() -> public_key * evaluation_key * secret_key:
	// generate public and secret keys
	a <- random poly
	s <- random "small" poly
	e <- random poly with Gaussian coeffs.
	pk = (- a * s + e, a)
	// generate evaluation keys
	a' <- random poly mod Pq
	e' <- random poly with Gaussian coeffs.
	evk = [(a' s + e', a') + (P * s^2, 0)] mod Pq
	return (pk, evk, s)

def encrypt(a: public_key, m: poly):
	nu <- random "small" poly
	e0, e1 <- random poly with Gaussian coeffs.
	return pk * nu + (e0, e1) + (m, 0)

def decrypt(sk: poly, (beta, alpha): poly^2):
	return beta + alpha * sk
```

### IND-CPA Game

We start by defining the "encryption oracle" interface exposed to the adversary.
```
interface EncryptionOracle:
	def encrypt(m0: poly, m1: poly) : ciphertext
```

Now we define such an encryption oracle in our "Game 0" = IND-CPA game.
```
INDCPA_Oracle: EncryptionOracle
	public_key pk
	secret_key sk
	bool b

	def initialize(pk_in, b_in):
		pk := pk_in
		b := b_in

	def encrypt(m_0, m_1):
		if b:
			return encrypt(pk, m_0)
		else
			return encrypt(pk, m_1)
```

We next define an interface that the adversary must follow.
```
interface INDCPA_Adversary(O: EncryptionOracle):
	def distinguish(pk: public_key, evk: evaluation_key) : bool
```

We are now finally ready to define the IND-CPA game.
Notice that this captures an intuition idea of what an encryption should accomplish.
```
def main():
	b <$ {0, 1}
	(pk, evk, sk) <- keygen()
	Oracle.initialize(pk, b)
	b' <- Adversary.distinguish(pk, evk)
	return (b == b')
```

### The RLWE Game

Another vital building block in the security proof is the RLWE assumption, which we now also restate as a game.
The RLWE problem says that it is difficult to distinguish samples of the form `(a, as + e)` from uniformly random.
The small polynomial `s` is chosen at the beginning of the game,
then the distinguisher receives as many samples as it wishes for freshly sampled `a` and `e`.

We begin by defining a (non-interactive) distinguisher that receives a list of samples.
```
interface RLWE_Distinguisher():
	def main(list<poly * poly> samples) -> bool
```
Now we define the RLWE Game with the number of samples `qs` as a parameter.
```
RLWE(Distinguisher : RLWE_Distinguisher) :
	def RWLE::main(q):
		b <$ {0, 1}
		samples = list()
		s <- random small poly
		for i in range(qs):
			alpha <- uniformly random poly
			if b:
				e <- Gaussian error
				beta = alpha * s + e
			else:
				beta <- uniformly random poly
			samples.append(beta, alpha)
		b' <- Distinguisher.main(samples)
		return (b == b')
```
For reasons that we will soon see, it is sometimes inconvenient to have the value `b` be sampled in runtime.
We now write RLWE instead as the difference between a pair of games.
```
def RLWE0::main():
	samples = list()
	s <- random small poly
	for i in range(qs):
		alpha <- uniformly random poly
		e <- Gaussian error
		beta = alpha * s + e
		samples.append(beta, alpha)
	b <- Distinguisher.main(samples)
	return b

def RLWE1::main():
	samples = list()
	s <- random small poly
	for i in range(qs):
		alpha <- uniformly random poly
		beta <- uniformly random poly
		samples.append(beta, alpha)
	b <- Distinguisher.main(samples)
	return b
```
There exists a standard simple reduction from this version to the version above.
That is, for all adversary `A` there exists a reduction `R(A)` so that
`|RLWE0(A)::main() - RLWE1(A)::main()| / 2 = RLWE(R(A))::main() - 1/2`.

We additionally need a "correlated" version for RLWE, where one runs `qc` instances of RLWE run in parallel,
each with their own secret `s_i`.
Moreover, the samples are correlated in that the `j`-th sample have the same `alpha_j` across all instances.
Roughly speaking, this is to address that the encryptions of different messages share the same public key.
I can throw together a proof that things are still hard to break, up to losing a factor of `qc`.
There could be a better proof out there with a tighter bound.

```
// a collection of samples (betas, alpha), one beta from each RLWE instance
type multi_sample = list<poly> * poly

interface MultiRLWE_Distinguisher():
	def main(samples: list<multi_sample>) -> bool

MultiRLWE0(Distinguisher : MultiRLWE_Distinguisher):
	samples = list<multi_sample>()
	for i in range(qs):
		alpha <- uniformly random poly
		for i in range(qc):
			si <- "small" poly
			ei <- Gaussian error
			betas.append(a * si + ei)
		samples.append(betas, alpha)
	b <- Adversary.distinguish(samples)
	return b

MultiRLWE1(Distinguisher : MultiRLWE_Distinguisher):
	samples = list<multi_sample>()
	for i in range(qs):
		alpha <- uniformly random poly
		for i in range(qc):
			beta_i <- uniformly random poly
			betas.append(beta_i)
		samples.append(betas, alpha)
	b <- Adversary.distinguish(alpha, betas)
	return b
```

We can bound the "distance" between `MultiRLWE0` and `MultiRLWE1` by a standard "hybrid argument".
More concretely, we define `qc - 1` intermediate games.
In the `i`-th intermediate game, the first `i - 1` samples are real RLWE samples and the rest are uniformly random.
We can then write down a reduction from distinguishing the `i`-th game to the next one to distinguishing `RLWE0` and `RLWE1` by lining up the interfaces accordingly and generating all samples except for the `i+1`-th one during the reduction.
We save the details for later since it just involves too much boilerplate.
For now we note that formalizing hybrid arguments indexed by parameters [is not new in EasyCrypt](https://github.com/formosa-crypto/dilithium/blob/main/proofs/security/ReprogHybrid.eca).

### First game hop

In the first game hop, we replace the evaluation key with a uniformly random polynomial, while the rest of the game stays unchanged.
```
def G1::main():
	b <$ {0, 1}
	(pk, _, sk) <- keygen()
	evk <- random poly
	Oracle.initialize(pk, b)
	b' <- Adversary.distinguish(pk, evk)
	return (b == b')
```

We next need to prove our `G1` gamme is indistinguishable from the INDCPA game.
We do so by a standard reduction to the RLWE problem.
More concretely, given any INDCPA adversary `A`,
we construct a RLWE distinguisher `G1_Reduce(A)` such that if `A` can distinguish between G1 and INDCPA then `G1_Reduce(A)` can distinguish between `RLWE0` and `RLWE1`.
```
G1_Reduce_Oracle = INDCPA_Oracle

G1_Reduce(A) : RLWE_Distinguisher
	def main(samples):
		(pk, _, sk) <- keygen()
		evk = samples[0]
		Oracle.initialize(pk, b)
		b' <- Adversary.distinguish(pk, evk)
		return (b == b')
```
By construction then we have for all adversaries `A` against INDCPA,
`INDCPA(A)` and `RLWE0(R1_Reduce(A))` are identical experiments, and same for `G1(A)` and `RLWE1(R1_Reduce(A))`.
We therefore have `|INDCPA(A)::main() - G1(A)::main()| / 2 <= RLWE(R'(A)) - 1 / 2` for some reduction `R'`.

### Second game hop

In the second game hop, we replace the public key with uniformly random polynomials.
The same encryption oracle stays unchanged except initialized with this uniformly random public key.
```
def G2::main():
	b <$ {0, 1}
	pk <$ (uniformly random poly)^2
	evk <- (random poly)^2
	Oracle.initialize(pk, b)
	b' <- Adversary.distinguish(pk, evk)
	return (b == b')
```

We now prove that the game `G1` is indistinguishable from the IND-CPA game by reducing to the `RWLE` game again.
The story is similar as the previous reduction.
For any an IND-CPA adversary `A`,
we define an RLWE adversary `G2_Reduce(A)` in a way that if `A` would behave differently between the IND-CPA game and `G1`,
then `G2_Reduce(A)` achieves the same indistinguishing advantage in the RLWE game.
We first define the oracle that we will provide for the IND-CPA adversary.
Now we write down our RLWE adversary itself.
```
G2_Reduce_Oracle = INDCPA_Oracle

G2_Reduce(A: INDCPA_Adversary) : RLWE_Adversary:
	def distinguish(samples):
		b <$ {0, 1}
		pk = samples[0]
		evk = (random poly)^2
		G2_Reduction_Oracle.init(pk, evk, b)
		b' <- A(G2_Reduction_Oracle).main()
		return (b = b')
```

By construction, we now have `G1(A) = RLWE0(G2_Reduce(A))` and `G2(A) = RLWE1(G2_Reduce(A))`.
By the RLWE assumption then, the games `INDCPA(A)` and `G1(A)` must be close.

### Third game hop

Now we define `G3` where the encryption algorithm is changed.
Instead of padding the messages with an RLWE instance, truly random polynomials are used.
```
G3_Encryption_Oracle : EncryptionOracle
	bool b

	def initialize(b_in):
		b = b_in

	def encrypt(m_0, m_1):
		beta <- random poly
		alpha <- random poly
		if b:
			return (beta, alpha) + (m_0, 0)
		else
			return (beta, alpha) + (m_1, 0)
```
We again bound the "distance" between `G2` and `G3` using a reduction.
As the difference in `G2` and `G3` only lie in how the messages are padded,
we now rewrite the oracle to take those pads as inputs rather than generating them as needed.
```
G3_Reduction_Oracle : EncryptionOracle
	bool b
	list<poly> pads0
	list<poly> pads1

	def initialize(b, pads0, pads1): b(b), pads0(pads0), pads1(pads1) {}

	def encrypt(m_0, m_1):
		pad0 = pads0[0]
		pads0 = pads0[1:]
		pad1 = pads1[0]
		pads1 = pads1[1:]
		if b:
			return (pad0, pad1) + (m_0, 0)
		else
			return (pad0, pad1) + (m_1, 0)
```
One can see that if this oracle is initialized with RLWE samples, then the result is equivalent with the encryption oracle used in `G1`.
On the other hand, if it is initialized with uniformly random strings, the result is equivalent to `G2_Encryption_Oracle` above.
We can then finish the reduction accordingly.
```
G3_Reduce(A: INDCPA_Adversary) : MultiRLWE_Distinguisher
	def main(samples):
		b <$ {0, 1}
		pk = (sample[0].alpha, sample[1].alpha)
		G3_Reduction_Oracle.initialize(b, samples[0].betas, samples[1].betas)
		evk = (random poly)^2
		b' <- A(G2_Reduction_Oracle).main(pk, evk)
		return (b = b')
```
Following the same story as before,
we have `G2(A) = MultiRLWE0(G3_Reduce(A))` and `G3(A) = MultiRLWE1(G3_Reduce(A))`.
We can therefore argue their indistinguishability using the RLWE assumption.

### Showing IND-CPA security

We finally notice that the adversary has exactly 1/2 probability of winning `G3`.
This is because it receives no information on `b` throughout the execution.
To be pedantic, we observe the only syntactic dependency to `b` is in the encryption oracle.
We can easily remove this dependency by rewriting the oracle as follows:
```
G4_Encryption_Oracle : EncryptionOracle
	def encrypt(m_0, m_1):
		beta <- random poly
		alpha <- random poly
		return (beta, alpha)
```
The experiment then becomes completely independent from `b`, which means a adversary guesses it correctly with probability exactly one half.
(Or it diverges and runs forever.)
