# Academia

Non-exhausive list of papers below, mostly from TCHES.

| Title | Motivation | Tools | Publication Venue | Repository (if available) |
|:-:|:-:|:-:|:-:|:-:|
|Pushing the Limits: A Very Compact and a Threshold Implementation of AES| |Mentor Graphics ModelSimXE 6.4b, Synopsys DesignCompiler| EUROCRYPT 2011 |N/A|
|An Efficient Side-Channel Protected AES Implementation with Arbitrary Protection Order|  |VHDL | CT-RSA 2017 | [GitHub](https://github.com/hgrosz/aes-dom) |
|FPGA-based Key Generator for the Niederreiter Cryptosystem using Binary Goppa Codes| PQC |Verilog|TCHES 2017|[Hosted at Yale](https://caslab.csl.yale.edu/code/keygen/)|
|Security on Plastics: Fake or Real?| Flexible electronics | Verilog | TCHES 2019 | N/A |
|Crypto Accelerators for Power-Efficient and Real Time on-Chip Implementation of Secure Algorithms|| SystemVerilog, Synopsys Design Compiler, Silvaco 45nm Open Cell Library | ICECS 2019 |
|FPGA-based SPHINCS+ Implementations: Mind the Glitch| PQC | VHDL, Xilinx Vivado 2018.2 | DSD 2020 | N/A |
|Masking FALCON's Floating-Point Multiplication in Hardware| PQC | SystemVerilog, Xilinx Vivado 2020.2 | TCHES 2024 | N/A |
|Efficient ASIC Architecture for Low Latency Classic McEliece Decoding| PQC |Verilog, SystemVerilog, Cadence Genus and Innovus software systems|TCHES 2024| N/A|

There is also a lot of C. Embedded and/or system-on-chip stuff.
Some papers are a bit unclear as well, in terms of tools used.

## OpenTitan Project

First open-source "silicon root of trust" (SiRoT).
Isolated silicon region with tamper-proof memory, crypto accelerators, voltage and temperature monitoring, etc; for PC, servers, and smartphones.

* [homepage](https://opentitan.org/book/doc/introduction.html)
* Partners/contributors include Google, ETH Zurich, Seagate.
* [Formal collaboration](https://lowrisc.org/news/lowrisc-and-microsoft-collaborate-to-help-bring-the-revolutionary-cheriot-ibex-core-to-production-grade/) with Microsoft as well.
  "We’re looking forward to seeing it broadly leveraged in commercial designs, bringing much-needed hardware security — in an efficient manner — to a broad swathe of critical applications."
* [Invited talk](https://www.youtube.com/watch?v=AEGlZkkXm2E) at CHES by Tech Lead / Manager @ Google
* Paper: "Unleashing OpenTitan’s Potential: a Silicon-Ready Embedded Secure Element for Root of Trust and Cryptographic Offloading"
* [GitHub Repository](https://github.com/lowRISC/opentitan) says ~45% SystemVerilog, ~20% C, and a bunch of other stuff.
* Crypto [library](https://opentitan.org/book/doc/security/cryptolib/cryptolib_api.html?utm_source=chatgpt.com) under development?

# Industry

Main examples:
* Government-issued ID such as passports
* Credit card chips
* SIM cards

Easy to find providers, but difficult to find the underlying technique.
Things are not open source.

* IBM [security co-processors](https://www.ibm.com/products/pcie-cryptographic-coprocessor)
* Apple [Secure Enclave](https://support.apple.com/guide/security/secure-enclave-sec59b0b31ff/web) in most iPhone, iPad, and Mac. Includes an AES engine and public key accelerator.
* Google Titan M, Samsung Knox, and Microsoft Pluton similar as above
* Thales Group: [US passport](https://www.businesswire.com/news/home/20230510005013/en/Thales-awarded-multi-year-contract-for-new-generation-US-Passport-eCovers), [Belgium passport](https://www.biometricupdate.com/202010/thales-zetes-contracted-for-embedded-biometrics-and-forgery-protections-of-new-belgian-passports), [SIM cards for mobile provider companies](https://www.thalesgroup.com/en/markets/digital-identity-and-security/mobile/secure-elements/SIM-product-offer), [mass deployment for credit card issuers](https://www.thalesgroup.com/en/markets/digital-identity-and-security/banking-payment/cards/emv/value-payment-cards)
* IDEMIA annd NXP Semiconductors: similar as above
* Infineon Technologies: [Chips for >50% EU passports and >75% Asia-Pacific passports](https://www.infineon.com/cms/en/applications/security-solutions/government-identification/electronic-passport/).
  Security co-processor such as  ["SLE 78CLFX1M10PH"](https://www.infineon.com/cms/en/product/security-smart-card-solutions/security-controllers/contactless-and-dual-interface-security-controllers/sle-78clfx1m10ph/).
