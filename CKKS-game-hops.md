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
	def encrypt(m0: poly, m1: poly) : c: ciphertext
```

Now we define such an encryption oracle in our "Game 0" = IND-CPA game.
```
INDCPA_Oracle: EncryptionOracle
	poly^2 pk
	poly sk
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
```
def main():
	b <$ {0, 1}
	(pk, evk, sk) <- keygen()
	Oracle.initialize(pk, b)
	b' <- Adversary.distinguish(pk, evk)
	return (b == b')
```

### The RLWE Game

We next restate our RLWE assumption as a game.
Recall that the adversary needs to distinguish samples of the form `(a, as + e)` from uniformly random.
In principle, the adversary can receive as many samples as it wishes.
As this adversary is non-interactive (makes no oracle calls other than to receive samples), here we choose to provide the adversary with its list of samples up-front.
```
interface RLWE_Adversary():
	def distinguish(samples: list<poly * poly>)
```
Now we define the RLWE Game as expected.
```
// number of samples
// corresponds to the adversary's time/query complexity
int q

def RWLE::main():
	b <$ {0, 1}
	samples = list()
	if q:
		s <- "small" poly
		for i in range(q):
			ai <- uniformly random poly
			ei <- Gaussian error
			samples.append(ai, ai * s + ei)
	else:
		for i in range(q):
			ai <- uniformly random poly
			bi <- uniformly random poly
			samples.append(ai, bi)
	b' <- Adversary.distinguish(samples)
	return (b == b')
```

### First game hop

In the first game hop, we replace the key generation as follows:
```
def G1::main():
	b <$ {0, 1}
	pk <$ (uniformly random poly)^2
	Oracle.initialize(pk, b)
	b' <- Adversary.distinguish(pk, evk)
	return (b == b')
```

We now prove... TODO

### Second game hop

Reduce to... TODO
