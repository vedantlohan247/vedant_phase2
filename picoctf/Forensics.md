# 1. Trivial Flag Transfer Protocol

> Figure out how they moved the flag.

## Solution:
In this challenge we have been given a `.pcapng` file, from which we have to figure out the flag.
1) First we open up this file in `Wireshark` to inspect it.
2) We get a hint from the challenge name that we must look at the `TFTP` protocols. So I extracted them.
3) The extracted files were- `instructions.txt`, `picture1.bmp`, `picture2.bmp`, `picture3.bmp`, `plan` and `program.deb`
4) We first open up `instructions.txt`, it looked like a cypher so I just tried the `ROT13` on it and it said `TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN`
This tells us that TFTP doesn't encrypt the traffic flowing so they disguised the flag in some other way!
5) Doing the same for `plan` give us- `IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS`, It means they used a program(`program.deb`) to hide the flag with some command `DUEDILIGENCE`.
6) Now we inspect the `program.deb`
```bash
lallu@VedantLohan:~/picoctf/string$ file program.deb
program.deb: Debian binary package (format 2.0), with control.tar.gz, data compression xz
```
and extract the archived files using `ar x program.deb`, now we get files `control.tar.gz`,`data.tar.xz` and `debian-binary`.  
Now we extract `data.tar.xz` using  
```bash
lallu@VedantLohan:~/picoctf/string$ tar -xvf data.tar.xz -C deb_extract
./
./usr/
./usr/share/
./usr/share/doc/
./usr/share/doc/steghide/
./usr/share/doc/steghide/ABOUT-NLS.gz
./usr/share/doc/steghide/LEAME.gz
./usr/share/doc/steghide/README.gz
./usr/share/doc/steghide/changelog.Debian.gz
./usr/share/doc/steghide/changelog.Debian.amd64.gz
./usr/share/doc/steghide/changelog.gz
./usr/share/doc/steghide/copyright
./usr/share/doc/steghide/TODO
./usr/share/doc/steghide/HISTORY
./usr/share/doc/steghide/CREDITS
./usr/share/doc/steghide/BUGS
./usr/share/man/
./usr/share/man/man1/
./usr/share/man/man1/steghide.1.gz
./usr/share/locale/
./usr/share/locale/ro/
./usr/share/locale/ro/LC_MESSAGES/
./usr/share/locale/ro/LC_MESSAGES/steghide.mo
./usr/share/locale/fr/
./usr/share/locale/fr/LC_MESSAGES/
./usr/share/locale/fr/LC_MESSAGES/steghide.mo
./usr/share/locale/de/
./usr/share/locale/de/LC_MESSAGES/
./usr/share/locale/de/LC_MESSAGES/steghide.mo
./usr/share/locale/es/
./usr/share/locale/es/LC_MESSAGES/
./usr/share/locale/es/LC_MESSAGES/steghide.mo
./usr/bin/
./usr/bin/steghide
```
>We see `steghide`there, which means they used it to hide the flag in the images!  

We try `steghide` with the passphrase `DUEDILGENCE` on all the picture and on the third one we get-  
```bash
lallu@VedantLohan:~/picoctf/string$ steghide extract -sf picture3.bmp
Enter passphrase:
wrote extracted data to "flag.txt".
```
This gives us the flag  
```bash
lallu@VedantLohan:~/picoctf/string$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

- `pcapng`  
It is a file which captures the packets transferred over a network

- `Wireshark`
It is a tool which helps us analyze network traffics

- `ROT13`  
It is a cypher technique, which just shifts each letter to the 13th letter after it.

- `ar` and `tar`  
`ar` helps us manipulate simple archive files.  
`tar` helps us manipulate tarballs, which are also archives.
## Notes:

I also tried `grep -R -i "picoCTF" .` on the extracted files to get the flag.

## Resources:
[ROT13](https://cryptii.com/pipes/rot13-decoder)


***
