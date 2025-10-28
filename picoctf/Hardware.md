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
