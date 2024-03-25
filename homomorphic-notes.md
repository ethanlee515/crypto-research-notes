# Roadblocks

What's stopping FHE from being widely adopted?

## Need for standardization

"But before Homomorphic Encryption can be adopted in medical, health, and financial sectors to protect data and patient and consumer privacy, it will have to be standardized, most likely by multiple standardization bodies and government agencies.": [Standardization](https://homomorphicencryption.org/wp-content/uploads/2018/11/HomomorphicEncryptionStandardv1.1.pdf)
* FHE interface is a lot more complicated! Different FHEs are good at different things.
* Parameter selection is hard! More complicated functions => larger q => less secure => larger n. Input (plaintext) magnitude also matters.

## Lack of support for non-experts

> the widespread adoption of FHE still requires available tools that allow software developers without cryptography expertise to incorporate FHE into their applications
from Google FHE compiler [paper](https://arxiv.org/abs/2106.07893)

* Even simple additions can be hard (see Sklansky or Kogge-Stone adders).
* Easy to misuse and lose security
* TODO expertise "don't scale" - find quote in Jeremy Kun's talk
* Current efforts
	* Google FHE compiler and HEIR
	* Microsoft EVA and SEAL
	* Zama's Concrete

## Need further optimization

> Dedicated FHE hardware accelerators, which are coming in the next two years, will bridge the final gap to enable web-scale applications such as confidential Large Language Models (LLMs) and encrypted Software-as-a-Service (SaaS).
> In the past two years alone, \[Homomorphic evaluation\] progressed from being 1,000,000 times slower to being 10,000 to 1,000 times slower, and we are on track to be less than 10 times slower by 2025. This marks the moment where FHE can become ubiquitous, to employ everywhere we desire privacy.
from [Zama](https://www.zama.ai/post/zama-fhe-master-plan)

> HE libraries provide basic cryptographic components, but organizations leveraging them must dedicate engineering, algorithmic, and integration resources in order to mature the basic building blocks into viable, enterprise-grade solutions.
From [Enveil FAQ](https://www.enveil.com/privacy-enhancing-technologies/) on research vs commercialization.

* [20,000x space overhead](https://www.jeremykun.com/2023/02/13/googles-fully-homomorphic-encryption-compiler-a-primer/) on encryption.
	* Putting things in perspective, a 3-min video (9 MiB) after encryption would be as large as ~20 films (188GiB).

Some GPU or ASIC efforts too...?

## Lack of Coordination

* "While it is clear that both significant advances have been made and many challenges remain open, there is no systematic understanding of the remaining engineering challenges that need to be addressed to help broaden FHE adoption.": from [SoK: FHE compiler](https://arxiv.org/abs/2101.07078)

## Maybe we're on track already?

What *is* wide adoption anyways?

* Microsoft's [password monitor](https://www.microsoft.com/en-us/research/blog/password-monitor-safeguarding-passwords-in-microsoft-edge/)

Start-up scene is also *very* active.

# So, what's been done today?

## [Zama](https://www.zama.ai/)

* [Raised 73 millions in a Series A](https://www.zama.ai/post/zama-fhe-master-plan)
* Based in France
* "making FHE generally accessible to non-cryptography developers for the first time"
* Projects: fhEVM, concrete

## [Enveil](https://www.enveil.com/)

* [Raised 10 millions USD](https://www.globenewswire.com/news-release/2020/02/18/1986152/0/en/Enveil-Raises-10-Million-in-Series-A-Funding.html)
* Selling points
	* [PET](https://www.enveil.com/privacy-enhancing-technologies/)
	* [Secure AI](https://www.enveil.com/secure-ai/)

## [Inpher](https://inpher.io/)

* Funded: [10 millions USD](https://www.prnewswire.com/news-releases/jp-morgan-leads-usd-10-million-financing-in-leading-data-security-and-machine-learning-provider-inpher-300743090.html)
* Selling point: ML. Financial, healthcare, defense (IoT).
* [Many conference publications](https://inpher.io/learn/research/)

## [Duality](https://dualitytech.com/)

* [Raised 16M](https://techcrunch.com/2019/10/30/duality-cybersecurity-16-million/)
* Financial, healthcare, insurance
* Collaborated with Dana Farber Cancer Institute
* Holds leadership positions in OpenFHE
* Co-founded by Vinod Vaikuntanathan

## [IXUP](https://ixup.com/)

* [Raised 5.75 millions](https://itmunch.com/data-encryption-provider-ixup-appoints-new-ceo-md-marcus-gracey/)
* Based in Australia

## [Cosmian](https://cosmian.com/)

* [Raised 1.4 millions EUR](https://www.eu-startups.com/2019/03/paris-based-cosmian-raises-e1-4-for-its-platform-that-analyses-encrypted-data-while-keeping-it-private/)
* Based in Paris
* Unclear (at a first glance) where is FHE in their products

# Schemes

* [Comparison of existing schemes](https://blog.sunscreen.tech/from-toy-programs-to-real-life-building-an-fhe-compiler/)

# Implementations

Here's mostly the software stuff.
There are some GPU as well as FPGA/ASIC works too.

## LibScarab

## OpenFHE

## HElib

## Google's [C++ Transpiler](https://github.com/google/fully-homomorphic-encryption) and [HEIR](https://heir.dev/)

* Use the [TFHE](https://tfhe.github.io/tfhe/) library based on [CGGI19](https://eprint.iacr.org/2018/421), an improvement over GSW.

## Microsoft's SEAL and EVA

##[FHEW](https://github.com/lducas/FHEW)

* [paper](https://eprint.iacr.org/2014/816)

## HEAAN

# Applications

* Information retrieval? (so, private search engine queries?) TODO watch MIT programming lab video?
* Zama's visions
	* HTTPZ
	* Encrypted cryptocurrency and smart contracts
	* Encrypted biometrics (face ID) - ML
	* Advertising? (How though... outputs leak information too.)
* ML? Virtual assistants maybe?
* Finance
* Healthcare
* Cloud computing
* [current uses?](https://www.future-fis.com/uploads/3/7/9/4/3794525/ffis_innovation_and_discussion_paper_-_case_studies_of_the_use_of_privacy_preserving_analysis_-_v.1.3.pdf)

## Research?

* [CryptoNets](https://github.com/microsoft/CryptoNets)
* [IBM + Bradesco, financial/ML application](https://eprint.iacr.org/2019/1113)
* [Phishing email detection](https://ieeexplore.ieee.org/abstract/document/9053729). Also see Google's virus scanning.
* [VoIP + FHE](https://ieeexplore.ieee.org/document/7782328) from 2010

## Non-applications

* Voting? Helios exist though.

# Other noteworthy observations

* Maybe it's worth double checking the thing is secure? We don't want [more](https://eprint.iacr.org/2020/1533) [bad news](https://eprint.iacr.org/2024/127)...
* [Construction allowing multiple clients](https://eprint.iacr.org/2013/094)
* "For an everyday example, consider the current process of searching for directions on your mobile phone application. By conducting this query, you reveal to the application your current location, destination, time of day, and intent to go from point A to point B." - [IBM](https://www.ibm.com/support/z-content-solutions/fully-homomorphic-encryption/)
* Transcipher from AES to FHE?

## Questions

* What about differential privacy? Don't we need to consider that too in some applications?
* MLaaS...? research [paper1](https://eprint.iacr.org/2019/294), [paper2](https://arxiv.org/abs/1912.11951)

# Resources

[BFV = BGV?](https://eprint.iacr.org/2021/204)
[open source library?](openfhe.org)
[youtube talks or something?](fhe.org)
[standardization?](homomorphicencryption.org)
[SoK?](https://arxiv.org/abs/2101.07078)
[???](https://www.databridgemarketresearch.com/reports/global-fully-homomorphic-encryption-market)

## Reading list

* [FHE survey 2017](https://arxiv.org/abs/1704.03578)
* [Eurocrypt 2021 talk](https://www.youtube.com/watch?v=487AjvFW1lk)
* [Provided by IBM](https://www.zdnet.com/article/ibm-launches-experimental-homomorphic-data-encryption-environment-for-the-enterprise/)
* [EU HEAT](https://cordis.europa.eu/project/id/644209)
* Data61?

