# 1. I like Logic

> i like logic and i like files, apparently, they have something in common, what should my next step be.

## Solution:
1) I downloaded the file `i_like_logic-20251028T141703Z-1-001.zip` and unzip it using the `unzip` command  
2) We get the files `i_like_logic`, it contains 2 files `challenge.sal` and `desc.txt`  
3) Unzipping `challenge.sal` gives some `.bin` files and a `meta.json` file.  
4) I search up what a `.sal` file type is and get,  
```
A .sal file is primarily a capture file from Saleae Logic Analyzer software, used to store data from hardware analysis
```  
5) I installed `saleae logic 2` and opened the `.sal` file in it to analyze.  
6) We see signals in the the 3rd channel by zooming in.  
![img1](https://drive.google.com/uc?id=1mprGwtnIu1Q470Ryg7lbvPlvmk4v3hga)  
![img2](https://drive.google.com/uc?id=1avswJZCkOApALCKrOGRyBYi8A3yvXnTK)  
This channel carries the message and the flag.  
7) We add Async Serial (UART) on D3.  
8) Set the data to `ascii` and we see characters forming from the signal.  
![img3](https://drive.google.com/uc?id=1aVEmNAsIJNueBfIn6eJc1KMz9uvS8B2d)  
9) We export this data to a `.txt` file.
10) We see data like this  
```bash
lallu@VedantLohan:~/picoctf/i_like_logic$ head exported.txt
Time [s],Value,Parity Error,Framing Error
0.000019680000000,o,,
0.001061440000000,n,,
0.002103200000000,',,
0.003144800000000,t,,
0.004186560000000, ,,
0.005228160000000,t,,
0.006269760000000,h,,
0.007311360000000,i,,
0.008353120000000,n,,
```  
We use cut command to extract the column, setting delimiter as ',' field 2  
```bash
lallu@VedantLohan:~/picoctf/i_like_logic$ cut -d',' -f2 exported.txt > column.txt
lallu@VedantLohan:~/picoctf/i_like_logic$ less column.txt
```  
```bash

h
e

m
a
k
e
s

a
n
y

c
l
a
i
m
s

o
f
\r
\n
t
h
a
t
:
```  
We see some '\r's int there, so we remove them using,
```bash
lallu@VedantLohan:~/picoctf/i_like_logic$ sed 's/\\r//g' column.txt > cleaned.txt
```  
This removes all the `\r` chars in the file.  
Now we delete the `\n` chars to get not get every character on a new line.  
```bash
lallu@VedantLohan:~/picoctf/i_like_logic$ tr -d '\n' < cleaned.txt > combined.txt
```  
It gives us-  
<details>
<summary><strong>CLICK TO SEE FULL TEXT RESULT</strong></summary>
Valueon't think he makes any claims ofthat kind.  But I do believe he has got something new.""Then for Heaven's sakeCOMMA manCOMMA write it up!""I'm longing toCOMMA but all I know he gave me in confidence and oncondition that I didn't."  I condensed into a few sentences theProfessor's narrative.  "That's how it stands."McArdle looked deeply incredulous."WellCOMMA Mr. MaloneCOMMA" he said at lastCOMMA "about this scientific meetingto-night; there can be no privacy about thatCOMMA anyhow.  I don't supposeany paper will want to report itCOMMA for Waldron has been reported alreadya dozen timesCOMMA and no one is aware that Challenger will speak.  We mayget a scoopCOMMA if we are lucky.  You'll be there in any caseCOMMA so you'lljust give us a pretty full report.  I'll keep space up to midnight."My day was a busy oneCOMMA and I had an early dinner at the Savage Clubwith Tarp HenryCOMMA to whom I gave some account of my adventures.  Helistened with a sceptical smile on his gaunt faceCOMMA and roared withlaughter on hearing that the Professor had convinced me."My dear chapCOMMA things don't happen like that in real life.  Peopledon't stumble upon enormous discoveries and then lose their evidence.Leave that to the novelists.  The fellow is as full of tricks as themonkey-house at the Zoo.  It's all bosh.""But the American poet?""He never existed.""I saw his sketch-book.""Challenger's sketch-book.""You think he drew that animal?""Of course he did.  Who else?""WellCOMMA thenCOMMA the photographs?""There was nothing in the photographs.  By your own admission you onlysaw a bird."FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}"A pterodactyl.""That's what HE says.  He put the pterodactyl into your head.""WellCOMMA thenCOMMA the bones?""First one out of an Irish stew.  Second one vamped up for theoccasion.  If you are clever and know your business you can fake a boneas easily as you can a photograph."I began to feel uneasy.  PerhapsCOMMA after allCOMMA I had been premature in myacquiescence.  Then I had a sudden happy thought."Will you come to the meeting?" I asked.Tarp Henry looked thoughtful."He is not a popular personCOMMA the genial ChallengerCOMMA" said he.  "A lotof people have accounts to settle with him.  I should say he is aboutthe best-hated man in London.  If the medical students turn out therewill be no end of a rag.  I don't want to get into a bear-garden.""You might at least do him the justice to hear him state his own case.""WellCOMMA perhaps it's only fair.  All right.  I'm your man for theevening."When we arrived at the hall we found a much greater concourse than Ihad expected.  A line of electric broughams discharged their littlecargoes of white-bearded professorsCOMMA while the dark stream of humblerpedestriansCOMMA who crowded through the arched door-wayCOMMA showed that theaudience would be popular as well as scientific.  IndeedCOMMA it becameevident to us as soon as we had taken our seats that a youthful andeven boyish spirit was abroad in the gallery and the back portions ofthe hall.  Looking behind meCOMMA I could see rows of faces of the familiarmedical student type.  Apparently the great hospitals had each sentdown their contingent.  The behavior of the audience at present wasgood-humoredCOMMA but mischievous.  Scraps of popular songs were chorusedwith an enthusiasm which was a strange prelude to a scientific lectureCOMMAand there was already a tendency to personal chaff which promised ajovial evening to othersCOMMA however embarrassing it might be to therecipients of these dubious honors.ThusCOMMA when old Doctor MeldrumCOMMA with his well-known curly-brimmedopera-hatCOMMA appeared upon the platformCOMMA there was such a universal queryof "Where DID you get that tile?" that he hurriedly removed itCOMMA andconcealed it furtively under his chair.  When gouty Professor Wadleylimped down to his seat there were general affectionate inquiries fromall parts of the hall as to the exact state of his poor toeCOMMA whichcaused him obvious embarrassment.  The greatest demonstration of allCOMMAhoweverCOMMA was at the entrance of my new acquaintanceCOMMA ProfessorChallengerCOMMA when he passed down to take his place at the extreme end ofthe front row of the platform.  Such a yell of welcome broke forth whenhis black beard first protruded round the corner that I began tosuspect Tarp Henry was right in his surmiseCOMMA and that this assemblagewas there not merely for the sake of the lectureCOMMA but because it hadgot rumored abroad that the famous Professor would take part in theproceedings.There was some sympathetic laughter on his entrance among the frontbenches of well-dressed spectatorsCOMMA as though the demonstration of thestudents in this instance was not unwelcome to them.  That greetingwasCOMMA indeedCOMMA a frightful outburst of soundCOMMA the uproar of the carnivoracage when the step of the bucket-bearing keeper is heard in thedistance.  There was an offensive tone in itCOMMA perhapsCOMMA and yet in themain it struck me as mere riotous outcryCOMMA the noisy reception of onewho amused and interested themCOMMA rather than of one they disliked ordespised.  Challenger smiled with weary and tolerant contemptCOMMA as akindly man would meet the yapping of a litter of puppies.  He satslowly downCOMMA blew out his chestCOMMA passed his hand caressingly down hisbeardCOMMA and looked with drooping eyelids and supercilious eyes at thecrowded hall before him.  The uproar of his advent had not yet diedaway when Professor Ronald MurrayCOMMA the chairmanCOMMA and Mr. WaldronCOMMA thelecturerCOMMA threaded their way to the frontCOMMA and the proceedings began.Professor Murray willCOMMA I am sureCOMMA excuse me if I say that he has thecommon fault of most Englishmen of being inaudible.  Why on earthpeople who have something to say which is worth hearing should not takethe slight trouble to learn how to make it heard is one of the strangemysteries of modern life.  Their methods are as reasonable as to try topour some precious stuff from the spring to the reservoir through anon-conducting pipeCOMMA which could by the least effort be opened.Professor Murray made several profound remarks to his white tie and tothe water-carafe upon the tableCOMMA with a humorousCOMMA twinkling aside tothe silver candlestick upon his right.  Then he sat downCOMMA and Mr.WaldronCOMMA the famous popular lecturerCOMMA rose amid a general murmur ofapplause.  He was a sternCOMMA gaunt manCOMMA with a harsh voiceCOMMA and anaggressive mannerCOMMA but he had the merit of knowing how to assimilatethe ideas of other menCOMMA and to pass them on in a way which wasintelligible and even interesting to the lay publicCOMMA with a happy knackof being funny about the most unlikely objectsCOMMA so that the precessionof the Equinox or the formation of a vertebrate became a highlyhumorous process as treated by him.It was a bird's-eye view of creationCOMMA as interpreted by scienceCOMMA whichCOMMAin language always clear and sometimes picturesqueCOMMA he unfolded beforeus.  He told us of t
</details>  

Now we just have to grep for the flag-  
```bash
lallu@VedantLohan:~/picoctf/i_like_logic$ grep -o '{[^}]*}' combined.txt  
{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
lallu@VedantLohan:~/picoctf/i_like_logic$ grep {b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925} combined.txt
```  
and we see the `b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925` marked in red to spot the flag that was  
`FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}`  

## Flag:

```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

## Concepts learnt:

- A `.sal` hold data from hardware analysis.  
- An async UART serial signal is a method of data transmission  
- learned about `sed` command.  
- `[^}]*` means any char except `}`, where `^` is for `not`.

## Notes:

- I also tried other other Analyzers like SPI, I2C but they didn't work.  
- I was making mistakes while grepping for the text with `{}`.
## Resources:

[saleae](https://www.youtube.com/watch?v=Ak9R4yxQPhs)
[saleae](https://www.youtube.com/watch?v=i2BmFl01CTE)

***

# 2. Bare Metal Alchemist

> my friend recommended me this anime but i think i've heard a wrong name.

## Solution:
The challenge came with the file `firmware.elf`  
```bash
lallu@VedantLohan:~/picoctf/bare$ file firmware.elf
firmware.elf: ELF 32-bit LSB executable, Atmel AVR 8-bit, version 1 (SYSV), statically linked, with debug_info, not stripped
```  
This program is supposed to be run on a microcontroller `Atmel AVR 8-bit`.  
We disassemble it using `avr objdump`  
This shows the code starting from the main() function and 100 lines after it.  

```bash
lallu@VedantLohan:~/picoctf/bare$ avr-objdump -D firmware.elf | grep -A100 "<main>"
  c8:   0e 94 bb 00     call    0x176   ; 0x176 <main>
  cc:   0c 94 70 01     jmp     0x2e0   ; 0x2e0 <_exit>

000000d0 <__bad_interrupt>:
  d0:   0c 94 00 00     jmp     0       ; 0x0 <__vectors>

000000d4 <_ZL2z1v>:
  d4:   8b b1           in      r24, 0x0b       ; 11
  d6:   87 70           andi    r24, 0x07       ; 7
  d8:   8b b9           out     0x0b, r24       ; 11
  da:   85 b1           in      r24, 0x05       ; 5
  dc:   8c 7f           andi    r24, 0xFC       ; 252
  de:   85 b9           out     0x05, r24       ; 5
  e0:   08 95           ret

000000e2 <__vector_16>:
  e2:   1f 92           push    r1
  e4:   0f 92           push    r0
  e6:   0f b6           in      r0, 0x3f        ; 63
  e8:   0f 92           push    r0
  ea:   11 24           eor     r1, r1
  ec:   2f 93           push    r18
  ee:   3f 93           push    r19
  f0:   8f 93           push    r24
  f2:   9f 93           push    r25
  f4:   af 93           push    r26
  f6:   bf 93           push    r27
  f8:   80 91 53 01     lds     r24, 0x0153     ; 0x800153 <timer0_millis>
  fc:   90 91 54 01     lds     r25, 0x0154     ; 0x800154 <timer0_millis+0x1>
 100:   a0 91 55 01     lds     r26, 0x0155     ; 0x800155 <timer0_millis+0x2>
 104:   b0 91 56 01     lds     r27, 0x0156     ; 0x800156 <timer0_millis+0x3>
 108:   30 91 52 01     lds     r19, 0x0152     ; 0x800152 <timer0_fract>
 10c:   23 e0           ldi     r18, 0x03       ; 3
 10e:   23 0f           add     r18, r19
 110:   2d 37           cpi     r18, 0x7D       ; 125
 112:   58 f5           brcc    .+86            ; 0x16a <__vector_16+0x88>
 114:   01 96           adiw    r24, 0x01       ; 1
 116:   a1 1d           adc     r26, r1
 118:   b1 1d           adc     r27, r1
 11a:   20 93 52 01     sts     0x0152, r18     ; 0x800152 <timer0_fract>
 11e:   80 93 53 01     sts     0x0153, r24     ; 0x800153 <timer0_millis>
 122:   90 93 54 01     sts     0x0154, r25     ; 0x800154 <timer0_millis+0x1>
 126:   a0 93 55 01     sts     0x0155, r26     ; 0x800155 <timer0_millis+0x2>
 12a:   b0 93 56 01     sts     0x0156, r27     ; 0x800156 <timer0_millis+0x3>
 12e:   80 91 4e 01     lds     r24, 0x014E     ; 0x80014e <__data_end>
 132:   90 91 4f 01     lds     r25, 0x014F     ; 0x80014f <__data_end+0x1>
 136:   a0 91 50 01     lds     r26, 0x0150     ; 0x800150 <__data_end+0x2>
 13a:   b0 91 51 01     lds     r27, 0x0151     ; 0x800151 <__data_end+0x3>
 13e:   01 96           adiw    r24, 0x01       ; 1
 140:   a1 1d           adc     r26, r1
 142:   b1 1d           adc     r27, r1
 144:   80 93 4e 01     sts     0x014E, r24     ; 0x80014e <__data_end>
 148:   90 93 4f 01     sts     0x014F, r25     ; 0x80014f <__data_end+0x1>
 14c:   a0 93 50 01     sts     0x0150, r26     ; 0x800150 <__data_end+0x2>
 150:   b0 93 51 01     sts     0x0151, r27     ; 0x800151 <__data_end+0x3>
 154:   bf 91           pop     r27
 156:   af 91           pop     r26
 158:   9f 91           pop     r25
 15a:   8f 91           pop     r24
 15c:   3f 91           pop     r19
 15e:   2f 91           pop     r18
 160:   0f 90           pop     r0
 162:   0f be           out     0x3f, r0        ; 63
 164:   0f 90           pop     r0
 166:   1f 90           pop     r1
 168:   18 95           reti
 16a:   26 e8           ldi     r18, 0x86       ; 134
 16c:   23 0f           add     r18, r19
 16e:   02 96           adiw    r24, 0x02       ; 2
 170:   a1 1d           adc     r26, r1
 172:   b1 1d           adc     r27, r1
 174:   d2 cf           rjmp    .-92            ; 0x11a <__vector_16+0x38>

00000176 <main>:
 176:   cf 93           push    r28
 178:   df 93           push    r29
 17a:   00 d0           rcall   .+0             ; 0x17c <main+0x6>
 17c:   cd b7           in      r28, 0x3d       ; 61
 17e:   de b7           in      r29, 0x3e       ; 62
 180:   78 94           sei
 182:   84 b5           in      r24, 0x24       ; 36
 184:   82 60           ori     r24, 0x02       ; 2
 186:   84 bd           out     0x24, r24       ; 36
 188:   84 b5           in      r24, 0x24       ; 36
 18a:   81 60           ori     r24, 0x01       ; 1
 18c:   84 bd           out     0x24, r24       ; 36
 18e:   85 b5           in      r24, 0x25       ; 37
 190:   82 60           ori     r24, 0x02       ; 2
 192:   85 bd           out     0x25, r24       ; 37
 194:   85 b5           in      r24, 0x25       ; 37
 196:   81 60           ori     r24, 0x01       ; 1
 198:   85 bd           out     0x25, r24       ; 37
 19a:   80 91 6e 00     lds     r24, 0x006E     ; 0x80006e <__TEXT_REGION_LENGTH__+0x7e006e>
 19e:   81 60           ori     r24, 0x01       ; 1
 1a0:   80 93 6e 00     sts     0x006E, r24     ; 0x80006e <__TEXT_REGION_LENGTH__+0x7e006e>
 1a4:   10 92 81 00     sts     0x0081, r1      ; 0x800081 <__TEXT_REGION_LENGTH__+0x7e0081>
 1a8:   80 91 81 00     lds     r24, 0x0081     ; 0x800081 <__TEXT_REGION_LENGTH__+0x7e0081>
 1ac:   82 60           ori     r24, 0x02       ; 2
 1ae:   80 93 81 00     sts     0x0081, r24     ; 0x800081 <__TEXT_REGION_LENGTH__+0x7e0081>
 1b2:   80 91 81 00     lds     r24, 0x0081     ; 0x800081 <__TEXT_REGION_LENGTH__+0x7e0081>
 1b6:   81 60           ori     r24, 0x01       ; 1
 1b8:   80 93 81 00     sts     0x0081, r24     ; 0x800081 <__TEXT_REGION_LENGTH__+0x7e0081>
 1bc:   80 91 80 00     lds     r24, 0x0080     ; 0x800080 <__TEXT_REGION_LENGTH__+0x7e0080>
 1c0:   81 60           ori     r24, 0x01       ; 1
 1c2:   80 93 80 00     sts     0x0080, r24     ; 0x800080 <__TEXT_REGION_LENGTH__+0x7e0080>
 1c6:   80 91 b1 00     lds     r24, 0x00B1     ; 0x8000b1 <__TEXT_REGION_LENGTH__+0x7e00b1>
 1ca:   84 60           ori     r24, 0x04       ; 4
 1cc:   80 93 b1 00     sts     0x00B1, r24     ; 0x8000b1 <__TEXT_REGION_LENGTH__+0x7e00b1>
 1d0:   80 91 b0 00     lds     r24, 0x00B0     ; 0x8000b0 <__TEXT_REGION_LENGTH__+0x7e00b0>
 1d4:   81 60           ori     r24, 0x01       ; 1
 1d6:   80 93 b0 00     sts     0x00B0, r24     ; 0x8000b0 <__TEXT_REGION_LENGTH__+0x7e00b0>
 1da:   80 91 7a 00     lds     r24, 0x007A     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1de:   84 60           ori     r24, 0x04       ; 4
 1e0:   80 93 7a 00     sts     0x007A, r24     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1e4:   80 91 7a 00     lds     r24, 0x007A     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1e8:   82 60           ori     r24, 0x02       ; 2
 1ea:   80 93 7a 00     sts     0x007A, r24     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1ee:   80 91 7a 00     lds     r24, 0x007A     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1f2:   81 60           ori     r24, 0x01       ; 1
 1f4:   80 93 7a 00     sts     0x007A, r24     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1f8:   80 91 7a 00     lds     r24, 0x007A     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 1fc:   80 68           ori     r24, 0x80       ; 128
 1fe:   80 93 7a 00     sts     0x007A, r24     ; 0x80007a <__TEXT_REGION_LENGTH__+0x7e007a>
 202:   10 92 c1 00     sts     0x00C1, r1      ; 0x8000c1 <__TEXT_REGION_LENGTH__+0x7e00c1>
 206:   8a b1           in      r24, 0x0a       ; 10
 208:   88 6f           ori     r24, 0xF8       ; 248
 20a:   8a b9           out     0x0a, r24       ; 10
 20c:   84 b1           in      r24, 0x04       ; 4
 20e:   83 60           ori     r24, 0x03       ; 3
 210:   84 b9           out     0x04, r24       ; 4
 212:   52 98           cbi     0x0a, 2 ; 10
 214:   8b b1           in      r24, 0x0b       ; 11
 216:   87 70           andi    r24, 0x07       ; 7
 218:   8b b9           out     0x0b, r24       ; 11
 21a:   85 b1           in      r24, 0x05       ; 5
 21c:   8c 7f           andi    r24, 0xFC       ; 252
 21e:   85 b9           out     0x05, r24       ; 5
 220:   95 ea           ldi     r25, 0xA5       ; 165
 222:   b9 2e           mov     r11, r25
 224:   20 e0           ldi     r18, 0x00       ; 0
 226:   c2 2e           mov     r12, r18
 228:   20 e0           ldi     r18, 0x00       ; 0
 22a:   d2 2e           mov     r13, r18
 22c:   89 b1           in      r24, 0x09       ; 9
 22e:   98 2f           mov     r25, r24
 230:   99 0f           add     r25, r25
 232:   89 27           eor     r24, r25
 234:   82 ff           sbrs    r24, 2
 236:   23 c0           rjmp    .+70            ; 0x27e <main+0x108>
 238:   88 e6           ldi     r24, 0x68       ; 104
 23a:   e8 2e           mov     r14, r24
 23c:   80 e0           ldi     r24, 0x00       ; 0
 23e:   f8 2e           mov     r15, r24
 240:   00 e0           ldi     r16, 0x00       ; 0
 242:   f7 01           movw    r30, r14
 244:   84 91           lpm     r24, Z
 246:   88 23           and     r24, r24
 248:   61 f0           breq    .+24            ; 0x262 <main+0xec>
 24a:   e8 2f           mov     r30, r24
 24c:   eb 25           eor     r30, r11
 24e:   85 3a           cpi     r24, 0xA5       ; 165
 250:   41 f0           breq    .+16            ; 0x262 <main+0xec>
 252:   89 b1           in      r24, 0x09       ; 9
 254:   98 2f           mov     r25, r24
 256:   99 0f           add     r25, r25
 258:   89 27           eor     r24, r25
 25a:   82 fd           sbrc    r24, 2
 25c:   18 c0           rjmp    .+48            ; 0x28e <main+0x118>
 25e:   0e 94 6a 00     call    0xd4    ; 0xd4 <_ZL2z1v>
 262:   1a 82           std     Y+2, r1 ; 0x02
 264:   19 82           std     Y+1, r1 ; 0x01
 266:   89 81           ldd     r24, Y+1        ; 0x01
 268:   9a 81           ldd     r25, Y+2        ; 0x02
 26a:   8c 32           cpi     r24, 0x2C       ; 44
```  
We use `avr-objdump -s -j .text firmware.elf` to see the encoded data   
```bash 
lallu@VedantLohan:~/picoctf/bare$ avr-objdump -s -j .text firmware.elf

firmware.elf:     file format elf32-avr

Contents of section .text:
 0000 0c944b00 0c946800 0c946800 0c946800  ..K...h...h...h.
 0010 0c946800 0c946800 0c946800 0c946800  ..h...h...h...h.
 0020 0c946800 0c946800 0c946800 0c946800  ..h...h...h...h.
 0030 0c946800 0c946800 0c946800 0c946800  ..h...h...h...h.
 0040 0c947100 0c946800 0c946800 0c946800  ..q...h...h...h.
 0050 0c946800 0c946800 0c946800 0c946800  ..h...h...h...h.
 0060 0c946800 0c946800 f1e3e6e6 f1e3def1  ..h...h.........
 0070 cd94d6fa 94d6fad6 cac896fa d694c8d5  ................
 0080 c996fa91 d7c1d094 cbcafac3 94d7c8d2  ................
 0090 91d7c0d8 00001124 1fbecfef d8e0debf  .......$........
 00a0 cdbf11e0 a0e0b1e0 e4eef2e0 02c00590  ................
 00b0 0d92ae34 b107d9f7 21e0aee4 b1e001c0  ...4....!.......
 00c0 1d92a735 b207e1f7 0e94bb00 0c947001  ...5..........p.
 00d0 0c940000 8bb18770 8bb985b1 8c7f85b9  .......p........
 00e0 08951f92 0f920fb6 0f921124 2f933f93  ...........$/.?.
 00f0 8f939f93 af93bf93 80915301 90915401  ..........S...T.
 0100 a0915501 b0915601 30915201 23e0230f  ..U...V.0.R.#.#.
 0110 2d3758f5 0196a11d b11d2093 52018093  -7X....... .R...
 0120 53019093 5401a093 5501b093 56018091  S...T...U...V...
 0130 4e019091 4f01a091 5001b091 51010196  N...O...P...Q...
 0140 a11db11d 80934e01 90934f01 a0935001  ......N...O...P.
 0150 b0935101 bf91af91 9f918f91 3f912f91  ..Q.........?./.
 0160 0f900fbe 0f901f90 189526e8 230f0296  ..........&.#...
 0170 a11db11d d2cfcf93 df9300d0 cdb7deb7  ................
 0180 789484b5 826084bd 84b58160 84bd85b5  x....`.....`....
 0190 826085bd 85b58160 85bd8091 6e008160  .`.....`....n..`
 01a0 80936e00 10928100 80918100 82608093  ..n..........`..
 01b0 81008091 81008160 80938100 80918000  .......`........
 01c0 81608093 80008091 b1008460 8093b100  .`.........`....
 01d0 8091b000 81608093 b0008091 7a008460  .....`......z..`
 01e0 80937a00 80917a00 82608093 7a008091  ..z...z..`..z...
 01f0 7a008160 80937a00 80917a00 80688093  z..`..z...z..h..
 0200 7a001092 c1008ab1 886f8ab9 84b18360  z........o.....`
 0210 84b95298 8bb18770 8bb985b1 8c7f85b9  ..R....p........
 0220 95eab92e 20e0c22e 20e0d22e 89b1982f  .... ... ....../
 0230 990f8927 82ff23c0 88e6e82e 80e0f82e  ...'..#.........
 0240 00e0f701 84918823 61f0e82f eb25853a  .......#a../.%.:
 0250 41f089b1 982f990f 892782fd 18c00e94  A..../...'......
 0260 6a001a82 19828981 9a818c32 914048f4  j..........2.@H.
 0270 00008981 9a810196 9a838983 f4cf0e94  ................
 0280 6a00c114 d10491f2 0e940000 cfcfe053  j..............S
 0290 10e0ee34 20f4f0e0 e050ff4f 10810e94  ...4 ....P.O....
 02a0 6a0010fd 5b9a11fd 5c9a12fd 5d9a13fd  j...[...\...]...
 02b0 5e9a14fd 5f9a15fd 289a16fd 299a802f  ^..._...(...)../
 02c0 8f71835d 815038f0 efe9ffe0 3197f1f7  .q.].P8.....1...
 02d0 00c00000 f7cfffef ef1aff0a 0b5db1cf  .............]..
 02e0 f894ffcf                             ....
```  
In the disassembly we see that the program reads bytes from its program memory, XORs each byte with the constant 0xA5, and uses the result.  
```bash
ldi  r25, 0xA5   ; loads the number A5 into a register
lpm  r30, Z      ; loads a byte from program memory where the flag must have been stored
eor  r30, r25    ; XORs the byte with 0xA5
```  
In this `.text` section we had a strange looking section like  
`f1 e3 e6 e6 f1 e3 de f1 ...`  
- Took those bytes and XOR’d each one with 0xA5.

Decoding the message by XOR'ing those strange bytes again with 0xA5.  
```c++
#include <iostream>
using namespace std;

int main() {
    unsigned char data[] = {
        0xf1, 0xe3, 0xe6, 0xe6, 0xf1, 0xe3, 0xde, 0xf1,
        0xcd, 0x94, 0xd6, 0xfa, 0x94, 0xd6, 0xfa, 0xd6,
        0xca, 0xc8, 0x96, 0xfa, 0xd6, 0x94, 0xc8, 0xd5,
        0xc9, 0x96, 0xfa, 0x91, 0xd7, 0xc1, 0xd0, 0x94,
        0xcb, 0xca, 0xfa, 0xc3, 0x94, 0xd7, 0xc8, 0xd2,
        0x91, 0xd7, 0xc0, 0xd8
    };

    int length = sizeof(data);
    unsigned char key = 0xA5;

    cout << "Decoded text: ";
    for (int i = 0; i < length; i++) {
        cout << (char)(data[i] ^ key);
    }
    cout << endl;

    return 0;
}
```  
`Decoded text: TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}`


## Flag:

```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```

## Concepts learnt:
- `simavr`helps simulate AVR microcontrollers  
- `avr-objdump` disassembles and inspects AVR binaries  
- XOR is its own reverse which means if you use XOR again with the key and the encoded data, you get the decoded data.  

## Notes:
- Mistake I made was that I tried to execute this program but obviously it didn't run on my machine.  
- I also tried `strings` command on it but it didn't provide anything useful.  
- `lallu@VedantLohan:~/picoctf/bare$ simavr -m atmega328p -f 16000000 firmware.elf` didn't work because it communicates with hardware and not the console. I don't have a physical UART port.  
- I STILL DON'T UNDERSTAND WHAT WAS THE DESC ABOUT??

## Resources:
[simavr](https://fabricesalvaire.github.io/simavr/gitorious-main.html)  
[avr objdump](https://stackoverflow.com/questions/919781/how-can-i-get-an-assembly-language-listing-of-my-arduino-sketches-on-windows)  
[chatgpt](https://chatgpt.com/) //to help understand the disassembly and instruction flow

***
# 3. IQ test

> let your input x = 30478191278.

wrap your answer with nite{ } for the flag.

As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## Solution:
We need to solve these logic gates to get the flag.  
1) First we convert given x=`30478191278` to binary so we can input it.  
```
11100011000101001000100101010101110
```
![SOLUTION](https://drive.google.com/uc?id=1gglJC6t7g4HC-oNrzDLmuTP0hmm3GH7N)  

## Flag:

```
nite{100010011000}
```

## Concepts learnt:
keep trying

## Notes:
nada

## Resources:
nada


***
