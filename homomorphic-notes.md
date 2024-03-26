# Why aren't we using FHE?

> While it is clear that both significant advances have been made and many challenges remain open, there is no systematic understanding of the remaining engineering challenges that need to be addressed to help broaden FHE adoption.

\- [SoK: FHE compiler](https://arxiv.org/abs/2101.07078)

There is no consensus on one bottleneck stopping the widespread adoption of FHE.
Most players identify and work on challenges within their own community as below:

## Need for standardization?

> But before Homomorphic Encryption can be adopted in medical, health, and financial sectors to protect data and patient and consumer privacy, it will have to be standardized, most likely by multiple standardization bodies and government agencies.

\- [Homomorphic Encryption Standard](https://homomorphicencryption.org/wp-content/uploads/2018/11/HomomorphicEncryptionStandardv1.1.pdf)

This is surprisingly challenging, even comparing to NIST PQC standardization.
* FHE interface is more complicated! Different FHEs are good at different things.
	* BFV and BGV (can be [unified](https://eprint.iacr.org/2021/204)) are leveled and batched
	* CKKS is fast but inexact - good for ML
	* TFHE introduces fast bootstrapping at the cost of batching
	* What about chimeric FHEs?
* Parameter selection is hard!
	* It's not like Dilithium that just has 3 sets of agreed parameters.
	* More complicated functions => larger q => less secure => need larger n.
	* Input magnitude also matters.
* None of the NIST PQC round 3 candidates are homomorphic.

## Not user-friendly enough?

> the widespread adoption of FHE still requires available tools that allow software developers without cryptography expertise to incorporate FHE into their applications

\- Google FHE compiler [paper](https://arxiv.org/abs/2106.07893)

> You know, Google's huge, and we have only so many crypto experts, so that expertise won't scale across all the people who could benefit it or want to use from it.

> The FHE team at Google is uh, like, three engineers

\- Google MLIR [talk](https://www.youtube.com/watch?v=kqDFdKUTNA4).

> HE libraries provide basic cryptographic components, but organizations leveraging them must dedicate engineering, algorithmic, and integration resources in order to mature the basic building blocks into viable, enterprise-grade solutions.

\- Enveil [FAQ](https://www.enveil.com/privacy-enhancing-technologies/): research vs commercialization.

FHEs require expertises that are out of reach from average programmers, and there are perhaps not enough experts.
* Even simple additions can be nontrivial. (See Sklansky or Kogge-Stone adders.)
* [Tricky](https://github.com/microsoft/SEAL/blob/main/SECURITY.md) to use *securely*
	* Security guarantees are [quite unintuitive](https://eprint.iacr.org/2020/1533).
	* Relevant recent [bad news](https://eprint.iacr.org/2024/127): some leading FHE libraries broken.
* Current efforts
	* Google [C++ transpiler](https://github.com/google/fully-homomorphic-encryption) and [HEIR](https://heir.dev/)
	* Microsoft [SEAL](https://github.com/microsoft/SEAL) and [EVA](https://github.com/microsoft/EVA)
	* IBM's [HElayers](https://ibm.github.io/helayers/index.html)
	* Many others from start-ups

## Need further optimizations?

> Dedicated FHE hardware accelerators, which are coming in the next two years, will bridge the final gap to enable web-scale applications such as confidential Large Language Models (LLMs) and encrypted Software-as-a-Service (SaaS).

> In the past two years alone, \[homomorphic evaluations\] progressed from being 1,000,000 times slower to being 10,000 to 1,000 times slower, and we are on track to be less than 10 times slower by 2025. This marks the moment where FHE can become ubiquitous, to employ everywhere we desire privacy.

\- [Zama](https://www.zama.ai/post/zama-fhe-master-plan)

* [DARPA program](https://www.darpa.mil/news-events/2021-03-08) for FHE hardware accelerator
* Encryption incurs [20,000x space overhead](https://www.jeremykun.com/2023/02/13/googles-fully-homomorphic-encryption-compiler-a-primer/) however.

### Algorithms need to be co-designed?

> (...) designing a new kind of search algorithm that is friendly for use with homomorphic encryption schemes (...)

\- MIT Schwarzman College of Computing [talk](https://www.youtube.com/watch?v=96PKpE1VWUs)

> batching (...) can lead to runtime improvements of many orders of magnitude. However, it (...) \[requires\] a deep understanding of both the application and the performance-tradeoffs of the FHE scheme
> (...) this remains an open problem for more general applications.

\- FHE compiler [SoK paper](https://arxiv.org/pdf/2101.07078.pdf)

A story that we're maybe all too familiar with.
Near-term applications may need to break abstractions.

## Lack of business incentives?

> I think the barrier to seeing this stuff used in practice very quickly will not be technical.
> (...) the problem is gonna be business incentives

\- MIT Schwarzman College of Computing [talk](https://www.youtube.com/watch?v=96PKpE1VWUs)

> Why does Google care about FHE?

> there is an option to allow enterprise customers to encrypt their data with their own encryption key, (...) but then you sort of lose some of the features (...) like virus scanning

\- Google HEIR [talk](https://www.youtube.com/watch?v=kqDFdKUTNA4). See also Microsoft's [paper](https://ieeexplore.ieee.org/abstract/document/9053729) on phishing detection.

> In fact, commercial and government entities are using HE operationally at scale today. Not working toward using it — actually using it in production environments to solve real problems.

\- Enveil [FAQ](https://www.enveil.com/faq/)

> by 2025, 60% of large organizations will use privacy-enhancing computation techniques to protect privacy in untrusted environments or for analytics purposes

\- Gartner "Innovation Insight for Federated Machine Learning", [quoted by Enveil](https://www.enveil.com/enveil-secures-25-million-in-series-b-funding/)

* [Password monitor](https://www.microsoft.com/en-us/research/blog/password-monitor-safeguarding-passwords-in-microsoft-edge/) deployed in Microsoft Edge
* IBM FHE [cloud computing service](https://he4cloud.com/public/about) for ML
* AWS "privacy-preserving ML" [talk](https://www.youtube.com/watch?v=ZQkB9XRqdnc)
* IBM's [collaboration](https://ibm-research.medium.com/top-brazilian-bank-pilots-privacy-encryption-quantum-computers-cant-break-92ed2695bf14) with [Banco Bradesco](https://en.wikipedia.org/wiki/Banco_Bradesco)

Start-up scene seems very active too.

### [Zama](https://www.zama.ai/)

* [Raised $73 millions in a Series A](https://www.zama.ai/post/zama-fhe-master-plan)
* Based in France
* "making FHE generally accessible to non-cryptography developers for the first time"
* Projects (all open source with permissive license)
	* [Concrete](https://github.com/zama-ai/concrete): Compiler for Python.
	* [Concrete ML](https://github.com/zama-ai/concrete-ml): Machine learning built on top of Concrete
	* [fhEVM](https://github.com/zama-ai/fhevm): Privacy for Ethereum blockchain smart contracts

### [Duality](https://dualitytech.com/)

* [Raised 16M in a Series A](https://techcrunch.com/2019/10/30/duality-cybersecurity-16-million/)
* Co-founded by Vinod Vaikuntanathan (2022 Gödel prize winner for efficient FHE scheme)
* Holds leadership positions in the [OpenFHE project](https://www.openfhe.org/)
* [Partnered with Scotiabank](https://www.prnewswire.com/news-releases/duality-technologies-launches-secureplus-query-the-first-privacy-enhanced-query-engine-for-data-collaboration-301022308.html), the third largest bank in Canada, for anti-money laundering detection
* Cancer research [PNAS paper](https://www.pnas.org/doi/10.1073/pnas.2304415120) resulting from collaborations with teaching hospitals.

### [Enveil](https://www.enveil.com/)

* [Raised 25 Millions in a Series B](https://www.enveil.com/enveil-secures-25-million-in-series-b-funding/)
* Washington D.C.-based
* [ZeroReveal](https://www.enveil.com/products/) search and ML. Unclear if anyone is buying it though.

### [Inpher](https://inpher.io/)

* Funded [10 millions USD](https://www.prnewswire.com/news-releases/jp-morgan-leads-usd-10-million-financing-in-leading-data-security-and-machine-learning-provider-inpher-300743090.html)
* Contributions to the open source [TFHE library](https://tfhe.github.io/tfhe/)
* [Many conference publications](https://inpher.io/learn/research/), some on FHE
* Sells non-FHE SaaS products

### [IXUP](https://ixup.com/)

* [Raised 5.75 millions](https://itmunch.com/data-encryption-provider-ixup-appoints-new-ceo-md-marcus-gracey/)
* Based in Australia. Approved cloud services supplier to the Australian Federal Government.
* [SaaS platform for data analysis](https://ixup.com/platform/)

### [Cosmian](https://cosmian.com/)

* [Raised 1.4 millions EUR](https://www.eu-startups.com/2019/03/paris-based-cosmian-raises-e1-4-for-its-platform-that-analyses-encrypted-data-while-keeping-it-private/) in 2019 while working on FHE applications
* Based in Paris
* Sells a lot of non-FHE stuff. No clue what happened to the FHE product development.
* Some FHE [papers](https://cosmian.com/technology/)...?

# Other observations

* Some problems cannot be solved using FHE alone
	* For example, E-voting requires verifiability
	* FHE might even be overkill sometimes. See [Helios](https://vote.heliosvoting.org/) voting and MIT [private web search](https://www.youtube.com/watch?v=ZQkB9XRqdnc).
* It is possible to "transcipher" from AES to FHE. Not sure if that line of work led anywhere though.
* What about differential privacy? Don't we need to consider that too for some applications?

# TODOs

* What's the impact of [CryptoNets](https://github.com/microsoft/CryptoNets)?
* [FHE survey 2017](https://arxiv.org/abs/1704.03578)
* [More current FHE uses](https://www.future-fis.com/uploads/3/7/9/4/3794525/ffis_innovation_and_discussion_paper_-_case_studies_of_the_use_of_privacy_preserving_analysis_-_v.1.3.pdf)
* What is Data61?
* How much has come out of iDASH?
