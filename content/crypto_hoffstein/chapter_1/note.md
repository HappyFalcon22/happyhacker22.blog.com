+++
title = "Chapter 1 : Note"
date = 2023-08-31T12:45:22+07:00
author = "HappyHacker22"
draft = false
katex = true
color = "pink"
description = "Notes from Chapter 1 : `An Introduction to Cryptography`"
tag = ["notes", "hoffstein", "cryptography"]
enableEmoji = true
+++

# Chapter 1 : An Introduction to Cryptography

## 1. Simple substitution ciphers

Firstly, I want to put some of the definitions here :
+ **plaintext** :
+ **ciphertext** :
+ **cryptography - cryptology** :
+ **cryptic** :

In this section, we explore the most fundamental cipher that almost anyone (including non-cryptographers) knows about : ***Caesar Cipher***, named in honor of ***Julius Caesar***. 

<center>
<img src="./../images/Caesar.jpg" width="250" height="250">
</center>

Basically, the cipher encrypts the *plaintext* by shifting each letter in it by a constant, unknown amount (we call this the *key*), the result of this action forms the *ciphertext*. In more mathematical terms, the ***Shift Cipher*** is the cipher : 
+ Has the alphabet : \\( A \\) of length \\( a \\)
+ A key : \\( 0 \leq k < A \\)
+ Plaintext : \\( m = m_1m_2m_3...m_n \\)
+ Ciphertext : \\( c = c_1c_2c_3...c_n \\)
+ Encryption : \\( c_i = m_i + k \pmod{a} \\)
+ Decryption : \\( m_i = c_i - k \pmod{a} \\)

The Caesar Cipher uses \\( k = 3 \\), his cipher named after him in honor.

Let's make an example : suppose we want to encrypt the message `Rules are made to be broken.` using the shift cipher with key \\( k = 15 \\) and the alphabet `abcdefghijklmnopqrstuvwxyz`. The result ciphertext is ``.

Note that we consider the ciphertext's letters to be in upper case while the plaintext's letters are in lower case for differentiation.

In practice, this cipher is completely broken, as if Bob receives the ciphertext from Alice using the shift cipher, Bob just have to bruteforce all \\( a \\) possible cases of the key to find the plaintext (which is human-readable).

To make the cipher harder to attack, Bob can instead encrypt each letter in the alphabet with a different letter in the same alphabet. For example, Bob can replace `p` with `a`, `c` with `z`, etc. This idea derives a new cipher : ***simple substitution cipher***, that is, a cipher that each letter is replaced with another letter in the alphabet. This cipher can be described as the rule of function :

\\[ \lbrace a, b, c, d, e, f, g, h, ..., x, y, z \rbrace. \longleftarrow \lbrace A, B, C, D, E, F, G, H, ..., X, Y, Z \rbrace \\]

assigning each plaintext letter in the domain a different ciphertext letter in the range. This function can be described as a **subtitution table**, for example : 


| a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Y | A | D | F | Z | P | J | W | U | T | O | R | C | B | L | X | Q | V | E | G | H | N | M | S | I | K |

Note that, a letter can be replaced by the same letter, it is still count as a substitution.

Why is it that doing this will make the cipher hard to break ? Statistically, there are total \\( 26! \\) ways of creating a substitution rule. Hence, Bob will have to bruteforce waaaaay to much cases to find the correct rule ! (Readers can verify just how much \\(26!\\) is).

But, in practice, this cipher is still breakable (easily, in fact). Let's look at the small subsection.

### 1.1. Cryptanalysis of simple substitution ciphers

In definition, **cryptanalysis** means the art of decrypting the ciphertext without any knowledge about the encryption key of a cipher.

This section also teaches us a very important lesson about the practical side of the science of cryptography :

> Your opponent always uses her/his best strategy to defeat you, not the strategy that you want her/him to use. Thus, the security of an encryption system depends on the ***best known method*** to break that system. As new and improved methods are developed, the level of security can only get words, never better.

When Bob uses the simple substitution cipher, he expects Alice to try the exhausted search for the key, which is infeasible in above mentions. But Bob should not feel safe because she has another trick in her sleeve : `frequency analysis`.

Now, what makes the substitution cipher breakable is that *the plaintext is human-readable* (yes, as we see in later chapters, the plaintext can be unmeaningful to be readable). Hence, there is an invariant ***letter frequency*** from each language (English, Turkey, Thailand, Vietnamese, etc). Just take a random \\( 100000 \\) letters' paragraph and calculate the ratio of each letter in that paragraph. For example, in English, the letter frequency table is shown here :

| *Letter* | *Frequency (%)* | *Letter* | *Frequency (%)* |
|:--------:|:---------------:|:--------:|:---------------:|
|     a    |       8.15      |     n    |       7.10      |
|     b    |       1.44      |     o    |       8.00      |
|     c    |       2.76      |     p    |       1.98      |
|     d    |       3.79      |     q    |       0.12      |
|     e    |      13.11      |     r    |       6.83      |
|     f    |       2.92      |     s    |       6.10      |
|     g    |       1.99      |     t    |      10.47      |
|     h    |       5.26      |     u    |       2.46      |
|     i    |       6.35      |     v    |       0.92      |
|     j    |       0.13      |     w    |       1.54      |
|     k    |       0.42      |     x    |       0.17      |
|     l    |       3.39      |     y    |       1.98      |
|     m    |       2.54      |     z    |       0.08      |

Therefore, the ciphertext should have a nearly-identical probability distribution as the above distribution, with the only difference is that the letters holding the probability are just permutated. 

The same analysis can be conducted on ***bigrams*** and ***trigrams***, so I will simply state these elements in decreasing order of frequencies :
+ Letters : `e, t, a, o, n, r, ....` : which follows from a famous nonsense phrase : `etaoin shrdlu`
+ Bigrams : `th, he, in, en, nt, re, er, an, ti, ...`
+ Trigrams : `the, and, tha, ent, ing, ion, tio, for, ...`

Using these interesting facts, Alice can now perform cryptanalysis on Bob's ciphertext:

## 1.2. The attack

Alice gets Bob's ciphertext by asking him to share her since he is confident that his cipher is totally secure : 

<code>
QV KJAIGP 14 HGVP WUNR FJKPNK PN HGUCGU, PYGUG QH J AFQGVP IGO GEAYJVBG. GQPYGU UHJ, CJUQNDH POKGH NW SYIG (SQWWQG-YGFFRJVV IGO GEAYJVBG), NU KHI (KUG-HYJUGS IGO) JFBNUQPYRH AJV TG DHGS WNU IGO GEAYJVBG. QW UHJ QH DHGS, PYGV PYG AFQGVP BGVGUJPGH J UJVSNR CJFDG AJFFGS PYG KUGRJHPGU HGAUGP, GVAUOKPH QP DHQVB PYG KDTFQA IGO WUNR PYG AGUPQWQAJPG HGVP TO PYG HGUCGU, JVS HGVSH PYQH PN PYG HGUCGU. TNPY FJKPNK JVS HGUCGU VNX YJCG PYG RGJVH PN AJFADFJPG PYG RJHPGU HGAUGP JVS PYGUGWNUG PYG HYJUGS HGHHQNV IGO, TO ANRTQVQVB PYG KUGRJHPGU HGAUGP XQPY UJVSNR CJFDGH. QW SQWWQG-YGFFRJV QH DHGS JH PYG IGO GEAYJVBG JFBNUQPYR, PYG AFQGVP JVS HGUCGU HGVS GJAY NPYGU PYGQU SQWWQG-YGFFRJVV KDTFQA CJFDGH, JFFNXQVB PYG HJRG KUGRJHPGU HGAUGP PN TG AJFADFJPGS TO TNPY HQSGH. PFH 1.3 ANRKFGPGFO UGRNCGS PYG AFQGVP IGO GEAYJVBG HPGK. QP QH VN FNVBGU VGGSGS PYJVIH PN PYG RNUG FQRQPGS HGP NW HDKKNUPGS JFBNUQPYRH. NVFO GKYGRGUJF SQWWQG YGFFRJV IGO GEAYJVBGH JUG HDKKNUPGS (KFDH PYG RDAY FGHH ANRRNVFO DHGS KHI). PYG AFQGVP VNX BDGHHGH XYQAY AQKYGU HDQPG PYG HGUCGU XQFF JAAGKP, JVS HGVSH PYG SQWWQG-YGFFRJV KJUJRGPGUH QV PYG AFQGVP YGFFN. DFPQRJPGFO PYQH HJCGH J WDFF VGPXNUI UNDVSPUQK XYGV GHPJTFQHYQVB J ANVVGAPQNV, RJIQVB PFH 1.3 VNPQAGJTFO WJHPGU XYGV TUNXHQVB PYG QVPGUVGP.
</code>

Firstly, we will produce the frequency table for this ciphertext : 

<center>
<img src="./../images/freq.png" width="100%">
</center>

So, `G` has the highest frequency above all. We can be confident that this is from the letter `e`. Moreover, the only letter in English that forms a word by itself is `a`. Therefore, `J`, which stands by itself in the ciphertext is highly be `a`. Let's try to substitute :

<code>
QV KaAIeP 14 HeVP WUNR FaKPNK PN HeUCeU, PYeUe QH a AFQeVP IeO eEAYaVBe. eQPYeU UHa, CaUQNDH POKeH NW SYIe (SQWWQe-YeFFRaVV IeO eEAYaVBe), NU KHI (KUe-HYaUeS IeO) aFBNUQPYRH AaV Te DHeS WNU IeO eEAYaVBe. QW UHa QH DHeS, PYeV PYe AFQeVP BeVeUaPeH a UaVSNR CaFDe AaFFeS PYe KUeRaHPeU HeAUeP, eVAUOKPH QP DHQVB PYe KDTFQA IeO WUNR PYe AeUPQWQAaPe HeVP TO PYe HeUCeU, aVS HeVSH PYQH PN PYe HeUCeU. TNPY FaKPNK aVS HeUCeU VNX YaCe PYe ReaVH PN AaFADFaPe PYe RaHPeU HeAUeP aVS PYeUeWNUe PYe HYaUeS HeHHQNV IeO, TO ANRTQVQVB PYe KUeRaHPeU HeAUeP XQPY UaVSNR CaFDeH. QW SQWWQe-YeFFRaV QH DHeS aH PYe IeO eEAYaVBe aFBNUQPYR, PYe AFQeVP aVS HeUCeU HeVS eaAY NPYeU PYeQU SQWWQe-YeFFRaVV KDTFQA CaFDeH, aFFNXQVB PYe HaRe KUeRaHPeU HeAUeP PN Te AaFADFaPeS TO TNPY HQSeH. PFH 1.3 ANRKFePeFO UeRNCeS PYe AFQeVP IeO eEAYaVBe HPeK. QP QH VN FNVBeU VeeSeS PYaVIH PN PYe RNUe FQRQPeS HeP NW HDKKNUPeS aFBNUQPYRH. NVFO eKYeReUaF SQWWQe YeFFRaV IeO eEAYaVBeH aUe HDKKNUPeS (KFDH PYe RDAY FeHH ANRRNVFO DHeS KHI). PYe AFQeVP VNX BDeHHeH XYQAY AQKYeU HDQPe PYe HeUCeU XQFF aAAeKP, aVS HeVSH PYe SQWWQe-YeFFRaV KaUaRePeUH QV PYe AFQeVP YeFFN. DFPQRaPeFO PYQH HaCeH a WDFF VePXNUI UNDVSPUQK XYeV eHPaTFQHYQVB a ANVVeAPQNV, RaIQVB PFH 1.3 VNPQAeaTFO WaHPeU XYeV TUNXHQVB PYe QVPeUVeP.
</code>

There is a trigram `aUe` in the cipher text, this could be from either `are` or `ate`. Judging from the frequency, `U` is more likely to be from `t`. Let's test this assumption :

<code>
QV KaAIeP 14 HeVP WtNR FaKPNK PN HetCet, PYete QH a AFQeVP IeO eEAYaVBe. eQPYet tHa, CatQNDH POKeH NW SYIe (SQWWQe-YeFFRaVV IeO eEAYaVBe), Nt KHI (Kte-HYateS IeO) aFBNtQPYRH AaV Te DHeS WNt IeO eEAYaVBe. QW tHa QH DHeS, PYeV PYe AFQeVP BeVetaPeH a taVSNR CaFDe AaFFeS PYe KteRaHPet HeAteP, eVAtOKPH QP DHQVB PYe KDTFQA IeO WtNR PYe AetPQWQAaPe HeVP TO PYe HetCet, aVS HeVSH PYQH PN PYe HetCet. TNPY FaKPNK aVS HetCet VNX YaCe PYe ReaVH PN AaFADFaPe PYe RaHPet HeAteP aVS PYeteWNte PYe HYateS HeHHQNV IeO, TO ANRTQVQVB PYe KteRaHPet HeAteP XQPY taVSNR CaFDeH. QW SQWWQe-YeFFRaV QH DHeS aH PYe IeO eEAYaVBe aFBNtQPYR, PYe AFQeVP aVS HetCet HeVS eaAY NPYet PYeQt SQWWQe-YeFFRaVV KDTFQA CaFDeH, aFFNXQVB PYe HaRe KteRaHPet HeAteP PN Te AaFADFaPeS TO TNPY HQSeH. PFH 1.3 ANRKFePeFO teRNCeS PYe AFQeVP IeO eEAYaVBe HPeK. QP QH VN FNVBet VeeSeS PYaVIH PN PYe RNte FQRQPeS HeP NW HDKKNtPeS aFBNtQPYRH. NVFO eKYeRetaF SQWWQe YeFFRaV IeO eEAYaVBeH ate HDKKNtPeS (KFDH PYe RDAY FeHH ANRRNVFO DHeS KHI). PYe AFQeVP VNX BDeHHeH XYQAY AQKYet HDQPe PYe HetCet XQFF aAAeKP, aVS HeVSH PYe SQWWQe-YeFFRaV KataRePetH QV PYe AFQeVP YeFFN. DFPQRaPeFO PYQH HaCeH a WDFF VePXNtI tNDVSPtQK XYeV eHPaTFQHYQVB a ANVVeAPQNV, RaIQVB PFH 1.3 VNPQAeaTFO WaHPet XYeV TtNXHQVB PYe QVPetVeP.
</code>

The word `HetCet` after substituing `U` with `t` looks quite wrong, this could be the counterexample of the previous assumption. Let's try substituing `U` with `r` instead :

<code>
QV KaAIeP 14 HeVP WrNR FaKPNK PN HerCer, PYere QH a AFQeVP IeO eEAYaVBe. eQPYer rHa, CarQNDH POKeH NW SYIe (SQWWQe-YeFFRaVV IeO eEAYaVBe), Nr KHI (Kre-HYareS IeO) aFBNrQPYRH AaV Te DHeS WNr IeO eEAYaVBe. QW rHa QH DHeS, PYeV PYe AFQeVP BeVeraPeH a raVSNR CaFDe AaFFeS PYe KreRaHPer HeAreP, eVArOKPH QP DHQVB PYe KDTFQA IeO WrNR PYe AerPQWQAaPe HeVP TO PYe HerCer, aVS HeVSH PYQH PN PYe HerCer. TNPY FaKPNK aVS HerCer VNX YaCe PYe ReaVH PN AaFADFaPe PYe RaHPer HeAreP aVS PYereWNre PYe HYareS HeHHQNV IeO, TO ANRTQVQVB PYe KreRaHPer HeAreP XQPY raVSNR CaFDeH. QW SQWWQe-YeFFRaV QH DHeS aH PYe IeO eEAYaVBe aFBNrQPYR, PYe AFQeVP aVS HerCer HeVS eaAY NPYer PYeQr SQWWQe-YeFFRaVV KDTFQA CaFDeH, aFFNXQVB PYe HaRe KreRaHPer HeAreP PN Te AaFADFaPeS TO TNPY HQSeH. PFH 1.3 ANRKFePeFO reRNCeS PYe AFQeVP IeO eEAYaVBe HPeK. QP QH VN FNVBer VeeSeS PYaVIH PN PYe RNre FQRQPeS HeP NW HDKKNrPeS aFBNrQPYRH. NVFO eKYeReraF SQWWQe YeFFRaV IeO eEAYaVBeH are HDKKNrPeS (KFDH PYe RDAY FeHH ANRRNVFO DHeS KHI). PYe AFQeVP VNX BDeHHeH XYQAY AQKYer HDQPe PYe HerCer XQFF aAAeKP, aVS HeVSH PYe SQWWQe-YeFFRaV KaraRePerH QV PYe AFQeVP YeFFN. DFPQRaPeFO PYQH HaCeH a WDFF VePXNrI rNDVSPrQK XYeV eHPaTFQHYQVB a ANVVeAPQNV, RaIQVB PFH 1.3 VNPQAeaTFO WaHPer XYeV TrNXHQVB PYe QVPerVeP.
</code>

Notice the double-bigram `PN Te`, which can imply that these words are `to be`. Let's substitue these 3 letters : `P -> t`, `N -> o`, `T -> b`

<code>
QV KaAIet 14 HeVt WroR FaKtoK to HerCer, tYere QH a AFQeVt IeO eEAYaVBe. eQtYer rHa, CarQoDH tOKeH oW SYIe (SQWWQe-YeFFRaVV IeO eEAYaVBe), or KHI (Kre-HYareS IeO) aFBorQtYRH AaV be DHeS Wor IeO eEAYaVBe. QW rHa QH DHeS, tYeV tYe AFQeVt BeVerateH a raVSoR CaFDe AaFFeS tYe KreRaHter HeAret, eVArOKtH Qt DHQVB tYe KDbFQA IeO WroR tYe AertQWQAate HeVt bO tYe HerCer, aVS HeVSH tYQH to tYe HerCer. botY FaKtoK aVS HerCer VoX YaCe tYe ReaVH to AaFADFate tYe RaHter HeAret aVS tYereWore tYe HYareS HeHHQoV IeO, bO AoRbQVQVB tYe KreRaHter HeAret XQtY raVSoR CaFDeH. QW SQWWQe-YeFFRaV QH DHeS aH tYe IeO eEAYaVBe aFBorQtYR, tYe AFQeVt aVS HerCer HeVS eaAY otYer tYeQr SQWWQe-YeFFRaVV KDbFQA CaFDeH, aFFoXQVB tYe HaRe KreRaHter HeAret to be AaFADFateS bO botY HQSeH. tFH 1.3 AoRKFeteFO reRoCeS tYe AFQeVt IeO eEAYaVBe HteK. Qt QH Vo FoVBer VeeSeS tYaVIH to tYe Rore FQRQteS Het oW HDKKorteS aFBorQtYRH. oVFO eKYeReraF SQWWQe YeFFRaV IeO eEAYaVBeH are HDKKorteS (KFDH tYe RDAY FeHH AoRRoVFO DHeS KHI). tYe AFQeVt VoX BDeHHeH XYQAY AQKYer HDQte tYe HerCer XQFF aAAeKt, aVS HeVSH tYe SQWWQe-YeFFRaV KaraReterH QV tYe AFQeVt YeFFo. DFtQRateFO tYQH HaCeH a WDFF VetXorI roDVStrQK XYeV eHtabFQHYQVB a AoVVeAtQoV, RaIQVB tFH 1.3 VotQAeabFO WaHter XYeV broXHQVB tYe QVterVet.
</code>

This `tYe` should be `the` :

<code>
QV KaAIet 14 HeVt WroR FaKtoK to HerCer, there QH a AFQeVt IeO eEAhaVBe. eQther rHa, CarQoDH tOKeH oW ShIe (SQWWQe-heFFRaVV IeO eEAhaVBe), or KHI (Kre-HhareS IeO) aFBorQthRH AaV be DHeS Wor IeO eEAhaVBe. QW rHa QH DHeS, theV the AFQeVt BeVerateH a raVSoR CaFDe AaFFeS the KreRaHter HeAret, eVArOKtH Qt DHQVB the KDbFQA IeO WroR the AertQWQAate HeVt bO the HerCer, aVS HeVSH thQH to the HerCer. both FaKtoK aVS HerCer VoX haCe the ReaVH to AaFADFate the RaHter HeAret aVS thereWore the HhareS HeHHQoV IeO, bO AoRbQVQVB the KreRaHter HeAret XQth raVSoR CaFDeH. QW SQWWQe-heFFRaV QH DHeS aH the IeO eEAhaVBe aFBorQthR, the AFQeVt aVS HerCer HeVS eaAh other theQr SQWWQe-heFFRaVV KDbFQA CaFDeH, aFFoXQVB the HaRe KreRaHter HeAret to be AaFADFateS bO both HQSeH. tFH 1.3 AoRKFeteFO reRoCeS the AFQeVt IeO eEAhaVBe HteK. Qt QH Vo FoVBer VeeSeS thaVIH to the Rore FQRQteS Het oW HDKKorteS aFBorQthRH. oVFO eKheReraF SQWWQe heFFRaV IeO eEAhaVBeH are HDKKorteS (KFDH the RDAh FeHH AoRRoVFO DHeS KHI). the AFQeVt VoX BDeHHeH XhQAh AQKher HDQte the HerCer XQFF aAAeKt, aVS HeVSH the SQWWQe-heFFRaV KaraReterH QV the AFQeVt heFFo. DFtQRateFO thQH HaCeH a WDFF VetXorI roDVStrQK XheV eHtabFQHhQVB a AoVVeAtQoV, RaIQVB tFH 1.3 VotQAeabFO WaHter XheV broXHQVB the QVterVet.
</code>

From here : `there QH a`, `QH` must be `is` :

<code>
iV KaAIet 14 seVt WroR FaKtoK to serCer, there is a AFieVt IeO eEAhaVBe. either rsa, CarioDs tOKes oW ShIe (SiWWie-heFFRaVV IeO eEAhaVBe), or KsI (Kre-shareS IeO) aFBorithRs AaV be DseS Wor IeO eEAhaVBe. iW rsa is DseS, theV the AFieVt BeVerates a raVSoR CaFDe AaFFeS the KreRaster seAret, eVArOKts it DsiVB the KDbFiA IeO WroR the AertiWiAate seVt bO the serCer, aVS seVSs this to the serCer. both FaKtoK aVS serCer VoX haCe the ReaVs to AaFADFate the Raster seAret aVS thereWore the shareS sessioV IeO, bO AoRbiViVB the KreRaster seAret Xith raVSoR CaFDes. iW SiWWie-heFFRaV is DseS as the IeO eEAhaVBe aFBorithR, the AFieVt aVS serCer seVS eaAh other their SiWWie-heFFRaVV KDbFiA CaFDes, aFFoXiVB the saRe KreRaster seAret to be AaFADFateS bO both siSes. tFs 1.3 AoRKFeteFO reRoCeS the AFieVt IeO eEAhaVBe steK. it is Vo FoVBer VeeSeS thaVIs to the Rore FiRiteS set oW sDKKorteS aFBorithRs. oVFO eKheReraF SiWWie heFFRaV IeO eEAhaVBes are sDKKorteS (KFDs the RDAh Fess AoRRoVFO DseS KsI). the AFieVt VoX BDesses XhiAh AiKher sDite the serCer XiFF aAAeKt, aVS seVSs the SiWWie-heFFRaV KaraReters iV the AFieVt heFFo. DFtiRateFO this saCes a WDFF VetXorI roDVStriK XheV estabFishiVB a AoVVeAtioV, RaIiVB tFs 1.3 VotiAeabFO Waster XheV broXsiVB the iVterVet.
</code>

This text is really almost broken. We can simply using some common sense in English to find out.
+ `seAret` can be `secret` `(A -> c)`
+ `bO` can be `be` or `by`, but since `e` is present, we can try `y` `(O -> y)`
+ `iV` can be `it`, `if` or `in`. Since the ciphertext has `iV the`, we can be confident that `in` is correct. `(V -> n)`

<code>
in KacIet 14 sent WroR FaKtoK to serCer, there is a cFient Iey eEchanBe. either rsa, CarioDs tyKes oW ShIe (SiWWie-heFFRann Iey eEchanBe), or KsI (Kre-shareS Iey) aFBorithRs can be DseS Wor Iey eEchanBe. iW rsa is DseS, then the cFient Benerates a ranSoR CaFDe caFFeS the KreRaster secret, encryKts it DsinB the KDbFic Iey WroR the certiWicate sent by the serCer, anS senSs this to the serCer. both FaKtoK anS serCer noX haCe the Reans to caFcDFate the Raster secret anS thereWore the shareS session Iey, by coRbininB the KreRaster secret Xith ranSoR CaFDes. iW SiWWie-heFFRan is DseS as the Iey eEchanBe aFBorithR, the cFient anS serCer senS each other their SiWWie-heFFRann KDbFic CaFDes, aFFoXinB the saRe KreRaster secret to be caFcDFateS by both siSes. tFs 1.3 coRKFeteFy reRoCeS the cFient Iey eEchanBe steK. it is no FonBer neeSeS thanIs to the Rore FiRiteS set oW sDKKorteS aFBorithRs. onFy eKheReraF SiWWie heFFRan Iey eEchanBes are sDKKorteS (KFDs the RDch Fess coRRonFy DseS KsI). the cFient noX BDesses Xhich ciKher sDite the serCer XiFF acceKt, anS senSs the SiWWie-heFFRan KaraReters in the cFient heFFo. DFtiRateFy this saCes a WDFF netXorI roDnStriK Xhen estabFishinB a connection, RaIinB tFs 1.3 noticeabFy Waster Xhen broXsinB the internet.
</code>

Naturally :
+ `serCer` -> `C -> v`
+ `onFy` -> `F -> l`
+ `anS` -> `S -> d`
+ `Xhen` -> `X -> w` (notice that we already substitute `t`, so only `when` is valid)

Let's see :

<code>
in KacIet 14 sent WroR laKtoK to server, there is a client Iey eEchanBe. either rsa, varioDs tyKes oW dhIe (diWWie-hellRann Iey eEchanBe), or KsI (Kre-shared Iey) alBorithRs can be Dsed Wor Iey eEchanBe. iW rsa is Dsed, then the client Benerates a randoR valDe called the KreRaster secret, encryKts it DsinB the KDblic Iey WroR the certiWicate sent by the server, and sends this to the server. both laKtoK and server now have the Reans to calcDlate the Raster secret and thereWore the shared session Iey, by coRbininB the KreRaster secret with randoR valDes. iW diWWie-hellRan is Dsed as the Iey eEchanBe alBorithR, the client and server send each other their diWWie-hellRann KDblic valDes, allowinB the saRe KreRaster secret to be calcDlated by both sides. tls 1.3 coRKletely reRoved the client Iey eEchanBe steK. it is no lonBer needed thanIs to the Rore liRited set oW sDKKorted alBorithRs. only eKheReral diWWie hellRan Iey eEchanBes are sDKKorted (KlDs the RDch less coRRonly Dsed KsI). the client now BDesses which ciKher sDite the server will acceKt, and sends the diWWie-hellRan KaraReters in the client hello. DltiRately this saves a WDll networI roDndtriK when establishinB a connection, RaIinB tls 1.3 noticeably Waster when browsinB the internet.
</code>

Naturally :
+ `certiWicate` -> `W -> f`
+ `acceKt` -> `K -> p`
+ `networI` -> `I -> k`
+ `establishinB` -> `B -> g`
+ `calcDlate` -> `D -> u`

<code>
in packet 14 sent froR laptop to server, there is a client key eEchange. either rsa, various types of dhke (diffie-hellRann key eEchange), or psk (pre-shared key) algorithRs can be used for key eEchange. if rsa is used, then the client generates a randoR value called the preRaster secret, encrypts it using the public key froR the certificate sent by the server, and sends this to the server. both laptop and server now have the Reans to calculate the Raster secret and therefore the shared session key, by coRbining the preRaster secret with randoR values. if diffie-hellRan is used as the key eEchange algorithR, the client and server send each other their diffie-hellRann public values, allowing the saRe preRaster secret to be calculated by both sides. tls 1.3 coRpletely reRoved the client key eEchange step. it is no longer needed thanks to the Rore liRited set of supported algorithRs. only epheReral diffie hellRan key eEchanges are supported (plus the Ruch less coRRonly used psk). the client now guesses which cipher suite the server will accept, and sends the diffie-hellRan paraReters in the client hello. ultiRately this saves a full network roundtrip when establishing a connection, Raking tls 1.3 noticeably faster when browsing the internet.
</code>

Naturally :
+ `froR` -> `R -> m`
+ `eEchanges` -> `E -> x`

We can finally reveal Bob's plaintext :

<code>
in packet 14 sent from laptop to server, there is a client key exchange. either rsa, various types of dhke (diffie-hellmann key exchange), or psk (pre-shared key) algorithms can be used for key exchange. if rsa is used, then the client generates a random value called the premaster secret, encrypts it using the public key from the certificate sent by the server, and sends this to the server. both laptop and server now have the means to calculate the master secret and therefore the shared session key, by combining the premaster secret with random values. if diffie-hellman is used as the key exchange algorithm, the client and server send each other their diffie-hellmann public values, allowing the same premaster secret to be calculated by both sides. tls 1.3 completely removed the client key exchange step. it is no longer needed thanks to the more limited set of supported algorithms. only ephemeral diffie hellman key exchanges are supported (plus the much less commonly used psk). the client now guesses which cipher suite the server will accept, and sends the diffie-hellman parameters in the client hello. ultimately this saves a full network roundtrip when establishing a connection, making tls 1.3 noticeably faster when browsing the internet.
</code>

We can now represent the substituion table (key) that Bob used to encrypt his plaintexts :

| a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| J | T | A | S | G | W | B | Y | Q | ? | I | F | R | V | N | K | ? | U | H | P | D | C | X | E | O | ? |

Notice that the order of the letters we found is : `e a r t o b h i s ...` , which can imply the fact that the common letters, or the vowels are likely the one to be found first.

**Remarks** :
+ We only use common letters, phrases in English to reveal the whole plaintext.
+ In the middle of the analysis, if you learnt cryptography to a certain extent, you can realize immediately that `SiWWie-heFFRaVV` is from `diffie-hellman` :), which saves a lot of time.
+ We did not recover the whole substitution table, this is because the plaintext does not contain all letters in the alphabet. The more plaintext we get from Bob like this, the more we can fill in the rest of the table.
+ This cryptanalysis is only useful when the plaintext is long (long enough for the frequency table to hold true/almost true). 

## 2. Divisibility and greatest common divisors

This section covers some of the most basic definitions in Number theory : divisibility and GCDs. Anyone that have the background of number theory can safely skip this section

<ins> **Divisibility** </ins>

> Let \\(a\\) and \\(b\\) be integers with \\(b \neq 0\\). We say that \\(b\\) ***divides*** \\(a\\), or \\(a\\) ***is divisible by*** \\(b\\) if there is an integer \\(c\\) such that
> \\(a = bc\\)

We notate that \\(b | a\\) to indicate that \\(b\\) divides \\(a\\), \\(b \nmid a\\) otherwise

Note that :
+ All integers is divisible by \\(1\\)
+ Integers that is divisible by \\(2\\) are even integers, otherwise, they are odd integers.

I will state some interesting properties of divisibility without proving it since that will be the exercise later on this chapter :

+ If \\(a | b\\) and \\(b | c\\), then \\(a | c\\)
+ If \\(a | b\\) and \\(b | a\\), then \\(a = \pm b\\)
+ If \\(a | b\\) and \\(a | c\\), then \\(a | (b + c)\\) and \\(a | (b - c)\\)

Let us move on to ***greatest common divisors***

A ***common divisor*** of two integers \\(a\\) and \\(b\\) is a positive integer \\(d\\) that divides both of them. The ***greatest common divisor*** of \\(a\\) and \\(b\\) is the largest positive integer \\(d\\) such that \\(d | a\\) and \\(d | b\\). 

Notation of greatest common divisor : \\(gcd(a, b)\\).

Let \\(a\\) and \\(b\\) be positive integers. Then \\(a\\) divided by \\(b\\) has ***quotient*** \\(q\\) and ***remainder*** \\(r\\), mean that : $$a = qb + r, \;with\;\; 0 \leq r \lt b$$



## 3. Modular arithmetics

## 4. Prime numbers, unique factorization, and finite fields

## 5. Powers and primitive roots in finite fields

## 6. Cryptography before the computer age

## 7. Symmetric and asymmetric ciphers



