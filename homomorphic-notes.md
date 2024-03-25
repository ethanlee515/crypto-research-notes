# Why aren't we using FHE?

> While it is clear that both significant advances have been made and many challenges remain open, there is no systematic understanding of the remaining engineering challenges that need to be addressed to help broaden FHE adoption.

\- [SoK: FHE compiler](https://arxiv.org/abs/2101.07078)

There is no consensus on one bottleneck stopping the widespread adoption of FHE.
Most players identify and solve challenges within their own community as below.

## Need for standardization?

> But before Homomorphic Encryption can be adopted in medical, health, and financial sectors to protect data and patient and consumer privacy, it will have to be standardized, most likely by multiple standardization bodies and government agencies.

\- [Homomorphic Encryption Standard](https://homomorphicencryption.org/wp-content/uploads/2018/11/HomomorphicEncryptionStandardv1.1.pdf)

* FHE interface is more complicated! Different FHEs are good at different things.
	* BFV and BGV (theoretically [equivalent](https://arxiv.org/abs/2101.07078)) are leveled and batched
	* CKKS is fast but inexact - good for ML
	* TFHE introduces fast bootstrapping
* Parameter selection is hard! More complicated functions => larger q => less secure => larger n. Input magnitude also matters.

## Lack of support for non-experts?

> the widespread adoption of FHE still requires available tools that allow software developers without cryptography expertise to incorporate FHE into their applications

\- Google FHE compiler [paper](https://arxiv.org/abs/2106.07893)

> You know, Google's huge, and we have only so many crypto experts, so that expertise won't scale across all the people who could benefit it or want to use from it.

> The FHE team at Google is uh, like, three engineers

\- Google MLIR [talk](https://www.youtube.com/watch?v=kqDFdKUTNA4).

> HE libraries provide basic cryptographic components, but organizations leveraging them must dedicate engineering, algorithmic, and integration resources in order to mature the basic building blocks into viable, enterprise-grade solutions.

\- Enveil [FAQ](https://www.enveil.com/privacy-enhancing-technologies/): research vs commercialization.

* Even simple additions can be nontrivial. (See Sklansky or Kogge-Stone adders.)
* [Tricky](https://github.com/microsoft/SEAL/blob/main/SECURITY.md) to use correctly
	* Involves [unintuitive security guarantees](https://eprint.iacr.org/2020/1533).
	* See recent [bad news](https://eprint.iacr.org/2024/127): leading FHE libraries broken.
* Current efforts
	* Google [C++ transpiler](https://github.com/google/fully-homomorphic-encryption) and [HEIR](https://heir.dev/)
	* Microsoft [SEAL](https://github.com/microsoft/SEAL) and [EVA](https://github.com/microsoft/EVA)
	* IBM's [HElayers](https://ibm.github.io/helayers/index.html)
	* Many others from start-ups

## Need further optimizations?

> Dedicated FHE hardware accelerators, which are coming in the next two years, will bridge the final gap to enable web-scale applications such as confidential Large Language Models (LLMs) and encrypted Software-as-a-Service (SaaS).

> In the past two years alone, \[homomorphic evaluations\] progressed from being 1,000,000 times slower to being 10,000 to 1,000 times slower, and we are on track to be less than 10 times slower by 2025. This marks the moment where FHE can become ubiquitous, to employ everywhere we desire privacy.

\- [Zama](https://www.zama.ai/post/zama-fhe-master-plan)

Encryption incurs [20,000x space overhead](https://www.jeremykun.com/2023/02/13/googles-fully-homomorphic-encryption-compiler-a-primer/).

## Maybe we're on the right track?

> In fact, commercial and government entities are using HE operationally at scale today. Not working toward using it — actually using it in production environments to solve real problems.

\- Enveil [FAQ](https://www.enveil.com/faq/)

> Why does Google care about FHE?

> there is an option to allow enterprise customers to encrypt their data with their own encryption key, (...) but then you sort of lose some of the features (...) like virus scanning

\- Google HEIR [talk](https://www.youtube.com/watch?v=kqDFdKUTNA4). See also Microsoft's [paper](https://ieeexplore.ieee.org/abstract/document/9053729) on phishing detection.

* [Password monitor](https://www.microsoft.com/en-us/research/blog/password-monitor-safeguarding-passwords-in-microsoft-edge/) in Microsoft Edge
* IBM FHE [cloud service](https://he4cloud.com/public/about)
* Start-up scene seems *very* active too.

# Status of Some Start-ups

## [Zama](https://www.zama.ai/)

* [Raised 73 millions in a Series A](https://www.zama.ai/post/zama-fhe-master-plan)
* Based in France
* "making FHE generally accessible to non-cryptography developers for the first time"
* Projects
	* [Concrete](https://github.com/zama-ai/concrete): Compiler for Python.
	* [Concrete ML](https://github.com/zama-ai/concrete-ml): Machine learning built on top of Concrete
	* [fhEVM](https://github.com/zama-ai/fhevm): Privacy for Ethereum blockchain smart contracts

## [Duality](https://dualitytech.com/)

* [Raised 16M](https://techcrunch.com/2019/10/30/duality-cybersecurity-16-million/)
* Co-founded by Vinod Vaikuntanathan
* Holds leadership positions in the [OpenFHE project](https://www.openfhe.org/)
* Collaborated with Dana Farber Cancer Institute
* TODO Selling point: financial, healthcare, insurance?

## [Enveil](https://www.enveil.com/)

* [Raised 10 millions USD in 2020](https://www.globenewswire.com/news-release/2020/02/18/1986152/0/en/Enveil-Raises-10-Million-in-Series-A-Funding.html)
* [ZeroReveal](https://www.enveil.com/products/) search and ML

## [Inpher](https://inpher.io/)

* Funded [10 millions USD](https://www.prnewswire.com/news-releases/jp-morgan-leads-usd-10-million-financing-in-leading-data-security-and-machine-learning-provider-inpher-300743090.html)
* Selling point: ML. Financial, healthcare, defense (IoT).
* [Many conference publications](https://inpher.io/learn/research/)

## [IXUP](https://ixup.com/)

* [Raised 5.75 millions](https://itmunch.com/data-encryption-provider-ixup-appoints-new-ceo-md-marcus-gracey/)
* Based in Australia
* TODO products?

## [Cosmian](https://cosmian.com/)

* [Raised 1.4 millions EUR](https://www.eu-startups.com/2019/03/paris-based-cosmian-raises-e1-4-for-its-platform-that-analyses-encrypted-data-while-keeping-it-private/)
* Based in Paris
* Unclear (at a first glance) where is FHE in their products

# Other noteworthy observations

* FHE might be overkill for secure voting. [Helios](https://vote.heliosvoting.org/) has been used for ACM and IACR just fine.
* It is possible to "transcipher" from AES to FHE. Not sure if that line of work led anywhere though.
* What about differential privacy? Don't we need to consider that too for some applications?

## Potentially interesting papers

* [CryptoNets](https://github.com/microsoft/CryptoNets)
* [VoIP + FHE](https://ieeexplore.ieee.org/document/7782328) from 2010

# TODO

* Learn more from each start-up. For example, what kind of ML? Biometric authentication? Finance?
* Did IBM's [collaboration with the bank](https://eprint.iacr.org/2019/1113) go anywhere?
* [FHE survey 2017](https://arxiv.org/abs/1704.03578)
* [Eurocrypt 2021 talk](https://www.youtube.com/watch?v=487AjvFW1lk)
* [EU HEAT](https://cordis.europa.eu/project/id/644209)
* [More current FHE uses](https://www.future-fis.com/uploads/3/7/9/4/3794525/ffis_innovation_and_discussion_paper_-_case_studies_of_the_use_of_privacy_preserving_analysis_-_v.1.3.pdf)
* What is Data61?
* MIT programming lab talk on information retrieval. FHE is maybe not sufficient here?
* MLaaS...? research [paper1](https://eprint.iacr.org/2019/294), [paper2](https://arxiv.org/abs/1912.11951)
