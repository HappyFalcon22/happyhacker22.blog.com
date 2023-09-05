+++
title = "Chapter 1 : Exercise"
date = 2023-08-31T12:43:10+07:00
author = "HappyHacker22"
draft = false
katex = true
color = "blue"
description = "Solutions for exercise Chapter 1 : `An Introduction to Cryptography`"
tag = ["cryptography", "hoffstein", "exercise"]
enableEmoji = true
+++

I suggest reading the Chapter 1 material or you can have a look of the note [here](./../note/)

Let's begin

## 1.1 
---
a. Encrypt the following plaintext using a rotation of 11 clockwise : 
<span> `“A page of history is worth a volume of logic.”` </span>
   
b. Decrypt the following message, which was encrypted with a rotation of 7 clockwise :
<span> `AOLYLHYLUVZLJYLAZILAALYAOHUAOLZLJYLALZAOHALCLYFIVKFNBLZZLZ` </span>
   
c. Decrypt the following message, which was encrypted by rotating 1 clockwise for the first letter, then 2 clockwise for the second letter, etc
<span> `XJHRFTNZHMZGAHIUETXZJNBWNUTRHEPOMDNBJMAUGORFAOIZOCC` </span>  

---

The original purpose of this exeercise is to build a cipher wheel and use it to encrypt-decrypt texts. But I decided to use programming language to solve, which is more practical.

Firstly, let's build the function to encrypt the plaintext using the shift cipher :

```Python
ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
def shift_encrypt(m: str, key: int):
    c = ""
    for x in m:
        if x not in ALPHABET and x not in ALPHABET.lower():
            c += x
        elif x in ALPHABET.lower():
            c += ALPHABET[(ALPHABET.index(x.upper()) + key) % len(ALPHABET)].lower()
        else:
            c += ALPHABET[(ALPHABET.index(x) + key) % len(ALPHABET)]
        print(c)
    return c
```

Here, the `ALPHABET` is not necessary to have only letters, it can contains any character. In this textbook's context, we will work with the standard alphabet (26 English letters). Additionally, I modified the code so it can encrypt the plaintext without changing the non-letter char in the plaintext or changing the case of the plaintext's letter.

So when we encrypt the plaintext : ```“A page of history is worth a volume of logic.”```, we get the ciphertext : ```“H whnl vm opzavyf pz dvyao h cvsbtl vm svnpj.”```


