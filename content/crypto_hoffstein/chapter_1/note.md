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

Im quite lazy to sort this...

So, `G` has the highest frequency above all. We can be confident that this is from the letter `e`. Moreover, the only letter in English that forms a word by itself is `a`. Therefore, `J`, which stands by itself in the ciphertext is highly be `a`. Let's try to substitute :

<code>
QV KaAIeP 14 HeVP WUNR FaKPNK PN HeUCeU, PYeUe QH a AFQeVP IeO eEAYaVBe. eQPYeU UHa, CaUQNDH POKeH NW SYIe (SQWWQe-YeFFRaVV IeO eEAYaVBe), NU KHI (KUe-HYaUeS IeO) aFBNUQPYRH AaV Te DHeS WNU IeO eEAYaVBe. QW UHa QH DHeS, PYeV PYe AFQeVP BeVeUaPeH a UaVSNR CaFDe AaFFeS PYe KUeRaHPeU HeAUeP, eVAUOKPH QP DHQVB PYe KDTFQA IeO WUNR PYe AeUPQWQAaPe HeVP TO PYe HeUCeU, aVS HeVSH PYQH PN PYe HeUCeU. TNPY FaKPNK aVS HeUCeU VNX YaCe PYe ReaVH PN AaFADFaPe PYe RaHPeU HeAUeP aVS PYeUeWNUe PYe HYaUeS HeHHQNV IeO, TO ANRTQVQVB PYe KUeRaHPeU HeAUeP XQPY UaVSNR CaFDeH. QW SQWWQe-YeFFRaV QH DHeS aH PYe IeO eEAYaVBe aFBNUQPYR, PYe AFQeVP aVS HeUCeU HeVS eaAY NPYeU PYeQU SQWWQe-YeFFRaVV KDTFQA CaFDeH, aFFNXQVB PYe HaRe KUeRaHPeU HeAUeP PN Te AaFADFaPeS TO TNPY HQSeH. PFH 1.3 ANRKFePeFO UeRNCeS PYe AFQeVP IeO eEAYaVBe HPeK. QP QH VN FNVBeU VeeSeS PYaVIH PN PYe RNUe FQRQPeS HeP NW HDKKNUPeS aFBNUQPYRH. NVFO eKYeReUaF SQWWQe YeFFRaV IeO eEAYaVBeH aUe HDKKNUPeS (KFDH PYe RDAY FeHH ANRRNVFO DHeS KHI). PYe AFQeVP VNX BDeHHeH XYQAY AQKYeU HDQPe PYe HeUCeU XQFF aAAeKP, aVS HeVSH PYe SQWWQe-YeFFRaV KaUaRePeUH QV PYe AFQeVP YeFFN. DFPQRaPeFO PYQH HaCeH a WDFF VePXNUI UNDVSPUQK XYeV eHPaTFQHYQVB a ANVVeAPQNV, RaIQVB PFH 1.3 VNPQAeaTFO WaHPeU XYeV TUNXHQVB PYe QVPeUVeP.
</code>



## 2. Divisibility and greatest common divisors

## 3. Modular arithmetics

## 4. Prime numbers, unique factorization, and finite fields

## 5. Powers and primitive roots in finite fields

## 6. Cryptography before the computer age

## 7. Symmetric and asymmetric ciphers



