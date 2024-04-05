# Theory

Disclaimer: I haven't read all of these yet. I might discover gaps as I work through the list myself.

Most of these papers have recorded conference talks, except the very recent ones still under submission.
Just search up the title on YouTube and you should easily find the available ones.

For FHE constructions, there are four generations of FHEs. All except first generations are in use today, as each is suited for different applications.
1. The "first generation" is very inefficient. Homomorphic evaluation overhead scales at n^4 with really bad constants. Some early works also base their security on non-standard assumptions. The theory was immature and we're better off reading later papers.
2. The "second generation" consists of the "BGV" and "BFV" constructions.
	* The "BGV" construction won the Godel prize. The theory is given by the FOCS 2011 paper [BV](https://eprint.iacr.org/2011/344) and its follow-up work [BGV](https://eprint.iacr.org/2011/277).
	* The "BFV" construction consists of the Brakerski [paper](https://eprint.iacr.org/2012/078) and the [FV](https://eprint.iacr.org/2012/144) paper.
	* It was [shown recently](https://eprint.iacr.org/2022/1363) that BGV and BFV can be unified under one framework, and we can convert between their ciphertexts.
3. The "third generation" [CKKS](https://eprint.iacr.org/2016/421) forgoes perfect correctness for efficiency. It is popular for potential machine learning applications.
4. From the "fourth generation", the [TFHE paper](https://eprint.iacr.org/2018/421) is itself a unification of two earlier papers and should describe the theory in full. Alternatively, [GSW](https://eprint.iacr.org/2013/340) is a beginner-friendly introduction and a personal favorite of mine ever since my theory days in Taiwan.

FHEs suffer from weaker security guarantees that are explored [here](https://eprint.iacr.org/2020/1533) in the context of third generation schemes. Related issues in other generations are subsequently identified and exploited recently in this [paper](https://eprint.iacr.org/2024/127).

For lattice background, I learned it some years ago from Regev's [survey paper](https://ieeexplore.ieee.org/document/5497885) and I would certainly recommend it if needed.

# Engineering

Popular open-source FHE implementations today include [OpenFHE](https://github.com/openfheorg/openfhe-development), [TFHE](https://tfhe.github.io/tfhe/), [TFHE-rs](https://github.com/zama-ai/tfhe-rs), and [SEAL](https://github.com/microsoft/SEAL).

As homomorphic encryptions are not user-friendly, there are many efforts to produce compilers.
* [SoK: FHE compilers](https://arxiv.org/abs/2101.07078) is a good place to start.
* From there, I recommend Google's FHE compiler [paper](https://arxiv.org/abs/2106.07893) and their [talk](https://www.youtube.com/watch?v=kqDFdKUTNA4) on their ongoing follow-up [HEIR](https://heir.dev/) project.
* There are also other compilers including Microsoft's ([EVA](https://github.com/microsoft/EVA)) and [Concrete](https://github.com/zama-ai/concrete) from the start-up Zama.
* IBM's [HElayers](https://ibm.github.io/helayers/index.html) probably also fits into this picture somewhere.

There is a [standardization effort](https://homomorphicencryption.org/) that produced a draft in 2019.
It's unclear what's the impact though. (Moreover, the SoK above complains that the suggested parameters here are bad.)

There is a DARPA [program](https://www.darpa.mil/news-events/2021-03-08) on hardware acceleration of FHEs, and I've heard they aren't the only ones interested. I don't know much about this effort yet unfortunately.

# Motivations and Applications

One widely deployed FHE application is Microsoft Edge's [password monitor](https://www.microsoft.com/en-us/research/blog/password-monitor-safeguarding-passwords-in-microsoft-edge/).

There are more research attempts to implement applications, including MIT's [private web search](https://www.youtube.com/watch?v=96PKpE1VWUs), Microsoft research's [phishing detection](https://ieeexplore.ieee.org/abstract/document/9053729), IBM's anti-money laundering [collaboration](https://ibm-research.medium.com/top-brazilian-bank-pilots-privacy-encryption-quantum-computers-cant-break-92ed2695bf14) with [Banco Bradesco](https://en.wikipedia.org/wiki/Banco_Bradesco), and Duality's cancer research [PNAS paper](https://www.pnas.org/doi/10.1073/pnas.2304415120) from collaborating with a teaching hospital.

There are many successful start-ups including [Zama](https://www.zama.ai/), [Duality](https://dualitytech.com/), [Enveil](https://www.enveil.com/),[Inpher](https://inpher.io/), and [IXUP](https://ixup.com/).
