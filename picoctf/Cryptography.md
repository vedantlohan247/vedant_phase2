# 1. miniRSA

> Let's decrypt this: `ciphertext`? Something seems a bit small.

## Solution:
We need to decypher the text to find the flag.  
```bash
lallu@VedantLohan:~/picoctf/rsa$ file ciphertext
ciphertext: ASCII text, with very long lines (620)
lallu@VedantLohan:~/picoctf/rsa$ cat ciphertext

N: 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673
e: 3

ciphertext (c): 2205316413931134031074603746928247799030155221252519872650073010782049179856976080512716237308882294226369300412719995904064931819531456392957957122459640736424089744772221933500860936331459280832211445548332429338572369823704784625368933
```
This is a RSA encryption, we go to an online RSA decoder and plug in the values for N,e and c to obtain the flag.

## Flag:

```
picoCTF{n33d_a_lArg3r_e_ccaa7776}
```

## Concept
- What RSA does is that it chooses two large primes and multiplies them, which is the easy part but to get back to the primes by factoring the product is hard. This is the basis for security.  
- The public key is (N,e) where N=p*q and e is the public exponent.  
- Encryption: c = m^e mod N
- Decryption: m = c^d mod N

## Resources:
[RSA decoder](https://www.dcode.fr/rsa-cipher)

***

# 2. RSA oracle

> Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the `message` (ciphertext) and the `password`that was used to encrypt the message.
Additional details will be available after launching your challenge instance.
After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here `nc titan.picoctf.net 53026`. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:
```bash
lallu@VedantLohan:~/picoctf/oracle$ file password.enc secret.enc
password.enc: ASCII text, with no line terminators
secret.enc:   openssl enc'd data with salted password

lallu@VedantLohan:~/picoctf/oracle$ cat password.enc
4228273471152570993857755209040611143227336245190875847649142807501848960847851973658239485570030833999780269457000091948785164374915942471027917017922546 lallu@VedantLohan:~/picoctf/oracle$
```  
Looking at the files provided, we need to get the password by using `oracle` and use that password to figure out the secret message.  
```bash
lallu@VedantLohan:~/picoctf/oracle$ nc titan.picoctf.net 53026
*****************************************
****************THE ORACLE***************
*****************************************
what should we do for you?
E --> encrypt D --> decrypt.
E
enter text to encrypt (encoded length must be less than keysize): adasdasd
adasdasd

encoded cleartext as Hex m: 6164617364617364

ciphertext (m ^ e mod n) 3954594363215873928249236003542731143545230093435658554375481270216959058158792511273805372436812914919647524970313276977402970601972740786919916994274432

what should we do for you?
E --> encrypt D --> decrypt.
^C
lallu@VedantLohan:~/picoctf/oracle$ nc titan.picoctf.net 53026
*****************************************
****************THE ORACLE***************
*****************************************
what should we do for you?
E --> encrypt D --> decrypt.
D
Enter text to decrypt: 4228273471152570993857755209040611143227336245190875847649142807501848960847851973658239485570030833999780269457000091948785164374915942471027917017922546
Lol, good try, can't decrypt that for you. Be creative and good luck

what should we do for you?
E --> encrypt D --> decrypt.
^C
```  
The `oracle` won't decrypt the password for us. We see that it uses a RSA encryption.  
Because RSA is multiplicative if we multiply the password ciphertext c by some small number x then ask the oracle to decrypt that product, it will return `m*x mod n`. We divide by x to get the password.  
Let that number be 2. Now  

We first find the encryption of `2`.  
```bash
lallu@VedantLohan:~/picoctf/oracle$ printf $'E\n\x02\n' | nc titan.picoctf.net 53317
*****************************************
****************THE ORACLE***************
*****************************************
what should we do for you?
E --> encrypt D --> decrypt.
enter text to encrypt (encoded length must be less than keysize):

encoded cleartext as Hex m: 2

ciphertext (m ^ e mod n) 5067313465613043651275429665315895824157755779222372979446076012356324498190828210335763979330272318657269048435311897896433721115606764442199497891219230

what should we do for you?
E --> encrypt D --> decrypt.
^C
```  
In `printf $'E\n\x02\n'` E tells the oracle we want to encrypt and `$'  '` lets you include escaped chars.  
Now we multiply it by the password enc and decrypt it.  
```bash
lallu@VedantLohan:~/picoctf/oracle$ nc titan.picoctf.net 53317
*****************************************
****************THE ORACLE***************
*****************************************
what should we do for you?
E --> encrypt D --> decrypt.
D
Enter text to decrypt: 21425987096665828273890242512580269131580523768761853466498617545871657240638146577218265276323529524517050698451769550135389839521530224104010607849286798348771103586544994838545601282890958932324042302491354938164052895386733858467685832923993870336548503670949230846657648281721087738031339376189645759580
decrypted ciphertext as hex (c ^ d mod n): c8c2607272
decrypted ciphertext: ÈÂ`rr

what should we do for you?
E --> encrypt D --> decrypt.
^C
```  
We convert the given `c8c2607272` hex to dec and divide it by 2 which gives `431127279929`.  
Now we convert it to hex and then to ascii to get the password.  
This gives us `da099`.  
Using this with openssl gives us the flag.  
```bash
lallu@VedantLohan:~/picoctf/oracle$ openssl enc -aes-256-cbc -d -in secret.enc -k da099
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
picoCTF{su((3ss_(r@ck1ng_r3@_da099d93}
```

## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_da099d93}
```

## Concepts learnt:
- RSA is multiplicative
- openssl is a tool for cryptographic operations

## Notes:
- Mistake I made was that typing 2 sends ASCII 0x32 (decimal 50). I needed to send raw \x02.

## Resources:
[multiplicative prop of RSA](https://crypto.stackexchange.com/questions/24880/how-multiplicative-property-of-rsa-can-be-exploited)  
[converter](https://www.rapidtables.com/convert/number)  
[multiplier](https://www.dcode.fr/big-numbers-multiplication)  

***
# 3. Custom encryption

> Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here `flag_info` and `code file` might be good to analyze and get the flag.

## Solution:
In the file `enc_file` we have  
```bash
a = 95
b = 21
cipher is: [237915, 1850450, 1850450, 158610, 2458455, 2273410, 1744710, 1744710, 1797580, 1110270, 0, 2194105, 555135, 132175, 1797580, 0, 581570, 2273410, 26435, 1638970, 634440, 713745, 158610, 158610, 449395, 158610, 687310, 1348185, 845920, 1295315, 687310, 185045, 317220, 449395]
```
and `custom_encryption` is a `.py` file which has-  
```py
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```
- `generator` computes g^x (mod p)  
- `encrypt` takes a string and a key.  
    - for every char it multiplies it's unicode by `key*311` and appends the product to the list `cipher`  
    - so each char becomes a large number  
- `dynamic_xor_encrypt` does 2 things-  
    - reverses the encryption  
    - It XORs each char of reversed message with the repeating key "trudeau".  
- `test` sets `p=97` and `g=31`, computes `u` and `v`. Performs the cipher and prints the list  

Python script to reverse it-  
```py
a, b, g, p = 95, 21, 31, 97
key = "trudeau"

cipher = [
    237915, 1850450, 1850450, 158610, 2458455, 2273410, 1744710, 1744710,
    1797580, 1110270, 0, 2194105, 555135, 132175, 1797580, 0, 581570,
    2273410, 26435, 1638970, 634440, 713745, 158610, 158610, 449395,
    158610, 687310, 1348185, 845920, 1295315, 687310, 185045, 317220, 449395
]

# calculate shared factor
factor = pow(pow(g, b, p), a, p) * 311

# undo multiplication
semi = "".join(
    chr(round(c / factor)) if c else chr(0)
    for c in cipher
)

# undo XOR, then reverse it
decoded = "".join(
    chr(ord(ch) ^ ord(key[i % len(key)]))
    for i, ch in enumerate(semi)
)

# Print
print(decoded[::-1].strip())

```  
1) We calculate the shared_key and find the multiplier used in enc  
2) we undo the multiplication
3) undo the XOR
4) reverse the text
5) print the flag

## Flag:

```
picoCTF{custom_d2cr0pt6d_66778b34}
```

## Concepts learnt:
- Character encoding in py using ord() and chr()

## Notes:
- I made the mistake that I did XOR before dividing numbers.

## Resources:
[modular exp](https://www.dcode.fr/modular-exponentiation)



***
